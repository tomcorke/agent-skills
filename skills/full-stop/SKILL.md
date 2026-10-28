---
name: full-stop
description: Use when the user requests "full stop".
license: MIT
---

# Full stop

Prepare the current change for merge, then hand it to the user. Never merge it.

Activate only for a clear workflow request. Quoting or discussing "full stop" does not start work.

Read [full-send](../full-send/SKILL.md) and its selected backend adapter completely. Follow its scope, proof, review, PR, repair, budget, and merge-gate rules. A request that triggers `full-stop` authorizes those steps without granting merge authority.

## Keep the PR unmerged

Never merge, enable auto-merge, enter a merge queue, push the default branch, bypass branch protection, or delegate merging. Before work and before each push, inspect this task's PR for auto-merge or queue enrollment. Disable or leave the queue only when needed to prevent this PR from merging, and report that change. Do not change repository-wide automation.

If automatic merging cannot be prevented safely, stop and report the blocker before changing the branch.

## Handoff gate

Apply full-send's merge gate to the live final head, but replace merge and cleanup with one outcome:

- `ready to merge`: every gate passes;
- `waiting for required approval`: all other gates pass and human approval is the only remaining condition;
- `blocked / budget exhausted`: name each remaining gate and the next action.

Confirm the PR remains open and unmerged. Follow the backend's handoff procedure. Keep the task worktree and branch because the PR still depends on them; this is a deliberate handoff state, not merged project completion.

Report the PR link, final head, outcome, proof, reviewer result, checks, remaining risks, worktree and branch, and any auto-merge or queue state changed.
