# Release evidence matrix

Apply only gates required by the target project's release policy or a concrete compatibility risk. Record the exact candidate identity and where each conclusion came from.

| Gate | Evidence | Insufficient substitute |
| --- | --- | --- |
| Scope and baseline | Resolved candidate SHA and relevant prior package/app release | Unresolved branch name, unrelated latest tag |
| Version and notes | Affected packages, intended bumps, configured propagation, accurate upgrade/release notes | Existing changeset file with no comparison to actual changes |
| Consumer compatibility | Relevant API/runtime/config/installation checks against supported consumers | New server and new client compiling together |
| Data compatibility | Reviewed migration/rollout constraints and the project's required local/CI migration evidence | Application image rollback described as database rollback |
| Current checks | Required successful checks/reviews for current head and applicable base | Successful prior head, partial/manual diagnostic run |
| Package artifact | Intended built contents/exports and required install/consumer evidence | Only source typechecking |
| Container artifact | Required candidate-linked image/digest/platform evidence and configured supply-chain attestations | A Dockerfile or unrelated image tag |
| Deployment readiness | Reviewed environment/config/migration sequence and rollback constraints | Registry artifact existence treated as deployed success |

Use not-applicable with the policy reason, not as a way to bypass missing evidence. For each blocker/gap, name the exact unresolved requirement and its next owner action. Do not fetch or display credentials to make a report look complete.
