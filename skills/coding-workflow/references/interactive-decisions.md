# Native interactive decisions

Use Codex's native structured question/selection UI for clarification, workflow choices, and approvals whenever an available tool supports the intended purpose in the current client mode. Prefer concise action-specific buttons over asking the user to type “yes.” Use the user's language and retain free-text input when the UI provides it.

## Choose the available control

- Inspect the actual tool descriptions, schemas, and mode restrictions. In clients that expose `request_user_input_async` for approvals and preferences, use it for execution-phase questions. Use `request_user_input` only where it is available and permitted for that purpose; a planning-only or preferences-only tool must not be used for execution permission.
- With usable Spec Kit, planning runs through its installed procedure in a writable mode. Ask whether to generate tasks after presenting the plan; that submitted choice accepts the plan for task generation. Require native Codex Plan mode only on the path without usable Spec Kit. For native plan acceptance, prefer the client's plan approval control when offered. A skill or submitted answer cannot toggle modes; wait for the actual transition before mutations.
- If no appropriate native control is available, briefly explain that limitation and ask the same focused question in chat. Do not emulate approval with Markdown buttons, a custom web page, terminal prompts, or an invented tool. An unavailable UI does not grant approval.

## Present a concrete decision

- Ask only for missing information or a decision required by the current workflow. Do not add extra approval gates or “test” questions just to show the UI. Keep existing valid approvals instead of asking again because a native form is now available.
- Name the relevant issue, PR/MR, or proposed standards change and include its link or reviewable context in the question. Use two or three mutually exclusive choices when they cover the decision. Offer a real defer/decline choice for an approval.
- Ask about task generation after presenting the plan: “Generate tasks,” “Revise plan,” or “Stop here.” In a prepared project, identify `speckit-tasks` and the target plan. Approval authorizes the task-generation procedure, not issue creation or product implementation.
- After presenting tasks, ask “Create issue and commit planning files,” “Revise tasks,” or “Stop here.” Identify the proposed scope and explain that approval covers recording the issue, establishing its branch in the selected checkout, and making a separate planning-only commit. Reuse an already associated issue and any valid approvals; never create a duplicate.
- Resolve a genuinely missing checkout choice before planning-file writes. If a suitable active task worktree was detected, reuse it without another location question. After issue creation and the planning commit, ask only “Start implementation” or “Keep committed plan only,” identifying the issue, commit, and worktree. A Codex-provisioned worktree settles the location choice, not implementation approval.
- For a feature PR/MR, offer “Merge after successful CI” and “Leave open.” The question identifies the specific feature PR/MR; all existing CI, review, scope, and release-PR restrictions remain in force. Do not offer skill-managed merging for Version Packages/Changesets release PRs.
- For conflicting coding standards, offer concrete actions such as “Keep existing rule” and “Adopt proposed rule,” stating the actual difference and affected scope. Discovery and audit can continue read-only; the user's choice does not authorize unrelated migrations.
- Keep task-generation approval, issue/planning-commit approval, implementation approval, and merge approval separate. Native plan approval on the non-Spec-Kit path does not replace any later gate. One answer must not silently approve subsequent phases.

## Await the submitted answer

- Only an explicitly submitted option or a clear user-authored answer counts. A tool acknowledgment that the question was displayed, a preselected option, elapsed time, cancellation, an empty result, or a timeout is not approval.
- With asynchronous input, keep the question pending and continue only work independent of its answer. Stop before every action that depends on the missing choice or approval. When needed, end the turn with the question still pending rather than assuming an answer.
- Match a delayed answer to its original question and task artifact. Reuse it if still valid; if the proposed action or scope materially changed, explain the change and obtain a new decision.
- Ordinary user free text remains valid. The UI removes the need to type “yes”; it does not reject a clear written approval or override the host's rules.
