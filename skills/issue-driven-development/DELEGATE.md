# Delegate contract

You own one GitHub issue from investigation through its selected delivery outcome. The orchestrator supplied the issue number, repository, backend adapter, delivery mode, and any scope limits or user clarifications. Work with the user in your own pane or thread.

Before planning:

1. Read the repository's `AGENTS.md` and `CLAUDE.md` when present, plus global instructions.
2. Read the selected backend adapter completely.
3. Read the selected shipping skill completely: [full-stop](../full-stop/SKILL.md) or [full-send](../full-send/SKILL.md). Follow its linked peer-review workflow and backend shipping adapter.
4. Read the issue title, body, and every comment:

   ```text
   gh issue view <n> --repo <owner/repo> --json title,body,comments
   ```

Apply supplied limits and clarifications without discarding requirements recorded on the issue.

## 1. Research

Research only what the issue needs. Handle small lookups directly. Use the selected adapter's helper procedure for a substantial, bounded research question. Helper work is optional.

Put long findings where repository or global instructions require; otherwise use a temporary location outside the repository. Wait for research that blocks implementation and read its result. Close or cancel completed helpers as the adapter requires.

When repository instructions name a knowledge base, preserve findings worth keeping. Follow the adapter's knowledge handoff rule. Knowledge persistence must not block the PR, merge, or selection of the next issue.

Done when the facts required by this issue are checked.

## 2. Ship

Follow the selected shipping skill from implementation through its final gate. Apply these issue-specific requirements within that workflow:

1. Use the branch and worktree supplied by the backend. Create them only when the adapter explicitly assigns that responsibility to you.
2. Treat the issue body, comments, supplied limits, and user clarifications as the intent reviewed by peer-review.
3. For UI changes, capture before screenshots before editing. Review the result in supported themes and relevant viewport sizes. Capture matching after screenshots with the same app state, theme, and viewport.
4. Use the `pr` skill when available and follow the repository's PR template. Include:
   - the problem and changes;
   - checks performed;
   - labelled before and after evidence;
   - each deferred item with a linked issue.
5. Open the PR and immediately complete the backend's PR registration step.
6. Create a labelled issue for every deferred follow-up and link it from the PR or related issue.

For `full-stop`, send the adapter's handoff report after it reaches a handoff outcome. Keep the pane or thread, branch, and worktree alive. Wait for the user there. If they later ask you to merge, treat that as a new full-send request for the current PR. If they merge it themselves, verify the merge before cleanup.

For `full-send`, send the adapter's done report only after the PR is merged, remote branch handling is complete, and backend-assigned cleanup is complete. Never remove a worktree that still hosts your active pane or thread.

Done when the selected shipping skill reaches its ready or merged outcome and the matching backend report is sent.
