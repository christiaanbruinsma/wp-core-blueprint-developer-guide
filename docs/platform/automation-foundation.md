# Automation Foundation

Core Blueprint Base provides a public Automation Foundation so extensions can describe automation-capable domain behavior without depending on Core Blueprint Automations.

The Foundation is an interoperability contract, not a workflow engine.

The ownership model is simple:

> **Extensions own business semantics. Base owns interoperability. Automations owns orchestration.**

A plugin can therefore become automation-ready with Base alone. If Core Blueprint Automations is later installed, it can discover and orchestrate those provider-owned capabilities without the provider changing its domain model.

## What a provider can expose

Automation Foundation has three capability kinds:

| Kind | Meaning | Owner responsibility |
| --- | --- | --- |
| Trigger | Something happened | Decide when the domain event is true and define its payload |
| State | Read what is true now | Provide a read-only resolver over canonical domain data |
| Action | Perform a provider-owned operation | Provide an executor that delegates to canonical domain services |

State resolvers and action executors remain provider-owned. Public discovery exposes their metadata, not their callbacks.

## Register the extension first

Automation capabilities belong to an extension identity already registered through `CB\Core\ExtensionRegistry`.

For example:

```php
use CB\Core\ExtensionRegistry;

add_action( 'cb_core_register_extensions', static function (): void {
    ExtensionRegistry::register( [
        'id'           => 'acme-reservations',
        'plugin_file'  => plugin_basename( __FILE__ ),
        'requires_api' => '1.0',
        'menu_url'     => '',
        'status_id'    => '',
    ] );
} );
```

Use the same provider ID in every automation capability owned by that extension.

## Register capabilities

Attach capability registration during normal plugin loading. Base performs the actual collection later, after its canonical extension-registration lifecycle.

```php
use CB\Core\Automation\ActionRegistry;
use CB\Core\Automation\StateRegistry;
use CB\Core\Automation\TriggerRegistry;

add_action( 'cb_core_register_automation_capabilities', static function (): void {
    TriggerRegistry::register( [
        'provider'       => 'acme-reservations',
        'id'             => 'reservation.confirmed',
        'label'          => __( 'Reservation confirmed', 'acme-reservations' ),
        'description'    => __( 'A reservation reached its confirmed state.', 'acme-reservations' ),
        'schema_version' => '1',
        'payload_schema' => [
            'reservation_id' => [
                'type'          => 'integer',
                'semantic_type' => 'acme-reservations.reservation_id',
                'required'      => true,
            ],
            'user_id' => [
                'type'          => 'integer',
                'semantic_type' => 'wp.user_id',
                'required'      => true,
            ],
        ],
    ] );

    StateRegistry::register( [
        'provider'            => 'acme-reservations',
        'id'                  => 'reservation.current',
        'label'               => __( 'Current reservation', 'acme-reservations' ),
        'description'         => __( 'Reads current reservation facts.', 'acme-reservations' ),
        'schema_version'      => '1',
        'input_schema'        => [
            'reservation_id' => [
                'type'          => 'integer',
                'semantic_type' => 'acme-reservations.reservation_id',
                'required'      => true,
            ],
        ],
        'output_schema'       => [
            'status' => [
                'type'     => 'string',
                'required' => true,
            ],
            'title' => [
                'type'          => 'string',
                'semantic_type' => 'core-blueprint.source_title',
                'required'      => true,
            ],
        ],
        'required_capability' => 'read',
        'resolver'            => static function ( array $input ): array {
            // Delegate to your plugin's canonical read model/service.
            return [
                'status' => 'confirmed',
                'title'  => 'Example reservation',
            ];
        },
    ] );

    ActionRegistry::register( [
        'provider'            => 'acme-reservations',
        'id'                  => 'reservation.add_note',
        'label'               => __( 'Add reservation note', 'acme-reservations' ),
        'description'         => __( 'Adds a note through the reservations domain service.', 'acme-reservations' ),
        'schema_version'      => '1',
        'input_schema'        => [
            'reservation_id' => [
                'type'          => 'integer',
                'semantic_type' => 'acme-reservations.reservation_id',
                'required'      => true,
            ],
            'note' => [
                'type'      => 'string',
                'required'  => true,
                'sensitive' => true,
            ],
        ],
        'output_schema'       => [],
        'required_capability' => 'edit_posts',
        'executor'            => static function ( array $input ): array {
            // Delegate to your plugin's canonical mutation/service layer.
            return [];
        },
    ] );
} );
```

Registration metadata is a contract. Do not put workflow decisions in these adapters.

## Semantic types

Primitive types tell consumers how a value travels. `semantic_type` tells consumers what that value means.

These two fields are intentionally separate:

```php
[
    'type'          => 'integer',
    'semantic_type' => 'wp.user_id',
]
```

The runtime value is still an integer. The semantic identity helps orchestration tools avoid binding unrelated integers such as a user ID, course ID and certificate profile ID to one another.

Useful naming patterns include:

```text
wp.user_id
acme-reservations.reservation_id
core-blueprint-lms.course_id
core-blueprint-certificates.profile_id
core-blueprint.completion_date
core-blueprint.source_title
```

Use a provider-specific semantic identity for provider-owned concepts. Use a shared `wp.*` or `core-blueprint.*` identity only when the meaning is genuinely shared and intentionally interoperable.

## Binding behavior

Core Blueprint Automations currently applies a strict-target semantic policy:

- if the target has no `semantic_type`, normal primitive compatibility applies;
- if the target has a `semantic_type`, a workflow output must carry the same semantic identity;
- a typed workflow output can still feed an untyped legacy target when the primitive types match;
- literals remain valid for typed targets when the literal's primitive type is valid;
- primitive widening, such as integer to number, does not bypass a semantic mismatch.

Providers do not implement these rules. Providers only declare accurate schemas; orchestration consumers validate bindings.

## Schema identity and versions

Capability references are identified by kind, provider, capability ID and schema version.

Use stable dotted IDs such as:

```text
reservation.confirmed
reservation.current
reservation.add_note
```

Do not repurpose an existing identity for different semantics. Increase `schema_version` when a transport contract changes incompatibly.

## Discovery is not execution

Public discovery is available through the registries:

```php
$triggers = TriggerRegistry::all();
$states   = StateRegistry::all();
$actions  = ActionRegistry::all();
```

Action discovery does not expose the executor. State discovery does not expose the resolver.

That boundary is deliberate. A plugin must not use reflection or Base internal classes to invoke a provider callback. Execution authority, principals, retries, audit behavior and other orchestration concerns belong to the orchestration runtime.

## Keep provider adapters thin

A state resolver should call the plugin's existing read model or service. An action executor should call its existing mutation/service API.

Do not create a second business implementation specifically for Automations. If the ordinary product and the automation adapter can produce different domain results for the same operation, the ownership boundary is already broken.

## Idempotency stays with the provider

When an action already has an idempotency identity, expose or transport that identity instead of inventing a second one in the orchestration layer.

For example, the Core Blueprint LMS → Certificates Golden workflow preserves the Certificates issuance identity by transporting LMS's stable completion source identity. Certificates remains authoritative for whether an issuance already exists.

Automation-ready does not mean orchestration-owned.

## What Base does not provide

Automation Foundation does not include a visual workflow builder, workflow persistence, condition evaluation, queues, workers, scheduling, delayed jobs, retries, branching, run history or public action/state invocation.

Those are orchestration concerns. Core Blueprint Automations is the optional product that builds workflows on top of the open provider/interoperability contract.

This separation lets extensions expose useful capabilities without importing paid Automations code into Base.

## Becoming automation-ready

Before calling an extension automation-ready, verify that it has a canonical ExtensionRegistry identity, capability IDs with stable semantics and schema versions, minimal transport schemas, semantic types for IDs/shared concepts where known, read-only state resolvers, thin action executors, explicit authorization capabilities, provider-owned idempotency where relevant, and no hard dependency on Core Blueprint Automations.

The Core Blueprint Extension Starter includes a non-loaded provider reference that can be adapted for a real extension.

## Normative API

The technical authority is the current Core Blueprint Base public API, especially:

- `docs/PUBLIC-API.md`
- `docs/AUTOMATION-FOUNDATION.md`
- `CB\Core\Automation\TriggerRegistry`
- `CB\Core\Automation\StateRegistry`
- `CB\Core\Automation\ActionRegistry`
- `CB\Core\Automation\Schema`

If this guide and Base disagree, current Base public contracts win.
