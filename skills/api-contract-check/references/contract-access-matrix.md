# Contract and access matrix

Select cases for the changed operation and actual project policy. Record fixture identity/scope without secrets and keep expected outcomes independent of the implementation under test.

| Case | Verify |
| --- | --- |
| Allowed request | Intended operation succeeds, correct serialized payload and field permissions |
| Invalid/missing input | Runtime validation, status, error shape, default/coercion behavior |
| Unauthenticated | Established authentication response; no protected fields or side effects |
| Authenticated but denied | Role/action denial according to the actual permission model |
| Missing resource | Established not-found response and no unintended mutation |
| Inaccessible resource | Required concealment/denial policy without existence leakage |
| Cross-tenant/workspace | Both direct identifiers and nested/filter/query paths preserve isolation |
| Cross-resource/group scope | Lists, details, counts, aggregates, search, exports, and applicable background jobs use the authorized scope |
| Role/grant changes | Applicable revocation and union/intersection rules use current authoritative state |
| Field filtering | Sensitive request/response fields respect both action and field permissions |
| Client compatibility | Existing accepted inputs and old-client interpretations compared with the intended target wire contract |
| Documentation/generation | Source schemas, emitted OpenAPI, generated clients, and runtime agree for supported constructs |

Use distinct tenants and disjoint resources to make isolation failures observable. A test that only compares empty responses or mocks the scope filter into correctness cannot demonstrate real isolation. For methods that mutate data, use approved disposable fixtures and verify both allowed effects and absence of denied effects. Mark non-applicable cases with the project reason rather than fabricating a multi-tenant policy.
