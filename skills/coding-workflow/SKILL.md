---
name: coding-workflow
description: Default workflow for coding project tasks in every project, from planning through a recorded task, approved implementation, validation, and delivery. Use automatically when planning or implementing coding changes, regardless of repository host, issue tracker, language, or framework, unless the user explicitly requests a different workflow.
---

# Coding workflow

Keep the conversation in the user's language. Write issues, branch slugs, PRs, commits, and changesets in en-US. Follow the target repository's contribution, signing, testing, and release conventions. The approval gates below remain required even if repository guidance permits autonomous implementation or self-merging.

Discover the project’s version control, repository host, issue tracker, review process, CI, and release tooling from repository instructions, remotes, configuration, and existing artifacts. Use their available connectors, CLIs, or APIs; use local version-control tools for checkout and commits. GitHub, GitLab, Bitbucket, Azure DevOps, and separate trackers such as Jira or Linear are examples, not dependencies. Discover actual commands and credentials; do not install mandatory tools or migrate providers merely to follow this skill. The bundled coding-guidelines skill supplies the standards policy below; other specialist skills are optional.

Below, “issue” means the project’s issue or task record; “PR” means its pull request, merge request, or equivalent code-review artifact. Use the provider’s actual terminology, identifiers, links, closing syntax, and merge policies. Git-specific branch/worktree instructions apply to Git projects; adapt to the configured version-control equivalents where supported. If the project lacks a tracker, remote, review mechanism, or version control, ask how to record the task and deliver the change (for example, a local task document and local-only delivery). Keep the same approval gates, adapt only the unavailable steps, and report the agreed local result without inventing remote artifacts or claiming an unperformed merge.

When resuming, reuse existing task artifacts and explicit task-specific approvals recorded in the conversation. Ask again only for a missing approval or materially changed scope. Implementation approval counts only when it was given after the issue was shown; plan approval alone never satisfies that gate.

Before mutating an existing branch or PR, classify it using repository automation and PR metadata. Changesets-generated Version Packages/release PRs (for example, `changeset-release/*`) are always merged manually by the user. Leave them unchanged and report their link; do not request a skill-managed merge, merge them, enable auto-merge, or run post-merge cleanup on their behalf, even with general merge approval and green CI. If classification is uncertain, establish the PR type first. A normal feature PR containing a `.changeset` file remains eligible for this workflow.

## 1. Plan and obtain plan approval

- Inspect repository instructions, remotes, working changes, worktrees, existing issues/PRs, CI, and release configuration. Identify the target repository, task tracker, and agreed integration/base branch from project conventions; use the configured default when unspecified (`main` when that is the project default).
- Read the bundled [coding-guidelines skill](../coding-guidelines/SKILL.md) and the target project's canonical guidelines and effective lint/format/type-check configuration before planning. Identify the applicable documentation coverage, framework conventions, and actual verification commands. For project setup or missing/conflicting standards, use that skill's audit/template process and include agreed setup work in the plan and issue. This analysis stays read-only until the existing implementation approval gate; ordinary feature work follows established rules without automatically launching a repository-wide standards migration.
- Plan in the actual Codex `/plan` mode. A skill cannot grant itself a mode change: if the client has not enabled Plan mode, ask the user to enable it before planning. Reuse a sufficiently detailed, explicitly approved plan already in this conversation rather than repeating it.
- Resolve material ambiguities and present a decision-complete plan with scope, implementation details, acceptance criteria, and validation. Obtain explicit plan approval. Issue creation and all later mutations also require the client to leave Plan mode; user approval alone does not override that restriction.

## 2. Record the issue and obtain implementation approval

- Create an issue from the approved plan, or reuse the matching issue supplied by the user or already associated with this task. Inspect existing artifacts before retrying a mutation to avoid duplicate issues, branches, or PRs.
- Follow repository issue templates while including the problem/current behavior, desired outcome, scope and non-goals, full implementation plan, relevant technical context, acceptance criteria, test strategy, and material risks or dependencies. Record applicable guideline documents, agreed standards decisions, and their verification commands. Include enough detail for another engineer to implement it without this chat; do not replace the plan with a chat link.
- Show the issue link and **ask whether implementation should begin**. Plan approval is not implementation approval. Wait for an affirmative answer after showing the created/reused issue before creating a branch, checking out code, creating a worktree, or implementing changes. If declined, leave the issue available and stop.
- Before checkout, ask whether to use the current checkout or a separate worktree, unless that choice is already explicit for this task. This choice can be bundled with the implementation question. Preserve unrelated changes; if the chosen checkout is unsafe, explain the concrete conflict and resolve the checkout choice before proceeding. Never silently stash or discard work.

## 3. Create the issue branch and implement

- Fetch the latest base and create the task branch from that remote base. Follow repository naming conventions; otherwise use `<issue-id>-<english-kebab-case-title>`. Link the branch to the issue through the provider’s supported association mechanism when available; otherwise record the branch in the issue and link the eventual PR. A name alone is not proof of a provider-side association.
- Reuse an existing branch/worktree only after verifying it belongs to this issue and inspecting its changes. Use the selected checkout/worktree; when Codex managed-worktree tools are available, use and attach those appropriately.
- Implement the agreed scope, required documentation, and meaningful tests. Run the repository's relevant checks and repair task-related failures. Record actual results and distinguish unrelated or externally blocked failures. Material scope changes require updating the issue and obtaining agreement before expanding the work.
- Apply the project's established and explicitly agreed coding guidelines throughout implementation. Before committing or opening a ready feature PR, run the applicable non-mutating lint, format, and type checks; inspect required documentation coverage and Vue block/attribute ordering when applicable. Use coding-guidelines for standards-specific verification, including formatting convergence when formatting rules change. Fix violations introduced by the task; report pre-existing failures and enforcement gaps without unrelated mass cleanup, silent rule changes, or claims that syntax-only checks prove documentation coverage. Retain the existing approval gates when resolving conflicting standards.
- Detect an existing Changesets setup from manifests, configuration, and repository instructions. When required by those rules, add a changeset for the affected packages with the appropriate SemVer bump using the repository's package manager and Changesets CLI. Do not invent a changeset requirement for a repository without Changesets, or run release/version/publish commands as part of a feature task.
- Stage only task-owned files, including any required changeset. Follow repository commit/signing conventions, commit even when no changeset is needed, and push the issue branch.

## 4. Open the feature PR and request merge approval

- Create or update one feature PR targeting the agreed base. Include a closing issue reference, the concrete problem and resulting behavior, implementation details relevant to review, and validation actually obtained, including applicable guideline checks and manual documentation review. Follow the repository PR template. Attach the PR to the current Codex chat when supported.
- Immediately show the PR link and **ask whether this feature PR may be merged after CI succeeds**. Do not merge or enable auto-merge before that approval. An unanswered or declined question leaves the PR open; approvals apply only to the identified PR and agreed scope.

## 5. Verify CI and merge the approved feature PR

- Check repository-required PR status checks, review/merge requirements, and any explicitly required full-CI workflows for the current head and applicable base. A push, old successful run, diagnostic run, pending check, or absence of visible checks is not proof that required CI passed. If the repository intentionally has no CI, disclose that and obtain an explicit exception before merging.
- Respect CI opt-in labels and repository policies. Fix task-related CI failures, validate, and push corrections. Verify checks again after every changed head or base; do not bypass branch protection or required reviews. For blocked checks or missing access, report the specific blocker and retain issue/branch/PR context for resumption instead of claiming completion or creating an unrequested background monitor.
- With the user's merge approval and all requirements satisfied, merge the feature PR. Default to squash where allowed; repository-enforced methods take precedence. Verify the hosting/review provider reports the intended PR as merged before cleanup.

## 6. Update the local base and delete the feature branch

- Fetch and fast-forward the local base branch after the confirmed feature merge. In a worktree flow, update the checkout that owns that branch rather than trying to check it out twice. Preserve dirty checkouts and divergent local history; report a concrete blocker instead of resetting them.
- Delete the merged feature branch remotely and locally, tolerating a remote branch already deleted by the host. Verify it is the task branch and has no new commits beyond the merged PR head. For squash merges, verify the provider-confirmed merge and matching branch head rather than treating Git ancestry alone as proof of unmerged work.
- Release a task-created worktree only when its work is safely merged and it contains no uncommitted or additional work; use Codex's managed-worktree lifecycle when applicable. Preserve pre-existing or dirty worktrees and explain if they prevent local branch deletion. Never delete the base branch.
- Finish with concise `Changed`, `Tests`, and `Notes` sections, omitting empty sections. Link the issue and PR, report merge and cleanup status, and identify any remaining blocker truthfully.
