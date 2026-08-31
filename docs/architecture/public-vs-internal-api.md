# Public vs internal API

Core Blueprint intentionally distinguishes **technical visibility** from **supported extension API**.

A PHP class or method being declared `public` does not make it a supported Core Blueprint extension contract.

## Supported public API

Treat a contract as supported when it is documented in current Base:

- `docs/PUBLIC-API.md`; or
- a Foundation/API document explicitly linked or referenced by that public contract.

Base public documentation is normative.

## Internal / unsupported

Do not build extension dependencies on implementation details that are merely reachable.

Examples explicitly identified as internal or unsupported include:

- private `cb-core-css-*` asset handles;
- `CB\Core\Admin\AdminAssetCatalog`;
- Base `PageBase` as an extension inheritance contract;
- direct `CB\Core\Log\AuditLog` access;
- Base repositories, query builders, storage classes, and migration locks;
- private Scanner repositories/controllers/storage;
- historical/deprecated hook families excluded by `PUBLIC-API.md`.

## Why the boundary matters

Base 1.x may refactor internal files, classes, asset handles, bundle boundaries, or enqueue order without treating that as a public API break.

Extensions remain compatible by depending on semantic contracts rather than implementation layout.

## Pre-v1 verification rule

Core Blueprint is still being prepared for its first public v1 release.

For this documentation phase:

1. check current Base `main`;
2. use `PUBLIC-API.md` as the authority;
3. verify linked Foundation/API docs;
4. compare current Starter examples;
5. do not promote source-only behavior to a supported contract.

## When a needed contract appears to be missing

Do not solve the gap by documenting an internal workaround as public API.

Record the gap for Base/Suite review.

Use the [Public API map](../reference/public-api-map.md) for navigation.
