# Integration and detail surfaces

Core Blueprint Base provides two related Core Admin presentation primitives for integration-oriented pages. They solve different levels of the UI and should not be collapsed into one component.

## Integration Grid: provider or integration level

Use `CB\Core\UI\IntegrationGrid` when the operator needs to understand the readiness of an integration, provider, optional product, builder, or other high-level connection.

Examples:

- Bricks — Ready
- Payment Provider — Needs setup
- Optional CRM — Optional

A registered Core Admin page requests:

```php
[
    'components' => [ 'integration-grid' ],
]
```

The consumer owns which integrations exist, detection/readiness logic, descriptions, visible status labels, and action destinations. Base owns the responsive grid/card presentation, status placement, CTA/footer presentation, dark/light behavior, and mapping of Integration Grid states to the shared Status primitive.

The four Integration Grid states are:

- `ready`
- `needs-setup`
- `optional`
- `unavailable`

Do not translate those states into local colour classes or redraw the cards in extension CSS.

Normative contract: Base [`docs/INTEGRATION-GRID-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/INTEGRATION-GRID-FOUNDATION.md).

## Detail Rows: object, target, or resource level

Use `CB\Core\UI\DetailRows` for concrete objects, setup targets, resources, assignments, destinations, jobs, or mappings that belong inside a consumer-owned section or card.

Examples:

- Course Single template — Ready — Edit
- Backup destination — Active — Open
- Storage endpoint — Needs attention

A registered Core Admin page requests:

```php
[
    'components' => [ 'detail-rows' ],
]
```

Detail Rows uses the existing generic Status semantics directly:

- `active`
- `ready`
- `warning`
- `error`
- `idle`

A row may omit status entirely. A CTA is rendered only when both its URL and label are present.

Base owns row anatomy, separators, responsive stacking, status placement, CTA alignment, focus/hover, and dark/light presentation. The extension owns the row inventory, domain meaning, status/readiness logic, labels, destinations, and surrounding guidance.

`DetailRows` does **not** render an outer Card. Compose it inside a Base Card or another appropriate consumer-owned section when a framed surface is needed.

Normative contract: Base [`docs/DETAIL-ROWS-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/DETAIL-ROWS-FOUNDATION.md).

## The hard boundary

Use this decision rule:

```text
provider / integration readiness
→ IntegrationGrid

concrete object / target / resource detail
→ DetailRows
```

Do not model nested setup targets as fake Integration Grid entries. Do not extend Integration Grid with nested target/domain semantics. Conversely, do not use Detail Rows as a replacement for provider-level integration readiness.

A page may use both:

```php
PageRegistry::register(
    new IntegrationsPage(),
    [
        'components' => [ 'cards', 'integration-grid', 'detail-rows' ],
    ]
);
```

For example, a Bricks integration may appear once in Integration Grid while a separate consumer-owned Card contains concrete Bricks template mappings rendered through Detail Rows.

## Dependency gating

Runtime compatibility should describe what the extension actually consumes.

Use the supported Core API family as the primary compatibility boundary, then fail closed when a required public contract is unavailable. For example, an extension that renders Detail Rows should verify that the documented renderer it calls is available rather than pinning itself to an internal development RC number.

Do not create a legacy local renderer as a fallback for a missing required Base primitive. If the public contract is required for the extension to boot safely, keep that runtime inert and explain the dependency to the operator.

## Presentation ownership

When either primitive is used, extension CSS may position the whole primitive within product composition but must not locally redraw:

- Integration Grid card surfaces or internal responsive geometry;
- Detail Rows spacing, separators, status placement, CTA alignment, or mobile stacking;
- shared Status or Button appearance;
- Base-owned colours, borders, radii, shadows, hover, focus, or dark/light states.

Product-specific guidance, workflow composition, and domain-specific visualizations remain extension-owned.

Continue with [Extension assets and composition](extension-assets.md).