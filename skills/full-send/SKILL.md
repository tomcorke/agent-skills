---
name: full-send
description: Use when the user requests "full send".
license: MIT
---

# Full send

Own the current change through a merged pull request and local cleanup. Activate only for a clear workflow request; quoting or discussing "full send" is not merge authority. A request that triggers this skill authorizes merging this task's PR after every gate passes. It does not override repository instructions, harness approvals, production safeguards, required human approval, or authority limits outside this repository.

## Backend

Honor an explicit backend first. Otherwise detect Herdr through `HERDR_ENV=1` and T3 Code through one bounded capability call. Ask once when both are available. Read the selected adapter completely:

- [Herdr](references/HERDR.md)
- [T3 Code](references/T3CODE.md)

If neither backend is available, continue direct implementation only when the remaining workflow can still satisfy every gate; otherwise report the missing capability.

## Work budget

Read [peer-review](../peer-review/SKILL.md) and its selected adapter, then follow it with the default maximum of two review calls. Allow one CI-repair cycle: one grouped response to failures or actionable automated findings after the first PR check completes. A later behavior change still needs review of the final head; stop when the remaining review budget cannot cover it.

Wait for external checks for at most 30 minutes per invocation. A watcher wait does not consume approval or imply success. Do not increase effort, buy credits, enable paid fallback, switch billing routes, or add agents without user authority.

## Workflow

1. **Understand and isolate.** Read repository instructions. Inspect status and existing PR state. Use the repository's branch and worktree rules; preserve unrelated work.
2. **Finish the change.** Implement the smallest complete solution. Do not expand the request to make autonomy easier.
3. **Prove it.** Observe direct evidence for each changed behavior. Reproduce a fixed failure when practical, run required checks, and inspect UI or runtime behavior when tests alone cannot prove it. Ask before any proof method that existing instructions or permissions reserve for the user.
4. **Review it.** Follow the linked peer-review workflow on the complete local diff. Continue only with approval of the current behavior diff.
5. **Open or update the PR.** Rebase or merge the latest target branch according to repository policy. Use the `pr` skill when available. Include intent, change summary, proof, known limits, and linked follow-ups. Push only this task branch.
6. **Register and wait.** Follow the adapter immediately after opening or adopting the PR. Inspect existing comments, reviews, checks, and merge state before starting its watcher.
7. **Repair once.** Fix valid CI failures and actionable findings as one cycle. Re-run direct proof. Repeat peer review when behavior changed or prior approval no longer covers the head. Push the reviewed head, then wait again within the time budget.
8. **Apply the merge gate.** Check every condition below against the live head.
9. **Merge and clean up.** Use the repository's normal merge method without bypasses. Confirm the merged commit contains the reviewed head. Update local default branch, remove only this task's worktree and local branch, and verify cleanup.

## Merge gate

Every condition must be true on the latest head:

- direct proof covers every behavior change;
- required checks pass;
- independent review explicitly approves the current behavior diff;
- every actionable automated finding is fixed or answered with evidence;
- no human review requests changes, and repository-required approval exists;
- the PR is open, current with its target under repository policy, and mergeable;
- branch protection and the configured merge method remain intact.

Never use administrator bypasses. Never treat absent required checks as proof. Never infer approval from timeout, quota exhaustion, reviewer silence, or a previous head.

## Stop states

If the gate cannot pass within the budget, leave the PR open and report `blocked / budget exhausted`. State the exact remaining gate, current head, worktree and branch, what was tried, and the next action. Use the harness question mechanism only when the user must choose or grant new authority.

On success, report the PR, merge commit, proof, reviewer outcome, decisions made under delegated authority, and cleanup verification.
