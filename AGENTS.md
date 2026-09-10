# AGENTS.md

This file provides repository-specific guidance for coding agents working on the BOX NOW Delivery WordPress/WooCommerce plugin.

## Source of project context

Read [CLAUDE.md](CLAUDE.md) before making changes. It contains the project overview, architecture, configuration keys, internationalization details, and WordPress/WooCommerce integration points.

`CLAUDE.md` was written for a checkout that contained sibling plugin directories. In this checkout, the repository root is the active plugin directory. Treat paths such as `bx_wp_plugin_v3.0.0_global/box-now-delivery.php` in that file as `box-now-delivery.php`, and make requested changes in this repository. If this file and `CLAUDE.md` conflict, follow this file and the user's current instructions.

## Repository layout

- `box-now-delivery.php`: plugin entry point and most hook/AJAX registration.
- `includes/`: shipping, API, validation, voucher, label, order-column, cancellation, and settings modules.
- `js/`: checkout and WordPress admin behavior. Checkout code supports both Classic and Block checkout.
- `css/`: frontend and admin styles.
- `languages/`: PHP translation maps for the `boxnowbulgaria` text domain.
- `readme.txt`: WordPress plugin metadata and changelog.

This is a plain PHP plugin. There is no Composer, npm, bundler, or generated build output. WordPress loads these files directly.

## Working rules

- Keep changes narrowly scoped; do not refactor large legacy files unless the task requires it.
- Preserve compatibility with PHP 7.0+, WordPress 6.2+, WooCommerce Classic checkout, Block checkout, legacy orders, and HPOS.
- Use WooCommerce order CRUD APIs instead of direct post-meta access for order data.
- Follow existing WordPress/WooCommerce hooks, naming, and local formatting in the file being edited.
- Sanitize and validate request data, escape rendered output, verify nonces, and check capabilities for privileged actions.
- Never expose API credentials, customer data, voucher PDFs, or authenticated AJAX operations publicly.
- Use the `boxnowbulgaria` text domain for user-visible translatable strings and update the relevant translation maps when changing them.
- Keep country-specific behavior valid for Bulgaria, Greece, Croatia, and Cyprus; do not assume Bulgarian endpoints or formats globally.
- When changing checkout JavaScript, check both popup and embedded locker maps and both Classic and Block checkout flows.
- Do not edit version numbers or changelog entries unless the task explicitly includes a release/version change.

## Validation

There is no automated test suite or configured linter. At minimum, syntax-check every changed PHP file:

```powershell
php -l box-now-delivery.php
php -l includes/path-to-changed-file.php
```

For a repository-wide PHP syntax check in PowerShell:

```powershell
Get-ChildItem -Recurse -Filter *.php | ForEach-Object { php -l $_.FullName; if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE } }
```

`check-syntax.php` is a WordPress-environment helper, not a standalone full-suite test: it checks the main plugin file and then attempts to load WordPress from a relative path. Use it only when the expected WordPress installation exists around this directory.

For behavior changes, manually verify the affected flow in a WordPress/WooCommerce installation when one is available. Report any validation that could not be run.
