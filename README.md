# Personal Codex skills

Private marketplace `tobiaswaelde`, plugin `personal-skills`.

## Install on another Mac

GitHub SSH access to this private repository is required.

```sh
codex plugin marketplace add git@github.com:tobiaswaelde/skills.git --ref main
codex plugin add personal-skills@tobiaswaelde
```

## Use the coding workflow by default

Automatic invocation is enabled for `coding-workflow`. To make it the default across projects on each device, add this instruction to that device’s `~/.codex/AGENTS.md`, preserving existing instructions:

```markdown
- Use the `coding-workflow` skill by default for coding project tasks in every project, regardless of repository host or issue tracker, unless I explicitly request a different workflow. Read and follow its instructions without requiring me to invoke it by name.
```

The plugin synchronizes the skill; global `AGENTS.md` settings are configured separately on each device.

## Create and publish skills

Edit the source checkout, never the installed cache. Ask `$skill-creator` to create new skills under this repository’s `skills/` directory. Each skill needs its own folder and `SKILL.md`.

After validating changes, increment the version in `plugin.json`, commit, and push to `main`. Update each device:

```sh
codex plugin marketplace upgrade tobiaswaelde
codex plugin add personal-skills@tobiaswaelde
```

Start a new Codex chat after installation or updates; restart the app if changes do not appear. No background services or automatic commits are configured.

## Personal skills

- `caveman`: concise responses; invoke with `$caveman` or `/caveman`.
- `ultracave`: maximum compression; invoke with `$ultracave` or `/caveman ultra`.
- `coding-workflow`: general coding delivery workflow, independent of repository host and issue tracker. Read the bundled `coding-guidelines` skill and project standards before planning; enforce agreed guidelines during implementation and verify them before the PR. Plan, record the task, obtain implementation and checkout/worktree approval, implement, validate, and open the project’s PR/MR. Merge feature work only after separate approval and successful CI, then update the local base and delete the feature branch. Changesets release PRs are always merged manually. Projects without hosted tracking/review agree on a local delivery path. Invoke with `$coding-workflow`.
- `coding-guidelines`: audit existing standards and fill agreed gaps, or establish consistent guidelines in new projects. Includes self-contained CRM tenant-web/Crateview registry-ui baselines for linting, formatting, and exact Vue block/attribute ordering. JSDoc/TSDoc coverage follows the user's preference: all hand-written functions, composables, and exported types. Preserve intentional project conventions; agree on conflicting migrations first. Invoke with `$coding-guidelines`.

## Imported skill collections

All collections are included in `personal-skills@tobiaswaelde`; no additional marketplace or plugin installation is required.

### Caveman family (22 skills)

Imported from the existing local installations of [Julius Brussee's Caveman](https://github.com/JuliusBrussee/caveman), including supporting scripts and resources. The two response modes listed above are part of this collection.

`cavecrew`, `caveman`, `caveman-commit`, `caveman-compress`, `caveman-discover`, `caveman-evidence-review`, `caveman-explore`, `caveman-help`, `caveman-learn`, `caveman-manage`, `caveman-optimize`, `caveman-review`, `caveman-setup`, `caveman-stats`, `investigate-first`, `lean-build`, `megacave`, `migration`, `safe-refactor`, `surgical-patch`, `ultracave`, `verify-and-stop`.

### Matt Pocock (36 skills)

Imported from the existing local installations of [Matt Pocock's skills](https://github.com/mattpocock/skills), including reference documents, templates, and helper scripts.

`ask-matt`, `chief-of-staff`, `code-review`, `codebase-design`, `diagnosing-bugs`, `domain-modeling`, `grill-me`, `grill-with-docs`, `grilling`, `handoff`, `implement`, `implement-spec`, `improve-codebase-architecture`, `loop-me`, `migrate-to-shoehorn`, `pr`, `prototype`, `research`, `retro`, `scaffold-exercises`, `setup-matt-pocock-skills`, `setup-pre-commit`, `setup-ts-deep-modules`, `tdd`, `teach`, `to-questionnaire`, `to-spec`, `to-tickets`, `triage`, `wait-what`, `wayfinder`, `wizard`, `writing-beats`, `writing-for-agents`, `writing-fragments`, `writing-shape`.

This preserves the installed collection, including the upstream `in-progress` and `misc` skills that were installed separately. Their inclusion does not promote them to stable upstream plugin features.

### Codex compatibility and external dependencies

- Existing explicit-only invocation policies are preserved in `agents/openai.yaml` with `policy.allow_implicit_invocation: false`. Unsupported Claude frontmatter (`disable-model-invocation`, `argument-hint`, `tools`, and `model`) is removed from skill entrypoints; instruction bodies remain intact except for documented Codex compatibility notes.
- Cavecrew's three task profiles are bundled under `agents/` as reference prompts. The `cavecrew` skill explains how to pass them to Codex delegation tools; these Markdown files do not register native Codex agent types or enforce a model/tool sandbox.
- Caveman Cloud and Learn skills require the upstream Caveman CLI, service access, and authentication for their respective commands. Installing this plugin does not install that CLI, connect a paid service, or enable Claude hooks.
- `caveman-stats` retains its host-specific behavior: Codex uses available native usage information; Claude transcript statistics require Claude Code's hooks.
- Skill source paths and installer folder hashes are recorded in [skill-sources.json](skill-sources.json). This is an explicit snapshot, not automatic upstream synchronization. Local installations under `~/.agents/skills` are preserved; overlapping copies can be removed separately after verification.

### Attribution

Caveman notices and license texts are preserved under [third-party/caveman](third-party/caveman/). Matt Pocock's MIT notice is preserved under [third-party/mattpocock/LICENSE](third-party/mattpocock/LICENSE). Original per-skill credit metadata is retained.

## Coding guideline template

The `coding-guidelines` skill includes a concrete [project guideline template](skills/coding-guidelines/assets/templates/coding-guidelines.md) and a [reference-project analysis](skills/coding-guidelines/references/source-analysis.md). The template contains the reusable standards; the analysis records CRM tenant-web/registry-ui source revisions, common rules, differences, and the user's additional documentation policy.

When applying it, resolve project fields and actual check commands, remove irrelevant technology sections, and merge with existing intentional standards. No access to either reference repository is required to use the installed template.
