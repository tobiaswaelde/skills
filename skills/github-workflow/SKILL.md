---
name: github-workflow
description: Run an issue-driven GitHub delivery workflow from planning through implementation, a feature PR, CI, and an approved merge. Use when the user requests this complete workflow or explicitly invokes github-workflow; ordinary edits or isolated GitHub operations do not require it.
---

# GitHub workflow

Keep the conversation in the user's language. Write issues, branch slugs, PRs, commits, and changesets in en-US. Follow the target repository's contribution, signing, testing, and release conventions. The approval gates below remain required even if repository guidance permits autonomous implementation or self-merging.

Use connected GitHub tools for supported operations and local Git for checkout, implementation, and commits. Use an available GitHub CLI/API for missing capabilities. Discover actual commands and credentials; do not add mandatory tools or install software merely to follow this skill. Optional specialist skills may help, but this workflow must remain usable without them.

When resuming, reuse existing task artifacts and explicit task-specific approvals recorded in the conversation. Ask again only for a missing approval or materially changed scope. Implementation approval counts only when it was given after the issue was shown; plan approval alone never satisfies that gate.

Before mutating an existing branch or PR, classify it using repository automation and PR metadata. Changesets-generated Version Packages/release PRs (for example, `changeset-release/*`) are always merged manually by the user. Leave them unchanged and report their link; do not request a skill-managed merge, merge them, enable auto-merge, or run post-merge cleanup on their behalf, even with general merge approval and green CI. If classification is uncertain, establish the PR type first. A normal feature PR containing a `.changeset` file remains eligible for this workflow.

## 1. Plan and obtain plan approval

- Inspect repository instructions, remotes, working changes, worktrees, existing issues/PRs, CI, and release configuration. Identify the target repository and base branch (`main` when present, otherwise the repository's configured default).
- Plan in the actual Codex `/plan` mode. A skill cannot grant itself a mode change: if the client has not enabled Plan mode, ask the user to enable it before planning. Reuse a sufficiently detailed, explicitly approved plan already in this conversation rather than repeating it.
- Resolve material ambiguities and present a decision-complete plan with scope, implementation details, acceptance criteria, and validation. Obtain explicit plan approval. Issue creation and all later mutations also require the client to leave Plan mode; user approval alone does not override that restriction.

## 2. Record the issue and obtain implementation approval

- Create an issue from the approved plan, or reuse the matching issue supplied by the user or already associated with this task. Inspect existing artifacts before retrying a mutation to avoid duplicate issues, branches, or PRs.
- Follow repository issue templates while including the problem/current behavior, desired outcome, scope and non-goals, full implementation plan, relevant technical context, acceptance criteria, test strategy, and material risks or dependencies. Include enough detail for another engineer to implement it without this chat; do not replace the plan with a chat link.
- Show the issue link and **ask whether implementation should begin**. Plan approval is not implementation approval. Wait for an affirmative answer after showing the created/reused issue before creating a branch, checking out code, creating a worktree, or implementing changes. If declined, leave the issue available and stop.
- Before checkout, ask whether to use the current checkout or a separate worktree, unless that choice is already explicit for this task. This choice can be bundled with the implementation question. Preserve unrelated changes; if the chosen checkout is unsafe, explain the concrete conflict and resolve the checkout choice before proceeding. Never silently stash or discard work.

## 3. Create the issue branch and implement

- Fetch the latest base and create the task branch from that remote base. Follow repository naming conventions; otherwise use `<issue-number>-<english-kebab-case-title>`. Link the branch to the issue using GitHub's linked-branch capability when available; otherwise record the branch in the issue and link the eventual PR. A name alone is not proof of a GitHub Development association.
- Reuse an existing branch/worktree only after verifying it belongs to this issue and inspecting its changes. Use the selected checkout/worktree; when Codex managed-worktree tools are available, use and attach those appropriately.
- Implement the agreed scope, required documentation, and meaningful tests. Run the repository's relevant checks and repair task-related failures. Record actual results and distinguish unrelated or externally blocked failures. Material scope changes require updating the issue and obtaining agreement before expanding the work.
- Detect an existing Changesets setup from manifests, configuration, and repository instructions. When required by those rules, add a changeset for the affected packages with the appropriate SemVer bump using the repository's package manager and Changesets CLI. Do not invent a changeset requirement for a repository without Changesets, or run release/version/publish commands as part of a feature task.
- Stage only task-owned files, including any required changeset. Follow repository commit/signing conventions, commit even when no changeset is needed, and push the issue branch.

## 4. Open the feature PR and request merge approval

- Create or update one feature PR targeting the agreed base. Include a closing issue reference, the concrete problem and resulting behavior, implementation details relevant to review, and validation actually obtained. Follow the repository PR template. Attach the PR to the current Codex chat when supported.
- Immediately show the PR link and **ask whether this feature PR may be merged after CI succeeds**. Do not merge or enable auto-merge before that approval. An unanswered or declined question leaves the PR open; approvals apply only to the identified PR and agreed scope.

## 5. Verify CI and merge the approved feature PR

- Check repository-required PR status checks, review/merge requirements, and any explicitly required full-CI workflows for the current head and applicable base. A push, old successful run, diagnostic run, pending check, or absence of visible checks is not proof that required CI passed. If the repository intentionally has no CI, disclose that and obtain an explicit exception before merging.
- Respect CI opt-in labels and repository policies. Fix task-related CI failures, validate, and push corrections. Verify checks again after every changed head or base; do not bypass branch protection or required reviews. For blocked checks or missing access, report the specific blocker and retain issue/branch/PR context for resumption instead of claiming completion or creating an unrequested background monitor.
- With the user's merge approval and all requirements satisfied, merge the feature PR. Default to squash where allowed; repository-enforced methods take precedence. Verify GitHub reports the intended PR as merged before cleanup.

## 6. Update the local base and delete the feature branch

- Fetch and fast-forward the local base branch after the confirmed feature merge. In a worktree flow, update the checkout that owns that branch rather than trying to check it out twice. Preserve dirty checkouts and divergent local history; report a concrete blocker instead of resetting them.
- Delete the merged feature branch remotely and locally, tolerating a remote branch already deleted by GitHub. Verify it is the task branch and has no new commits beyond the merged PR head. For squash merges, verify the GitHub merge and matching branch head rather than treating Git ancestry alone as proof of unmerged work.
- Release a task-created worktree only when its work is safely merged and it contains no uncommitted or additional work; use Codex's managed-worktree lifecycle when applicable. Preserve pre-existing or dirty worktrees and explain if they prevent local branch deletion. Never delete the base branch.
- Finish with concise `Changed`, `Tests`, and `Notes` sections, omitting empty sections. Link the issue and PR, report merge and cleanup status, and identify any remaining blocker truthfully.
