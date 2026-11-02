# Personal Codex skills

Private marketplace `tobiaswaelde`, plugin `personal-skills`.

## Install on another Mac

GitHub SSH access to this private repository is required.

```sh
codex plugin marketplace add git@github.com:tobiaswaelde/skills.git --ref main
codex plugin add personal-skills@tobiaswaelde
```

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
- `github-workflow`: plan, create an issue, obtain implementation approval, choose a checkout/worktree, implement, and open a feature PR. Merge only after separate approval and successful CI, then update the local base and delete the feature branch. Changesets release PRs are always merged manually. Invoke with `$github-workflow`.

The imported skill instructions and existing resources are retained. The legacy ultracave `disable-model-invocation` frontmatter flag is represented by Codex’s supported `agents/openai.yaml` setting `policy.allow_implicit_invocation: false`.
