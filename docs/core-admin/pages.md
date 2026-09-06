# Core Admin pages

Use the public Core Admin page contract when an extension has a genuine operational surface that belongs beneath the Core Blueprint admin menu.

Extension configuration is a separate concern. Configuration/settings belong in **Core Blueprint → Extensions** through `CB\Core\Admin\SettingsRegistry`; do not create a flat Core Blueprint submenu merely to expose settings.

The public operational-page boundary is:

- `CB\Core\Admin\Page`
- `CB\Core\Admin\PageRegistry`
- `cb_core_register_pages`

Do not inherit Base's internal `PageBase` convenience implementation.

## Operational page pattern

Implement the public page interface and register the page during `cb_core_register_pages`.

```php
use CB\Core\Admin\Page as PageContract;
use CB\Core\Admin\PageRegistry;

final class OperationsPage implements PageContract
{
    public const SLUG = 'vendor-feature';

    public static function init(): void
    {
        add_action('cb_core_register_pages', [self::class, 'register']);
    }

    public static function register(): void
    {
        PageRegistry::register(
            new self(),
            [
                'components' => ['panels', 'notices', 'form-controls'],
            ]
        );
    }

    public function slug(): string
    {
        return self::SLUG;
    }

    public function title(): string
    {
        return __('Vendor Feature', 'vendor-feature');
    }

    public function menu_title(): string
    {
        return __('Vendor Feature', 'vendor-feature');
    }

    public function capability(): string
    {
        return 'manage_options';
    }

    public function position(): ?int
    {
        return null;
    }

    public function render(): void
    {
        if (! current_user_can($this->capability())) {
            wp_die(esc_html__('You do not have permission to access this page.', 'vendor-feature'));
        }

        // Extension-owned operational content.
    }
}
```

Verify exact interface requirements against current Base source/public documentation before copying this example into production.

## Extension configuration

Use `SettingsRegistry` only for extension configuration/settings. Operational queues, lists, editors, reports, schedules and other product workflows do not automatically move into Extensions.

A settings provider must reuse the extension's existing canonical `ExtensionRegistry` ID and register during `cb_core_register_settings`.

```php
use CB\Core\Admin\SettingsRegistry;

add_action('cb_core_register_settings', static function (): void {
    SettingsRegistry::register(
        'vendor-feature',
        [
            'label'       => __('Vendor Feature', 'vendor-feature'),
            'description' => __('Configure Vendor Feature.', 'vendor-feature'),
            'group'       => SettingsRegistry::GROUP_OTHER,
            'capability'  => 'manage_options',
            'renderer'    => [Vendor\Feature\Admin\Settings::class, 'render'],
            'icon'        => 'settings',
            'support_url' => 'https://vendor.example/support',
            'requirements' => [
                'foundations' => [],
                'components'  => ['panels', 'notices', 'form-controls'],
            ],
        ]
    );
});
```

Build settings links through the canonical helper:

```php
$url = SettingsRegistry::url('vendor-feature');
$tab = SettingsRegistry::url('vendor-feature', ['tab' => 'integrations']);
```

The extension owns its fields, persistence, validation/save behavior, mutation authorization, domain semantics, provider-specific query arguments and inner renderer.

Base owns the **Core Blueprint → Extensions** shell and routing, capability filtering, grouping, shared semantic Foundation resolution, developer attribution and first-party/third-party provenance presentation.

A provider may supply the documented provider fields, including an optional developer `support_url`. It must not submit `official`, `first_party`, `developer_name` or `developer_url`. Base derives developer identity and first-party provenance from the registered extension identity. A third-party provider cannot promote itself to official Core Blueprint status through settings metadata.

For third-party providers, developer/support attribution remains visible and belongs to that developer, not Core Blueprint.

Do not retain a second flat `PageRegistry` settings page as a pre-v1 compatibility alias after migrating configuration to `SettingsRegistry`.

## Page registration rules

Current public documentation requires for operational `PageRegistry` pages:

- globally unique lower-case kebab-case page slugs;
- an explicit valid WordPress capability;
- extension position `null` or `>= 100`;
- only documented semantic `foundations` and `components`;
- no raw asset handles as requirements.

Unknown requirements or duplicate/reserved registrations fail safely rather than replacing existing pages.

Settings providers have their own documented validation rules. Verify the current Base `SETTINGS-HUB-FOUNDATION.md` and `PUBLIC-API.md` before relying on exact provider fields or group constants.

## Page-scoped extension assets

Base owns the assets for the semantic requirements declared on a registered operational page. Do not manually enqueue those same Base Foundations or depend on private `cb-core-css-*` handles.

Your extension remains free to enqueue its own feature-specific CSS/JavaScript.

Use `PageRegistry::hook_suffix($slug)` after WordPress menu registration to scope extension-owned assets to the exact registered operational page.

Do not depend on a guessed `hook_suffix` pattern.

For settings providers, Base resolves the selected provider's declared semantic requirements through the Settings Hub. Do not depend on Base-private handles there either.

For the full ownership model, see [Extension assets and composition](extension-assets.md).

## Rendering security

Page or settings-provider registration does not replace authorization in mutation handlers.

Use explicit capabilities for mutations, nonces where applicable, and normal WordPress validation/escaping rules.

Continue with [Design Foundation](design-foundation.md).
