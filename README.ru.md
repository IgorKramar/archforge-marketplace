# archforge marketplace

[English](./README.md) | **Русский**

Маркетплейс плагинов [Claude Code](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces), содержащий **`archforge`** — архитектурный инструментарий, превращающий Claude Code в собеседника уровня staff/principal-инженера для архитектурных задач.

## Что внутри

| Плагин | Назначение |
|---|---|
| **`archforge`** | Цикл Discover → Design → Decide → Document → Review. Маршрутизирующий скилл `architect` плюс специализированные скиллы (C4, ADR, проектирование систем, фронт/бэк/AI-агенты, архитектурное код-ревью, исследование). Slash-команды, сабагенты, мягкие предупреждающие хуки. Интеграция с плагином Compound Engineering от EveryInc. |

## Установка

В Claude Code:

```text
/plugin marketplace add https://github.com/IgorKramar/archforge-marketplace
/plugin install archforge@archforge-marketplace
```

Для локальной разработки (сначала склонируй репозиторий):

```text
/plugin marketplace add /absolute/path/to/archforge-marketplace
/plugin install archforge@archforge-marketplace
```

После установки запусти `/reload-plugins` (или перезапусти Claude Code) и проверь через `/plugin list`.

## Быстрый старт

В любом проекте:

```text
/archforge:init
```

Эта команда подготавливает проект: создаёт `ARCHITECTURE.md` и каркас `docs/architecture/` (decisions, diagrams, research, reviews).

Дальше — цикл:

```text
/archforge:discover     # фаза 1 — собрать ограничения, силы, прецеденты
/archforge:design       # фаза 2 — предложить 2–3 альтернативы с компромиссами
/archforge:decide       # фаза 3 — выбрать одну с явным обоснованием
/archforge:document     # фаза 4 — написать ADR + обновить ARCHITECTURE.md
/archforge:review       # фаза 5 — архитектурное ревью кода
```

Или сразу к специализированной команде:

```text
/archforge:adr "переход с REST на gRPC для межсервисного взаимодействия"
/archforge:c4 container "веб-приложение и его зависимости"
/archforge:cycle "введение очереди для исходящих уведомлений"
```

Маршрутизирующий скилл `architect` активируется автоматически, как только в разговоре появляется архитектурная тема — даже без slash-команд.

Полная документация плагина — в [`plugins/archforge/README.ru.md`](./plugins/archforge/README.ru.md).

## Интеграция с Compound Engineering

Если в проекте установлен плагин [`compound-engineering`](https://github.com/EveryInc/compound-engineering-plugin) от EveryInc, выполни один раз:

```text
/archforge:remember-compound-integration
```

Эта команда записывает в `AGENTS.md` проекта правила взаимодействия двух плагинов. После этого Claude в любой сессии знает, как чередовать архитектурный цикл `archforge` с цикломом CE (Brainstorm → Plan → Work → Review → Compound) — без дублирования работы.

## Структура распространения

```
archforge-marketplace/
├── .claude-plugin/
│   └── marketplace.json        ← это ищет /plugin marketplace add
├── README.md                   ← английская версия
├── README.ru.md                ← этот файл
└── plugins/
    └── archforge/             ← сам плагин
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
3. Обнови `plugins/archforge/.claude-plugin/plugin.json`, если меняешь идентичность плагина (переименование, версия).
4. Запушь в публичный Git-хост (GitHub / GitLab / собственный сервер).
5. Пользователи устанавливают через `/plugin marketplace add <url-репозитория>`.

Для распространения без Git маркетплейс можно отдавать как JSON-эндпоинт — см. [официальную документацию по marketplace плагинов](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces).

## Дорожная карта

План на версии 0.5, 0.6, 0.7 описан в [ROADMAP.md](./ROADMAP.md). Документ англоязычный — там же зафиксировано, что **намеренно остаётся за пределами** плагина (anti-roadmap), чтобы не было ложных ожиданий.

## Лицензия

MIT — см. `LICENSE` внутри директории плагина.
