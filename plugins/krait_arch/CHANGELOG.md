# Changelog

All notable changes to `krait_arch` are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - Compound Engineering integration + bilingual docs + Russian language discipline

### Added
- New skill **`compound-integration`** — defines how `krait_arch` interleaves with the EveryInc `compound-engineering` plugin: phase-by-phase workflow, artifact ownership split, hand-off rules, anti-patterns to avoid double work.
- New command **`/krait_arch:remember-compound-integration`** — materializes the integration as an idempotent rule block in the project's `AGENTS.md` (or `CLAUDE.md`), wrapped in HTML-comment markers so re-runs replace in place.
- Russian-language editions of both READMEs (`README.ru.md` at top level and inside the plugin) with cross-language switchers in headers.
- New section **Language and terminology** in the `architect` router skill: explicit Russian technical terminology guide with a calque-avoidance table (e.g., «развёртывание» instead of «деплоймент», «наблюдаемость» instead of «обзервабилити»). Active when the user writes in Russian; English unaffected.

### Changed
- Plugin and marketplace versions bumped to 0.2.0.
- Plugin README expanded with a Compound Engineering integration section and updated component lists (new skill, new command).
- Top-level marketplace README expanded with a Compound Engineering integration section.

### Notes
- The integration assumes the official EveryInc `compound-engineering` plugin (commands `/ce-ideate`, `/ce-brainstorm`, `/ce-plan`, `/ce-work`, `/ce-code-review`, `/ce-compound`). If CE renames a command in a future release, re-run `/krait_arch:remember-compound-integration` to refresh the rule block in `AGENTS.md`.

## [0.1.0] - Initial release

### Added
- Router skill `architect` and 8 specialist skills (c4-diagrams, adr-writing, system-design, frontend-architecture, backend-architecture, ai-agents-architecture, code-review-architectural, architecture-research).
- Slash commands: `/krait_arch:init`, `:discover`, `:design`, `:decide`, `:document`, `:review`, `:cycle`, `:adr`, `:c4`.
- Sub-agents: `architect`, `reviewer`, `researcher`.
- Soft-warning hooks (`PostToolUse`) for large changes, new top-level modules, new dependencies.
- Templates: `ARCHITECTURE.md`, ADR boilerplate, `docs/architecture/README.md`.
- Architecture Cycle (Discover → Design → Decide → Document → Review) as the organizing process.
