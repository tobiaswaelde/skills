# Spec Kit integration

Use Spec Kit as the specification and task layer inside `coding-workflow`. This workflow continues to own approvals, task tracking, checkout selection, commits, validation, PRs, and merges.

## Detect a prepared project

- Inspect from the repository root, including when the working directory is a subdirectory. Check `.specify/` configuration, templates, scripts, and project-local Spec Kit skills or commands, including current `.agents/skills/speckit-*` and legacy `.codex/prompts/speckit.*.md` layouts. Classify the setup before continuing an existing plan or starting implementation.
- A prepared project has usable Spec Kit templates/procedures and the helpers needed for its installed version. Locally readable skills or command files can be followed directly even when they are not exposed in the current chat's skill picker. A missing chat shortcut is not an unusable setup. A `.specify/` directory alone, without usable contents, is not sufficient.
- Use the project's installed version and invocation instructions. Current Codex integrations expose `.agents/skills/speckit-*/SKILL.md`; older layouts may differ. Read the relevant installed instructions before invoking a step instead of assuming command names or upgrading the integration.
- Resolve the active feature from the project's configured feature context and the current task. Current versions use `.specify/feature.json` or `SPECIFY_FEATURE_DIRECTORY`; a Git branch name alone does not identify the active specification. Reuse matching artifacts and clarify conflicting feature selection rather than overwriting another feature.
- If the setup is absent, incomplete, or unusable, report the specific gap and continue the ordinary workflow where possible. Installations, initialization, integration switches, and migrations require their own agreed scope. Merely finding an old specification does not authorize repairs to the tooling.

## Continue from an existing plan

- Treat a plan created before this skill was invoked as input to Spec Kit, not as a reason to bypass it. Establish its scope, approval state, existing issue, and any implementation approval. Reuse valid approvals and the selected task worktree; ask only for missing decisions under the main workflow.
- If still in Plan mode, read the project's templates and map the existing plan to the specification, design, and ordered task structure in chat. If the plan is already sufficiently detailed and approved, proceed to the next permitted phase without asking the user to recreate it or reenter Plan mode solely for Spec Kit.
- After the issue and implementation approvals are satisfied, automatically complete the artifact handoff described below before product implementation. This applies equally to a new feature and to resuming a feature whose chat plan exists but whose Spec Kit files were never created.
- Reuse complete matching artifacts. Reconcile partial or stale artifacts against the approved plan while preserving completed tasks; ask for renewed agreement only when reconciliation changes approved scope or substantive decisions. Conflicting feature selection still requires clarification.

## Plan without writing files

- Read the existing constitution, relevant specification, plan, tasks, and project standards. Reuse approved content and completed work instead of restarting specification discovery.
- In actual Codex Plan mode, apply the relevant Spec Kit templates and procedures to draft requirements, implementation decisions, acceptance criteria, and ordered tasks in chat. Keep the same task identifiers when reusing existing artifacts.
- Defer steps that create or update files, feature context, branches, commits, or runtime events until this workflow permits those mutations. In particular, do not invoke the standard `speckit-specify`, `speckit-plan`, or `speckit-tasks` steps unchanged in Plan mode: they persist artifacts. A read-only draft is not a claim that those steps ran.
- Present one decision-complete plan for approval. After leaving Plan mode, record the full approved plan and task checklist in the issue, then obtain implementation and checkout/worktree approval under the main workflow. Links to Spec Kit files supplement the issue rather than replacing its implementation context.

## Persist and implement the approved work

- After implementation approval and branch/worktree selection, perform the project's specification, planning, and task-generation procedures automatically, using the approved plan as input. Follow the installed skills/commands directly where available, or apply their templates and scripts when no chat invocation is exposed. Save the approved artifacts in the selected checkout using the project's layout. Reconcile existing files rather than generating a competing feature. If required feature setup or templates introduce new substantive decisions, resolve them before implementation.
- Preserve this workflow's Git ownership. Inspect Spec Kit extensions, hooks, and events before invoking mutating steps. Use an available documented option to avoid duplicate branching, automatic commits, or unapproved commands. If an installed step cannot honor these boundaries, apply its relevant templates and procedures directly and report that limitation; do not silently rewrite generated skills or project configuration.
- Treat `spec.md` as the feature requirements, `plan.md` as the implementation design, and `tasks.md` as the ordered implementation checklist where that layout applies. Keep stable task IDs and matching status in the issue; record validation and commit SHAs in the issue. Mark implementation tasks complete in both places only after their required checks and commit succeed. Requirements-review checklists have their own criteria and are not implementation completion markers.
- Before editing product code, verify that the feature context resolves to the intended feature and its specification, plan, and task files exist, reflect the approved scope, and identify the next unfinished task. Report their actual paths once at this handoff. If this check fails, resolve or report the concrete gap rather than silently proceeding as though Spec Kit was used.
- Execute the next approved task through the installed Spec Kit implementation procedure within this workflow's per-task validation and commit boundaries. When a skill cannot be invoked directly or would violate those boundaries, follow its relevant instructions and task artifacts directly, explaining the specific adaptation. Merely writing three files and then ignoring the task procedure is not Spec Kit integration. A Spec Kit invocation does not authorize additional issues, branches, delegation, releases, or deployments.

## Check consistency and finish

- When available and relevant, use Spec Kit's read-only analysis to compare requirements, design, and tasks before implementation and review. Honor the installed step's actual side effects and permissions rather than assuming every check is read-only.
- A convergence step can append tasks. Review such additions against the approved scope; obtain renewed agreement for material changes and synchronize accepted tasks with the issue. Do not automatically run an unbounded implement/converge loop.
- Continue the main workflow's feature checks, PR creation, separate merge approval, current CI verification, and cleanup. Preserve pre-existing Codex worktrees under its existing cleanup rules.

Sources for current layouts and behavior: [Codex integration](https://github.github.io/spec-kit/integrations/codex.html), [SDD quickstart](https://github.github.io/spec-kit/quickstart.html), [optional Git extension](https://github.com/github/spec-kit/blob/main/extensions/git/README.md). The project's installed version remains the authority for its available commands and configuration.
