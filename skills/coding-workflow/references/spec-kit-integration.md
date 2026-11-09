# Spec Kit integration

Use Spec Kit as the specification and task layer inside `coding-workflow`. This workflow continues to own approvals, task tracking, checkout selection, commits, validation, PRs, and merges.

## Detect a prepared project

- Inspect the repository's `.specify/` configuration, templates, scripts, and project-local Spec Kit skills or commands. Confirm that the installed integration is usable by the current Codex client and that any CLI or helpers it requires are available. A `.specify/` directory alone is not sufficient.
- Use the project's installed version and invocation instructions. Current Codex integrations expose `.agents/skills/speckit-*/SKILL.md`; older layouts may differ. Read the relevant installed instructions before invoking a step instead of assuming command names or upgrading the integration.
- Resolve the active feature from the project's configured feature context and the current task. Current versions use `.specify/feature.json` or `SPECIFY_FEATURE_DIRECTORY`; a Git branch name alone does not identify the active specification. Reuse matching artifacts and clarify conflicting feature selection rather than overwriting another feature.
- If the setup is absent, incomplete, or unusable, report the specific gap and continue the ordinary workflow where possible. Installations, initialization, integration switches, and migrations require their own agreed scope. Merely finding an old specification does not authorize repairs to the tooling.

## Plan without writing files

- Read the existing constitution, relevant specification, plan, tasks, and project standards. Reuse approved content and completed work instead of restarting specification discovery.
- In actual Codex Plan mode, apply the relevant Spec Kit templates and procedures to draft requirements, implementation decisions, acceptance criteria, and ordered tasks in chat. Keep the same task identifiers when reusing existing artifacts.
- Defer steps that create or update files, feature context, branches, commits, or runtime events until this workflow permits those mutations. In particular, do not invoke the standard `speckit-specify`, `speckit-plan`, or `speckit-tasks` steps unchanged in Plan mode: they persist artifacts. A read-only draft is not a claim that those steps ran.
- Present one decision-complete plan for approval. After leaving Plan mode, record the full approved plan and task checklist in the issue, then obtain implementation and checkout/worktree approval under the main workflow. Links to Spec Kit files supplement the issue rather than replacing its implementation context.

## Persist and implement the approved work

- After implementation approval and branch/worktree selection, save the approved artifacts in the selected checkout using the project's layout and installed integration. Reconcile existing files rather than generating a competing feature. If required feature setup or templates introduce new substantive decisions, resolve them before implementation.
- Preserve this workflow's Git ownership. Inspect Spec Kit extensions, hooks, and events before invoking mutating steps. Use an available documented option to avoid duplicate branching, automatic commits, or unapproved commands. If an installed step cannot honor these boundaries, apply its relevant templates and procedures directly and report that limitation; do not silently rewrite generated skills or project configuration.
- Treat `spec.md` as the feature requirements, `plan.md` as the implementation design, and `tasks.md` as the ordered implementation checklist where that layout applies. Keep stable task IDs and matching status in the issue; record validation and commit SHAs in the issue. Mark implementation tasks complete in both places only after their required checks and commit succeed. Requirements-review checklists have their own criteria and are not implementation completion markers.
- Use the installed implementation skill only within the approved scope and this workflow's per-task validation and commit boundaries. A Spec Kit invocation does not authorize additional issues, branches, delegation, releases, or deployments.

## Check consistency and finish

- When available and relevant, use Spec Kit's read-only analysis to compare requirements, design, and tasks before implementation and review. Honor the installed step's actual side effects and permissions rather than assuming every check is read-only.
- A convergence step can append tasks. Review such additions against the approved scope; obtain renewed agreement for material changes and synchronize accepted tasks with the issue. Do not automatically run an unbounded implement/converge loop.
- Continue the main workflow's feature checks, PR creation, separate merge approval, current CI verification, and cleanup. Preserve pre-existing Codex worktrees under its existing cleanup rules.

Sources for current layouts and behavior: [Codex integration](https://github.github.io/spec-kit/integrations/codex.html), [SDD quickstart](https://github.github.io/spec-kit/quickstart.html), [optional Git extension](https://github.com/github/spec-kit/blob/main/extensions/git/README.md). The project's installed version remains the authority for its available commands and configuration.
