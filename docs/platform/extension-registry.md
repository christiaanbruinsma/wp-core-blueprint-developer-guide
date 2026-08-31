# ExtensionRegistry

`CB\Core\ExtensionRegistry` is the canonical public identity and inventory boundary for active Core Blueprint extensions.

Register during `cb_core_register_extensions`.

## Registration example

```php
use CB\Core\ExtensionRegistry;

add_action('cb_core_register_extensions', static function (): void {
    ExtensionRegistry::register([
        'id'           => 'vendor-security',
        'plugin_file'  => plugin_basename(VENDOR_SECURITY_FILE),
        'requires_api' => '1.0',
        'menu_url'     => admin_url('admin.php?page=vendor-security'),
        'status_id'    => 'vendor-security',
    ]);
});
```

Check current Base public documentation before relying on optional fields.

## One canonical identity

`id` is the Core Blueprint platform identity.

`plugin_file` is only the canonical WordPress plugin basename used to resolve installed/active state. It is not an alternate extension ID.

`status_id`, when present, references the separate module status registry. It is not a second extension identity.

## ID rules

Current public documentation requires strict lower-case namespaced kebab-case IDs.

The `core-blueprint-*` namespace is reserved for first-party Core Blueprint plugins and is protected by additional folder/Author ownership checks.

Duplicate IDs or duplicate plugin basename ownership are rejected rather than overwritten.

## API compatibility

`requires_api` is required and uses Core API `major.minor`.

A normal extension should depend on the public API contract rather than an exact Base release. Use `requires_base` only when a concrete Base product release is genuinely required beyond the public API.

## Inventory projection

Base exposes `ExtensionRegistry::snapshot()` and `ExtensionRegistry::get($id)` for the Base-owned inventory projection.

The public model keeps these concerns distinct:

- installed;
- active;
- registered;
- compatible;
- health.

Do not infer trust from plugin headers or installed state.

## Health is separate

An extension can reference a status provider, but extension identity and health are separate platform concerns.

Activation/state and health are also separate: an enabled feature can be unhealthy, and a deliberately disabled feature can be healthy in the sense that its state is known.

For the exact current fields and validation rules, see Base `docs/PUBLIC-API.md`.
