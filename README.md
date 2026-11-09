# Personal Codex skills

Personal marketplace `tobiaswaelde`, plugin `personal-skills`.

## Set up a new computer

Follow the complete [setup guide](docs/setup.md) on a new Linux computer, Mac, or Windows computer using WSL2. It covers system prerequisites, Codex and GitHub authentication, [uv](https://docs.astral.sh/uv/), [Spec Kit](https://github.com/github/spec-kit), [RTK](https://github.com/rtk-ai/rtk), [Context7](https://context7.com/docs/clients/codex), all three skill marketplaces, global instructions, verification, and updates.

If Codex and the other prerequisites are already configured, install the personal skills directly:

```sh
codex plugin marketplace add https://github.com/tobiaswaelde/skills.git --ref main
codex plugin add personal-skills@tobiaswaelde
```

The repository is public; installation over HTTPS does not require a GitHub SSH key. GitHub authentication is still needed to push changes and work with issues or pull requests.

## Use the coding workflow by default

Automatic invocation is enabled for `coding-workflow`. To make it the default across projects on each device, add these instructions to that device’s `~/.codex/AGENTS.md`, preserving existing instructions:

```markdown
- Use the `coding-workflow` skill by default for coding project tasks in every project, regardless of repository host or issue tracker, unless I explicitly request a different workflow. Read and follow its instructions without requiring me to invoke it by name.
- Prefer native Codex question/selection controls for clarifications and approvals whenever an available tool supports that purpose in the current mode. Offer concise action-specific choices instead of requiring a typed "yes"; wait for an actually submitted answer before dependent work.
```

The plugin synchronizes the skill; global `AGENTS.md` settings are configured separately on each device.

Workflow and guideline decisions use the bundled [native interactive decision policy](skills/coding-workflow/references/interactive-decisions.md), including implementation/worktree selection and feature-PR merge approval. Actual UI availability depends on the client and tools exposed in the current mode; unavailable controls fall back to a focused chat question without bypassing approval.

The workflow recognizes a suitable active task worktree, including one already created by Codex, and reuses it without asking for the location again. In prepared projects, the [Spec Kit integration](skills/coding-workflow/references/spec-kit-integration.md) starts with `speckit-plan` in a writable mode, establishing a missing specification first when required. Codex Plan mode is required only without usable Spec Kit; installation or migration is separate scope.

The approval sequence is: present the plan; ask to generate tasks; present tasks and ask to create the issue and commit planning files; create the issue and a separate planning commit; ask to start implementation. Each completed implementation task gets its own validated commit and, when Changesets is configured, its own changeset under the repository's package-impact policy. After implementation and feature-wide checks, the workflow creates the PR and asks whether it may merge after current CI succeeds. Existing plans, artifacts, and valid approvals are reused rather than restarting the sequence.

Larger features are split into small, ordered implementation tasks with acceptance checks. Validation and commit SHAs are recorded in the issue and task completion stays synchronized with Spec Kit. This normally stays within one feature issue, branch, and PR, and retains the agreed merge method.

## Create and publish skills

Edit the source checkout, never the installed cache. Ask `$skill-creator` to create new skills under this repository’s `skills/` directory. Each skill needs its own folder and `SKILL.md`.

After validating changes, increment the version in `plugin.json`, commit, and push to `main`. Update each device:

```sh
codex plugin marketplace upgrade tobiaswaelde
codex plugin add personal-skills@tobiaswaelde
```

Start a new Codex chat after installation or updates; restart the app if changes do not appear. No background services or automatic commits are configured.

## Personal skills

- `coding-workflow`: general coding delivery workflow, independent of repository host and issue tracker. Read bundled guidelines and project standards, use Spec Kit planning when available or Codex Plan mode otherwise, ask before tasks and issue creation, commit planning files, then ask to implement. Validate and commit each implementation task separately with its Changesets metadata when configured, create the PR, and ask about merging after successful CI. Reuse suitable task worktrees and valid approvals. Changesets release PRs stay manual; projects without hosted tracking/review agree on local delivery. Invoke with `$coding-workflow`.
- `coding-guidelines`: audit existing standards and fill agreed gaps, or establish consistent guidelines in new projects. Includes self-contained CRM tenant-web/Crateview registry-ui baselines for linting, formatting, and exact Vue block/attribute ordering. JSDoc/TSDoc coverage follows the user's preference: all hand-written functions, composables, and exported types. Preserve intentional project conventions; agree on conflicting migrations first. Invoke with `$coding-guidelines`.
- `vue-feature-check`: verify changed Vue/Nuxt behavior, interaction states, accessibility, responsive layout, and supported locales. Distinguish mocked component checks from browser/backend evidence. Invoke with `$vue-feature-check`.
- `api-contract-check`: compare runtime validation/serialization, DTOs/schemas, OpenAPI, and consuming clients; verify relevant authorization and tenant/resource isolation. Invoke with `$api-contract-check`.
- `release-readiness`: assess a fixed release candidate's version/changeset metadata, compatibility notes, current checks, and required package/container evidence. Assessment only; release PRs stay manual and it never publishes or deploys. Invoke with `$release-readiness`.
- `database-change`: plan and implement approved schema/data migrations with disposable local checks, compatible rollout stages, and explicit rollback/data-recovery constraints. It does not execute production migrations. Invoke with `$database-change`.
- `dependency-update`: apply scoped upgrades using actual runtime/package-manager pins, catalogs and lockfile ownership; verify coupled consumers and migration impact. Invoke with `$dependency-update`.

All seven personal skills are included in `personal-skills@tobiaswaelde`. The coding workflow selects [relevant specialist checks](skills/coding-workflow/references/specialist-routing.md) by the current task; the specialists reuse its approvals, task commits, and coding guidelines. They do not automatically run every audit or change project tooling.

## External marketplaces

[marketplaces.json](marketplaces.json) is the extensible directory of upstream marketplace Git URLs and plugin selectors. Add another entry to its `marketplaces` array when adopting another source. This file records the sources; registering `tobiaswaelde` alone does not execute the commands below.

Install upstream plugins from their own marketplaces on each device:

```sh
codex plugin marketplace add https://github.com/mattpocock/skills.git
codex plugin add mattpocock-skills@mattpocock
codex plugin marketplace add https://github.com/JuliusBrussee/caveman.git
codex plugin add caveman@caveman
```

Refresh each source independently:

```sh
codex plugin marketplace upgrade mattpocock
codex plugin add mattpocock-skills@mattpocock
codex plugin marketplace upgrade caveman
codex plugin add caveman@caveman
```

`personal-skills` contains the seven personal workflows listed above. Caveman and Matt Pocock skills, resources, and compatibility behavior come directly from their upstream plugins. Caveman Cloud and Learn commands still require their upstream CLI and service authentication.

The nine previously installed Matt Pocock `in-progress` and `misc` skills were removed locally; the upstream plugin determines its own supported skill set.

## Coding guideline template

The `coding-guidelines` skill includes a concrete [project guideline template](skills/coding-guidelines/assets/templates/coding-guidelines.md) and a [reference-project analysis](skills/coding-guidelines/references/source-analysis.md). The template contains the reusable standards; the analysis records CRM tenant-web/registry-ui source revisions, common rules, differences, and the user's additional documentation policy.

When applying it, resolve project fields and actual check commands, remove irrelevant technology sections, and merge with existing intentional standards. No access to either reference repository is required to use the installed template.
