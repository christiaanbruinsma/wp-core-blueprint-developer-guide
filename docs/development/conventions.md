# Development conventions

Core Blueprint extensions remain WordPress plugins. Use normal WordPress engineering practices unless Base explicitly owns a public cross-suite concern.

## Security

Treat security boundaries as architecture, not as isolated snippets.

For mutations:

- use an explicit capability check;
- use a nonce for browser-originated mutations where applicable;
- use object-level capabilities for object-level actions when appropriate;
- validate and sanitize input at the boundary;
- escape output for its actual context;
- fail closed around filesystem, network, remote, and privilege boundaries where partial execution would be unsafe.

Do not treat a Foundation, source identifier, or Access Mode bypass as authentication.

Never place secrets in logs, HTML, or localized browser data.

## WordPress lifecycle

Choose the narrowest appropriate runtime.

REST, AJAX, admin-post, cron, options, post meta, taxonomies, roles, and other WordPress mechanisms remain valid. Do not invent a Base abstraction when no public Base contract exists.

Keep disabled or irrelevant features inert on request-hot paths.

Read-only health/status providers should remain cheap and must not perform hidden repair or migration work.

## Internationalization

Canonical source language is English.

Current Core Blueprint release languages include English, Dutch, German, French, Spanish, Italian, and Portuguese.

Load the extension text domain at `init` or later. Do not resolve extension translations during early plugin-file inclusion.

## JavaScript

Use vanilla JavaScript and native browser APIs for new extension development.

Do not add jQuery.

When a documented Base Foundation owns a shared interaction, consume that Foundation rather than cloning its behavior in extension JavaScript.

Scope scripts to the exact screen/runtime that needs them and keep browser-provided runtime data minimal.

## Data and storage

Prefer a WordPress-native storage model when it represents the domain correctly.

Before adding a custom table, answer:

1. Why are options/meta/posts/taxonomies not appropriate?
2. Who owns the schema?
3. Is the installer idempotent and re-runnable?
4. What is the repair/verification policy?
5. What happens on uninstall?

If a custom table is appropriate, use the documented `Database\SchemaRegistry` boundary for Base-managed reconciliation.

The extension still owns its schema and uninstall policy.

## Uninstall

Delete only data the extension owns and only according to an explicit product policy.

Do not remove Base-owned state, shared audit storage, unrelated WordPress content, or data owned by another plugin.

If the extension owns no persistent data, a no-op uninstall file is unnecessary.
