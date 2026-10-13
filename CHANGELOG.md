# Changelog

Laravel 9 compatibility fork of livewire/livewire. Upstream release notes: https://github.com/livewire/livewire/releases

## Unreleased (laravel9-support fork, based on v3.8.10)

Laravel 9.52 compatibility. On Laravel 10+ each change defers to the original upstream code path.

### Changed

- `composer.json`: widened `illuminate/database`, `routing`, `support` and `validation` to `^9.52|^10.0|^11.0|^12.0|^13.0`.
- `composer.json`: removed `laravel/prompts` from `require`. Every Prompts version requires `illuminate/collections ^10+`; it is only used by `make:livewire`.
- `Mechanisms\FrontendAssets\FrontendAssets`: `Str::isUrl()` (Laravel 10+) replaced by a helper that defers to it when available, otherwise uses a verbatim copy of Laravel 10.48.4's implementation.
- `Features\SupportConsoleCommands\Commands\MakeCommand`: skips the optional "inline component?" and "create test?" questions when Laravel Prompts is not installed. On Laravel 9 this behaves like answering "No"; unchanged where Prompts is installed.

### Known limitations

- On Laravel 10.0–10.16 (no bundled Prompts) `make:livewire` also skips those two questions.
- Livewire's own test suite cannot run on Laravel 9 (dev dependencies require Laravel 10+).
- Security fixes released upstream after v3.8.10 must be merged manually.

### Validation performed

- `php -l` on changed PHP files; `composer validate --no-check-publish --no-check-lock`.
- `isUrl` differential test: 29 inputs, 0 mismatches between the Laravel 9 fallback, the Laravel 10.48.4 path and the reference `Str::isUrl()`.
- `MakeCommand` on Laravel 9.52.20: returns cleanly without Prompts; prompts still run when Prompts functions exist; unpatched control reproduces the `undefined function Laravel\Prompts\confirm()` error.
- Backend integration test (disposable Laravel 9.52.20 app, PHP 8.1.12, with the Laravel 9 Pulse fork): `package:discover`, `artisan list` and `make:livewire` succeed; `/livewire/livewire.js?id=…` is generated through the `isUrl` fallback and served.
- Browser test (headless Chrome via DevTools protocol, Laravel Pulse dashboard): Livewire boots, lazy components load via `/livewire/update` (all HTTP 200), `wire:poll` refreshes, actions update `#[Url]` properties in the browser URL, and URL query parameters hydrate component state; no JavaScript or console errors.
- Not yet run: Livewire's own test suite (requires Laravel 10+ dev dependencies), Laravel 10+ runtime with this fork.
