# Ownership boundaries

The central Core Blueprint development rule is:

> **Base owns the language. Extensions own the sentence.**

This is an ownership rule, not a requirement to push all behavior into Base.

## Base owns shared language

When Base defines a public platform contract, it owns the shared semantics and the parts of the lifecycle described by that contract.

Examples:

- `ExtensionRegistry` owns canonical platform identity validation;
- `PageRegistry` owns Core Admin page registration, menu wiring, and semantic Foundation resolution;
- Design Foundation contracts own generic shared component behavior/presentation;
- module status owns normalization of `ok|warn|err|off`;
- Governance owns the public audit write boundary and shared audit policy;
- SchemaRegistry owns when declared extension schemas are reconciled.

## Extensions own product meaning

The extension remains responsible for what its feature means.

Examples:

- a modal's destructive business action belongs to the extension;
- Object Picker search authorization and object semantics belong to the extension;
- module status data belongs to the extension even when Base normalizes presentation;
- database table schema and uninstall policy remain extension-owned;
- Governance context must be chosen by the extension and must not contain secrets.

## Composition is not presentation ownership

On Core Admin pages, an extension may compose Base primitives into product-specific layouts.

For example, extension CSS may define a grid and use `--cb-*` spacing tokens. It must not locally redraw Base-owned panels, notices, buttons, form controls, tabs, badges, or interaction states.

This separation allows Base to improve shared presentation without requiring every extension to ship the same visual update.

## A practical test

Before adding a Base dependency, ask:

1. Is this concern explicitly documented as a public Base contract?
2. Is the concern genuinely shared across the suite?
3. Does Base own semantics/validation/presentation here, or only provide a primitive?
4. What business meaning and data remain mine?

If the answer to the first question is no, use WordPress or extension-owned code rather than an internal Base class.

Next: [Public vs internal API](public-vs-internal-api.md).
