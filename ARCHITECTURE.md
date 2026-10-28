# Architecture

`skills/` contains five sibling skill directories. Each has a `SKILL.md` entrypoint with YAML discovery metadata. Backend details live in linked adapters; issue delegates read `DELEGATE.md` before shipping.

`issue-driven-development` coordinates issue selection and delegation. `full-stop` shares the shipping gates in `full-send` but retains the PR for the user. Both use `peer-review`; `pr` supplies the PR description format.

Herdr and T3 Code adapters belong with their workflow. Herdr's own skill is an external dependency supplied by Herdr. Keep backend commands in their adapters and preserve relative cross-skill paths. Host tools, provider availability, and repository policy are runtime dependencies.

The repository packages skill source, not installed copies, provider credentials, or backend implementations.
