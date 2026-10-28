# Agent skills

Skills for shipping GitHub issues with isolated agents, bounded review, and explicit delivery gates. Supports Herdr and T3 Code.

## Included skills

| Skill | Purpose |
| --- | --- |
| `issue-driven-development` | Choose an issue and coordinate one isolated delegate at a time. |
| `full-stop` | Prepare and review a PR, then leave it open for the user. |
| `full-send` | Carry an authorized change through review, merge, and cleanup. |
| `peer-review` | Run an independent review with a bounded follow-up. |
| `pr` | Write PR descriptions with evidence and merge risk. |

Each skill includes its supporting references and backend adapters. Install the complete set to preserve cross-skill links.

## Requirements

- Git and GitHub CLI (`gh`), authenticated for the target repository. Use SSH remotes for Git operations.
- An agent host that loads `SKILL.md` files and supports the chosen backend.
- **Herdr:** install Herdr and its own `herdr` skill. That skill is supplied by Herdr and is deliberately not bundled here. Run within Herdr with `HERDR_ENV=1`.
- **T3 Code:** run in T3 Code with orchestration, delegation, and PR tools available.
- Access to the selected model providers. Model preferences in the skills are checked against live capabilities; unavailable routes require a user decision.

These skills describe workflows; they do not install backends or grant access to providers. Repository instructions and required approvals still apply.

## Installation

Use the skills installer and select all five skills:

```bash
npx skills@latest add tomcorke/agent-skills
```

Choose your agent and installation scope when prompted. Restart the agent session after installation. Update installed copies with:

```bash
npx skills update
```

For manual installation, clone over SSH and copy all five directories from `skills/` into your host's skills directory. Keep them as siblings so relative links resolve:

```bash
git clone git@github.com:tomcorke/agent-skills.git
```

For example, Codex users can copy the five directories into `~/.codex/skills/`; Claude Code users can use `~/.claude/skills/`. Preserve existing installations rather than overwriting local edits blindly. To uninstall, remove only the five directories installed from this repository.

## Usage

Ask your agent to use `issue-driven-development` for a target GitHub repository. This workflow requires explicit invocation. It confirms backend, prioritisation, model routing, and delivery mode before choosing an issue.

Choose `full-stop` to leave a reviewed PR open, or explicitly request `full-send` to authorize merge once all gates pass. You can also request `peer review` for a scoped change, or use `pr` when writing a PR body.

## Development and versions

See [CONTRIBUTING.md](CONTRIBUTING.md) for authoring and validation, [ARCHITECTURE.md](ARCHITECTURE.md) for layout, and [CHANGELOG.md](CHANGELOG.md) for history.

`VERSION` records the repository version, starting at `0.1.0`. Bump it once per merged change set: patch for fixes and prose, minor for skills or capabilities added or removed, major for breaking layout or workflow contracts. It is reference metadata, not a release channel; tags and GitHub releases are optional. Installations from the default branch follow the current source.

## License and credits

MIT. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md), including preserved upstream attribution within individual skills.
