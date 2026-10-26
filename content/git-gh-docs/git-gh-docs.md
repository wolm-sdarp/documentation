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

| Репозиторий                                                                      | Тип    | Описание                                                                                                |
| -------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------- |
| [`.git`](https://github.com/wolm-sdarp/.github.git)                              | repo   | [[org-profile-docs\|Профиль организации]]: технический репозиторий для хранения документации сообщества |
| [`wolm-sdarp.github.io`](https://github.com/wolm-sdarp/wolm-sdarp.github.io.git) | quarto | Хаб проекта: входная точка, информации о проекте и авторах и другое на сайте проекта.                   |
| [`documentation`](https://github.com/wolm-sdarp/documentation.git)               | vault  | [[vault-docs\|Документация проекта]]                                                                    |
| [`kit`](https://github.com/wolm-sdarp/kit.git)                                   | repo   | *TBA*                                                                                                   |
| [`book0`](https://github.com/wolm-sdarp/book0.git)                               | quarto | Книга 0: Математика для анализа данных                                                                  |
| [`book1`](https://github.com/wolm-sdarp/book1.git)                               | quarto | Книга 1: Статистика и анализ данных                                                                     |
| [`assessment0`](https://github.com/wolm-sdarp/assessment0.git)                   | quarto | Формы контроля к Книге 0                                                                                |
| [`assessment1`](https://github.com/wolm-sdarp/assessment1.git)                   | quarto | Формы контроля к Книге 1                                                                                |
| [`annex`](https://github.com/wolm-sdarp/annex.git)                               | quarto | Дополнительные материалы проекта, публикуемые для широкого круга читателей                              |

### Локальная структура

| Директория      | Связанный репозиторий      | Тег     | Описание                                                                                               |
| --------------- | -------------------------- | ------- | ------------------------------------------------------------------------------------------------------ |
| `org`           | [`.git`]()                 | Orange  | Профиль организации                                                                                    |
| `hub`           | [`wolm-sdarp.github.io`]() | Orange  | Хаб проекта                                                                                            |
| `documentation` | [`documentation`]()        | Blue    | Документация проекта                                                                                   |
| `kit`           | [`kit`]()                  | TBA     | *TBA*                                                                                                  |
| `book0`         | [`book0`]()                | Green   | Книга 0: Математика для анализа данных                                                                 |
| `book1`         | [`book1`]()                | Green   | Книга 1: Статистика и анализ данных                                                                    |
| `assessment0`   | [`assessment0`]()          | Green   | Формы контроля к Книге 0                                                                               |
| `assessment1`   | [`assessment1`]()          | Green   | Формы контроля к Книге 1                                                                               |
| `annex`         | [`annex`]()                | Green   | Дополнительные материалы проекта                                                                       |
| `manim`         |                            |         | Скрипты генерации видеоиллюстраций через `manim` (Python). *Пока отсутствует.*                         |
| `inbox`         | —                          | Purple  | Сырые входящие материалы, ещё не разобранные                                                           |
| `design`        | —                          | Yellow  | Элементы визуального стиля, не включенные непосредственно в материалы проекта (аватары, баннеры и др.) |
| `archive`       | —                          | No Tags | Архив файлов, которые больше не используются, но ещё могут пригодится.                                 |
| `claude`        | —                          | No Tags | Ауптупы с результатами работы Claude.                                                                  |

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
