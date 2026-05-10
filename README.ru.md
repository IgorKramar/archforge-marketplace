# `archforge-marketplace` — переехал

[English](./README.md) | **Русский**

> **Этот marketplace поглощён.** Плагин `archforge` теперь живёт в **[`kramar-studio-marketplace`](https://github.com/Kramar-IT-Studio/kramar-studio-marketplace)** рядом с `product` и переименован в **`architect`**. Изменение действует с 2026-05-10 (архитектурное обоснование — в [ADR-0001](https://github.com/Kramar-IT-Studio/kramar-studio-marketplace/blob/main/docs/architecture/decisions/0001-absorb-archforge-into-kramar-studio-marketplace.md) нового marketplace).

## Команды миграции

Если у тебя установлен `archforge-marketplace`, выполни:

```text
/plugin marketplace remove archforge-marketplace                           # опционально
/plugin marketplace add https://github.com/Kramar-IT-Studio/kramar-studio-marketplace
/plugin install architect@kramar-studio-marketplace
```

## Что изменилось (breaking)

- **Имя плагина:** `archforge` → `architect`. Все slash-команды теперь `/architect:*` вместо `/archforge:*`.
- **Router skill:** `archforge:architect` → `architect:role`. Скилл, активирующий архитектурную persona, переименован, чтобы избежать collision'а `architect:architect` и установить pattern `<plugin>:role` для будущих плагинов suite.
- **Marker-файл:** маркер версии в проекте теперь `.architect-version` (был `.archforge-version`). В каждом проекте, где раньше был инициализирован `archforge`, переименуй файл или перезапусти `/architect:init`.
- **Маркеры integration-блока:** HTML-комментарии `<!-- archforge × compound-engineering integration -->` в `AGENTS.md` теперь пишутся как `<!-- architect × compound-engineering integration -->`. Перезапусти `/architect:remember-compound-integration` для обновления.

Полный список breaking changes с обоснованием — в [CHANGELOG entry для v1.0.0](https://github.com/Kramar-IT-Studio/kramar-studio-marketplace/blob/main/plugins/architect/CHANGELOG.md) нового плагина.

## Почему

У `archforge-marketplace` и `kramar-studio-marketplace` был один автор, одна методология (цикл + frontmatter + lifecycle + soft hooks + push-back tone) и одна аудитория (соло-инди и маленькие студии, строящие продукты с Claude Code). Поддержание их двумя раздельными marketplace порождало вопросы про cross-marketplace зависимости (как product-артефакты ссылаются на ADR из соседа) и расщепляло documentation surface. Поглощение `archforge` (теперь `architect`) в studio marketplace делает conceptual coherence структурной, а не документарной, и согласует именование с роль-плагинами suite (`product`, `ops`, `security`).

## Что остаётся здесь

Этот репозиторий (и его git-история) сохраняется как redirect-stub. Старые ссылки продолжают работать. Pinned/cloned/vendored установки, указывающие на этот репо, увидят `plugins[]: []` и понятную ошибку `plugin not found` при попытке `/plugin install archforge@archforge-marketplace`. Репо **не архивировано** — issues и discussions остаются доступны для пользователей в процессе миграции.

## Лицензия

MIT — см. [`LICENSE`](./LICENSE).
