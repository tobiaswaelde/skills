---
name: api-contract-check
description: Check consistency between runtime API validation, transport DTOs/schemas, OpenAPI, and consuming clients, including relevant authorization and tenant/resource isolation. Use for API contract changes or compatibility checks, not a general infrastructure security audit or a database-model export.
---

# API contract check

Follow the bundled [specialist execution boundaries](../coding-workflow/references/specialist-execution.md). Inspect the actual stack; NestJS, Zod, CASL, Prisma, and Querry Kit are relevant project patterns, not required dependencies. Preserve the active workflow and project coding-guidelines.

## Establish the contract and comparison

- Read the issue/diff, routes/controllers, validators/pipes, request/response schemas or DTOs, serializers, error mapping, OpenAPI generation, and affected clients/composables. Trace the real runtime path and its ownership; database entities are not automatically public transport contracts.
- Identify the intended baseline and target revision or approved contract. Resolve a supplied ref; if none is supplied, use the current task's diff/requirements and disclose that historical compatibility was not established. Do not call a contract backward-compatible merely because the current server/client compile together.
- Inventory changed operations: route/version, method, query/path/body shape, optional versus nullable values, defaults/coercion, pagination/filter/sort/field selection, success/error status and payload, and authentication/authorization requirements. Include serializers and response-field filtering; inspect affected server-side consumers and jobs as applicable.

## Compare declarations to runtime and clients

- Verify the request accepted by runtime validation and the serialized response actually sent match the intended transport contract, documentation, and client assumptions. TypeScript types or OpenAPI annotations alone do not prove runtime validation or serialization.
- Trace schema transforms, preprocessing, coercion, refinements, and input/output differences. Match the schema generator's version and mode to request versus response use; if a runtime construct is not representable, document or model the supported contract explicitly instead of silently widening it to an unconstrained schema.
- Check OpenAPI operation/schema identity, request/response requiredness, enums/unions, content types, error forms, and reusable-schema references as applicable. Generate documentation/clients only through the project-owned toolchain, into their established locations; do not hand-edit generated output to conceal a source mismatch.
- Verify client imports, typed DTOs, validation assumptions, date/number encoding, null handling, error handling, and cache/query behavior against the actual serialized wire shape. Retain canonical contract modules; do not leak Prisma/server-only types into browser bundles or copy types that should be shared.
- Classify changes from the consumer's perspective. Removal, newly required input, narrowed accepted values, altered meanings/statuses, or changed response encoding can break existing clients even when the new server tests pass. Check additive changes against actual strict consumers rather than assuming every addition is harmless. Record required API versioning, rollout, migration notes, and changeset scope according to project rules.

## Verify access boundaries

- Build cases from the [contract and access matrix](references/contract-access-matrix.md) using approved synthetic identities and fixtures. Check allowed behavior as well as denial, invalid input, not-found, and relevant cross-tenant/resource requests; a blanket denial is not a passing implementation.
- Follow the project's actual role/scope model. Where query-level authorization is required, verify the database query is scoped before records, counts, aggregates, search, exports, nested relations, or job inputs are returned. Frontend hiding and post-query DTO filtering are not substitutes for required row/resource restrictions. When RLS is used, test with the runtime database role instead of only a privileged test owner.
- Check authentication versus authorization and the project's inaccessible-resource response policy without leaking existence or sensitive fields. Include revocation/stale-membership/group effects when the changed operation depends on them. Do not replace a project's permission model or invent additional role semantics during this check.

## Validate and report

- Run focused schema/validator, controller/integration, and client checks using actual project commands. Prefer the real local request/response path for wire behavior and authorization; schema snapshots and typechecks are supporting evidence, not substitutes for runtime cases. Report mocked authentication/database behavior explicitly.
- For authorized fixes, add a regression test that exercises the observed mismatch or access case, align the canonical contract/runtime/client layers, and run the affected checks. Keep unrelated API redesign or security hardening outside scope.
- Report operation, baseline/target, expected and observed contract, affected consumers, compatibility impact, and evidence. Distinguish a contract mismatch, a supported access-boundary finding, and an untested risk. Return verified/failed/incomplete for the assessed scope without claiming full security certification.

Version-specific sources: [NestJS OpenAPI types](https://docs.nestjs.com/openapi/types-and-parameters) and [Zod JSON Schema conversion](https://zod.dev/json-schema). Verify the installed generator and runtime version before applying their behavior.
