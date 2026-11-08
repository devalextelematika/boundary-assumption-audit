# Подготовка публикации на GitHub

## Названия

| Поле | Значение |
|---|---|
| Репозиторий | `devalextelematika/boundary-assumption-audit` |
| Видимость | Public |
| Скилл: поле `name` | `boundary-assumption-audit` |
| Отображаемое название | Boundary Assumption Audit |
| Основной язык документации | Английский, с отдельной русской версией |
| Лицензия | MIT |
| Предлагаемые темы GitHub | `agent-skills`, `code-review`, `static-analysis`, `scalability`, `read-only-audit` |

## Краткое описание репозитория

Для поля Description на GitHub предлагается английский вариант:

> An explicitly invoked, read-only agent skill for static audits of limits, scaling assumptions, and cross-layer contracts, with source-backed findings and remedies.

Русский вариант:

> Скилл для статического аудита ограничений, предположений о масштабировании и межслойных контрактов: явный вызов, режим только чтения, выводы и рекомендации на основе исходного кода.

Полные описания и паттерны проектирования находятся в [README.md](../README.md) и [README.ru.md](../README.ru.md). Названия паттернов в обеих версиях сохранены на английском языке.

## Комплект

```text
boundary-assumption-audit/
├── .gitignore
├── SKILL.md
├── README.md
├── README.ru.md
├── LICENSE
├── CONTRIBUTING.md
└── docs/
    └── PUBLISHING.md
```

`SKILL.md` сохранён без изменения поведения. Репозиторий содержит инструкции и документацию; исполняемые зависимости и сборка отсутствуют.

## Согласованные параметры публикации

Пользователь согласовал публикацию комплекта в отдельном публичном репозитории своего GitHub. Аккаунт: `devalextelematika`. Адрес репозитория: https://github.com/devalextelematika/boundary-assumption-audit.

Лицензия MIT выбрана пользователем. Строка copyright в комплекте: `Copyright (c) 2026 boundary-assumption-audit contributors`.

Не добавлять в публикацию локальные отчёты аудитов, токены, конфигурацию аккаунтов и материалы проверяемых проектов.

## Последовательность публикации после согласования

1. Определить нужный аккаунт GitHub и видимость репозитория.
2. Создать репозиторий `boundary-assumption-audit` с согласованным описанием.
3. Загрузить проверенный комплект и сверить файлы, ссылки README и отображение лицензии.
4. Проверить метаданные репозитория и темы. Предоставить пользователю ссылку на опубликованный репозиторий.

Установка в BB — отдельное действие после согласования. При установке имя папки должно быть `boundary-assumption-audit`; текущее имя рабочей папки `AiSkill-boundary-assumption-audit` отличается. Можно использовать копию с правильным именем, не переименовывая рабочую папку.
