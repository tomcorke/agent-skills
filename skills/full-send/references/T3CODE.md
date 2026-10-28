# T3 Code shipping adapter

Use T3 Code's app-owned tools. Tool names may carry an MCP prefix; match by their final name.

## Review

Follow the linked peer-review workflow with its T3 Code adapter. Review rounds are delegated child tasks; the parent thread retains implementation, PR ownership, monitoring, merge, and cleanup.

## Pull request lifecycle

Immediately after opening a PR or starting work on an existing one, call `link_pull_request` with its full URL. Link every layer of a stack. Before completion, call `list_thread_pull_requests` and link any missing PR from this work. Report linking failure instead of claiming registration.

Read the live PR head, base, checks, comments, reviews, mergeability, draft state, auto-merge, and queue state. Handle existing events, then call `watch_pull_request` and end the turn. Do not poll or run a second watcher. A wake is news, not approval; reread every merge gate.

The 30-minute work budget limits active repair and review time. T3's event watcher may remain dormant without model turns; after a later wake, report pending work instead of starting another repair cycle when the active budget is exhausted.

## Handoff and cleanup

For `full-stop`, call `unwatch_pull_request` before handing the open PR to the user. Report the exact worktree and branch from `t3_worktree_status`; leave both intact.

For `full-send`, merge only after the live gate passes. A merge or close ends the watcher. Confirm the merged commit includes the reviewed head. If this thread is bound to a worktree, another thread must perform removal after this thread is idle or archived; report the exact path and branch from `t3_worktree_status`. When this thread owns a removable external worktree, verify it against `t3_worktree_list` before removing it. Update the main checkout and verify only this task's branch and worktree are gone.
