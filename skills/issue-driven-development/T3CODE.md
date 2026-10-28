# T3 Code adapter

Use T3 Code's app-owned tools. Tool names may carry an MCP prefix; match by the final tool name.

## Detection and capabilities

Call `orchestrator_capabilities` before offering routing. Retain its `parentThreadId`, provider instance IDs, models, option descriptors, runtime modes, constraints, and inherited settings for this session.

Treat a provider or model as available only when the live response permits the work required by the route and reports no disabling constraint. Use the advertised option IDs in `modelSelection.options` for a launched issue thread and `target.options` for `delegate_task`. Never translate, omit, or silently downgrade requested settings.

If the first catalog scan omits T3 tools, make one bounded direct call to the known `orchestrator_capabilities` tool before reporting T3 unavailable. When the injected tools are absent and `T3_ACP_MCP_NODE` is present, use the supported transport fallback:

```text
ELECTRON_RUN_AS_NODE=1 "$T3_ACP_MCP_NODE" ${T3_ACP_MCP_ENTRYPOINT:+"$T3_ACP_MCP_ENTRYPOINT"} acp-mcp-call orchestrator_capabilities '{}'
```

## Orchestrator setup

To preserve Herdr's direct user-to-delegate review model, each issue uses an ordinary top-level T3 thread in its own worktree. Ask the user to confirm this topology once during session setup. That confirmation is the explicit request required to use `t3_thread_launch`. If the user declines, this adapter cannot preserve the workflow's ownership model; stop and explain that a delegated child would move review and approval into the orchestrator thread.

Use `parentThreadId` as the orchestrator identity. Keep the chosen provider instance, model, and exact options in session context.

Use only runtime and interaction modes allowed by the selected provider and the calling thread. Leave them inherited unless the user requests a different supported mode.

## Start a delegate

Call `t3_thread_launch` with:

- a title containing the issue number and short title;
- `workspaceStrategy.type: "worktree"`;
- the intended base ref and a unique issue branch;
- `startFromOrigin` chosen from repository policy;
- the confirmed model selection and allowed modes;
- a message containing the issue number, absolute repository path, absolute `DELEGATE.md`, `T3CODE.md`, and selected shipping-skill paths, delivery mode, `parentThreadId`, and any scope limits or user clarifications.

Do not ask the delegate to create its worktree in the shell. Uncommitted parent edits are not copied.

Retain the returned `threadId` immediately. `t3_thread_launch` has no idempotency key; after an error or lost response, inspect `t3_thread_list` before retrying so one issue does not get duplicate threads.

Give the user a link in this exact form:

```text
[Issue #<n>: <title>](t3-thread://v1/<threadId>)
```

## Snooze the orchestrator while a delegate works

An idle orchestrator thread looks like it is waiting for the user. After posting the delegate link, call `t3_thread_organize` with `action: "snooze"`, no `threadId`, and `snoozedUntil` set to an ISO timestamp seven days ahead. Then end the turn.

The delegate calls `t3_thread_organize` with `action: "unsnooze"` and `threadId: <parentThreadId>` immediately before it sends a handoff, done, or blocked message to `parentThreadId`. When the orchestrator wakes for any reason, it calls `unsnooze` on itself before doing anything else. If the snooze expires with the delegate still working, check the delegate with `t3_thread_read` and snooze again.

## Delegate helpers

Use `delegate_task` for substantial bounded research. Call `orchestrator_capabilities` in the issue thread, select an enabled model, and pass exact options. Prefer `mode: "async"` for long work. Retain `taskId`; completion wakes the parent thread, so end the turn instead of polling. Use `task_status` only when the result is needed during the current turn and `task_cancel` when abandoning live helper work.

Use file handoffs only when findings are too large for the task summary or repository instructions require a durable note.

T3 child tasks are not fire-and-forget knowledge workers. Put unfinished knowledge-base persistence in the final report. The orchestrator may start separate knowledge work without blocking selection of the next issue.

## PR lifecycle

Immediately after opening or starting work on a PR, call `link_pull_request` in the issue thread with its full URL. For a stack, link every PR.

When waiting for checks or external review, call `watch_pull_request` and end the turn. Do not poll. Keep the watch active through a `full-stop` handoff so a merge from the T3 PR view, GitHub, or elsewhere wakes the delegate. A merge or close ends the watch automatically. Call `unwatch_pull_request` only when abandoning the PR.

Before the final report, call `list_thread_pull_requests` and link any missing PR from this issue. Report any linking failure rather than claiming success.

## Ready handoff, completion, and cancellation

For `full-stop`, the delegate keeps or restarts `watch_pull_request` on the PR, reads its exact worktree and branch with `t3_worktree_status`, unsnoozes `parentThreadId`, and sends this message to it:

```text
Issue #<n> handoff: Outcome: <ready-to-merge|waiting-for-approval|blocked>. PR: <url-or-none>. Head: <sha-or-none>. Worktree: <absolute-path>. Branch: <branch>. New issues: <numbers-or-none>. KB follow-ups: <items-or-none>. Notes: <one-line-or-none>.
```

The orchestrator keeps the issue thread, worktree, and branch. It directs the user to the issue thread and does not archive it or mark the issue complete. When the PR merges, the watch wakes the delegate, which verifies the merge, finishes its cleanup, and reports done. If the delegate does not report done after a merge, the user tells it in the issue thread.

After merge, the delegate deletes the remote branch when allowed, reads its exact worktree path and branch with `t3_worktree_status`, and leaves the T3-bound worktree and local branch for the orchestrator. It unsnoozes `parentThreadId`, then sends it this message with `t3_thread_send`:

```text
Issue #<n> done: PR #<pr> merged. New issues: <numbers-or-none>. KB follow-ups: <items-or-none>. Worktree: <absolute-path>. Branch: <branch>. Notes: <one-line-or-none>.
```

The orchestrator then:

1. Confirms the PR is merged and the issue thread is idle.
2. Archives the issue thread with `t3_thread_organize`.
3. Verifies the reported absolute worktree and branch against `git worktree list`.
4. Removes that worktree, deletes its local branch, and updates the local default branch under repository policy.
5. Confirms `git worktree list` and branch state show cleanup complete.

If the user abandons the issue, interrupt the issue thread, archive it, inspect its worktree for uncommitted changes, and ask before discarding material work. Clean an unchanged worktree and branch after verifying their exact paths.
