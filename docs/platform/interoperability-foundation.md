# Interoperability Foundation

Core Blueprint Base provides a generic Interoperability Foundation so extensions can define versioned domain contracts and other extensions can implement, discover and resolve them without depending on one concrete provider.

Use it when the shared concern is a **domain contract with one or more implementations**.

For automation triggers, read-only states and actions, use [Automation Foundation](automation-foundation.md) instead. Generic Interoperability does not replace the Automation registries.

The public technical authority is always current Core Blueprint Base documentation, especially:

- [`docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)
- [`docs/INTEROPERABILITY-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/INTEROPERABILITY-FOUNDATION.md)
- `CB\Core\Interoperability\Registry`

This guide explains how to use that contract; it does not redefine it.

## The model

Generic Interoperability has three distinct identities:

```text
extension identity
    ↓
domain-owned contract
    ↓
one or more implementations
```

`CB\Core\ExtensionRegistry` remains the canonical identity boundary. Interoperability does not create a second provider identity system.

A contract answers:

> What shared behavior does this domain define?

An implementation answers:

> Which extension provides that behavior for this exact contract version, and which optional features does it support?

## Register extension identity first

A contract owner and every implementation provider must already be valid Core Blueprint extensions.

```php
use CB\Core\ExtensionRegistry;

add_action( 'cb_core_register_extensions', static function (): void {
    ExtensionRegistry::register( [
        'id'           => 'acme-resources',
        'plugin_file'  => plugin_basename( ACME_RESOURCES_FILE ),
        'requires_api' => '1.0',
        'menu_url'     => '',
        'status_id'    => '',
    ] );
} );
```

An adapter/provider extension registers its own canonical identity separately. Do not invent an adapter ID outside `ExtensionRegistry` and treat it as a second product identity.

## Define a domain contract

The extension that owns the domain semantics defines the PHP interface and registers the contract during:

```text
cb_core_register_interoperability_contracts
```

Example interface:

```php
namespace Acme\Resources\Contracts;

interface ResourceProvider {
    public function get( string $id ): ?array;
}
```

Register it:

```php
use Acme\Resources\Contracts\ResourceProvider;
use CB\Core\Interoperability\Registry;

add_action( 'cb_core_register_interoperability_contracts', static function (): void {
    Registry::register_contract( [
        'owner'       => 'acme-resources',
        'id'          => 'resource-provider',
        'version'     => '1',
        'label'       => __( 'Resource provider', 'acme-resources' ),
        'description' => __( 'Provides Acme resources to compatible consumers.', 'acme-resources' ),
        'interface'   => ResourceProvider::class,
    ] );
} );
```

The owner controls the meaning of the contract. Base controls registration, validation, discovery and runtime interface enforcement.

Do not put provider-specific behavior into the shared interface merely because the first implementation needs it.

## Implement a contract

An implementing extension registers during:

```text
cb_core_register_interoperability_implementations
```

Example:

```php
use Acme\Resources\Contracts\ResourceProvider;
use CB\Core\Interoperability\Registry;
use Vendor\Adapter\AcmeResourceProvider;

add_action( 'cb_core_register_interoperability_implementations', static function (): void {
    Registry::register_implementation( [
        'provider'         => 'vendor-acme-adapter',
        'id'               => 'default',
        'label'            => __( 'Vendor resource adapter', 'vendor-acme-adapter' ),
        'description'      => __( 'Provides Acme resources through Vendor.', 'vendor-acme-adapter' ),
        'contract_owner'   => 'acme-resources',
        'contract'         => 'resource-provider',
        'contract_version' => '1',
        'supports'         => [
            'resource.discovery',
            'resource.read',
        ],
        'factory'          => static fn (): ResourceProvider => new AcmeResourceProvider(),
    ] );
} );
```

The factory is runtime wiring. It is not exposed in public implementation descriptors.

Keep the implementation thin: translate from the provider system into the domain contract and delegate to canonical provider/domain services instead of rebuilding either product inside the adapter.

## `supports` describes functional support

`supports` is for optional features defined by the contract owner.

For example:

```text
resource.discovery
resource.read
resource.write
```

It is **not** a WordPress capability or authorization declaration.

Think of the vocabulary as:

```text
capability → permission / authorization
supports   → functional contract support
```

Declare only features the implementation actually supports. Consumers should not infer support from the provider name, class methods or implementation-specific knowledge.

## Discover implementations

Discover every implementation of one exact contract version:

```php
use CB\Core\Interoperability\Registry;

$providers = Registry::discover(
    'acme-resources',
    'resource-provider',
    '1'
);
```

Require one or more optional features:

```php
$providers = Registry::discover(
    'acme-resources',
    'resource-provider',
    '1',
    [ 'resource.discovery', 'resource.read' ]
);
```

A matching implementation must support every requested token.

Discovery returns metadata, not the private factory or service object.

## Do not assume one provider

The Foundation intentionally supports multiple implementations of the same exact contract version.

Do not build generic consumer code around a hidden singleton assumption such as:

```php
get_resource_provider();
```

unless the **domain contract itself** deliberately defines singleton semantics.

Normally, discover the eligible implementations and use explicit provider/implementation identity when selecting or resolving one.

This allows a site to have multiple compatible integrations active at the same time.

## Resolve an implementation

After selecting a descriptor, resolve the implementation explicitly:

```php
$provider = Registry::resolve(
    'acme-resources',
    'resource-provider',
    '1',
    'vendor-acme-adapter',
    'default'
);

if ( is_wp_error( $provider ) ) {
    // Handle unavailable or invalid implementation safely.
    return;
}

$resource = $provider->get( 'example' );
```

Base verifies that the factory returns an object satisfying the PHP interface declared by the contract.

Resolution can fail. Treat `WP_Error` as part of the public failure boundary rather than assuming that registration guarantees runtime success.

## Exact contract versions

Generic Interoperability v1 uses exact positive-integer contract versions such as:

```text
1
2
3
```

These versions are separate from:

- the extension version;
- the Core Blueprint Base product version;
- `CB_CORE_API_VERSION`.

Do not implement your own version ranges, fallback or automatic negotiation around the registry.

If a contract changes incompatibly, publish a new contract version and let consumers request the version they actually understand.

## Registration lifecycle

Attach your registration callbacks during normal plugin bootstrap. Base collects them later through the controlled lifecycle.

The order is:

```text
ExtensionRegistry
    ↓
contract registration
    ↓
implementation registration
    ↓
Interoperability registry frozen for the request
```

Do not register implementations opportunistically after discovery has already caused canonical collection to finish.

## Contract ownership matters

The contract owner should define only stable shared semantics.

A useful test is:

- **Contract owner:** what does this operation/data mean?
- **Implementation:** how does this provider satisfy that meaning?
- **Base:** how are contracts and implementations registered, discovered and resolved safely?

If provider-specific details leak into Base, the boundary is wrong.

If an adapter contains a second copy of the domain business logic, the boundary is also wrong.

## First-party and third-party integrations use the same API

Core Blueprint's own integrations use the same public Interoperability Foundation available to external developers.

There is no separate first-party registry or privileged adapter path in the public model.

That means a Core Blueprint-maintained adapter can be used as a reference implementation, but its first-party status does not change the contract mechanics.

## Generic Interoperability vs Automation Foundation

Choose Generic Interoperability when extensions need to discover/resolve implementations of a shared domain interface.

Choose Automation Foundation when a provider needs to expose:

- a trigger — something happened;
- a state — read what is true now;
- an action — perform a provider-owned operation.

Do not duplicate Automation triggers, states or actions inside a Generic Interoperability contract just to create another invocation path.

See [Automation Foundation](automation-foundation.md) for the automation-specific provider contract.

## What the Foundation does not decide

Generic Interoperability does not define:

- a Forms contract;
- builder integrations;
- storage semantics;
- mail semantics;
- payment semantics;
- authorization rules for your domain operation;
- workflow orchestration;
- event delivery;
- provider-specific configuration.

Those belong to the appropriate domain or another documented Base Foundation.

## Before publishing a contract

Ask:

1. Is this genuinely shared across extensions?
2. Which extension/domain owns the semantics?
3. Is there already a WordPress or Core Blueprint public contract for this concern?
4. Can more than one implementation satisfy the interface without provider-specific rules?
5. Which optional features need explicit `supports` tokens?
6. Can the adapter remain thin?
7. Does the contract avoid duplicating Automation Foundation or another Base surface?

If those answers are unclear, keep the concern extension-owned until the interoperability need is concrete.

## Normative API

For exact accepted fields, identifier grammar, validation limits, lifecycle behavior, error codes and compatibility promises, always use current Base documentation:

- [Base `docs/PUBLIC-API.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/PUBLIC-API.md)
- [Base `docs/INTEROPERABILITY-FOUNDATION.md`](https://github.com/christiaanbruinsma/wp-core-blueprint/blob/main/docs/INTEROPERABILITY-FOUNDATION.md)
- `CB\Core\Interoperability\Registry`

If this guide and Base disagree, current Base public documentation wins.
