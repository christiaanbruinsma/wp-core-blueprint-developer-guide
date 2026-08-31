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
5. [Create an extension](docs/getting-started/creating-an-extension.md)
6. [Register extension identity](docs/platform/extension-registry.md)
7. [Choose the correct admin presentation context](docs/core-admin/presentation-boundaries.md)
8. [Add a Core Admin page](docs/core-admin/pages.md)
9. [Use the Design Foundation](docs/core-admin/design-foundation.md)
10. [Follow development conventions](docs/development/conventions.md)
11. [Record governance-relevant events](docs/platform/governance-and-audit.md)
12. [Run conformance checks](docs/distribution/conformance-testing.md)
13. [Package the plugin correctly](docs/distribution/packaging.md)

## Core principle

> **Base owns the language. Extensions own the sentence.**

Base owns shared platform contracts, validation, shared semantics, and shared presentation boundaries. Extensions own their product/domain logic, workflows, feature-specific composition, persistent product data, and uninstall policy.

## Canonical repositories

- Core Blueprint Base: <https://github.com/christiaanbruinsma/wp-core-blueprint>
- Extension Starter: <https://github.com/christiaanbruinsma/wp-core-blueprint-starter-plugin>
- Developer Guide: <https://github.com/christiaanbruinsma/wp-core-blueprint-developer-guide>

## Reference navigation

- [Public API map](docs/reference/public-api-map.md)
- [Foundation map](docs/reference/foundation-map.md)

These maps are indexes, not replacement API specifications.
