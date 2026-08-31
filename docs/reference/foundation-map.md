# Foundation map

This page is a navigation/index layer. It does not redefine Foundation behavior.

Verify exact current behavior in Core Blueprint Base.

## Teaching model

The Developer Guide groups shared UI concepts into three teaching categories:

1. **Core Admin semantic components** — requested through `PageRegistry`; Base owns Core Admin presentation.
2. **Behavioral Foundations** — shared runtime/interaction contracts with context-appropriate presentation.
3. **Composition primitives** — shared composition semantics where child components retain their own ownership.

This grouping is guidance, not a new Base registry or formal API taxonomy.

## Core Admin semantic components

The current public Core Admin Design Foundation documents semantic requirements such as:

- `nav-tabs`
- `panels`
- `cards`
- `metric-tiles`
- `notices`
- `fields`
- `radio-cards`
- `master-switch`
- `disclosure`
- `badges`
- `state-badges`
- `status`
- `empty-state`
- `kv-table`
- `form-controls`
- `description-toggle`

**Normative source:** Base [`docs/CORE-ADMIN-DESIGN-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/CORE-ADMIN-DESIGN-FOUNDATION.md) and current [`docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md).

Do not translate these IDs into private CSS handles.

## Behavioral Foundations with dedicated Base documents

### Toast
Public runtime feedback primitive.

**Normative source:** [`docs/TOAST-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/TOAST-FOUNDATION.md)

### Modal
Public `<dialog>`-based confirmation/input/reference runtime.

**Normative source:** [`docs/MODAL-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/MODAL-FOUNDATION.md)

### Clipboard
Public copy-to-clipboard behavior with shared feedback/accessibility.

**Normative source:** [`docs/CLIPBOARD-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/CLIPBOARD-FOUNDATION.md)

### Token Input
Progressive enhancement for serialized consumer-defined tokens.

**Normative source:** [`docs/TOKEN-INPUT-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/TOKEN-INPUT-FOUNDATION.md)

### Choice Group
Grouped native checkbox/radio presentation primitive.

**Normative source:** [`docs/CHOICE-GROUP-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/CHOICE-GROUP-FOUNDATION.md)

### Object Picker
Async object search/selection primitive; consumer owns transport authorization and object semantics.

**Normative source:** [`docs/OBJECT-PICKER-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/OBJECT-PICKER-FOUNDATION.md)

### Select Picker
Progressive enhancement for larger/grouped native single selects.

**Normative source:** [`docs/SELECT-PICKER-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/SELECT-PICKER-FOUNDATION.md)

## Public primitives listed by the Foundation contract

Base's current public Foundation contract also lists public primitives such as:

- Icons
- Time Picker
- Icon Picker
- Capability Picker

Some publicly listed primitives do not currently have an equivalent dedicated normative Foundation document.

For those primitives:

- the public API/Foundation map may identify their availability;
- follow the current Base public contract for what is actually specified;
- do not infer missing normative behavior from internal implementation source.

## Composition

### Stack

`cb-core-stack` owns vertical spacing between direct child components.

**Normative source:** [`docs/STACK-LAYOUT-PRIMITIVE.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/STACK-LAYOUT-PRIMITIVE.md)

## Presentation boundary

Core Admin presentation and standalone WordPress-native presentation are different supported contexts.

See [Presentation boundaries](../core-admin/presentation-boundaries.md).
