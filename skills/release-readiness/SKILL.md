---
name: release-readiness
description: Assess a specified release candidate for changeset/version correctness, compatibility and migration notes, current CI, and required package/container artifacts. Use when preparing or reviewing a release gate, not for routine feature validation or executing a release, publish, deployment, or release-PR merge.
---

# Release readiness

Follow the bundled [specialist execution boundaries](../coding-workflow/references/specialist-execution.md). This skill produces a release assessment. Changesets/Version Packages PRs are always merged manually by the user. A ready verdict is not permission to merge, tag, publish, deploy, or change a live environment.

## Fix the release scope

- Identify the package/application(s), candidate revision or release PR, channel, and intended destination from the request and release configuration. Resolve refs to exact commits and state whether uncommitted work is excluded. If the release target is materially ambiguous, ask through the native decision policy before making a readiness claim.
- Find the relevant previous release using the project's actual package/application tags and channel, not the first tag or lexical version ordering. Compare that baseline with the candidate and inspect relevant release PRs, notes, changesets, and deployment/migration configuration. If no reliable baseline exists, assess available evidence and mark historical compatibility unverified.
- Read manifests, Changesets configuration where present, linked/fixed groups, dependency release rules, private/ignored package handling, publishing/build scripts, container workflows, and the project's required gates. Do not impose npm publishing or Changesets on a project that releases through another mechanism.

## Review release content and metadata

- Reconcile user-visible changes with release entries, affected packages/apps, proposed versions/changelogs, and internal dependency changes. Evaluate the highest required SemVer impact and the configured package grouping/propagation behavior. A patch label is not evidence that a breaking contract change is safe.
- Compare public API/wire behavior, runtime requirements, configuration/env defaults, authentication/permissions, data compatibility, and installation/upgrade behavior. Check supported existing consumers, not only the new code compiled against itself. Use API/database specialists for identified boundaries within the assessment scope; do not silently execute migrations.
- Verify breaking changes have usable migration/upgrade notes and an explicit compatibility/rollout decision. Include database and application rollback constraints where relevant; reverting an image or package does not automatically reverse data changes.
- Distinguish a normal feature PR carrying a changeset from the release/versioning PR that consumes it. Inspect existing generated release output rather than running version/publish scripts merely to see what they do. Any isolated generation/build explicitly authorized for this review stays outside the source checkout and uses controlled fixtures; do not invoke publishing-capable scripts as checks.

## Review execution evidence

- Use the [readiness matrix](references/readiness-matrix.md) for the artifacts actually required by the project. Check candidate-specific formatting, lint, type, unit/integration, build, migration, and install/runtime evidence where required. Local checks, CI, registry existence, and a deployed runtime are distinct forms of evidence.
- Inspect required CI/review checks for the candidate PR's current head and applicable base. Old successful runs or manual diagnostics do not substitute for required current PR gates. Preserve required opt-in labels and branch-protection policy; never bypass them to produce a ready verdict.
- For package releases, verify the intended package contents/exports and relevant clean install/consumer evidence when available. For container/application releases, verify required images reference the candidate revision, package/app version and immutable digest/tag, supported platforms, and configured SBOM/signature/provenance. Mark unavailable artifacts as gaps; a source build alone does not prove a published artifact or successful deployment.
- Read-only assessment is the default. Run local non-publishing checks only when requested or part of the approved task, after inspecting their actual scripts and target. Redact credential-bearing workflow logs and registry output. Missing access must remain explicit rather than inferred as success.

## Return a decision, not a release

- Report candidate and baseline revisions, applicable gates with evidence, compatibility/migration findings, and remaining owner actions. Use ready only when every applicable required gate has adequate evidence. Distinguish a known blocker (not ready) from unavailable required evidence (incomplete); both prevent an unconditional ready verdict.
- Keep release PRs unchanged for manual user merging. Do not enable auto-merge, create/push release tags, run release publication, or deploy as a consequence of this assessment. If the user requests those actions, identify the separate execution task and preserve the manual release-PR boundary.
- For discovered defects, propose bounded follow-up work or use already-approved fixes in their existing workflow; do not rewrite the release candidate, bump versions, or expand audit scope automatically.

Version-specific reference: [Changesets CLI responsibilities](https://github.com/changesets/changesets/blob/main/docs/command-line-options.md). Inspect installed tooling and project release scripts before using any command.
