# Changelog

All notable changes to `krait_arch` are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.0-rc1] - Release candidate based on real-world cycle experience

This RC is forged from running v0.2 through one full architecturally-significant cycle on a real project (the Krait SaaS — AI-driven landing page builder). Every change below is traceable to an observation from that run.

### Added
- New command **`/krait_arch:research`** — Phase 1.5 of the cycle. Gathers current information from the web between Discover and Design when claims depend on versions, releases, benchmarks, regulatory status, or pricing. Was an emergent phase in v0.2; now first-class.
- New command **`/krait_arch:map`** — builds or updates a decision map (`docs/architecture/decision-map.md`): groups of open architectural decisions with hard / soft / mutual dependencies and a topologically-sorted suggested order. Also lists explicitly-deferred decisions with their "wait for" conditions. Recommended at project start when ≥3 architectural questions are open at once.
- **`Status` and `Closeout` sections in the review template.** Reviews now open as `Status: Open` and are explicitly closed out by updating the same file with a `## Closeout — YYYY-MM-DD` block listing how each blocker was resolved. This was an emergent practice in v0.2; now codified.
- **Detail scaling** for `/krait_arch:cycle` — new `--scale=light|standard|deep` flag (with autodetect heuristics). Light cycles produce ≤30-line discovery docs and 2 alternatives; deep cycles run mandatory research and produce ≥4-alternatives ADRs. Solves the "every cycle is full-scale" problem of v0.2.
- **Mandatory second round of discover** when the user answers open questions. Round 2 looks for push-back opportunities, surfaces constraints that the answers reveal, and marks each question as resolved-or-deferred. Appended to the same discovery document as `Section 7`.
- **Mandatory `What we explicitly do NOT consider — and why` section** in the design phase. Captures dead-end alternatives with one-line dismissals so they don't get re-litigated later.
- **Russian-localized integration block** in `/krait_arch:remember-compound-integration`. New `--lang=en|ru|auto` flag with autodetect from `STRATEGY.md`, `ARCHITECTURE.md`, and the most recent ADR. Idempotent across language switches.

### Changed
- **Architect router skill — extended terminology table** with calques observed in real artifacts: `архитектурный шов → развилка`, `routing-policy → правила маршрутизации`, `pipeline / sanitizer` (allowed only as proper-noun component names), `fail-closed/open`, `breaking change`, `graceful degradation`, `backpressure`, `feature flag`, `hype`, and others.
- **Mandatory terminology pass** for Russian-language artifacts. After generating any Russian document, the architect skill now runs an explicit pass against the calque table and reports non-trivial corrections to the user in one line.
- Pre-response checklist gains a language-pass item.
- Plugin and marketplace versions: `0.3.0-rc1`.
- Plugin README and `README.ru.md` updated with the new commands and flags.

### Fixed (carried over from 0.2.1 hot-fix)
- `hooks/hooks.json` wraps events under top-level `hooks` key as required by Claude Code's hook validator schema.
- `plugin.json` no longer references `hooks` field — Claude Code auto-discovers `hooks/hooks.json` from the plugin root.
- Manifest URLs now point to `IgorKramar/krait-arch-marketplace`.

### Notes for users running v0.2 in production
- The new `Section 7` in discovery and `What we do NOT consider` in design are additive — existing v0.2 documents won't be regenerated, but new cycles will include them.
- `decision-map.md` is recommended but not required. Existing projects without one continue to work; runs of `/krait_arch:cycle` simply skip the map check.
- Re-run `/krait_arch:remember-compound-integration` to migrate the integration block to the new (research-aware) workflow diagram.

## [0.2.1] - Hook schema fix

### Fixed
- `hooks/hooks.json` now wraps events under the top-level `hooks` key as required by Claude Code's hook validator schema.
- Removed `hooks` field from `plugin.json` — Claude Code auto-discovers `hooks/hooks.json` from the plugin root.

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
