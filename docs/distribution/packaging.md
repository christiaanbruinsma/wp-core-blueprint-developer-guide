# Packaging

A WordPress release ZIP must preserve the plugin's canonical installed identity.

## Canonical root rule

The archive must contain the canonical plugin folder as its single plugin root:

```text
vendor-feature.zip
└── vendor-feature/
    ├── vendor-feature.php
    └── ...
```

Do not rename the internal root folder to:

- a version;
- a branch;
- a build label;
- a temporary work directory;
- a GitHub archive name.

Changing the root folder can cause WordPress to treat an update as a different plugin.

## Stable plugin identity

Choose and keep stable:

- canonical plugin folder;
- main plugin filename;
- plugin basename.

The identity pass happens when deriving the plugin from the Starter. Release packaging must not change it later.

## Build tooling

Core Blueprint Base currently has its own release tooling and release guardrails.

Do not assume Base's current build command is automatically the universal extension build contract. The Starter intentionally does not invent a separate release pipeline while suite-wide packaging is still being finalized.

For v0.1, the canonical extension rule is the stable WordPress package identity above.

## Release checks

Before creating a release archive:

- run conformance checks;
- run syntax/static checks;
- exclude development-only files according to the project's release process;
- verify the ZIP root and main plugin file;
- smoke-test the packaged artifact, not only the development worktree.
