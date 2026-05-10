# `archforge-marketplace` — moved

**English** | [Русский](./README.ru.md)

> **This marketplace has been absorbed.** The `archforge` plugin now lives in **[`kramar-studio-marketplace`](https://github.com/Kramar-IT-Studio/kramar-studio-marketplace)** as a peer of `product`, and was renamed to **`architect`**. The change is effective as of 2026-05-10 (see [ADR-0001](https://github.com/Kramar-IT-Studio/kramar-studio-marketplace/blob/main/docs/architecture/decisions/0001-absorb-archforge-into-kramar-studio-marketplace.md) in the new marketplace for the architectural rationale).

## Migration commands

If you have `archforge-marketplace` installed, run:

```text
/plugin marketplace remove archforge-marketplace                           # optional cleanup
/plugin marketplace add https://github.com/Kramar-IT-Studio/kramar-studio-marketplace
/plugin install architect@kramar-studio-marketplace
```

## What changed (breaking)

- **Plugin name:** `archforge` → `architect`. All slash commands now use the `/architect:*` prefix instead of `/archforge:*`.
- **Router skill:** `archforge:architect` → `architect:role`. The skill that activates the architect persona was renamed to avoid the `architect:architect` collision and to establish a `<plugin>:role` pattern for future suite plugins.
- **Marker file:** the project-version marker is now `.architect-version` (was `.archforge-version`). In each project where `archforge` was previously initialized, rename the file or re-run `/architect:init`.
- **Integration block markers:** the `<!-- archforge × compound-engineering integration -->` HTML markers in users' `AGENTS.md` are now written as `<!-- architect × compound-engineering integration -->`. Re-run `/architect:remember-compound-integration` to refresh.

The full breaking-change list with rationale is in the new plugin's [CHANGELOG entry for v1.0.0](https://github.com/Kramar-IT-Studio/kramar-studio-marketplace/blob/main/plugins/architect/CHANGELOG.md).

## Why

`archforge-marketplace` and `kramar-studio-marketplace` had the same author, the same methodology (cycle + frontmatter + lifecycle + soft hooks + push-back tone), and the same audience (solo-indies and small studios building products with Claude Code). Maintaining them as two separate marketplaces created cross-marketplace dependency questions (how `product` artifacts cross-link to ADRs from the soused) and split documentation surface. Absorbing `archforge` (renamed `architect`) into the studio marketplace makes the conceptual coherence structural rather than documentary, and aligns naming with the role-named peers in the suite (`product`, `ops`, `security`).

## What stays here

This repo (and its git history) is preserved as a redirect-stub. Old links continue to resolve. Pinned/cloned/vendored installs that point at this repo will see `plugins[]: []` and a clear `plugin not found` response on `/plugin install archforge@archforge-marketplace`. The repo is **not archived** so issues and discussions remain reachable for users in the middle of migrating.

## License

MIT — see [`LICENSE`](./LICENSE).
