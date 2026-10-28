<div align="center">

# Archforge Marketplace

**English** · [Русский](README.ru.md)

### The plugin moved. The old links stay.

![Markdown](https://img.shields.io/badge/Markdown-7aa2f7?style=flat-square)
![JSON](https://img.shields.io/badge/JSON-e0af68?style=flat-square)
![Status](https://img.shields.io/badge/status-moved-e0af68?style=flat-square)

[Migrate](#migration-commands) · [Breaking changes](#what-changed-breaking) · [Compatibility](#what-stays-here)

</div>

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

The two marketplaces shared an author, methodology and audience. Combining them removes separate installation and documentation paths and keeps architecture and product material together. The name `architect` follows the role-based naming used by `product`, `ops` and `security`.

## What stays here

This repository and its history remain as a redirect. Old links continue to resolve. Installs from the current revision receive an empty `plugins[]: []` list, so `/plugin install archforge@archforge-marketplace` no longer finds the plugin. Use the migration commands above for a new installation.

## License

The previous README declares MIT, but this redirect snapshot does not include a license file. Check the license at the source revision when reusing historical code.
