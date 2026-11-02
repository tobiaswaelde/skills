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

## Included skills

- `caveman`: concise responses; invoke with `$caveman` or `/caveman`.
- `ultracave`: maximum compression; invoke with `$ultracave` or `/caveman ultra`.
- `coding-workflow`: general coding delivery workflow, independent of repository host and issue tracker. Plan, record the task, obtain implementation and checkout/worktree approval, implement, validate, and open the project’s PR/MR. Merge feature work only after separate approval and successful CI, then update the local base and delete the feature branch. Changesets release PRs are always merged manually. Projects without hosted tracking/review agree on a local delivery path. Invoke with `$coding-workflow`.

The imported skill instructions and existing resources are retained. The legacy ultracave `disable-model-invocation` frontmatter flag is represented by Codex’s supported `agents/openai.yaml` setting `policy.allow_implicit_invocation: false`.
