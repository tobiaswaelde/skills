# Select the relevant specialist check

Read only entries relevant to the current request and changed boundaries. Each specialist inherits the active task's scope and approvals; selecting a check does not create a second issue/branch or authorize extra implementation, infrastructure changes, or external operations.

| Trigger in the current task | Bundled skill | Purpose and boundary |
| --- | --- | --- |
| Changed user-visible Vue/Nuxt flow, interaction, locale, or responsive behavior | [vue-feature-check](../../vue-feature-check/SKILL.md) | Verify acceptance with appropriate component/browser evidence; coding-guidelines still owns style |
| Changed transport schema, validator/serializer, API endpoint, client contract, or relevant access scope | [api-contract-check](../../api-contract-check/SKILL.md) | Check runtime/document/client agreement and affected access cases; not a general infrastructure security audit |
| Schema/data evolution, migration implementation or review | [database-change](../../database-change/SKILL.md) | Plan compatible stages and verify approved local fixtures; no production migration execution |
| Requested dependency upgrade or reviewed update PR | [dependency-update](../../dependency-update/SKILL.md) | Preserve version ownership/pins and verify coupled consumers; not an automatic update of every outdated package |
| Explicit release preparation/readiness review | [release-readiness](../../release-readiness/SKILL.md) | Assess candidate-specific gates; no publication/deployment or skill-managed release-PR merge |

Where more than one boundary changes, combine the applicable checks and reuse their evidence. For example, an ORM dependency update with a schema migration can need dependency and database checks; a DTO change consumed by a Vue form can need API and UI checks. Keep the matrix proportional to the agreed scope rather than running all five reviews on every task.

Include the relevant acceptance/check requirements in the plan and issue. Apply them during implementation and before the ready feature PR; defects introduced by the task must be addressed within the current workflow. Required missing evidence remains a gap. Preserve the distinction between local tests, CI, published artifacts, and deployed behavior.
