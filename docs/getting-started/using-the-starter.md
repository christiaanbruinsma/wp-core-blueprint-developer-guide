# Using the Starter Plugin

The Core Blueprint First-Party Extension Starter is the canonical minimal example of a plugin that consumes the Base public extension boundary.

It currently demonstrates:

- Base dependency enforcement;
- `CB_CORE_API_VERSION` compatibility;
- inert runtime behavior without compatible Base;
- `ExtensionRegistry`;
- an extension configuration provider registered through `SettingsRegistry` on `cb_core_register_settings`;
- canonical settings/deep links through `SettingsRegistry::url()`;
- semantic Design Foundation requirements;
- provider-scoped extension CSS;
- lazy module health/status;
- Governance `EventRegistry` metadata;
- Governance writes through `Audit::record()`;
- WordPress-safe translation loading on `init`;
- source conformance checks.

The current Starter reference baseline targets Base public API `1.0`. Runtime compatibility is based on the public API plus the concrete public contracts the Starter consumes rather than a specific Base RC build.

Its settings example models **configuration**, so it lives in **Core Blueprint → Extensions**. Do not infer from the Starter that operational workflows should also move there.

## What the Starter deliberately omits

The Starter does not enable custom database tables, REST endpoints, AJAX handlers, cron, module activation, or an operational Core Admin workspace by default.

That is intentional.

Add an optional subsystem only when the product actually needs it, and verify the relevant public Base contract before implementation. Normal WordPress APIs remain valid where Base does not own the concern.

## Read code as an example, not authority

When copying or adapting an example:

1. verify the current Base `docs/PUBLIC-API.md`;
2. verify any linked Foundation document, including `SETTINGS-HUB-FOUNDATION.md` when using extension settings;
3. compare the current Starter implementation;
4. prefer Base documentation when they disagree.

The Starter records the Base source revision against which it was verified, but Core Blueprint is still pre-v1. Re-check current `main` before publishing new guide examples.

## Remove what you do not need

A derived extension should become smaller when possible.

For example:

- no extension configuration → remove the example settings provider/Admin classes and remove `SettingsRegistry` from the bootstrap readiness check;
- no operational Core Admin workspace → do not add `Page`/`PageRegistry` merely because the Starter has Core Admin-styled settings;
- no health projection → remove the status definition and `status_id`;
- no governance-relevant mutation → remove the example Governance class;
- no product-owned persistent data → do not add an `uninstall.php` placeholder.

The bootstrap dependency gate should contain only public Base contracts the derived extension actually consumes.

When adapting the settings example, preserve these boundaries:

- reuse the existing `ExtensionRegistry` ID;
- register during `cb_core_register_settings`;
- use `SettingsRegistry::url()` for provider links;
- keep fields, validation/save authority and domain semantics extension-owned;
- do not supply `official`, `first_party`, `developer_name` or `developer_url` provider metadata;
- do not retain a legacy flat Core Blueprint settings submenu after migration.

Base derives developer identity and first-party provenance from extension identity. Third-party providers remain attributed to and supported by their own developer; settings metadata cannot promote them to official Core Blueprint provenance.

## Next steps

- [Identity and compatibility](identity-and-compatibility.md)
- [Creating an extension](creating-an-extension.md)
- [ExtensionRegistry](../platform/extension-registry.md)
- [Core Admin pages](../core-admin/pages.md)
- [Design Foundation](../core-admin/design-foundation.md)
- [Extension assets and composition](../core-admin/extension-assets.md)
- [Conformance testing](../distribution/conformance-testing.md)
