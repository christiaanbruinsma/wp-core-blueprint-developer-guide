# Core Blueprint Developer Guide

Developer-facing guidance for building extensions for the Core Blueprint WordPress platform.

Core Blueprint uses three documentation layers with different responsibilities:

1. **Core Blueprint Base** — the normative technical authority. Its `docs/PUBLIC-API.md` and linked Foundation contracts define the supported extension API.
2. **Core Blueprint Extension Starter** — the canonical minimal implementation example and conformance specimen.
3. **This Developer Guide** — the learning, architecture, pattern, and navigation layer that explains how to use those contracts correctly.

If this guide, the Starter, and Base documentation ever disagree, **current Base public documentation wins**.

> Core Blueprint is still being prepared for its first public v1 release. During this pre-v1 documentation phase, verify examples against the current `main` branches of both Base and the Starter.

## Start here

A useful learning path is:

1. [Core Blueprint overview](docs/getting-started/overview.md)
2. [Platform ownership model](docs/architecture/ownership-boundaries.md)
3. [Public vs internal API](docs/architecture/public-vs-internal-api.md)
4. [Use the Starter Plugin](docs/getting-started/using-the-starter.md)
5. [Identity and compatibility](docs/getting-started/identity-and-compatibility.md)
6. [Register extension identity](docs/platform/extension-registry.md)
7. [Define or implement shared contracts through Interoperability Foundation](docs/platform/interoperability-foundation.md)
8. [Expose automation capabilities through Automation Foundation](docs/platform/automation-foundation.md)
9. [Choose the correct admin presentation context](docs/core-admin/presentation-boundaries.md)
10. [Add a Core Admin page](docs/core-admin/pages.md)
11. [Use the Design Foundation](docs/core-admin/design-foundation.md)
12. [Use Integration Grid and Detail Rows at the correct level](docs/core-admin/integration-and-detail-surfaces.md)
13. [Own extension assets and composition](docs/core-admin/extension-assets.md)
14. [Follow development conventions](docs/development/conventions.md)
15. [Record governance-relevant events](docs/platform/governance-and-audit.md)
16. [Run conformance checks](docs/distribution/conformance-testing.md)
17. [Package the plugin correctly](docs/distribution/packaging.md)

## Core principle

> **Base owns the language. Extensions own the sentence.**

Base owns shared platform contracts, validation, shared semantics, and shared presentation boundaries. Extensions own their product/domain logic, workflows, feature-specific composition, persistent product data, and uninstall policy.

For Automation Foundation specifically, this becomes:

> **Extensions own business semantics. Base owns interoperability. Automations owns orchestration.**

## Canonical repositories

- Core Blueprint Base: <https://github.com/christiaanbruinsma/wp-core-blueprint>
- Extension Starter: <https://github.com/christiaanbruinsma/wp-core-blueprint-starter-plugin>
- Developer Guide: <https://github.com/christiaanbruinsma/wp-core-blueprint-developer-guide>

## Reference navigation

- [Public API map](docs/reference/public-api-map.md)
- [Foundation map](docs/reference/foundation-map.md)

These maps are indexes, not replacement API specifications.
