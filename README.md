# krait_arch marketplace

**English** | [Русский](./README.ru.md)

A Claude Code [plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces) hosting **`krait_arch`** — an architecture toolkit that turns Claude Code into a staff/principal-grade architectural collaborator.

## What's inside

| Plugin | Purpose |
|---|---|
| **`krait_arch`** | Discover → Design → Decide → Document → Review cycle. Router skill `architect` plus specialist skills (C4, ADR, system design, frontend/backend/AI-agents architecture, architectural code review, research). Slash commands, sub-agents, soft-warning hooks. |

## Installation

From inside Claude Code:

```text
/plugin marketplace add https://github.com/IgorKramar/krait-arch-marketplace
/plugin install krait_arch@krait-arch-marketplace
```

For local development (clone first):

```text
/plugin marketplace add /absolute/path/to/krait-arch-marketplace
/plugin install krait_arch@krait-arch-marketplace
```

After install, run `/reload-plugins` (or restart Claude Code) and verify with `/plugin list`.

## Quick start

In any project:

```text
/krait_arch:init
```

This bootstraps the project with `ARCHITECTURE.md` and a `docs/architecture/` skeleton (decisions, diagrams, research, reviews).

Then use the cycle:

```text
/krait_arch:discover     # phase 1 — gather constraints, forces, prior art
/krait_arch:design       # phase 2 — propose 2–3 alternatives with trade-offs
/krait_arch:decide       # phase 3 — pick one with explicit reasoning
/krait_arch:document     # phase 4 — emit ADR + update ARCHITECTURE.md
/krait_arch:review       # phase 5 — architectural code review
```

Or jump straight to a specialist:

```text
/krait_arch:adr "switch from REST to gRPC for service-to-service"
/krait_arch:c4 container "user-facing web app and its dependencies"
/krait_arch:cycle "introduce a job queue for outbound notifications"
```

The router skill `architect` activates automatically on architectural language even without slash commands.

See [`plugins/krait_arch/README.md`](./plugins/krait_arch/README.md) for the full reference.

## Compound Engineering integration

If you also use the [`compound-engineering`](https://github.com/EveryInc/compound-engineering-plugin) plugin from EveryInc, run once per project:

```text
/krait_arch:remember-compound-integration
```

This writes a rule block into the project's `AGENTS.md` describing how the krait architecture cycle (Discover → Design → Decide → Document → Review) interleaves with the CE workflow (Brainstorm → Plan → Work → Review → Compound). After that, Claude knows in every session how to chain the two without duplicating work.

## Distribution structure

```
krait-arch-marketplace/
├── .claude-plugin/
│   └── marketplace.json        ← this is what /plugin marketplace add looks for
├── README.md                   ← English (this file)
├── README.ru.md                ← Russian
└── plugins/
    └── krait_arch/             ← the plugin itself
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
3. Update `plugins/krait_arch/.claude-plugin/plugin.json` if you change the plugin identity (rename, version bump, etc).
4. Push to a public Git host (GitHub / GitLab / self-hosted).
5. Users install with `/plugin marketplace add <your-repo-url>`.

For non-Git distribution, the marketplace can be served as a JSON endpoint — see [the official plugin marketplaces docs](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces).

## Roadmap

The plan for v0.5, v0.6, and v0.7 is documented in [ROADMAP.md](./ROADMAP.md). It includes both what's coming and what's deliberately out of scope.

## License

MIT — see `LICENSE` inside the plugin directory.
