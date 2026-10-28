# T3 Code review adapter

Use T3 Code's app-owned tools. Tool names may carry an MCP prefix; match by their final name.

## Discover and select

Call `orchestrator_capabilities` before choosing the reviewer. If tools are missing from an initial catalog, make one bounded direct call before reporting T3 Code unavailable. Use the documented ACP MCP transport fallback only when injected tools are absent and `T3_ACP_MCP_NODE` is present.

Retain the inherited provider, model, options, runtime mode, enabled providers, model IDs, option descriptors, and constraints. Prefer an enabled provider different from the implementer. Never treat a native subagent model list as the complete catalog.

Pass requested options using their advertised IDs. Options inherit only for the same provider instance and model. Use `delegate_task` even for a same-provider model when options are requested or inherited options are nonempty. Reject unsupported values instead of translating them.

## Run rounds

Use `delegate_task` for every round with:

- `role: "review"`;
- `mode: "async"` for substantive work;
- shared checkout isolation so the frozen working-tree diff is visible;
- the selected provider instance, model, and exact options;
- a distinct `clientRequestId` per round, stable across retries of that round.

Record `taskId`, `childThreadId`, returned provider, model, and options. Verify the returned selection matches the requested route. The child thread is storage, not the continuation target.

End the turn while an async review runs; completion wakes the parent. Use `task_status` only when the result is needed in the current turn. A wait timeout does not cancel work. Cancel a live task with `task_cancel` when abandoning the review budget.

Create a fresh `delegate_task` for the follow-up. Its prompt must repeat the original brief and include all prior findings, responses, evidence, unresolved objections, current diff, and delta. Never continue a delegated review through `t3_thread_send`.

Treat pending questions and approvals as pending, not findings or approval. The parent owns responses under its existing authority. A delegated reviewer cannot watch a PR; PR monitoring remains with the parent shipping workflow.

Report provider, model, verified options, round count, task IDs, and usage when returned. Otherwise report usage as unavailable.
