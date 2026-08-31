# Public API map

This page is a navigation index. It is **not** a second `PUBLIC-API.md`.

For exact signatures, validation rules, lifecycle details, and compatibility promises, use current Core Blueprint [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md) and its linked documents.

## Core lifecycle and compatibility

**Use for:** knowing when Base is ready and which public API version is available.

**Public surface:** `CB_CORE_API_VERSION`, `cb_core_booted`

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## Extension identity

**Use for:** registering/discovering a Core Blueprint extension and expressing API compatibility.

**Public surface:** `CB\Core\ExtensionRegistry`, `cb_core_register_extensions`

**Guide:** [ExtensionRegistry](../platform/extension-registry.md)

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## Core Admin pages

**Use for:** contributing a page beneath the Core Blueprint admin menu.

**Public surface:** `CB\Core\Admin\Page`, `CB\Core\Admin\PageRegistry`, `cb_core_register_pages`

**Guide:** [Core Admin pages](../core-admin/pages.md)

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md) and [`docs/CORE-ADMIN-DESIGN-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/CORE-ADMIN-DESIGN-FOUNDATION.md)

## Module activation/state

**Use for:** a feature that genuinely needs a canonical Base-managed enabled/disabled state.

**Public surface:** `cb_core_module_activation_definitions`, `CB\Core\Modules\ModuleStateInterface`

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## Module health/status

**Use for:** exposing cheap, read-only `ok|warn|err|off` health information.

**Public surface:** `cb_core_module_status_definitions`

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## Governance and Audit

**Use for:** recording governance-relevant extension events and registering their metadata.

**Public surface:** `CB\Core\Governance\Audit`, `EventRegistry`, retention contracts

**Guide:** [Governance and Audit](../platform/governance-and-audit.md)

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## Database schema registration

**Use for:** extension-owned custom tables that require Base-managed reconciliation.

**Public surface:** `CB\Core\Database\SchemaRegistry`

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## Dashboard Card API

**Use for:** declaratively contributing navigation shortcuts to the extension/module dashboard card.

**Public surface:** Dashboard Card registration hooks/registry

**Normative source:** [Base `docs/DASHBOARD-CARD-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/DASHBOARD-CARD-API.md)

## HUD Registry API

**Use for:** declarative HUD sections/items using controlled Base presentation.

**Public surface:** HUD registration hooks/registries

**Normative source:** [Base `docs/HUD-MENU-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/HUD-MENU-API.md)

## Access Mode integration

**Use for:** keeping a narrow extension-owned machine/webhook route reachable through Access Mode.

**Public surface:** `CB\Core\Security\AccessMode::register_bypass()` and documented advanced filter

**Normative source:** [Base `docs/access-mode-integration.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/access-mode-integration.md) and [`docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## Content Models

**Use for:** registering runtime-owned Content Models or consuming the public schema/value API.

**Public surface:** `CB\Core\ContentModels\Api`, `cb_core_content_models_register`, Content Models JSON Schema v1

**Normative source:** [Base `docs/CONTENT_MODELS.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/CONTENT_MODELS.md)

## Core Scanner integration

**Use for:** starting a Scanner run and reading its canonical public result projection.

**Public surface:** `CB\Core\Integrity\Api\IntegrityApi`

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)

## UI / Foundation contracts

**Use for:** documented shared Core Admin semantics or reusable interaction primitives.

**Guide:** [Foundation map](foundation-map.md)

**Normative source:** [Base `docs/foundation-v1-contract.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/foundation-v1-contract.md), [`docs/CORE-ADMIN-DESIGN-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/CORE-ADMIN-DESIGN-FOUNDATION.md), and individual Foundation documents.

## Capability catalog

**Use for:** extending the suite-wide capability catalog where documented.

**Public surface:** `cb_core_capability_catalog`

**Normative source:** [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)
