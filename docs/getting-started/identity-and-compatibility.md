# Identity and compatibility

A Core Blueprint extension has several identifiers, but they do not mean the same thing. Keep them deliberately separate.

## WordPress plugin identity

The installed WordPress plugin identity is anchored by the canonical plugin folder and main plugin file:

```text
vendor-feature/
└── vendor-feature.php
```

That produces the canonical WordPress plugin basename, for example `vendor-feature/vendor-feature.php`.

Choose this identity during the initial Starter identity pass and keep it stable. Packaging must not replace the plugin root with a branch, version, build or temporary directory name.

## Core Blueprint extension identity

`ExtensionRegistry::register()` uses one Core Blueprint platform ID:

```php
ExtensionRegistry::register([
    'id'           => 'vendor-feature',
    'plugin_file'  => plugin_basename(VENDOR_FEATURE_FILE),
    'requires_api' => '1.0',
]);
```

The registry `id` is the platform identity. `plugin_file` is only the WordPress inventory locator.

Do not create alternate aliases for the same extension. `status_id`, when used, points to the separate health registry and is not another identity.

First-party Core Blueprint extensions use the reserved `core-blueprint-*` namespace and keep the plugin Author header exactly `Core Blueprint`.

## API compatibility

Normal extension compatibility targets `CB_CORE_API_VERSION`, not a concrete Base release number.

The v1 Core API uses `major.minor` compatibility:

```text
required 1.0
available 1.0  → compatible
available 1.1  → compatible
available 2.0  → incompatible
```

Use `requires_base` only when the extension genuinely requires behavior from a specific Base product release that is not covered by the documented API contract.

Do not use an exact Base release merely because that was the version present during development.

## Activation and runtime are separate gates

A canonical extension handles dependency failure in two places.

### Activation

Refuse activation when the required public Base API/contracts are unavailable.

This prevents a newly installed extension from entering a partially functional state.

### Runtime after activation

If Base is later deactivated or becomes incompatible, keep the extension inert. Do not recreate a private fallback Core Admin, compatibility shim or duplicate Base implementation.

An administrator dependency notice may explain why the extension is inactive.

## Require only what the extension actually consumes

The Starter demonstrates several public contracts at once. A derived plugin may need fewer.

If you remove the Core Admin example, remove `PageRegistry`/`Page` from the derived bootstrap readiness check as well. If you remove Governance, do not keep Governance classes as artificial prerequisites.

The dependency gate should describe real public API consumption, not the full feature set of the original Starter.

## No WordPress `Requires Plugins` header for first-party Base dependency

First-party Core Blueprint extensions use the runtime Core API dependency gate rather than a WordPress `Requires Plugins: core-blueprint` header.

This preserves the intended platform lifecycle: inactive extensions remain inert, and compatibility is decided against the documented Core API rather than only WordPress plugin presence.

## Identity checklist

Before feature development, verify:

- canonical plugin folder;
- main plugin filename;
- plugin basename;
- plugin header name, author and text domain;
- constant prefix;
- PHP namespace/autoloader prefix;
- ExtensionRegistry ID;
- Core Admin page slug if present;
- extension-owned asset handles/classes;
- Governance event namespace if present;
- required Core API version;
- bootstrap readiness checks include only consumed public contracts.

Next: [ExtensionRegistry](../platform/extension-registry.md).
