# Creating an extension

For most new Core Blueprint extensions, start from the [Core Blueprint Extension Starter](https://github.com/christiaanbruinsma/wp-core-blueprint-first-party-starter-plugin).

The Starter is intentionally small. Treat it as a conformance specimen, not as a framework that every feature must retain.

## 1. Perform a complete identity pass

Before feature development, replace the Starter identity consistently:

- canonical plugin folder;
- main plugin filename;
- plugin name and description;
- text domain;
- constant prefix;
- PHP namespace and autoloader prefix;
- ExtensionRegistry ID;
- Core Admin operational page slug, if used;
- extension-owned asset handles/classes;
- example Governance namespace/event, if governance is needed.

First-party Core Blueprint extensions use the reserved `core-blueprint-*` namespace and keep the plugin Author header exactly `Core Blueprint`.

Then narrow the Starter dependency check to the public Base contracts the derived extension actually consumes.

See [Identity and compatibility](identity-and-compatibility.md).

## 2. Keep the Base dependency explicit

Define the Core API version your extension requires and refuse activation when those public contracts are unavailable.

Runtime readiness should also fail closed when a concrete documented public contract the product actually consumes is unavailable. Prefer API-family compatibility plus required public capability checks over pinning runtime behavior to an internal RC number.

If Base later disappears or becomes incompatible, the extension should remain inert. Do not create a duplicate fallback Core Admin implementation or a local legacy replacement for a required Base Foundation.

See [Requirements](requirements.md) and [Identity and compatibility](identity-and-compatibility.md).

## 3. Register canonical extension identity

Register the active extension through `CB\Core\ExtensionRegistry` during `cb_core_register_extensions`.

The extension ID is the platform identity. The WordPress plugin basename is an inventory locator, not a second identity.

When the extension contributes configuration through `SettingsRegistry`, reuse this same extension ID. Settings registration does not create a second product identity.

See [ExtensionRegistry](../platform/extension-registry.md).

## 4. Choose the correct admin surface

Do not assume every extension surface belongs in the same Core Blueprint submenu.

- **Configuration/settings** → use `CB\Core\Admin\SettingsRegistry` during `cb_core_register_settings`; configuration lives in **Core Blueprint → Extensions**.
- **Genuine operational Core Admin page** → use `CB\Core\Admin\Page` and `PageRegistry` when the workflow truly belongs beneath the Core Blueprint admin menu.
- **Standalone WordPress administration surface** → keep WordPress-native presentation and opt into narrow Foundation behavior only where supported.
- **Frontend presentation** → remains product-owned.

Operational queues, lists, editors, reports, schedules and other workflows do not automatically belong in Extensions merely because the product also has settings. `SettingsRegistry` is for configuration/settings, not a generic workspace registry.

See [Core Admin pages](../core-admin/pages.md) and [Presentation boundaries](../core-admin/presentation-boundaries.md).

## 5. Add only the shared contracts you actually need

Examples:

- SettingsRegistry for extension configuration/settings;
- PageRegistry for a genuine operational Core Admin page;
- Integration Grid for provider/integration-level readiness cards;
- Detail Rows for concrete object/target/resource rows inside a consumer-owned Card or section;
- a Foundation runtime for a shared interaction such as modal or toast;
- module status when a useful health projection exists;
- Governance for meaningful mutations;
- SchemaRegistry only when a custom table is genuinely appropriate.

Do not keep optional Starter examples as dormant boilerplate.

When configuration is present:

- register it on `cb_core_register_settings`;
- reuse the existing ExtensionRegistry ID;
- use `SettingsRegistry::url()` for canonical settings/deep links;
- do not keep a legacy flat Core Blueprint settings submenu as a compatibility alias;
- do not submit `official`, `first_party`, `developer_name` or `developer_url` as settings-provider metadata.

Base derives developer identity and first-party provenance from the extension identity. A third-party provider cannot claim Core Blueprint first-party provenance through Settings metadata. An optional provider `support_url` remains developer support attribution.

When both Integration Grid and Detail Rows are present, keep their levels separate: provider readiness belongs in Integration Grid; nested setup targets/resources belong in Detail Rows. See [Integration and detail surfaces](../core-admin/integration-and-detail-surfaces.md).

## 6. Keep product logic and composition extension-owned

The extension owns its domain logic, persistence semantics, feature-specific components, workflows, business validation, settings fields/save behavior, and product-specific composition.

Base owns shared Core Admin presentation and documented cross-suite primitives. For the Extensions settings surface, Base also owns the route/shell, provenance/developer presentation, capability filtering and shared semantic requirement resolution.

See [Extension assets and composition](../core-admin/extension-assets.md).

Base should not become a service locator for ordinary WordPress development.

## 7. Validate before packaging

At minimum:

- run the Starter-derived conformance tooling;
- run PHP/static checks;
- test activation with compatible Base;
- test dependency failure/inert behavior;
- test Core Admin light and dark themes when applicable;
- when settings are exposed, verify the provider appears under Core Blueprint → Extensions and its canonical links use `SettingsRegistry::url()`;
- verify no obsolete flat settings submenu remains after migration;
- when used, verify Integration Grid and Detail Rows at desktop and narrow/mobile widths;
- verify no local CSS redraws Base-owned primitives;
- verify there are no PHP notices, missing dependencies, or early translation warnings;
- package with the canonical plugin root folder.

Continue with [ExtensionRegistry](../platform/extension-registry.md).
