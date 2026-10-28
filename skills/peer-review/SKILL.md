---
name: peer-review
description: Use when the user requests "peer review" or "sol loop".
license: MIT
---

# Peer review

Activate only for a clear review request. Quoting or discussing the trigger phrases does not start a review.

Use one independent reviewer to check a scoped change without taking ownership of it. The caller owns the code, findings ledger, fixes, and final decision.

## Select the backend and reviewer

Honor an explicit backend, provider, model, effort, or round budget first. Otherwise detect available backends:

1. Work Terminal is available when current tab has authenticated broker context.
2. Herdr is available when `HERDR_ENV=1`.
3. T3 Code is available when one bounded `orchestrator_capabilities` call succeeds.
4. If multiple are available, ask once. If none are available, report the blocker.

Read the selected adapter completely:

- [Work Terminal](references/WORK-TERMINAL.md)
- [Herdr](references/HERDR.md)
- [T3 Code](references/T3CODE.md)

Prefer a reviewer from a different provider than the implementer: Claude reviews OpenAI work; GPT Sol reviews Claude work. Use medium effort for substantive work and low effort only for small, low-risk changes. These are preferences, not permission to invent or silently substitute an unavailable route. Confirm live support through the selected adapter.

Default budget: one full review and one focused follow-up. The user may change it. Quota exhaustion, timeout, silence, errors, and budget exhaustion are never approval.

## 1. Freeze the scope

1. Read repository instructions and `git status --short`. Preserve unrelated changes.
2. Resolve the review base:
   - PR: read its base and head, confirm the current checkout represents that head, then use the merge-base with the remote base.
   - Local branch: use the merge-base with its remote default branch.
   - Uncommitted-only work without a remote: use the base named by the user, or `HEAD` when that is unambiguous.
3. Define the change as the base-to-working-tree diff in the scoped paths, including untracked files.
4. State intent and exclusions in one or two sentences. Run the smallest covering checks before review and record missing or pre-existing failures.
5. Freeze scoped files while the reviewer reads.

Stop and ask when unrelated work cannot be separated safely.

Done when the base, paths, intent, exclusions, and check results identify one reproducible review snapshot.

## 2. Run the first review

Use the adapter to start one reviewer. Work Terminal creates a reviewer tab; it has no pane/thread link. It must not edit files, commit, push, comment on PRs, merge, or start helpers.

Give it:

- absolute repository path and base SHA;
- intent, exclusions, and check results;
- exact diff command and instruction to inspect untracked files;
- this severity contract:
  - `blocker`: the intended behavior is wrong, unsafe, or destructive;
  - `should`: a reachable defect within scope;
  - `nit`: optional hardening, style, rare low-impact behavior, or work outside intent.

Rare security, authorization, data-loss, and destructive-concurrency defects may still be blockers. Require stable finding IDs and this result shape:

```text
VERDICT: APPROVE or CHANGES
R1 [blocker|should|nit] <file>:<line> <claim>. Fix: <smallest correction>
```

Done when the reviewer returns a verdict and every finding has an ID, severity, location, claim, and correction.

## 3. Resolve findings

Verify each finding yourself:

- Agree: apply the smallest fix and add a focused regression check when the behavior lacks one.
- Disagree: record concrete evidence such as a code path, test result, or requirement.
- Nit or out of scope: decline it in one line and identify a follow-up only when useful.

Maintain a ledger with ID, severity, disposition, evidence, and usage when the backend reports it. Never estimate usage. Rerun affected checks and compare diff size; cut review-driven scope growth before another round.

Done when every finding is fixed, rebutted with evidence, or declined as a nit or out of scope.

## 4. Run at most one follow-up

Use the follow-up only when behavior changed, a blocker or `should` remains disputed, or the first approval no longer covers the current behavior diff.

Give the reviewer the original brief, full ledger, current base diff, and the delta since the prior snapshot. Ask only about open IDs and changed behavior. The adapter defines whether this reuses the reviewer session or creates a new task.

Stop early when only nits remain. Documentation, formatting, and comment-only fixes do not require another round unless they change behavior or requirements.

## 5. Decide

Approved means the current behavior diff has an explicit approval and no open blocker or `should`. Otherwise report `blocked / review budget exhausted`, showing each unresolved claim and response. The user decides whether to change scope or authorize another round.

Do not push unless the invoking workflow or user already authorized it. Report reviewer identity, verified effort when available, rounds used, fixed and declined findings, checks, diff-size change, usage or `unavailable`, and approval state.
