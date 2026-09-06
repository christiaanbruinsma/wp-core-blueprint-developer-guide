# Requirements

A Core Blueprint extension is still a WordPress plugin first. It must satisfy both the normal requirements of its product and the documented requirements of the Core Blueprint Base contracts it consumes.

## Current platform baseline

At the time of this pre-v1 guide:

- WordPress: **7.0 or newer**
- PHP: **8.4 or newer**
- Core Blueprint Base public API: **1.0**
- Base itself requires PHP Sodium (`libsodium`)

Do not copy these values into long-lived compatibility logic without checking current Base documentation. The normative requirements live in the current Core Blueprint Base repository.

## Compatibility should target the public API

A normal Core Blueprint extension should use `CB_CORE_API_VERSION` as its primary Base compatibility boundary.

The current public API uses a `major.minor` version. Compatibility requires the same major version and an available Base API minor that is at least the requested minor.

Only require a specific Base product version when the extension genuinely depends on behavior beyond the public API contract.

Runtime readiness should then verify the concrete documented public contracts the extension actually consumes. This keeps compatibility precise: the API family establishes the supported platform line, while required renderer/registry/foundation entrypoints establish that the capabilities used by the product are present.

Do not use `CB_CORE_VERSION` as a substitute for checking a documented required capability merely because an internal RC first introduced it.

Keep WordPress plugin identity, ExtensionRegistry identity and API compatibility separate; see [Identity and compatibility](identity-and-compatibility.md).

## Base dependency behavior

A Core Blueprint extension should fail safely when compatible Base contracts are unavailable.

The canonical Starter demonstrates two distinct cases:

- **Activation time:** refuse activation when the required Base API/contracts are unavailable.
- **Runtime after activation:** if Base is later deactivated or becomes incompatible, keep the extension inert and show an administrator dependency notice rather than creating a second standalone Core Admin runtime.

Do not use an undocumented compatibility layer to emulate Base behavior, and do not revive local legacy presentation when a required shared Foundation is missing.

A derived plugin should require only the public Base contracts it actually consumes after unused Starter examples have been removed.

## Development environment

For development, use:

- a current checkout of Base `main`;
- a current checkout of the Starter `main`;
- `WP_DEBUG` during development;
- PHP syntax/static validation appropriate to the project;
- browser testing for both Core Admin light and dark themes when your extension has a Core Admin page.

Next: [Creating an extension](creating-an-extension.md).
