# Using the Starter Plugin

The Core Blueprint Extension Starter is the canonical minimal example of a plugin that consumes the Base public extension boundary.

It currently demonstrates:

- Base dependency enforcement;
- `CB_CORE_API_VERSION` compatibility;
- inert runtime behavior without compatible Base;
- `ExtensionRegistry`;
- a Core Admin `Page` registered through `PageRegistry`;
- semantic Design Foundation requirements;
- exact screen-scoped extension CSS;
- lazy module health/status;
- Governance `EventRegistry` metadata;
- Governance writes through `Audit::record()`;
- WordPress-safe translation loading on `init`;
- source conformance checks.

The current Starter 0.1.1 reference baseline is Base public API `1.0`, verified against Base `1.0.0-rc3.32`. Runtime compatibility is still based on the API contract rather than that concrete Base release.

## What the Starter deliberately omits

The Starter does not enable custom database tables, REST endpoints, AJAX handlers, cron, or module activation by default.

That is intentional.

Add an optional subsystem only when the product actually needs it, and verify the relevant public Base contract before implementation. Normal WordPress APIs remain valid where Base does not own the concern.

## Read code as an example, not authority

When copying or adapting an example:

1. verify the current Base `docs/PUBLIC-API.md`;
2. verify any linked Foundation document;
3. compare the current Starter implementation;
4. prefer Base documentation when they disagree.

The Starter records the Base source revision against which it was verified, but Core Blueprint is still pre-v1. Re-check current `main` before publishing new guide examples.

## Remove what you do not need

A derived extension should become smaller when possible.

For example:

- no Core Admin page → remove the example `Admin` classes/CSS **and** remove Page/PageRegistry from the bootstrap readiness check;
- no health projection → remove the status definition and `status_id`;
- no governance-relevant mutation → remove the example Governance class;
- no product-owned persistent data → do not add an `uninstall.php` placeholder.

The bootstrap dependency gate should contain only public Base contracts the derived extension actually consumes.

## Next steps

- [Identity and compatibility](identity-and-compatibility.md)
- [Creating an extension](creating-an-extension.md)
- [ExtensionRegistry](../platform/extension-registry.md)
- [Core Admin pages](../core-admin/pages.md)
- [Design Foundation](../core-admin/design-foundation.md)
- [Extension assets and composition](../core-admin/extension-assets.md)
- [Conformance testing](../distribution/conformance-testing.md)
