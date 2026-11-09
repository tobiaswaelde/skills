# Spec Kit integration

Spec Kit owns the specification, design, and task procedures inside `coding-workflow`; the main workflow owns approval order, tracking, Git operations, per-task validation/commits, PRs, and merges.

## Detect the installed setup

- Inspect from the repository root, including when the current directory is a subdirectory. Check `.specify/` configuration, templates, scripts, and local integration instructions. Current Codex layouts use `.agents/skills/speckit-*`; legacy projects may use `.codex/prompts/speckit.*.md`.
- Use the project's installed version and actual procedures. Locally readable skills or commands can be followed directly even if no chat shortcut is exposed. A `.specify/` directory without usable templates/procedures and required helpers is insufficient. Report concrete missing prerequisites and follow the non-Spec-Kit path where possible; initialization, upgrades, or migrations remain separate scope.
- Resolve the intended feature from configured context and the current task. Current versions use `.specify/feature.json` or `SPECIFY_FEATURE_DIRECTORY`; a Git branch alone does not identify the active feature. Reuse matching artifacts and clarify ambiguous or conflicting selection before overwriting files.
- Inspect extensions, hooks, events, and command handoffs before execution. Keep Git operations under the main workflow and stop at each explicit approval gate. Suppress automatic tasks, commits, branching, or implementation only through documented supported controls. Otherwise follow the relevant installed procedure/templates directly, explain the adaptation, and preserve generated skills and project configuration.

## Start with planning outside Codex Plan mode

- When Spec Kit is usable, start with the installed `speckit-plan` procedure instead of requiring Codex Plan mode. Reuse the existing constitution and feature specification. If the specification required by planning is missing, establish it from the requested scope with the installed specification procedure first and resolve substantive ambiguities.
- An existing approved chat plan is input to this procedure, not a reason to skip it. Reconcile its decisions into the matching feature's plan and related design artifacts without asking the user to repeat accepted decisions. Preserve existing completed work.
- Spec Kit planning is allowed to persist planning artifacts before issue creation or implementation approval under this workflow. If the client is actually in read-only Plan mode, request the real transition to a writable mode before invoking file-writing procedures; never claim a skill changed the mode.
- Finish the planning procedure, show its result and paths, and stop for the main workflow's explicit `speckit-tasks` decision. Do not let a built-in handoff run task generation automatically.

## Generate tasks only after approval

- Run the installed `speckit-tasks` procedure only after the user accepts that step for the presented plan. Existing matching tasks and approvals can be reused; material changes return to the affected gate.
- Give implementation tasks stable IDs, dependency order, scope, acceptance criteria, and checks. Each increment includes its code, tests, docs, and its own commit, plus a changeset when Changesets is implemented under the main workflow's policy.
- Present the task list before asking whether to create the issue and commit planning files. Read-only consistency analysis may inform that decision when supported by the installed version. Verify side effects rather than assuming every check is read-only.

## Commit planning artifacts before implementation approval

- After the approved issue is created or associated, establish or reuse its branch in the selected worktree and commit the reviewed specification, plan, tasks, related design artifacts, and required nonsecret feature context separately. Keep unrelated/generated runtime files out of the commit. Record its SHA and artifact paths in the issue.
- Only then request implementation approval, identifying the issue, planning commit, worktree, and next task. Task-generation and issue approval do not authorize product implementation. A declined implementation request leaves a reviewable issue and committed plan.
- Before implementation, confirm that the feature context resolves to the intended artifacts, reflects the approved scope, and matches the issue checklist. A plan in chat alone does not satisfy this handoff.

## Execute tasks and verify completion

- Execute the next approved task through the installed implementation procedure within the main workflow's checks and commit boundaries. If direct invocation is unavailable or cannot preserve those boundaries, follow its relevant instructions and task artifacts directly and explain the concrete adaptation. Do not merely write the artifacts and then ignore the task procedure.
- Treat the feature specification as requirements, the plan as design, and the task list as implementation order. Keep task IDs and status synchronized with the issue. Mark implementation tasks complete only after validation and their individual commit; record evidence and commit SHAs in the issue. Requirements-review checklists are separate from implementation completion.
- Convergence can append tasks. Review additions against approved scope, obtain renewed agreement for material changes, and synchronize accepted tasks. Do not start an unbounded implement/converge loop or infer permission for extra issues, branches, agents, releases, or deployments.
- Finish through the main workflow's feature checks, automatic PR creation, separate merge approval, current CI verification, and cleanup.

Sources: [Codex integration](https://github.github.io/spec-kit/integrations/codex.html), [SDD quickstart](https://github.github.io/spec-kit/quickstart.html), [optional Git extension](https://github.com/github/spec-kit/blob/main/extensions/git/README.md). The project's installed version determines its available commands and configuration.
