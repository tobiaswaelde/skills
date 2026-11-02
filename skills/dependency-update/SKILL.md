---
name: dependency-update
description: Plan and apply scoped dependency upgrades with version/peer/runtime compatibility, workspace catalog and lockfile ownership, migration notes, and affected-consumer validation. Use for dependency-update requests or reviewed update PRs, not automatically upgrading every outdated package or bypassing explicit project version pins.
---

# Dependency update

Follow the bundled [specialist execution boundaries](../coding-workflow/references/specialist-execution.md), current coding-workflow approvals, and project guidelines. An assessment/update-plan request stays read-only; an approved update changes only its agreed dependency group and necessary compatibility fixes.

## Establish ownership and scope

- Identify requested packages, target versions/ranges or security advisory, affected workspaces, and allowed update scope. Inspect runtime/package-manager pins, manifests, lockfiles, workspace catalogs, overrides/resolutions, patches, peer dependencies, package exports, and CI/container runtime versions. Use the owning package manager and version; do not generate a second manager's lockfile.
- Read explicit repository constraints before selecting targets. A generic outdated-package report does not authorize changing an exact TypeScript/runtime/framework pin, broadening ranges, switching registries, or migrating package managers. If a requested update requires crossing a protected boundary, present the concrete target and compatibility consequence through native choices before dependent edits.
- Distinguish declared ranges, resolved lockfile versions, and transitive/peer requirements. In monorepos find the actual catalog/config owner and all affected consumers, including generated-code consumers and tooling plugins. Do not inline catalog-managed versions into each package just to perform an update.

## Choose a compatible update group

- Use the [compatibility matrix](references/compatibility-matrix.md). Check version-matched official release notes, migration guides, registry metadata, and relevant advisories for the proposed target. Verify current targets rather than assuming latest from memory; do not automatically select a prerelease or major migration because it exists.
- Group dependencies only where compatibility couples them, such as runtime/framework/compiler, ESLint and parsers/plugins, UI library/framework, ORM/client generation, or a bundler and its plugins. Keep unrelated updates separately reviewable instead of refreshing the entire tree without need.
- Record current to target versions, direct/transitive/peer impact, engine requirements, supported runtime(s), relevant breaking behavior/config changes, affected consumers, and planned checks. Resolve incompatible peer/runtime constraints by selecting an appropriate target or an agreed migration, not by disabling peer checks, forcing installation, or loosening security/version constraints.
- Inspect a proposed override or patch as part of the graph: confirm why it exists and whether the update makes it obsolete. Do not remove it, expand trust/install-script permissions, or switch dependency sources without a concrete scope reason and required agreement.

## Apply the approved update

- Update the owning manifest/catalog/override and lockfile with the project's supported package-manager commands. Preserve workspace protocols, exact pins, unrelated resolutions, and existing trusted sources. Review lockfile changes for scope, engine/peer warnings, integrity/source changes, and significant lifecycle/native build changes when present; investigate unexplained churn instead of hand-editing checksums or deleting the lockfile as a shortcut.
- Apply necessary source/config/test migrations and regenerate project-owned outputs through the existing generation process. A new library's default behavior is not automatically the intended product behavior; preserve supported API, UI, authorization, localization, and data contracts unless an actual migration is agreed.
- Keep task boundaries and per-task commits from the active workflow. Add release metadata according to repository rules; dev-only dependencies are not automatically exempt where the repository requires a changeset. Avoid duplicate release entries solely because one compatibility group spans multiple commits.

## Verify resolved behavior

- Confirm final declared and resolved versions, peers, engines, and catalog consumers. Perform the project's appropriate clean/immutable-lockfile install check in an isolated supported environment; a successful install in a stale working node_modules tree is not reproducibility evidence. Do not reset or delete a user's unrelated environment to get a clean state.
- Run focused checks for the changed group, then affected-project lint/format/types/tests/build checks required by project policy. For framework/UI updates verify relevant browser/SSR/hydration behavior; for API/ORM updates verify contracts, generation and local migration compatibility. Pure compilation cannot prove unchanged runtime behavior.
- Where formatter or lint packages change, verify agreed rule activation and a stable fix/format pipeline, following coding-guidelines. Do not blanket-update snapshots, disable rules, rewrite broad source areas, or suppress errors merely because the new tool reports them.
- Record baseline failures separately and distinguish missing registry/network/runtime access from code defects. If compatibility cannot be established, keep the candidate scoped and report the blocker; do not claim success or silently fall back to a different version/range.

## Report the update

- List requested versus applied version changes, affected consumers, migration/behavior impact, checks and install environment, and remaining gaps. Identify deferred protected pins or major upgrades explicitly.
- Retain existing commit/PR/CI/merge gates. Completing a dependency update does not authorize publishing, deployments, production migrations, or merging a release/versioning PR.

Version-specific source: [pnpm catalogs](https://pnpm.io/catalogs) explains shared version ownership. Catalog features and commands vary by installed package-manager version; preserve the actual project mechanism rather than copying the newest configuration format.
