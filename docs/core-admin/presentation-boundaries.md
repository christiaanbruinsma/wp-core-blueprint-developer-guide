# Presentation boundaries

Before choosing a Core Blueprint UI primitive, determine **who owns the screen**.

This is a fundamental architectural decision.

## Core Admin

A page beneath the Core Blueprint parent menu that participates in the Core Admin platform should register through `PageRegistry`.

```text
Core Admin
→ PageRegistry
→ minimal Core Admin shell
→ semantic Base requirements
→ Base-owned Core presentation
```

Base owns the shared Core Admin visual language, including light/dark integration and the canonical appearance of declared shared primitives.

The extension owns feature-specific composition and product-specific components.

## Standalone WordPress admin

A standalone plugin-owned wp-admin screen should remain WordPress-native.

```text
Standalone wp-admin
→ normal WordPress presentation
→ narrow Foundation opt-ins where supported
```

Some behavioral Foundations expose a WordPress-native presentation adapter. For example, Modal, Toast, Clipboard, Token Input, Object Picker, Select Picker, and other documented primitives can be enqueued through their public `CB\Core\UI\Assets` helpers.

Do **not** load the Core Admin Theme merely to obtain one shared interaction.

## Frontend

Frontend presentation is extension/product-owned unless a separate documented public contract says otherwise.

Do not imply Core Admin tokens, theme assets, or wp-admin presentation on the public site.

## Behavioral Foundations are context-aware

A shared Foundation can own the same interaction semantics while presenting differently:

```text
shared behavior
├── Core Admin adapter
└── WordPress-native adapter
```

The consumer still owns the business meaning.

## Testing implication

If your extension has a Core Admin page, test it in both Core Blueprint light and dark themes.

If your extension has a standalone wp-admin page, verify that narrow Foundation opt-ins do not unintentionally import Core Admin presentation.

See:

- [Core Admin pages](pages.md)
- [Design Foundation](design-foundation.md)
