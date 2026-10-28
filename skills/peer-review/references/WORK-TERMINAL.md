# Work Terminal review adapter

Use when Work Terminal is selected. Call the Skill tool with `work-terminal`, confirm authenticated broker context, then verify reviewer profile and `create-tab` capability.

Create one reviewer tab with `createTab`. Use a profile configured for the requested provider/model. Initial prompt must require read-only review: no edits, commits, pushes, PR comments, merges, or helpers. Include absolute repo path, base SHA, intent, exclusions, checks, exact diff command, and finding format.

Record exact reviewer target. Send one focused follow-up with `promptTab` only when allowed by the peer-review budget. Use `waitForTab` plus `readOutput` to obtain the actual verdict; idle is not approval. If output is too large, ask reviewer to write a temporary Markdown result and return its path.

Close reviewer tab after settled review or budget exhaustion. Report profile identity and usage as unavailable unless reviewer output provides it. Never treat timeout, silence, or tab closure as approval.
