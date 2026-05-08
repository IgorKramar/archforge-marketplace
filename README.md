# archforge marketplace

**English** | [Русский](./README.ru.md)

A Claude Code [plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces) hosting **`archforge`** — an architecture toolkit that turns Claude Code into a staff/principal-grade architectural collaborator.

## What's inside

| Plugin | Purpose |
|---|---|
| **`archforge`** | Discover → Design → Decide → Document → Review cycle. Router skill `architect` plus specialist skills (C4, ADR, system design, frontend/backend/AI-agents architecture, architectural code review, research). Slash commands, sub-agents, soft-warning hooks. |

## Installation

From inside Claude Code:

```text
/plugin marketplace add https://github.com/IgorKramar/archforge-marketplace
/plugin install archforge@archforge-marketplace
```

For local development (clone first):

```text
/plugin marketplace add /absolute/path/to/archforge-marketplace
/plugin install archforge@archforge-marketplace
```

After install, run `/reload-plugins` (or restart Claude Code) and verify with `/plugin list`.

## Quick start

In any project:

```text
/archforge:init
```

This bootstraps the project with `ARCHITECTURE.md` and a `docs/architecture/` skeleton (decisions, diagrams, research, reviews).

Then use the cycle:

```text
/archforge:discover     # phase 1 — gather constraints, forces, prior art
/archforge:design       # phase 2 — propose 2–3 alternatives with trade-offs
/archforge:decide       # phase 3 — pick one with explicit reasoning
/archforge:document     # phase 4 — emit ADR + update ARCHITECTURE.md
/archforge:review       # phase 5 — architectural code review
```

Or jump straight to a specialist:

```text
/archforge:adr "switch from REST to gRPC for service-to-service"
/archforge:c4 container "user-facing web app and its dependencies"
/archforge:cycle "introduce a job queue for outbound notifications"
```

The router skill `architect` activates automatically on architectural language even without slash commands.

See [`plugins/archforge/README.md`](./plugins/archforge/README.md) for the full reference.

## Compound Engineering integration

If you also use the [`compound-engineering`](https://github.com/EveryInc/compound-engineering-plugin) plugin from EveryInc, run once per project:

```text
/archforge:remember-compound-integration
```

This writes a rule block into the project's `AGENTS.md` describing how the archforge cycle (Discover → Design → Decide → Document → Review) interleaves with the CE workflow (Brainstorm → Plan → Work → Review → Compound). After that, Claude knows in every session how to chain the two without duplicating work.

## Distribution structure

```
archforge-marketplace/
├── .claude-plugin/
│   └── marketplace.json        ← this is what /plugin marketplace add looks for
├── README.md                   ← English (this file)
├── README.ru.md                ← Russian
└── plugins/
    └── archforge/             ← the plugin itself
        ├── .claude-plugin/
        │   └── plugin.json
        ├── commands/
        ├── agents/
        ├── skills/
        ├── hooks/
        ├── scripts/
        ├── templates/
        └── README.md
```

The marketplace and plugin manifests are deliberately separate. A marketplace can host many plugins; one plugin can be referenced from many marketplaces. Keep this separation when you fork.

## Publishing your own fork

1. Fork or copy this repository.
2. Update `.claude-plugin/marketplace.json` with your `name` and `owner`.
3. Update `plugins/archforge/.claude-plugin/plugin.json` if you change the plugin identity (rename, version bump, etc).
4. Push to a public Git host (GitHub / GitLab / self-hosted).
5. Users install with `/plugin marketplace add <your-repo-url>`.

For non-Git distribution, the marketplace can be served as a JSON endpoint — see [the official plugin marketplaces docs](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces).

## Roadmap

The plan for v0.5, v0.6, and v0.7 is documented in [ROADMAP.md](./ROADMAP.md). It includes both what's coming and what's deliberately out of scope.

## License

MIT — see `LICENSE` inside the plugin directory.
