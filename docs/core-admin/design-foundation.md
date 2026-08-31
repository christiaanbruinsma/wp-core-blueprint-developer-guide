# Design Foundation

Core Blueprint Base owns the canonical shared Core Admin visual language and several shared behavior primitives.

The Developer Guide uses three **teaching categories** to help developers reason about ownership. They are not new registries or a new technical API layer.

## 1. Core Admin semantic components

These are requested by a registered Core Admin page through semantic `PageRegistry` component requirements.

Examples include:

- panels;
- cards;
- notices;
- fields;
- nav tabs;
- radio cards;
- master switch;
- disclosure;
- badges/state badges;
- status;
- empty state;
- key/value tables;
- form controls.

The model is:

```text
semantic component ID
→ documented markup/behavior contract
→ Base-owned presentation/assets
```

Base may change CSS filenames, handles, bundle boundaries, or internal asset organization while preserving the semantic contract.

## 2. Behavioral Foundations

Behavioral Foundations expose a shared runtime/interaction contract and a context-appropriate presentation.

Examples documented by Base include Modal, Toast, Clipboard, Token Input, Choice Group, Object Picker, and Select Picker, with additional public primitives listed in the current Foundation contract.

On Core Admin screens, PageRegistry should normally request the primitive semantically.

On standalone wp-admin screens, use the documented narrow `CB\Core\UI\Assets` enqueue helper when the Foundation supports that context.

Do not import Foundation implementation files directly.

## 3. Composition primitives

Composition primitives solve shared layout relationships without taking ownership of child components.

For example, `cb-core-stack` owns vertical spacing between direct siblings. Child Foundations retain ownership of their own internal geometry.

## Extension-owned composition

An extension may own:

- grids;
- workflow layout;
- feature-specific preview sizing;
- product-specific components;
- layout constraints.

Prefer `--cb-*` tokens where practical on Core Admin pages.

Do not locally redraw shared Base primitives by changing their generic colours, typography, surfaces, borders, radii, spacing, shadows, focus, hover, or semantic states.

## Do not depend on asset handles

Private handles such as `cb-core-css-*` are not public extension API.

Ask for the semantic requirement or use the documented Foundation helper/runtime identifier instead.

## Normative source

Exact markup, options, variants, and behavior belong in Base documentation, not this guide.

Use the [Foundation map](../reference/foundation-map.md) to find the current normative contract.
