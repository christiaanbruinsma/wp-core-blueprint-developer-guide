# Platform model

Core Blueprint Base is a platform foundation, not a superclass that product plugins inherit from.

The architecture is contract-driven:

```text
Base public contracts
        ↓
extension declarations / calls
        ↓
Base validation, normalization, shared presentation or shared service behavior
        ↓
extension-owned product behavior
```

## Base-owned concerns

Where documented as public contracts, Base may own:

- platform lifecycle points;
- extension identity/discovery;
- Core Admin page registration and shell;
- shared semantic UI behavior and presentation;
- module activation/state normalization;
- module health/status normalization;
- Governance/Audit integration;
- extension-owned database schema reconciliation;
- other explicitly documented cross-suite services.

## Extension-owned concerns

An extension owns:

- product/domain logic;
- workflows and mutations;
- feature-specific UI and composition;
- product-specific CSS;
- persistent product data;
- authentication/authorization for its own endpoints;
- uninstall policy;
- runtime behavior not delegated to a public Base contract.

## WordPress remains part of the architecture

Core Blueprint does not replace normal WordPress APIs merely for consistency.

REST, AJAX, admin-post, cron, options, post meta, taxonomies, users, roles, and other WordPress systems remain valid mechanisms unless Base explicitly owns a shared contract for the concern.

Use the narrowest appropriate WordPress lifecycle.

See also:

- [Ownership boundaries](ownership-boundaries.md)
- [Public vs internal API](public-vs-internal-api.md)
