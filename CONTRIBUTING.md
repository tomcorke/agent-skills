# Contributing

Read `AGENTS.md` and the affected skill and adapters before editing. Make changes on a dedicated branch and worktree. Keep skill entrypoints focused and move backend-specific detail into adapters.

Preserve explicit authorization, bounded review budgets, final-head approval, repository merge gates, and full-stop's open-PR contract. Do not substitute providers silently. Keep all relative references valid and preserve upstream credits and licenses.

Before committing:

- Compare changed skills with the intended workflow and check both backend paths.
- Check Markdown links and required YAML name and description fields.
- Check for credentials, personal paths, unrelated files, and installed caches.
- Exercise changed commands in their supported backend when available; document any untested behavior.
- Bump `VERSION` once and add a dated `CHANGELOG.md` entry describing the user-visible change.

Open a focused PR with scope, validation, and limitations. After merge, sync `main` and remove the task branch and worktree. Versioning rules are in `README.md`.
