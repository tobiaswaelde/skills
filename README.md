# Personal Codex skills

Private marketplace `tobiaswaelde`, plugin `personal-skills`.

## Install on another Mac

GitHub SSH access to this private repository is required.

```sh
codex plugin marketplace add git@github.com:tobiaswaelde/skills.git --ref main
codex plugin add personal-skills@tobiaswaelde
```

## Use the coding workflow by default

Automatic invocation is enabled for `coding-workflow`. To make it the default across projects on each device, add these instructions to that device’s `~/.codex/AGENTS.md`, preserving existing instructions:

```markdown
- Use the `coding-workflow` skill by default for coding project tasks in every project, regardless of repository host or issue tracker, unless I explicitly request a different workflow. Read and follow its instructions without requiring me to invoke it by name.
- Prefer native Codex question/selection controls for clarifications and approvals whenever an available tool supports that purpose in the current mode. Offer concise action-specific choices instead of requiring a typed "yes"; wait for an actually submitted answer before dependent work.
```

The plugin synchronizes the skill; global `AGENTS.md` settings are configured separately on each device.

Workflow and guideline decisions use the bundled [native interactive decision policy](skills/coding-workflow/references/interactive-decisions.md), including implementation/worktree selection and feature-PR merge approval. Actual UI availability depends on the client and tools exposed in the current mode; unavailable controls fall back to a focused chat question without bypassing approval.

Larger features are planned as small, ordered implementation tasks with acceptance checks. Each completed task gets its own commit and recorded validation before the next dependent task starts; feature-wide checks run before the PR. This normally stays within one feature issue, branch, and PR, and retains the agreed approval gates and merge method.

## Create and publish skills

Edit the source checkout, never the installed cache. Ask `$skill-creator` to create new skills under this repository’s `skills/` directory. Each skill needs its own folder and `SKILL.md`.

After validating changes, increment the version in `plugin.json`, commit, and push to `main`. Update each device:

```sh
codex plugin marketplace upgrade tobiaswaelde
codex plugin add personal-skills@tobiaswaelde
```

Start a new Codex chat after installation or updates; restart the app if changes do not appear. No background services or automatic commits are configured.

## Personal skills

- `coding-workflow`: general coding delivery workflow, independent of repository host and issue tracker. Read the bundled `coding-guidelines` skill and project standards before planning; enforce agreed guidelines during implementation and verify them before the PR. Plan, record the task, obtain implementation and checkout/worktree approval, implement, validate, and open the project’s PR/MR. Merge feature work only after separate approval and successful CI, then update the local base and delete the feature branch. Changesets release PRs are always merged manually. Projects without hosted tracking/review agree on a local delivery path. Invoke with `$coding-workflow`.
- `coding-guidelines`: audit existing standards and fill agreed gaps, or establish consistent guidelines in new projects. Includes self-contained CRM tenant-web/Crateview registry-ui baselines for linting, formatting, and exact Vue block/attribute ordering. JSDoc/TSDoc coverage follows the user's preference: all hand-written functions, composables, and exported types. Preserve intentional project conventions; agree on conflicting migrations first. Invoke with `$coding-guidelines`.

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

`personal-skills` contains only `coding-workflow` and `coding-guidelines`. Caveman and Matt Pocock skills, resources, and compatibility behavior come directly from their upstream plugins. Caveman Cloud and Learn commands still require their upstream CLI and service authentication.

The nine previously installed Matt Pocock `in-progress` and `misc` skills were removed locally; the upstream plugin determines its own supported skill set.

## Coding guideline template

The `coding-guidelines` skill includes a concrete [project guideline template](skills/coding-guidelines/assets/templates/coding-guidelines.md) and a [reference-project analysis](skills/coding-guidelines/references/source-analysis.md). The template contains the reusable standards; the analysis records CRM tenant-web/registry-ui source revisions, common rules, differences, and the user's additional documentation policy.

When applying it, resolve project fields and actual check commands, remove irrelevant technology sections, and merge with existing intentional standards. No access to either reference repository is required to use the installed template.
