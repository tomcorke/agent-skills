# Herdr shipping adapter

Use this adapter only when `HERDR_ENV=1`. Read the installed `herdr` skill before controlling panes or agents, then check the installed CLI help. Herdr provides process coordination; it does not add quota, approval, or merge authority.

## Review

Follow the linked peer-review workflow with its Herdr adapter. Keep its reviewer in the same task worktree, preserve user focus, and close only panes created by the workflow.

## Pull request lifecycle

Herdr has no app-level pull request registry. Retain the full PR URL and use GitHub's live state as the source of truth.

Read existing checks, reviews, comments, requested changes, head SHA, base, draft state, auto-merge state, mergeability, and merge queue state before waiting. Use a process wait instead of repeated agent prompts:

```text
gh pr checks <pr> --watch --interval 60 --fail-fast
```

Bound an active wait to the skill's remaining time budget. A timeout means pending work, not failure or approval. After each wake or completed wait, reread all live PR gates; `gh pr checks` alone does not cover human review, comments, mergeability, or every queue state.

## Handoff and cleanup

For `full-stop`, stop local watchers and report the open PR, head, branch, and worktree. Leave both branch and worktree intact.

For `full-send`, merge only after the live gate passes. Then confirm the merged commit includes the reviewed head, return to the repository's main checkout, update its default branch, remove only this task's worktree and local branch, and verify both are gone.
