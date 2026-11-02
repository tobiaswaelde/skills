---
name: database-change
description: Plan and implement approved schema/data changes with reviewed migrations, disposable-environment tests, backward compatibility, and rollback constraints. Use for database evolution or migration review, not executing production migrations, resetting shared databases, or replacing a project's ORM.
---

# Database change

Follow the bundled [specialist execution boundaries](../coding-workflow/references/specialist-execution.md), project guidelines, and current delivery approvals. A migration-plan request is read-only. This skill can prepare approved source changes and run confirmed disposable/local checks; it does not execute production migrations or authorize shared-database destruction.

## Establish schema ownership and environment

- Inspect the actual database engine/version, ORM/migration-tool version, project schema/contract ownership, migration history, application/worker consumers, CI, and deployment process. Identify the canonical schema and generated-client boundary; do not create a second schema owner or hand-edit generated client code.
- Inspect project scripts before invoking them. Resolve version-appropriate commands from the installed tool and its matching primary documentation. Prisma versions and migration models differ; do not assume `schema.prisma`, a particular CLI command, or a current online tutorial matches this repository. Installing/upgrading an ORM is not part of a schema change unless explicitly agreed.
- Before connecting or running state-changing checks, identify the actual target and disposable fixture/reset boundary from approved configuration without displaying credentials. A local hostname, port forward, or TEST variable name is not proof of a disposable database. If the target is ambiguous, continue code-level planning but wait for clarification before dependent operations.
- Read the requested behavior and existing data assumptions: nulls/defaults, uniqueness, foreign keys, tenant/resource scope, indexes, row volume, write concurrency, and operational constraints relevant to the change. Preserve applied migration history; add a corrective migration instead of rewriting already-applied migrations.

## Plan a compatible change

- Use [compatibility checks](references/compatibility-checks.md) to identify old/new application and data combinations. For changes that would break running consumers, plan expand, backfill/migrate, consumer transition, verification, then contract as separate rollout stages. A rename/drop is not safe merely because a schema diff compiles.
- Define the data transformation explicitly: mapping/defaults, null/invalid/duplicate handling, tenant boundaries, batching or transaction boundaries as appropriate, and observable pre/postconditions. Specify retry/restart behavior and prove that rerunning supported steps does not duplicate or corrupt data.
- Review generated migration SQL/operations for destructive drop-and-recreate behavior, lock/downtime risks, index/constraint validation, and engine-specific transactional limits. Use the tool's actual migration lifecycle; do not assume all DDL or backfills are atomic or that a warning can be ignored.
- Define a release/deployment sequence and owners for each stage. Separate application rollback from schema/data recovery; identify irreversible transforms and the backup/restore or forward-fix requirement. Do not promise a down migration will reconstruct lost data.
- Keep destructive contract steps gated on evidence that old readers/writers are retired and transformed data passes verification. Source-code changes alone do not establish live consumer retirement. Record unknown production facts as rollout prerequisites for the operator.

## Implement and test in the approved local scope

- After implementation approval, create the reviewed migration/transform and needed application/tests/docs in the owning project. Keep stages separable and coherent with the active task/commit plan. Preserve unrelated records, access controls, and existing compatibility obligations.
- Test a clean migration path and an upgrade path from representative old fixtures, including existing rows and relevant null/duplicate/edge cases. Verify actual data preservation and constraints, not only schema generation. Exercise backfill retry/interruption behavior where applicable.
- Test old/new consumers against the expanded state when required, and new consumers against the intended final state. Recheck authorization/RLS using runtime roles where schema/table/index/policy changes affect access; privileged-owner tests alone do not verify isolation.
- Run focused migration validation, ORM generation/type checks, relevant unit/integration checks, and affected builds using real project commands. Inspect committed output ownership and resulting diff; generated files follow repository policy and must not be hand-edited to hide drift.
- Use only confirmed disposable resources for resets or destructive fixture tests, within the approved scope. Do not replace a reviewed migration with a schema-sync/force-reset shortcut. When local fixtures cannot exercise a required path, report the gap rather than running it against a shared or production target.

## Deliver migration evidence and operator steps

- Report the schema/data intent, reviewed source artifacts, tested baseline/target/tool version, transformation pre/postconditions, compatibility results, and local checks. Mark untested engine/volume/production assumptions explicitly.
- Provide the version-matched operator sequence, contract-stage prerequisites, verification queries/conditions, and distinct application rollback/data recovery strategy. Production credentials and data remain outside the report.
- Leave production execution, shared resets, deployment, and destructive contract-stage approval to the separate explicit operational workflow. A successful local test or source PR merge is not evidence that the production migration ran.

Version-specific starting point: [Prisma expand-and-contract guidance](https://www.prisma.io/docs/guides/database/data-migration). Match the guide to the installed major version; the approach applies through the project's actual migration tooling.
