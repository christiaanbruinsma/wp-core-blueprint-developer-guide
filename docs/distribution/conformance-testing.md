# Conformance testing

The Starter ships `tools/conformance.php` as a lightweight source check for the canonical extension pattern.

Run it after the identity pass and again before packaging.

```text
php tools/conformance.php
```

## What the Starter check guards against

The current Starter check detects several architectural violations, including:

- private `cb-core-css-*` dependencies;
- direct `CB\Core\Log\AuditLog` usage;
- private `AdminAssetCatalog` usage;
- direct Core Admin `add_menu_page()` / `add_submenu_page()` wiring;
- inheriting internal `PageBase`;
- a first-party `Requires Plugins` header instead of the runtime dependency guard;
- jQuery;
- extension CSS that redraws Base-owned `.cb-core-*` appearance.

Treat the current Starter script as executable guidance. Verify it against current Starter `main` before relying on the exact rule list.

## Conformance is not complete testing

Also perform:

- PHP syntax/static validation;
- activation/deactivation smoke tests;
- activation failure without compatible Base;
- inert runtime behavior if Base later becomes unavailable;
- exact-screen asset scoping checks;
- Core Admin light and dark checks where applicable;
- standalone wp-admin checks where applicable;
- mutation capability/nonce tests;
- failure-path tests for filesystem/network/remote operations;
- no PHP notices or early translation warnings.

## Public contract check

Before release, review every `CB\Core\...` dependency in the extension.

For each dependency, be able to point to the current Base `PUBLIC-API.md` or an explicitly linked public contract.

If you cannot, treat the dependency as suspect and verify it before release.

Next: [Packaging](packaging.md).
