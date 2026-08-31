# Governance and Audit

Core Blueprint provides a public governance write boundary for meaningful extension events.

Use:

- `CB\Core\Governance\EventRegistry` for human-readable event metadata;
- `CB\Core\Governance\Audit::record()` for writes.

Do not write directly to Base's internal AuditLog/storage implementation.

## Register event metadata

Register translated metadata on `init` or later.

```php
use CB\Core\Governance\EventRegistry;

EventRegistry::register([
    'id'                 => 'vendor.item.updated',
    'label'              => __('Vendor item updated', 'vendor-plugin'),
    'retention_category' => 'maintenance',
]);
```

## Record the real mutation

```php
use CB\Core\Governance\Audit;

Audit::record(
    'vendor.item.updated',
    'notice',
    ['item_id' => $item_id]
);
```

`Audit::record()` is best-effort and non-fatal. Check current Base documentation for supported event ID syntax, severities, and retention categories.

## What should be audited?

Audit events should represent meaningful governance or operational changes, not every function call.

Typical candidates include:

- security-relevant changes;
- permission or policy changes;
- destructive operations;
- important configuration mutations;
- maintenance operations worth reviewing later.

Do not emit a demo event merely because the Starter contains one.

## Context and privacy

Audit context should contain identifiers and factual operational metadata.

Never place secrets, credentials, tokens, full sensitive payloads, or unnecessary personal data in audit context.

Base applies defensive sanitization, but the extension remains responsible for choosing safe context.

## Retention

Base currently defines five canonical AuditLog retention categories:

- `security`
- `maintenance`
- `logins`
- `settings`
- `general`

Custom AuditLog categories are not part of the public contract.

Extension-owned operational log tables are a different concern and may use the dedicated public retention-store contract where appropriate.

## Internal boundaries

Do not depend on:

- `CB\Core\Log\AuditLog`;
- historical `cb_core_event_labels`;
- Base query/repository/storage classes.

For exact constraints, see current Base `docs/PUBLIC-API.md`.
