---
title: Git и GitHub проекта
aliases:
  - Git и GitHub проекта
---
> Это документация по работе с GitHub'ом проекта WoLM SDARP.
> Документацию по самой платформе GitHub от разработчиков можно найти [тут](https://docs.github.com/ru).

## GitHub Organization

Работа над проектом ведётся в [организации WoLM SDARP](https://github.com/wolm-sdarp). Там же публикуются результаты (продукты) проекта — книги, формы контроля, дополнительные материалы.

## GitHub Repositories

| Репозиторий                | Описание                                  |
| -------------------------- | ----------------------------------------- |
| [`.git`]()                 | [[org-profile-docs\|Профиль организации]] |
| [`wolm-sdarp.github.io`]() | Хаб проекта                               |
| [`documentation`]()        | [[vault-docs\|Документация проекта]]      |
| [`kit`]()                  | TBA                                       |
| [`book0`]()                | Книга 0: Математика для анализа данных    |
| [`book1`]()                | Книга 1: Статистика и анализ данных       |
| [`assessment0`]()          | Формы контроля к Книге 0                  |
| [`assessment1`]()          | Формы контроля к Книге 1                  |
| [`annex`]()                | Дополнительные материалы проекта          |

### Рекомендуемая локальная структура

| Директория      | Связанный репозиторий      | Тег     |
| --------------- | -------------------------- | ------- |
| `org`           | [`.git`]()                 | Orange  |
| `hub`           | [`wolm-sdarp.github.io`]() | Orange  |
| `documentation` | [`documentation`]()        | Blue    |
| `kit`           | [`kit`]()                  | TBA     |
| `book0`         | [`book0`]()                | Green   |
| `book1`         | [`book1`]()                | Green   |
| `assessment0`   | [`assessment0`]()          | Green   |
| `assessment1`   | [`assessment1`]()          | Green   |
| `annex`         | [`annex`]()                | Green   |
| `design`        | —                          | Yellow  |
| `archive`       | —                          | No Tags |
| `claude`        | —                          | No Tags |

## GitHub Projects

В организации существует проектный менеджмент через [GitHub Projects](https://github.com/orgs/wolm-sdarp/projects).

[[gh-issues-docs]]
[[git-workflow]]
[[gh-project-review-answers]]

| GH Project             | Описание | Дефолтный репозиторий | Привязанные репозитории |
| ---------------------- | -------- | --------------------- | ----------------------- |
| WoLM SDARP PM          |          | `.github`             |                         |
| Bug & Mistake Tracker  |          | `.github` ??          |                         |
| Docs Tracker           |          | `documentation`       |                         |
| Learning Design        |          | `documentation`       |                         |
| Writing Workflow       |          | ``                    |                         |
| Assessment Development |          |                       |                         |
