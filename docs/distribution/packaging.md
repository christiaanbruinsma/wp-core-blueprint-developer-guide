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

See [Identity and compatibility](../getting-started/identity-and-compatibility.md).

## Build tooling

No suite-wide executable **extension** build command is currently frozen as a public/canonical platform contract.

Do not infer one from Base-internal release tooling and do not invent a plugin-specific release pipeline and present it as a Core Blueprint guarantee.

Until the extension build contract is explicitly frozen, the canonical requirements are the stable WordPress package identity above plus the project's verified release checks.

## Release checks

Before creating a release archive:

- run conformance checks;
- run syntax/static checks;
- exclude development-only files according to the project's verified release process;
- verify the ZIP root and main plugin file;
- smoke-test the packaged artifact, not only the development worktree.

The packaged artifact is what ships; a clean source checkout alone is not a release PASS.
