<div align="center">

# Archforge Marketplace

[English](README.md) · **Русский**

### Плагин переехал. Старые ссылки сохранены.

![Markdown](https://img.shields.io/badge/Markdown-7aa2f7?style=flat-square)
![JSON](https://img.shields.io/badge/JSON-e0af68?style=flat-square)
![Status](https://img.shields.io/badge/status-moved-e0af68?style=flat-square)

[Миграция](#команды-миграции) · [Изменения](#что-изменилось-breaking) · [Совместимость](#что-остаётся-здесь)

[![MIT](https://img.shields.io/badge/license-MIT-9ece6a?style=flat-square)](LICENSE)

</div>

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
- **Router skill:** `archforge:architect` → `architect:role`. Скилл архитектурной роли переименован, чтобы избежать повтора `architect:architect` и использовать схему `<plugin>:role`.
- **Marker-файл:** маркер версии в проекте теперь `.architect-version` (был `.archforge-version`). В каждом проекте, где раньше был инициализирован `archforge`, переименуй файл или перезапусти `/architect:init`.
- **Маркеры integration-блока:** HTML-комментарии `<!-- archforge × compound-engineering integration -->` в `AGENTS.md` теперь пишутся как `<!-- architect × compound-engineering integration -->`. Перезапусти `/architect:remember-compound-integration` для обновления.

Полный список breaking changes с обоснованием — в [CHANGELOG entry для v1.0.0](https://github.com/Kramar-IT-Studio/kramar-studio-marketplace/blob/main/plugins/architect/CHANGELOG.md) нового плагина.

## Почему

У двух marketplace был один автор, общая методология и аудитория. Объединение убирает необходимость поддерживать отдельные установки и согласовывать ссылки между архитектурными и продуктовыми материалами. Имя `architect` соответствует роли плагина рядом с `product`, `ops` и `security`.

## Что остаётся здесь

Этот репозиторий и его история сохраняются как указатель на новое место. Старые ссылки продолжают работать. Установки из текущей версии этого репозитория получают пустой список `plugins[]: []`; команда `/plugin install archforge@archforge-marketplace` больше не найдёт плагин. Для новой установки используйте команды миграции выше.

## Лицензия

[MIT](LICENSE) · Copyright © 2026 Igor Kramar. Сторонние материалы и ресурсы по ссылкам сохраняют собственные лицензии и права.