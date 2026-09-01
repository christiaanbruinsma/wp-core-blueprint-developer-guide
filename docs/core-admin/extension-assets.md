# Extension assets and composition

A registered Core Admin page has two asset owners:

```text
Base
→ shared Core Admin shell
→ declared Foundation/components
→ canonical shared presentation

Extension
→ product behavior
→ feature-specific layout/composition
→ product-specific components/assets
```

The governing rule is:

> **Base owns the language. The extension owns the composition.**

## Declare shared UI through PageRegistry

For a registered Core Admin page, request shared UI semantically:

```php
PageRegistry::register(
    new SettingsPage(),
    [
        'foundations' => [ 'modal', 'toast' ],
        'components'  => [ 'nav-tabs', 'panels', 'form-controls' ],
    ]
);
```

Do not manually enqueue Base Foundation assets on the same page when `PageRegistry` can own them declaratively.

Do not depend on private Base handles such as `cb-core-css-*`, CSS filenames, internal bundles or `AdminAssetCatalog`.

## Scope extension-owned assets to the exact page

Use the registered hook suffix:

```php
$expected = PageRegistry::hook_suffix(SettingsPage::SLUG);
if ('' === $expected || $hook !== $expected) {
    return;
}

wp_enqueue_style(
    'vendor-feature-admin',
    VENDOR_FEATURE_URL . 'assets/css/admin.css',
    [],
    VENDOR_FEATURE_VERSION
);
```

Do not guess WordPress hook suffixes and do not load feature assets across all Core Admin pages.

## What extension CSS may own

Typical extension-owned composition includes:

- product grids;
- workflow layout;
- table/grid track distribution for feature-specific data views;
- preview dimensions;
- product-specific toolbar arrangement;
- feature-specific cards or visualizations that are not Base semantic components;
- responsive composition for the product workflow.

On Core Admin pages, use `--cb-*` tokens where practical so product composition follows the shared visual language without copying Base styling.

Example:

```css
.cb-vendor-jobs-grid {
    display: grid;
    grid-template-columns: minmax(0, 1fr) auto;
    gap: var(--cb-space-4);
}
```

## What extension CSS must not redraw

When Base owns a shared primitive, extension CSS must not redefine its generic appearance.

Do not locally redraw:

- buttons;
- text/search inputs, selects or textareas;
- nav tabs;
- panels;
- notices;
- shared cards;
- badges/state badges;
- focus rings;
- shared semantic colours/states;
- generic typography, borders, radii, surfaces or shadows of Base primitives.

A product-specific size/layout constraint can still be legitimate. For example, a fleet search field may own a workflow width or icon-space padding while Base retains its border, radius, typography and focus treatment.

## Product components remain product-owned

Do not force every recurring shape into Base.

A feature-specific fleet table, KPI visualization, deployment workflow or domain-specific control may stay extension-owned when its semantics are product-specific. It should still consume Base tokens/shared primitives where appropriate.

Before requesting a new Base primitive, ask whether the concept is genuinely reusable platform language or merely one product's composition.

## Modal boundary

When Base Modal can represent a workflow lifecycle exactly, Base should own the modal shell/lifecycle/accessibility while the extension owns workflow body/state.

Do not half-migrate a modal shell and do not broaden Base solely to accommodate one product-specific workflow. If the public Modal contract cannot represent the lifecycle exactly, keep the workflow boundary intact and revisit the platform contract deliberately.

## Standalone wp-admin is different

These Core Admin rules do not mean every WordPress admin screen should use the Core Admin theme.

A standalone plugin-owned wp-admin screen remains WordPress-native. It may opt into documented narrow behavioral Foundations through their supported WordPress-native adapters, but it should not import the Core Admin visual shell merely to obtain one interaction.

See [Presentation boundaries](presentation-boundaries.md).

## Testing

For a Core Admin page:

- test light and dark themes;
- verify only required Base primitives are loaded;
- verify extension assets load only on their exact page;
- inspect common inputs/buttons/tabs/panels for local redraws;
- test responsive/product-specific layout separately from Base presentation;
- verify removing/hiding product content does not leave layout gaps caused by extension-owned grid tracks.

Continue with [Design Foundation](design-foundation.md).
