# Schema and data compatibility checks

Use combinations relevant to the actual rollout; record a reason when one is not required. An application rollback and data rollback are different operations.

| Stage | Questions and evidence |
| --- | --- |
| Baseline | What schema/data and application versions are in scope? Which engine, migration version, role, and fixture represent them? |
| Expand | Can the currently supported old application still read and write? Are new fields/tables/constraints introduced without premature data loss? |
| Backfill/migrate | Are mapping, defaults, invalid rows, tenant boundaries, retry/checkpoint behavior, and pre/postconditions verified on existing data? |
| Transition | Can the new application use the expanded state? Do required dual-read/write or compatibility adapters behave as designed? |
| Contract eligibility | What authoritative operational evidence proves old consumers are retired and the backfill complete? Which checks must the operator approve? |
| Contract result | Are new consumers and constraints correct after removal? Which old versions can no longer run, and is that documented? |
| Failure/recovery | What happens on interruption, partial batches, duplicate retries, or failed constraints? Which steps are reversible, forward-fixable, or restore-dependent? |

Cover relevant allowed/denied, cross-tenant, and runtime-role access cases when policies or data scope are affected. Fresh empty-database success cannot substitute for upgrade/data-preservation evidence. Small synthetic fixtures cannot establish production locking/performance characteristics; report that boundary and provide the required operator verification.
