---
name: coding-workflow
description: Default coding delivery workflow for new tasks or existing plans. Use Spec Kit planning when available and Codex Plan mode otherwise, with separate task-generation, issue, implementation, and merge approvals, existing task worktree reuse, and per-task commits. Use across repositories, hosts, languages, and frameworks unless the user requests a different workflow.
---

# Coding workflow

Keep the conversation in the user's language. Write guides, specifications, plans, tasks, issues, branch slugs, PRs, commits, and changesets in en-US. Follow the target repository's contribution, signing, testing, and release conventions.

Use the bundled [native interactive decisions](references/interactive-decisions.md) for the approval gates below. Ask only for missing decisions; reuse valid approvals and completed artifacts. Approval of a plan or task list is not permission to create an issue, implement product code, or merge. An approved issue step includes the disclosed planning-file commit, not implementation.

Discover the actual version control, repository host, tracker, review process, CI, release tooling, and available tools. Below, “issue” and “PR” mean the project's corresponding task record and review artifact. Use its real conventions and links. If hosted tracking or review is unavailable, agree on a local delivery path and keep equivalent gates; do not invent remote artifacts.

Changesets-generated Version Packages/release PRs remain manual: leave them unchanged, report their link, and do not merge, enable auto-merge, or clean up their branches. A normal feature PR containing changesets follows this workflow.

## 1. Inspect the workspace and plan

- On every entry or resumption, including “implement this plan,” inspect repository instructions, the current task and approvals, remotes, working changes, existing issues/PRs, CI, and release configuration. Identify the intended base branch from project conventions.
- Detect the selected checkout with `git rev-parse --show-toplevel --git-dir --git-common-dir` and `git worktree list --porcelain`, plus Codex workspace/task metadata when available. Reuse a suitable active task worktree, including one already provisioned by Codex, without another location question. Record its path, branch or detached HEAD, task association, and whether it predates this workflow. Resolve concrete conflicts or ambiguous selection; a directory name alone does not prove suitability.
- If no checkout choice exists, resolve current checkout versus separate worktree before the first planning-file write. Preserve unrelated changes. A detected worktree supplies the location choice, not later task, issue, implementation, or merge approvals.
- Read the bundled [coding-guidelines](../coding-guidelines/SKILL.md), project standards, and actual check configuration. Select [relevant specialist checks](references/specialist-routing.md) for the current task. Include their acceptance evidence without launching unrelated audits or tooling migrations.
- Inspect the repository root for `.specify/` and project-local Spec Kit skills or commands. When markers exist, read [Spec Kit integration](references/spec-kit-integration.md). Report whether the setup is usable, absent, or blocked by a concrete gap. Missing chat shortcuts alone do not make locally readable instructions unusable. Installation or migration is separate scope.
- **Usable Spec Kit:** start with the installed `speckit-plan` procedure, reusing the matching feature specification and any existing plan. If a required specification is missing, derive it from the requested scope using the installed specification procedure first; clarify missing substantive requirements. Planning may write specification/design artifacts, but must stop before task generation or product implementation. Do not require Codex Plan mode for this path. If the client is already in Plan mode, perform read-only work and ask it to leave that mode before file-writing Spec Kit steps; a skill cannot change the mode itself.
- **Spec Kit unavailable:** require actual Codex Plan mode for planning and obtain native plan approval. Reuse an already detailed, explicitly approved plan instead of forcing another planning pass. Wait for the actual return to a writable mode before mutations. Prepare the design and acceptance criteria here; detailed task generation has its own next gate.
- Reuse a preexisting approved plan as input to the selected path. In a prepared Spec Kit project, reconcile it through the installed planning procedure rather than skipping Spec Kit or asking the user to recreate the plan. Present the resulting plan and actual artifact paths before the next decision.

## 2. Ask before generating tasks

- Ask whether to run `speckit-tasks` from the presented plan. Offer task generation, plan revision, or stopping here. A submitted task-generation choice accepts the presented plan for this phase and authorizes task-file generation only. Do not run tasks automatically as a handoff from `speckit-plan`.
- Without usable Spec Kit, ask for the equivalent detailed task breakdown and produce it in the agreed local format or chat. Do not claim Spec Kit ran.
- After approval, generate or reconcile ordered implementation tasks with stable IDs, scope, dependencies, acceptance criteria, and relevant checks. Each task should form a coherent increment containing its code, tests, and documentation. Plan a separate commit for every completed implementation task.
- Reuse a matching existing task list and explicit task-generation approval on resumption. Present the actual tasks and any relevant consistency findings; material plan changes return to the affected approval gate.

## 3. Ask to record the issue, then commit planning files

- Show the proposed task scope and ask whether to create the issue and commit the generated planning files. Offer creation, revision, or stopping with the current artifacts. Wait for explicit approval before creating the issue or making the planning commit. If a matching issue already exists, reuse it and obtain any missing approval to associate the plan and commit its files; do not create a duplicate.
- Create or update the approved issue using project templates. Include the problem, desired outcome, scope and non-goals, full implementation plan, ordered task checklist, dependencies, acceptance checks, guideline decisions, validation strategy, and material risks. Artifact links supplement the issue rather than replacing its implementation context.
- After the issue exists, fetch the base and establish the task branch in the selected checkout. Follow repository branch conventions; otherwise use `codex/<issue-id>-<english-kebab-case-title>`. For a new branch, verify the starting commit against the intended remote base. Reuse verified task branches and preserve their commits. For a suitable detached Codex worktree, create the task branch there; do not create another worktree. Preserve generated planning files and other relevant work when selecting the starting point; resolve conflicts without silent stash, reset, or discard.
- Associate the branch with the issue through the provider's supported mechanism, or record the branch in the issue and link the eventual PR. A branch name alone is not a provider-side association.
- Inspect the planning diff and validate the generated artifacts. Commit only the task's specification, plan, tasks, related design artifacts, and necessary nonsecret feature context in a separate en-US planning commit referencing the issue. Follow the repository's policy for metadata-only changes; this planning commit does not need a fabricated package release entry. Without Spec Kit, commit the agreed planning files if any; a chat-only plan does not require an empty commit.
- Record the planning commit SHA and artifact paths in the issue. Inspect existing commits on resumption and skip a planning commit already completed for the same artifacts. A changed plan needs a new reviewed commit, not an empty duplicate. If this step fails, report the blocker before asking to start implementation.

## 4. Ask to implement, then commit each task

- Show the issue link, planning commit, selected worktree, and next unfinished task. Ask whether implementation should begin. Offer starting implementation or keeping the committed plan only. Wait for this separate approval after the issue and planning commit are reviewable. Earlier planning, task-generation, and issue approvals do not authorize product implementation.
- Before editing product code in a prepared Spec Kit project, verify that the feature context resolves to the intended specification, plan, and ordered tasks and that their IDs match the issue checklist. Execute the installed Spec Kit implementation procedure within these gates and commit boundaries; use the integration reference for adaptations.
- Implement approved tasks in dependency order. For each task, complete its scoped code, tests, and documentation; run its relevant checks and repair introduced failures before committing or starting a dependent task. Keep valid implementation approval for later tasks in the same scope. Material additions require updating artifacts and obtaining agreement before expansion.
- Detect Changesets from repository configuration. **When Changesets is implemented, include a separate changeset in each implementation-task commit**, generated with the project's package manager and Changesets CLI. Select affected packages and the appropriate SemVer bump for that task. For an increment with no releasable package impact, use an empty changeset if supported by the project's policy; if policy forbids it, disclose that concrete exception rather than inventing a package bump. Without Changesets, do not add the tool or invent metadata. Never run version, release, or publish commands as a task step.
- Run applicable non-mutating lint, format, type, and task checks; verify required documentation and framework conventions. Report preexisting failures and evidence gaps without unrelated cleanup or relaxed checks. If a task cannot form a working increment, revise its boundary within approved scope rather than committing broken work.
- Stage only the completed task, its tests/docs, and its changeset when applicable. Create its own meaningful en-US commit referencing task and issue IDs before moving to the next dependent task. Do not accumulate all tasks into a final commit, combine future tasks, or rewrite completed task commits during ongoing work. Pure read-only verification does not require an empty commit.
- Mark implementation tasks complete only after checks and their commit succeed. Synchronize task status between Spec Kit and the issue; record validation and commit SHAs in the issue. Resume at the first incomplete task after verifying recorded work.
- After all tasks, run feature-wide integration and guideline checks and applicable specialists. Repair regressions in scoped follow-up commits with their Changesets metadata under the same policy. Push completed commits according to project rules, at latest before opening the PR.

## 5. Create the PR and ask about merging

- Once implementation and feature-wide checks are complete, create or update one feature PR against the agreed base without an extra PR-creation gate. Follow its template, link the issue with the provider's closing syntax, and summarize resulting behavior, relevant implementation decisions, and actual validation. Attach the PR to the Codex chat when supported.
- Immediately show the PR link and ask whether it may be merged after CI succeeds. Offer “Merge after successful CI” and “Leave open.” Wait for explicit approval for that PR before merging or enabling auto-merge. Implementation approval never includes merge approval.

## 6. Verify current CI and merge the approved feature PR

- Verify required checks, reviews, branch protection, and explicitly required full-CI workflows for the current head and applicable base. An old run, pending check, diagnostic run, or absence of visible checks is not proof of passing CI. If the repository intentionally has no CI, disclose that and obtain an explicit exception before merging.
- Respect CI opt-in policies. Fix task-related failures, validate, push, and recheck after every changed head or base. Preserve the approved scope; do not bypass protections or required review. Report concrete blockers without claiming completion or creating an unrequested background monitor.
- With separate merge approval and all requirements satisfied, merge the feature PR. Default to squash where permitted, respecting repository policy. Confirm the provider reports the intended PR as merged before cleanup.

## 7. Update the base and clean up task-owned branches

- Fetch and fast-forward the local base in the checkout that owns it. Preserve dirty checkouts and divergent history; report blockers rather than resetting them.
- Delete the merged task branch remotely and locally after verifying it has no newer work beyond the provider-confirmed PR head. For squash merges, use the confirmed merge and matching branch head rather than Git ancestry alone. Tolerate a remote branch already deleted by the host; never delete the base branch.
- Release a worktree created by this workflow only when its work is safely merged and no additional changes or commits remain. Use Codex's managed lifecycle where applicable. Preserve preexisting Codex worktrees and unrelated work; explain any resulting branch-cleanup limitation.
- Finish with concise `Changed`, `Tests`, and `Notes` sections, omitting empty sections. Link the issue and PR, report verified merge and cleanup status, and state remaining blockers.
