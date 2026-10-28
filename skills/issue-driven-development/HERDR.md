# Herdr adapter

Use this adapter only inside a Herdr-managed pane. Read the `herdr` skill completely before the first control command, then verify `HERDR_ENV=1`. The installed `herdr --help` and relevant command-group help are the authority for current syntax.

## Orchestrator setup

Map each confirmed routing preference to a Herdr agent kind and native arguments. Confirm the installed kind and options before launch. Never silently substitute a model or effort.

Name the current agent so delegates can report back:

```text
herdr agent rename "$HERDR_PANE_ID" orchestrator
```

If the name is taken, choose another valid name and use it in every brief.

## Start a delegate

Inspect the current pane layout. Split right when the pane is wide; otherwise split down. Preserve the repository working directory and user focus:

```text
herdr pane split --current --direction <right-or-down> --cwd "$PWD" --no-focus
herdr agent start issue-<n> --kind <kind> --pane <pane-id> -- <native-agent-arguments>
herdr agent prompt issue-<n> "You own issue #<n> in <absolute-repository-path> using <full-stop-or-full-send>. Read <absolute-skill-directory>/DELEGATE.md, then <absolute-skill-directory>/HERDR.md, then <absolute-skill-directory>/../<full-stop-or-full-send>/SKILL.md, and follow them. Report handoff or done to the Herdr agent named <orchestrator-name>.<optional-scope-and-clarifications>"
```

Submit without waiting. Retain the returned pane ID and delegate name. Tell the user which pane contains the delegate, then end the orchestrator turn.

The delegate creates its issue branch and worktree under repository and global rules, then changes its pane into that worktree before editing. Keep them for a full-stop handoff. Before final cleanup after merge, change back to the repository's main checkout, then remove only the issue worktree and local branch.

## Delegate helpers

Coordinate through explicit `herdr agent prompt` messages. Use files for long handoffs and send their absolute paths with a short summary. Use lifecycle commands only for status, not to recover another agent's answer.

For bounded research:

1. Split a sibling pane without focus and preserve the current working directory.
2. Start a uniquely named research agent with the confirmed live routing.
3. Prompt it with the question, file handoff rule, recipient agent name, and completion format.
4. Wait for its explicit message when implementation depends on it.
5. Close its pane after completion.

For fire-and-forget knowledge work, tell the helper to report its commit or blocker directly to the orchestrator. Include the helper name in the delegate's final report. The orchestrator closes its pane after explicit completion.

## PR lifecycle

Herdr has no app-level PR registry. Opening the PR and retaining its URL completes the registration step. Use the repository's check command or `gh pr checks --watch` when waiting for checks; do not spend agent turns polling.

## Ready handoff, completion, and cancellation

For `full-stop`, the delegate sends:

```text
herdr agent prompt <orchestrator-name> "Issue #<n> handoff: Outcome: <ready-to-merge|waiting-for-approval|blocked>. PR: <url-or-none>. Head: <sha-or-none>. Worktree: <path>. Branch: <branch>. New issues: <numbers-or-none>. KB agents: <names-or-none>. Notes: <one-line-or-none>."
```

The orchestrator keeps the delegate pane open and directs the user there. It does not close the pane, delete the branch or worktree, or mark the issue complete.

After merge, the delegate sends one final message:

```text
herdr agent prompt <orchestrator-name> "Issue #<n> done: PR #<pr> merged. New issues: <numbers-or-none>. KB agents: <names-or-none>. Notes: <one-line-or-none>."
```

After the done report, the orchestrator resolves the delegate pane with `herdr agent get issue-<n>` when needed, closes only the pane it created, and tracks any knowledge helpers until their explicit completion. The delegate's final report is valid only after its issue worktree and local branch are gone.

If the user abandons the issue, stop the delegate safely, close only its created pane, and return to issue selection.
