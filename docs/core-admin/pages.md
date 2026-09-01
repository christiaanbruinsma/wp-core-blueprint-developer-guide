# Core Admin pages

Use the public Core Admin page contract when an extension genuinely belongs beneath the Core Blueprint admin menu.

The public boundary is:

- `CB\Core\Admin\Page`
- `CB\Core\Admin\PageRegistry`
- `cb_core_register_pages`

Do not inherit Base's internal `PageBase` convenience implementation.

## Minimal pattern

Implement the public page interface and register the page during `cb_core_register_pages`.

```php
use CB\Core\Admin\Page as PageContract;
use CB\Core\Admin\PageRegistry;

final class SettingsPage implements PageContract
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

    public function slug(): string { return self::SLUG; }
    public function title(): string { return __('Vendor Feature', 'vendor-feature'); }
    public function menu_title(): string { return __('Vendor Feature', 'vendor-feature'); }
    public function capability(): string { return 'manage_options'; }
    public function position(): ?int { return null; }

    public function render(): void
    {
        if (! current_user_can($this->capability())) {
            wp_die(esc_html__('You do not have permission to access this page.', 'vendor-feature'));
        }

        // Extension-owned page content.
    }
}
```

Verify exact interface requirements against current Base source/public documentation before copying this example into production.

## Registration rules

Current public documentation requires:

- globally unique lower-case kebab-case page slugs;
- an explicit valid WordPress capability;
- extension position `null` or `>= 100`;
- only documented semantic `foundations` and `components`;
- no raw asset handles as requirements.

Unknown requirements or duplicate/reserved registrations fail safely rather than replacing existing pages.

## Page-scoped extension assets

Base owns the assets for the semantic requirements declared on the page. Do not manually enqueue those same Base Foundations or depend on private `cb-core-css-*` handles.

An extension may enqueue its own feature-specific CSS/JavaScript. Use `PageRegistry::hook_suffix($slug)` after WordPress menu registration to scope those assets to the exact registered page.

Do not depend on a guessed `hook_suffix` pattern.

For the full ownership model, see [Extension assets and composition](extension-assets.md).

## Rendering security

Page registration does not replace authorization in mutation handlers.

Use explicit capabilities for mutations, nonces where applicable, and normal WordPress validation/escaping rules.

Continue with [Design Foundation](design-foundation.md).
