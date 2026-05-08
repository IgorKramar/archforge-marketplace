# Roadmap

This document describes the planned evolution of `archforge` through versions 0.5, 0.6, and 0.7. It is **not a commitment to ship by a specific date** — it's a commitment to a direction, with each version having a coherent theme rather than being a grab-bag of features.

## Principles

A few guardrails that bind every version on this roadmap:

1. **Every feature is forged on real use.** Items here exist because something in actual practice (mine or a user's) made them necessary. Speculation gets parked, not built. If a v0.4 cycle doesn't surface evidence that v0.5 needs feature X, we don't build feature X — we update the roadmap.
2. **No feature creep into adjacent products.** `archforge` is an architectural-thinking tool. It does not become a project tracker, a code generator, a documentation website builder, or a team management tool. When ideas point in those directions, they're rejected — see "Out of scope" below.
3. **The plugin should not grow past what one engineer can hold in their head.** Each minor version adds at most a handful of commands, agents, or skills. Once a single user can no longer remember what the plugin offers without consulting the README, we've gone too far.
4. **Backward compatibility within the 0.x line.** Renames get aliases (as `c4-diagrams` got `architectural-diagrams` in 0.4). Schemas evolve additively. `/archforge:upgrade` handles the migrations.
5. **Bias to skills over commands.** Skills compound; commands proliferate. When a feature can be a skill loaded by existing commands, prefer that.

## Anti-roadmap — what's deliberately out of scope

Things that will not appear in any 0.x version, no matter how often asked:

- **Code generation from ADRs.** ADRs are decisions, not specifications. Codegen is a different product class.
- **Project management integrations** (Linear, Jira, Asana). The boundary between architectural reasoning and task tracking matters; merging them dilutes both. Use `compound-engineering` for feature workflow; use a real PM tool for tasks.
- **Documentation site generators** (Docusaurus, MkDocs, Astro). The plugin produces markdown that those tools can render. Building our own renderer is reinventing wheels.
- **Visual-first diagram editors.** Mermaid-as-text is the right format for compound knowledge. Drag-and-drop diagrams don't merge cleanly, can't be diffed, drift silently.
- **Voting / approval workflows on ADRs.** ADRs capture a decision once made; the deliberation that produced them happens elsewhere (chat, PR, design review). The plugin is not a governance tool.
- **AI-generated user stories or product requirements.** Product work and architectural work are different roles. Conflating them encourages architecture as cargo-cult ("the spec said use microservices") and product as incremental refactor ("first design the right architecture, then we'll figure out what users want").
- **Per-language stack-specific variants.** Universality is a feature. The plugin works for Rust, TypeScript, Python, Go projects with the same skills. Forking by language would not improve outputs enough to justify the maintenance.

---

## v0.5 — "Sharper feedback"

**Theme.** Make the existing commands more precise, not more numerous. Most of v0.5 is about giving Claude a better vocabulary and a better sense for what to look for.

**When.** After v0.4 has been used on at least 3 real cycles by at least 2 users, with feedback collected. We don't ship v0.5 on top of unvalidated v0.4.

### Headline feature: `/archforge:diff <ADR-NNNN>`

Check whether an accepted ADR still lives in the code.

The command extracts the **verifiable architectural rules** from a given ADR (usually the numbered constraints in the Decision section), then walks the actual codebase to check each one. Output is a structured diff: which rules are upheld, which are violated, which can't be checked from source.

This addresses a real failure mode: ADRs accumulate, code drifts, and no one notices until a regression surfaces a year later. `/archforge:diff` makes the drift visible at any time, without requiring a code review.

The command is read-only — it never modifies code or the ADR. It outputs a report and offers to run `/archforge:roast` on the violations or to open a superseding-ADR cycle.

### New skill: `architectural-anti-patterns`

A library of concrete, named architectural anti-patterns — not generic ones like "high coupling" but specific ones engineers recognize when they see them:

- Distributed monolith disguised as microservices.
- Database as integration layer (multiple services owning the same schema).
- Lasagna architecture (excessive abstraction layers for simple operations).
- Anemic domain model.
- Sync chains across services (A → B → C with retries; cascade fails).
- Microservices for an organization that doesn't need them.
- Cache as source of truth.
- Tool-use loops without bounded iterations or timeouts.
- Outbox pattern misused as a queue.
- RAG as source of truth (system fails when retrieval fails).

Each entry: what it looks like, why it's bad, how to escape it. The skill is loaded automatically by `roast`, `review`, and `observe` so Claude can identify these specifically rather than describe symptoms generically.

### New skill: `architectural-metrics`

Operational definitions for the words that get used loosely in architectural conversation:

- Cohesion — what high vs low looks like at module boundary, with concrete signals.
- Coupling — types (data, control, content), measurement heuristics, thresholds.
- ADR conformance — proposed scoring (rules upheld / total verifiable rules).
- Decision velocity — ADRs per quarter, open-question resolution rate.
- Architectural drift — quantitative signals that point at it (file-count growth without ADR growth, dependency growth without ADR growth).
- Bus factor per module — how to estimate it from commit history.

This is not an analytics dashboard. It's a vocabulary skill — when `roast` or `review` or `observe` runs, the agents have a sharper way to express what they're seeing.

### New command: `/archforge:export [--format=md|html|pdf|zip]`

Bundle the project's architectural artifacts into a portable package for sharing — for blog posts, portfolio submissions, onboarding handoffs, archival.

Defaults: a single Markdown file with TOC and resolved cross-references. Options for HTML (rendered with Mermaid inline), PDF (via pandoc if available), and zip (the directory tree, untouched).

`--public` flag scrubs internal-only sections marked with HTML comments (`<!-- internal -->`), strips author handles, and removes pricing details from research documents.

### Smaller changes

- `/archforge:roast` gains a `--severity-threshold=high|medium|low` flag — by default, only `high` and `medium` are surfaced in the chat summary; the full role outputs always include all severities.
- The `architect` skill's calque table grows (driven by what real cycles actually produce — pattern from v0.3).
- `decision-map.md` adds a `## Resolved` section so closed decisions stay visible without bloating the active sections.

### What v0.5 does not do

- No new agents. The 8 agents in v0.4 are enough; if we find a gap, it's a v0.6 conversation.
- No new diagram types. Five is enough.
- No project-management integration.
- No automatic application of `/archforge:diff` findings — the command finds drift; the human (with Claude's help) decides what to do about it.

---

## v0.6 — "Memory and history"

**Theme.** A long-running project accumulates a history. The plugin should learn from that history, surface patterns in past decisions, and help the architect not repeat past mistakes.

**Prerequisite.** A meaningful body of work — at least one project in the wild with 10+ ADRs across 6+ months. Without that, history-aware features have nothing to act on.

### Headline feature: `historian` agent

A new sub-agent that reads the **archive** of architectural work for a project and produces structured observations:

- Which ADRs were superseded? After how long? What forced the supersession?
- Which deferred-to-V2 decisions in old ADRs are now actually due — and which have been quietly forgotten?
- Which `roast` findings did the architect apply? Which were dismissed and what came of them?
- What patterns recur across decisions — both productive (consistently choosing the modular path) and worrying (consistently underestimating operational cost)?
- Where in the timeline did the project's architectural posture genuinely shift, and why?

The historian is invoked manually via `/archforge:retro [--period=quarter|half|year]`, or as part of v0.6's quarterly review flow.

### New command: `/archforge:retro <period>`

Architectural retrospective. Runs `historian` over a named time window and surfaces:

- Decisions taken (with their outcomes if visible — superseded, drifted, working).
- Decisions deferred or dropped silently.
- Recurring topics that came up multiple times across cycles.
- Quality metrics over time — ADR conformance trend, decision velocity, time-from-discover-to-document.
- Recommendations: what to discuss as a team, what to revisit, what to formally close out.

Output is saved to `docs/architecture/retros/YYYY-Qn.md` and forms the seed for the next architectural planning round.

### Pre-commit hooks support

Optional integration with git pre-commit hooks. A new command `/archforge:install-git-hooks` installs project-level hooks that:

- Warn (don't block) when a commit touches package manifests but doesn't modify any ADR.
- Warn when >10 files change and no ADR / decision-map / ARCHITECTURE.md is touched.
- Warn when a substantive (non-status) change is made to an accepted ADR — accepted ADRs should be superseded, not edited.

The hooks are non-blocking by default. A `--strict` flag makes them blocking for teams that want hard enforcement.

The install command is idempotent and respects existing hook setups (husky, pre-commit framework, raw `.git/hooks`). It refuses to overwrite without confirmation.

### Decision-map evolution

`decision-map.md` becomes more sophisticated:

- Decisions get **temporal metadata** — when added, when last reviewed, when expected to be revisited.
- A new `## Stale` section automatically lists decisions deferred more than 6 months without resolution or re-deferral.
- The `/archforge:map` command can take a `--age-check` flag to surface decisions whose "wait for" condition appears to have been met.

### Smaller changes

- `/archforge:upgrade` gains a `--dry-run` flag for environments where automatic mutation is risky.
- The `compound-integration` block in `AGENTS.md` learns about CE workflow versions — re-running on a new CE version produces a diff of what changed in the integration, not a silent overwrite.
- `/archforge:observe` gains historical awareness — items that were observed previously and explicitly dismissed by the user don't surface again unless re-introduced.

### What v0.6 does not do

- No team coordination features (multi-architect support, ADR ownership, conflict resolution). That's v0.7.
- No analytics dashboard. The `historian` produces narrative reports, not graphs. Graphs without context mislead more than they help.
- No automated supersession of stale ADRs. Identifying staleness is the plugin's job; deciding what to do about it is the architect's.

---

## v0.7 — "Teams and budgets"

**Theme.** Architectural work in real teams has two dimensions the earlier versions don't engage with: multiple people contributing to the same architecture, and explicit accounting for the cost of architectural choices over time.

**Prerequisite.** Several teams (>1 person) using the plugin in production over multiple quarters, with feedback on coordination friction.

### Headline feature: `architecture-cost` skill

A first-class accounting of cost as an architectural variable, beyond what `system-design` and `pragmatist` cover today.

The skill defines:

- **Engineering cost** — hours per quarter to maintain a chosen architecture, scaled to team size.
- **Operational cost** — infrastructure, third-party services, with current pricing as research-augmented inputs.
- **Cognitive cost** — onboarding overhead, mental model complexity, cross-team handoff cost.
- **Switching cost** — what it takes to undo this decision later. Decisions with high switching cost get flagged in cycles for extra deliberation.
- **Cost compounding** — how each new decision multiplies or amortizes against existing decisions.

Loaded by `pragmatist` (real cost lens), by `design` phase (alternatives compared on cost), and by `historian` (cost over time, cost of decisions that drifted).

A new optional column in design phase comparison matrices: "5-year total cost, order of magnitude."

### Multi-architect coordination

For teams where multiple people contribute to architecture:

- **ADR ownership and review** — ADRs gain a reviewer field; before promoting to Accepted, the reviewer signs off in a structured way (file edit, not just chat).
- **Decision-map collaborative editing** — convention for who can move items between groups, and under what conditions.
- **Concurrent ADR numbers** — when two people draft ADRs in parallel branches, the merge process is documented (in `compound-integration` skill terms).
- **Anti-bikeshedding rules** — when reviewers disagree, the deliberation belongs in a research document, not in successive ADR edits. The plugin enforces this by refusing to substantively edit Accepted ADRs.

This is governance-light, not governance-heavy. The plugin doesn't replace whatever the team uses for actual decision authority; it just structures the artifacts so the authority is implementable.

### Long-running project artifacts

`STRATEGY.md` becomes a first-class artifact alongside `ARCHITECTURE.md`:

- A template lives in the plugin, generated by `/archforge:init --with-strategy` for new projects.
- `STRATEGY.md` is read by `discover`, `design`, `roast/futurist`, and `historian` as authoritative product context.
- Changes to `STRATEGY.md` can trigger a `/archforge:observe` to surface architectural implications of strategic shifts.

A new artifact: `INVARIANTS.md`. Architectural invariants — things that must remain true regardless of which decisions get superseded. Smaller than `ARCHITECTURE.md`, more durable than ADRs. Examples: "user data never leaves the EU jurisdiction", "all writes are auditable", "no service may have direct access to another service's database". Maintained as an append-only list with rare retractions; each invariant references the ADRs that depend on it.

### Smaller changes

- `/archforge:export --format=presentation` produces a slide-deck-shaped Markdown for presentations to non-architects.
- The `architect` skill gets a `team-mode` flag (autodetected from `STRATEGY.md` team size) that adjusts pushback intensity — solo architects get the full hold-position-and-argue treatment; team architects get a softer "here's what I'd argue" framing because the actual deliberation happens with humans.
- A new sub-agent: `historian-for-onboarding` — produces a structured "what an architect joining this project must know" document by reading the full archive.

### What v0.7 does not do

- No real-time collaboration features. Editing architectural artifacts in real time is what code editors and PR review are for.
- No bot integration with chat platforms. The plugin runs in Claude Code, not in Slack.
- No analytics platform integration. Cost data lives in the plugin's skills as research-augmented references, not in an integrated billing dashboard.

---

## After v0.7 — possible directions

Beyond v0.7, several directions become possible. None of them is committed; each requires evidence from real use.

- **Domain-specific skill packs.** A `archforge-fintech` extension with regulation-specific defaults, terminology, and anti-patterns. Or `archforge-ml` for ML-system architecture. These would ship as separate plugins consuming `archforge` as a base.
- **First-class learning loop.** A version of `historian` that doesn't just observe history but proposes structural changes to the project's architectural process based on what it sees — meta-architecture for the architectural work itself.
- **Multi-repo orchestration.** Architectural work that spans multiple repositories (microservices, monorepos with multiple deployable units). Today, `archforge` assumes one repo; this would be a structural change.
- **Public ADR registries.** A convention for sharing ADRs across organizations — open-source projects publishing their architectural decisions in a discoverable format.

These are speculative and will only enter a real roadmap when there's pressure from real use to address them.

---

## How this roadmap evolves

This document is updated:

- **At each minor version release** (0.5.0, 0.6.0, etc.) with the actual delivered scope and any reshuffling of the next two versions.
- **When real use surfaces unanticipated needs** that should bump priority.
- **When something on the roadmap turns out to be a bad idea.** Items get dropped, not silently postponed; if you see "removed in 2026-XX" in the changelog, it's a real signal.

Suggestions, criticism, and reports of "I tried v0.X and the X feature didn't work the way you said" are the most valuable input. Open an issue or PR.
