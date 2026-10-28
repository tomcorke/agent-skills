# Agent skills

Read `CONTRIBUTING.md` and affected skill entrypoints and linked adapters before planning changes. Edit source under `skills/`; preserve sibling paths and upstream attribution.

Use a dedicated branch and worktree. Keep unrelated local work intact. Update `VERSION` and `CHANGELOG.md` once per change set, following `README.md`.

Validate local Markdown links, YAML discovery fields, and changed backend behavior where available. Report runtime checks that could not be performed. Never commit credentials or machine-specific paths.

Use skill-creator and writing-for-agents when available for skill changes. After merge, sync `main` and remove only the task's branch and worktree.
