# Herdr review adapter

Use this adapter only when `HERDR_ENV=1`. Read the installed `herdr` skill completely before control commands, then inspect the current `herdr agent` and `herdr pane` help. The installed CLI defines syntax and supported agent kinds.

## Select and start the reviewer

Map the requested reviewer to an installed Herdr agent kind and its native model and effort arguments. Confirm the executable supports those arguments before launch. Prefer the opposite provider from the implementer and medium effort unless the request or risk calls for another supported choice. Never silently substitute a provider, model, or option.

Inspect the caller pane layout. Split right when wide and down when narrow, preserving focus and the current worktree:

```text
herdr pane split --current --direction <right-or-down> --cwd "$PWD" --no-focus
herdr agent start review-<unique-id> --kind <kind> --pane <pane-id> -- <verified-native-arguments>
```

Retain the pane ID and reviewer name. Use native sandbox or read-only controls when the selected agent exposes them. Prompt restrictions alone are not a security boundary.

## Run rounds

Submit the complete review brief with:

```text
herdr agent prompt review-<unique-id> "<brief>" --wait --timeout 120000
```

The timeout limits the caller's wait, not reviewer runtime or spend. On `blocked`, `timeout`, `agent_prompt_stalled`, or `unknown`, inspect `herdr agent get` and `herdr agent read --source recent-unwrapped` before deciding what to do. Never resend blindly.

After a settled state, read the actual verdict from the reviewer output. `idle` and `done` mean the interaction settled; they are not approval. If the answer is not recoverable from a larger recent read, ask the reviewer to write it to a temporary Markdown file and return only the path.

Reuse the same reviewer for the focused follow-up. Send the original brief, ledger, current diff, and delta. Do not start a second reviewer.

When review ends or its budget expires, stop any still-running turn safely and close only the pane created by this workflow. Report the verified agent kind, model, effort, round count, and usage when the reviewer exposes it.
