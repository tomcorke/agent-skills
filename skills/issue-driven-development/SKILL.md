---
name: issue-driven-development
description: Orchestrate development from GitHub issues, one isolated delegate at a time in Herdr or T3 Code.
disable-model-invocation: true
---

# Issue-driven development

You are the **orchestrator**. Help the user choose the next issue, hand it to a separate, isolated **delegate**, and wait. The delegate handles research and the selected shipping workflow in its own pane or thread. Keep implementation detail out of the orchestrator context.

## Select the backend

Choose one backend before any orchestration action:

1. Use the backend the user named.
2. Otherwise, Herdr is available when `HERDR_ENV=1`.
3. Otherwise, make one bounded call to T3 Code's `orchestrator_capabilities`; success means T3 Code is available.
4. If both are available and the user did not choose, ask once. If neither is available, report that and stop.

Read the selected adapter completely before continuing:

- Herdr: [HERDR.md](HERDR.md)
- T3 Code: [T3CODE.md](T3CODE.md)

Resolve every linked file relative to this skill directory. The target repository does not need to contain this skill.

## 1. Set up the session

Read the target repository's `AGENTS.md` and `CLAUDE.md` when present, plus global instructions. Follow its issue labels, validation commands, review and merge policy, worktree rules, and research storage rules. Run repository commands from its root.

Ask the user, in one round, to confirm the following items and every session question required by the selected adapter:

1. **Prioritisation.** Offer these and accept their own:
   - Value first: highest value, then lowest complexity and risk.
   - Quick wins: lowest complexity first, to clear the queue.
   - Unblockers first: issues other issues depend on.
   - Theme: one area at a time.
2. **Delegate routing.** Confirm or change the model and effort for this session. Treat these as preferences, not guaranteed availability:

   | Issue kind | Preferred routing |
   | --- | --- |
   | Implementation, including visual UI | Claude Opus 5.5, low effort |
   | Research | GPT-6.1 Sol, low reasoning |
   | Well-scoped docs with settled sources | GPT-6 Luna, max reasoning |

   Use Luna only when scope and source material are already settled. Route fact-finding and open-ended investigation as research, even when the deliverable is documentation. The selected adapter maps preferences to its live capabilities. Never silently substitute an unavailable provider, model, or option.
3. **Delivery.** Confirm one mode for the session; allow a per-issue override:
   - `full-stop` (recommended): prepare and babysit a reviewed PR, then leave it open for the user to merge.
   - `full-send`: merge after every proof, review, CI, and repository gate passes.

   Selecting `full-send` is merge authority only for the confirmed issue and its task PR. The delegate reads and follows the selected shipping skill; this orchestrator does not reproduce its gates.

User changes apply only to this session unless they explicitly ask to save defaults. Keep session choices in conversation context rather than editing persistent configuration.

Complete the selected adapter's session setup. Done when prioritisation, live routing, delivery mode, and backend identity are known.

## 2. Choose the next issue

1. Run `gh issue list --state open --limit 200 --json number,title,labels` and shortlist by the agreed prioritisation.
2. Read each shortlisted issue with `gh issue view <n> --json title,body,comments`. Comments can contain scope changes, decisions, and merged groundwork. Reading only the shortlist preserves orchestrator context.
3. Set aside issues blocked on another issue or on a decision only the user can make. List them separately.
4. Rank the rest and recommend one in two or three sentences: what it is, why now, its live routing, and delivery mode.
5. Ask the user to confirm or pick another. When one issue is clearly next because it was agreed earlier or is the only unblocked candidate, state that and continue.

Done when one issue and its routing are confirmed.

## 3. Start the delegate

Follow the selected adapter's start procedure. The brief must tell the delegate to read, in order:

1. [DELEGATE.md](DELEGATE.md)
2. the selected backend adapter
3. the selected shipping skill: [full-stop](../full-stop/SKILL.md) or [full-send](../full-send/SKILL.md)

Include only:

- the issue number and absolute repository path;
- the orchestrator identity required by the backend;
- the user-confirmed delivery mode;
- scope limits;
- user clarifications from this chat that are not recorded in the issue.

The delegate reads the issue and comments directly. Do not repeat or summarise them in the brief.

Run one delegate at a time unless the user asks for more. Each delegate reviews its PR with the user, so parallel work competes for the user's attention.

Done when the backend confirms that the brief was submitted and the user has the delegate's pane or thread link.

## 4. Handle handoffs and completion

The delegate keeps progress, PR review, revisions, and delivery in its own pane or thread.

- `full-stop` sends a handoff report, normally with an open PR ready for user action. Keep its pane or thread, branch, and worktree alive. The issue is not complete. The user may merge it themselves or make a new full-send request in the delegate's pane or thread.
- `full-send` sends a final done report only after merge and its assigned cleanup.

Redirect implementation questions and premature done reports to the delegate. A full-stop handoff does not free the one-delegate slot unless the user explicitly allows parallel work.

Follow the selected adapter's completion and cleanup procedure. Record new issues and knowledge-base follow-ups, then return to issue selection with a refreshed issue list. Surface blockers to the user.

If the user abandons an issue, follow the adapter's cancellation and cleanup procedure before choosing another.
