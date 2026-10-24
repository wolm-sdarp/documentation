---
---
# Подключение GitHub CLI (`gh`) в сессии Claude

> **Статус:** v1.0 · 2026-10-01 · проверено на практике в сессии от 1 октября 2026.
> Нужен, когда в новом диалоге требуется читать и менять GitHub организации `wolm-sdarp` (метки, issues, настройки репозиториев, доска).

Фраза для нового чата: «Подключи gh по инструкции `claude/gh-access.md`».

## 1. Что нужно знать заранее

| Вопрос | Ответ |
| --- | --- |
| Сохраняется ли доступ между диалогами | **Нет.** `gh` и токен лежат в домашней папке рабочей среды конкретной сессии и пропадают вместе с ней |
| Где хранится токен | Только в этой среде (`~/.config/gh/hosts.yml`). **В `claude/`, в папках проекта и в чате токен не сохраняется и не вставляется** |
| Что можно без вопросов | Чтение: issues, milestones, ветки, метки, настройки, dry-run скриптов |
| Что только после явного «да» | Любая запись в организацию: создание и правка issues, метки, rulesets, настройки репозиториев, добавление на доску, смена видимости |
| Что требует от пользователя | Один раз за сессию ввести одноразовый код на `github.com/login/device` и подтвердить права (пароль и 2FA вводятся только им самим, если GitHub их запросит) |
| Альтернатива | Запускать скрипты из `claude/` самому на Mac, где `gh` уже авторизован: dry-run и разбор результата делаются в диалоге, запись выполняется одной командой |

## 2. Порядок подключения

Все команды выполняются в оболочке рабочей среды на компьютере пользователя (`device_bash`), а не в облачном контейнере. Каждый вызов — новая оболочка; фоновые процессы между вызовами **не переживают**, поэтому всё разбито на короткие шаги.

### Шаг 1. Проверить, есть ли `gh`

```bash
export PATH=$HOME/bin:$PATH; which gh && gh auth status
```

Если `gh` найден и авторизован, остальные шаги не нужны.

### Шаг 2. Установить `gh` (если нет)

Установка идёт из релиза на GitHub в `$HOME/bin` (вне папок проекта). Сеть нестабильна, поэтому загрузка с докачкой и проверкой архива:

```bash
mkdir -p $HOME/bin $HOME/ghtmp && cd $HOME/ghtmp
V=$(curl -s -m 20 https://api.github.com/repos/cli/cli/releases/latest | python3 -c "import sys,json;print(json.load(sys.stdin)['tag_name'].lstrip('v'))")
curl -sL -m 150 -C - --retry 3 -o gh.tgz "https://github.com/cli/cli/releases/download/v${V}/gh_${V}_linux_amd64.tar.gz"
tar tzf gh.tgz >/dev/null && echo archive-ok      # если ошибка — повторить загрузку: архив оборвался
```

Затем отдельным вызовом:

```bash
cd $HOME/ghtmp && tar xzf gh.tgz && cp gh_*_linux_amd64/bin/gh $HOME/bin/gh && chmod +x $HOME/bin/gh && $HOME/bin/gh --version && rm -rf $HOME/ghtmp
```

Архитектуру проверить через `uname -m` (в прошлый раз `x86_64`).

### Шаг 3. Запросить одноразовый код (вызов 1)

Обычный `gh auth login --web` не годится: процесс ожидания завершается вместе с вызовом, и токен не создаётся. Поэтому используется тот же OAuth device flow, который выполняет сам GitHub CLI, но в два шага. `178c6fc778ccc68e1d6a` — публичный идентификатор приложения GitHub CLI; на экране подтверждения пользователь увидит именно «GitHub CLI».

```bash
cd $HOME && curl -s -m 20 -X POST https://github.com/login/device/code \
  -H "Accept: application/json" \
  -d "client_id=178c6fc778ccc68e1d6a&scope=repo project read:org" > devcode.json
chmod 600 devcode.json
python3 -c "import json;d=json.load(open('devcode.json'));print(d['user_code'],d['verification_uri'],d['expires_in'])"
```

Код действует 15 минут (`expires_in`). Выданный код **сообщается пользователю в чате** вместе со ссылкой `https://github.com/login/device` и списком запрашиваемых прав. После этого нужно дождаться его ответа.

### Шаг 4. Получить токен и войти (вызов 2, после ответа пользователя)

```bash
export PATH=$HOME/bin:$PATH; cd $HOME
DC=$(python3 -c "import json;print(json.load(open('devcode.json'))['device_code'])")
for i in $(seq 1 30); do
  R=$(curl -s -m 15 -X POST https://github.com/login/oauth/access_token -H "Accept: application/json" \
      -d "client_id=178c6fc778ccc68e1d6a&device_code=$DC&grant_type=urn:ietf:params:oauth:grant-type:device_code")
  if echo "$R" | grep -q access_token; then
    echo "$R" | python3 -c "import sys,json;print(json.load(sys.stdin)['access_token'])" \
      | gh auth login -h github.com -p https --with-token && echo LOGIN_OK
    break
  else
    echo "$R" | python3 -c "import sys,json;print(json.load(sys.stdin).get('error'))"; sleep 5
  fi
done
rm -f devcode.json
gh auth status
```

Результат: `Logged in to github.com account angelgardt`, права `project`, `read:org`, `repo`. Ошибка `authorization_pending` означает, что код ещё не подтверждён; `expired_token` — нужно вернуться к шагу 3.

### Шаг 5. Расширенные права (только когда нужны)

| Задача | Дополнительное право |
| --- | --- |
| Создавать и менять типы issues организации | `admin:org` |
| Менять workflows в `.github/workflows` | `workflow` |

Запрашивается тем же способом (шаги 3–4), строка `scope=` расширяется. Права добавляются только под конкретную задачу и с ведома пользователя; сначала предлагается сделать то же в интерфейсе GitHub.

## 3. Особенности среды (проверено)

| Особенность | Что делать |
| --- | --- |
| Запросы к API идут по 3–5 секунд, бывают `TLS handshake timeout` | Чтение — повторять; запись — повторять только при ошибках соединения (запрос не дошёл до сервера). Скрипт `apply-issues.py` уже так делает |
| Один вызов `device_bash` живёт не больше 170–180 секунд, затем процесс завершается | Длинные операции делить на порции: `apply-issues.py --apply --no-project --only КЛЮЧ ...` по 5–6 задач; `apply-labels.py --repo ИМЯ` по одному репозиторию (около 80 секунд на репозиторий) |
| Добавление на доску (`gh project item-add`) иногда отвечает `unknown owner type` | Повторить для пропущенных; операция идемпотентна |
| Полный dry-run `apply-labels.py` по всем репозиториям не укладывается в лимит вызова | Запускать с `--repo` |
| В названиях milestones и других полях встречаются неразрывные пробелы | Сравнивать названия с нормализацией (в `apply-issues.py` это сделано) |
| Типы issues организации (`orgs/wolm-sdarp/issue-types`) читаются только с авторизацией; создавать их токену без `admin:org` нельзя | Читать после входа; создавать вручную в Settings → Planning → Issue types |

## 4. После работы

- Токен можно отозвать в любой момент: `github.com/settings/applications` → «GitHub CLI» → Revoke (или «Authorized OAuth Apps»). Он автоматически перестаёт быть доступным и при завершении сессии, но на стороне GitHub остаётся действительным, пока его не отозвать.
- Рекомендуется отзывать токен по завершении крупной серии изменений, если следующая сессия не планируется.
- Нельзя: сохранять токен в файлы проекта или `claude/`, присылать его в чат, класть в `~/.claude` или другие места, не входящие в сессию.

## 5. Что уже сделано на GitHub этой сессией

См. `claude/issues-bootstrap/README.md`: 4 milestones, 37 issues и 3 эпика, связи sub-issues, добавление на доску. Не сделано и ждёт решения: типы issues в организации (задача 1.1), применение меток (задача 1.3), повторный запуск `apply-issues.py` после них.

## История изменений

| Версия | Дата | Изменение |
| --- | --- | --- |
| 1.0 | 2026-10-01 | Первая версия по итогам подключения `gh` в сессии |
