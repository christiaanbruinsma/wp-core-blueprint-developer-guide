# Core Blueprint overview

Core Blueprint Base is the foundation plugin for the Core Blueprint WordPress platform. An extension is a normal WordPress plugin that intentionally integrates with documented Base contracts where the platform owns a shared concern.

An extension is **not** required to route every WordPress task through Base. Standard WordPress mechanisms remain appropriate for product-owned concerns unless Base exposes a public cross-suite contract for that concern.

## The platform model

A useful mental model is:

```text
Core Blueprint Base
→ shared platform contracts and shared language

Core Blueprint extension
→ product/domain behavior built with those contracts where appropriate
```

Or more compactly:

> **Base owns the language. Extensions own the sentence.**

Base may own concerns such as extension identity, Core Admin page registration, shared UI semantics, module state/status normalization, governance/audit integration, shared schema reconciliation, and other contracts explicitly documented in Base.

An extension owns its feature behavior, workflows, product-specific UI composition, its own persistent data, and its own uninstall policy.

## Platform principles

Core Blueprint's public platform principles affect implementation decisions:

- **Local-first and sovereign by default** — avoid unnecessary external dependencies.
- **Privacy by architecture** — collect and retain only what the feature needs; keep secrets out of logs.
- **Governance by design** — use explicit capability/intent boundaries and audit meaningful changes.
- **Exit freedom** — prefer understandable WordPress-native storage and portable formats.
- **Fail closed for trust boundaries** — ambiguous remote, filesystem, and privilege operations must not silently proceed.
- **Extensions through contracts** — depend on documented APIs rather than Base implementation details.
- **No security theatre** — describe and implement controls according to what they actually protect.
- **European operational reality** — ordinary managed WordPress hosting remains an important baseline.

For the normative principle text, see [Base `docs/PLATFORM-PRINCIPLES.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PLATFORM-PRINCIPLES.md).

## What to read next

Continue with:

- [Ownership boundaries](../architecture/ownership-boundaries.md)
- [Public vs internal API](../architecture/public-vs-internal-api.md)
- [Using the Starter Plugin](using-the-starter.md)
- [Creating an extension](creating-an-extension.md)
