# Creating an extension

For most new Core Blueprint extensions, start from the [Core Blueprint Extension Starter](https://github.com/christiaanbruinsma/wp-core-blueprint-starter-plugin).

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
- Core Admin page slug, if used;
- extension-owned asset handles/classes;
- example Governance namespace/event, if governance is needed.

First-party Core Blueprint extensions use the reserved `core-blueprint-*` namespace and keep the plugin Author header exactly `Core Blueprint`.

## 2. Keep the Base dependency explicit

Define the Core API version your extension requires and refuse activation when those public contracts are unavailable.

If Base later disappears or becomes incompatible, the extension should remain inert. Do not create a duplicate fallback Core Admin implementation.

See [Requirements](requirements.md).

## 3. Register canonical extension identity

Register the active extension through `CB\Core\ExtensionRegistry` during `cb_core_register_extensions`.

The extension ID is the platform identity. The WordPress plugin basename is an inventory locator, not a second identity.

See [ExtensionRegistry](../platform/extension-registry.md).

## 4. Decide whether the feature needs Core Admin

Do not assume every extension page belongs under the Core Blueprint menu.

- If the page belongs to Core Admin, use `CB\Core\Admin\Page` and `PageRegistry`.
- If it is a standalone WordPress administration surface, keep WordPress-native presentation and opt into narrow Foundation behavior only where supported.
- Frontend presentation remains product-owned.

See [Presentation boundaries](../core-admin/presentation-boundaries.md).

## 5. Add only the shared contracts you actually need

Examples:

- PageRegistry for a Core Admin page;
- a Foundation runtime for a shared interaction such as modal or toast;
- module status when a useful health projection exists;
- Governance for meaningful mutations;
- SchemaRegistry only when a custom table is genuinely appropriate.

Do not keep optional Starter examples as dormant boilerplate.

## 6. Keep product logic extension-owned

The extension owns its domain logic, persistence semantics, feature-specific components, workflows, and business validation.

Base should not become a service locator for ordinary WordPress development.

## 7. Validate before packaging

At minimum:

- run the Starter-derived conformance tooling;
- run PHP/static checks;
- test activation with compatible Base;
- test dependency failure/inert behavior;
- test Core Admin light and dark themes when applicable;
- verify there are no PHP notices, missing dependencies, or early translation warnings;
- package with the canonical plugin root folder.

Continue with [Using the Starter](using-the-starter.md).
