# krait_arch marketplace

[English](./README.md) | **Русский**

Маркетплейс плагинов [Claude Code](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces), содержащий **`krait_arch`** — архитектурный инструментарий, превращающий Claude Code в собеседника уровня staff/principal-инженера для архитектурных задач.

## Что внутри

| Плагин | Назначение |
|---|---|
| **`krait_arch`** | Цикл Discover → Design → Decide → Document → Review. Маршрутизирующий скилл `architect` плюс специализированные скиллы (C4, ADR, проектирование систем, фронт/бэк/AI-агенты, архитектурное код-ревью, исследование). Slash-команды, сабагенты, мягкие предупреждающие хуки. Интеграция с плагином Compound Engineering от EveryInc. |

## Установка

В Claude Code:

```text
/plugin marketplace add https://github.com/IgorKramar/krait-arch-marketplace
/plugin install krait_arch@krait-arch-marketplace
```

Для локальной разработки (сначала склонируй репозиторий):

```text
/plugin marketplace add /absolute/path/to/krait-arch-marketplace
/plugin install krait_arch@krait-arch-marketplace
```

После установки запусти `/reload-plugins` (или перезапусти Claude Code) и проверь через `/plugin list`.

## Быстрый старт

В любом проекте:

```text
/krait_arch:init
```

Эта команда подготавливает проект: создаёт `ARCHITECTURE.md` и каркас `docs/architecture/` (decisions, diagrams, research, reviews).

Дальше — цикл:

```text
/krait_arch:discover     # фаза 1 — собрать ограничения, силы, прецеденты
/krait_arch:design       # фаза 2 — предложить 2–3 альтернативы с компромиссами
/krait_arch:decide       # фаза 3 — выбрать одну с явным обоснованием
/krait_arch:document     # фаза 4 — написать ADR + обновить ARCHITECTURE.md
/krait_arch:review       # фаза 5 — архитектурное ревью кода
```

Или сразу к специализированной команде:

```text
/krait_arch:adr "переход с REST на gRPC для межсервисного взаимодействия"
/krait_arch:c4 container "веб-приложение и его зависимости"
/krait_arch:cycle "введение очереди для исходящих уведомлений"
```

Маршрутизирующий скилл `architect` активируется автоматически, как только в разговоре появляется архитектурная тема — даже без slash-команд.

Полная документация плагина — в [`plugins/krait_arch/README.ru.md`](./plugins/krait_arch/README.ru.md).

## Интеграция с Compound Engineering

Если в проекте установлен плагин [`compound-engineering`](https://github.com/EveryInc/compound-engineering-plugin) от EveryInc, выполни один раз:

```text
/krait_arch:remember-compound-integration
```

Эта команда записывает в `AGENTS.md` проекта правила взаимодействия двух плагинов. После этого Claude в любой сессии знает, как чередовать архитектурный цикл `krait_arch` с цикломом CE (Brainstorm → Plan → Work → Review → Compound) — без дублирования работы.

## Структура распространения

```
krait-arch-marketplace/
├── .claude-plugin/
│   └── marketplace.json        ← это ищет /plugin marketplace add
├── README.md                   ← английская версия
├── README.ru.md                ← этот файл
└── plugins/
    └── krait_arch/             ← сам плагин
        ├── .claude-plugin/
        │   └── plugin.json
        ├── commands/
        ├── agents/
        ├── skills/
        ├── hooks/
        ├── scripts/
        ├── templates/
        ├── README.md
        └── README.ru.md
```

Манифесты маркетплейса и плагина намеренно разделены. Один маркетплейс может хостить несколько плагинов; один плагин может быть подцеплен из разных маркетплейсов. Сохраняй это разделение, если будешь форкать.

## Публикация своего форка

1. Форкни или скопируй этот репозиторий.
2. Обнови `.claude-plugin/marketplace.json` — поля `name` и `owner`.
3. Обнови `plugins/krait_arch/.claude-plugin/plugin.json`, если меняешь идентичность плагина (переименование, версия).
4. Запушь в публичный Git-хост (GitHub / GitLab / собственный сервер).
5. Пользователи устанавливают через `/plugin marketplace add <url-репозитория>`.

Для распространения без Git маркетплейс можно отдавать как JSON-эндпоинт — см. [официальную документацию по marketplace плагинов](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces).

## Дорожная карта

План на версии 0.5, 0.6, 0.7 описан в [ROADMAP.md](./ROADMAP.md). Документ англоязычный — там же зафиксировано, что **намеренно остаётся за пределами** плагина (anti-roadmap), чтобы не было ложных ожиданий.

## Лицензия

MIT — см. `LICENSE` внутри директории плагина.
