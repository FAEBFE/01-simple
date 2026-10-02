# GitHub: настройка Git и создание репозитория из терминала

Настраиваем GitHub для работы из терминала и проверяем полный рабочий цикл:

- авторизация GitHub CLI;
    
- SSH-аутентификация;
    
- имя и email Git-коммитов;
    
- создание репозитория с одновременной отправкой локального содержимого;
    
- проверка результата и авторства коммита;
    
- при необходимости — удаление тестового репозитория.
    

Цель — получить рабочую конфигурацию, при которой можно из локального каталога **создать готовый GitHub-репозиторий и сразу отправить в него существующую Git-историю**.

## 1. Проверяем GitHub CLI

Проверяем авторизацию:

```bash
gh auth status
```

Если `gh` ещё не авторизован:

```bash
gh auth login
```

Выбираем:

```text
GitHub.com
SSH
Login with a web browser
```

GitHub покажет одноразовый код непосредственно в терминале и адрес:

```text
https://github.com/login/device
```

Открываем адрес в браузере, вводим код из терминала и подтверждаем авторизацию.

Проверяем:

```bash
gh auth status
```

## 2. Проверяем GitHub-аккаунт

```bash
gh api user --jq '{login,id,email}'
```

Например:

```text
{
  "login": "FAEBFE",
  "id": 336804604,
  "email": null
}
```

`email: null` здесь нормально: GitHub API не обязан возвращать публичный email аккаунта.

Нам нужны прежде всего `login` и `id`.

## 3. Проверяем SSH

```bash
ssh -T git@github.com
```

Ожидаемый результат:

```text
Hi FAEBFE! You've successfully authenticated, but GitHub does not provide shell access.
```

Это подтверждает, что SSH-ключ используется для нужного GitHub-аккаунта.

## 4. Проверяем SSH-конфигурацию

Открываем:

```bash
zed ~/.ssh/config
```

Для обычного подключения к GitHub:

```text
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

После изменения:

```bash
ssh -T git@github.com
```

## 5. Настраиваем Git identity

Проверяем:

```bash
git config --global --get user.name
git config --global --get user.email
```

Устанавливаем:

```bash
git config --global user.name "FAEBFE"
git config --global user.email "336804604+FAEBFE@users.noreply.github.com"
```

Проверяем:

```bash
git config --global --get user.name
git config --global --get user.email
```

Здесь работают два независимых механизма:

```text
SSH
└── определяет GitHub-аккаунт для Git-операций

user.name + user.email
└── записываются в Git-коммит
    и используются GitHub для его атрибуции
```

Для ID-based `noreply` адреса используется GitHub ID именно текущего аккаунта.

## 6. Проверяем, не переопределён ли SSH для Git

Git может иметь собственную настройку `core.sshCommand`.

Проверяем:

```bash
git config --global --get core.sshCommand
```

Если такая настройка не нужна:

```bash
git config --global --unset core.sshCommand
```

После этого Git использует обычную SSH-конфигурацию из:

```text
~/.ssh/config
```

Проверяем:

```bash
ssh -T git@github.com
```

## 7. Подготавливаем локальный Git-репозиторий

Переходим в каталог проекта:

```bash
cd ~/mdbd/pub/01-simple
```

Проверяем:

```bash
git status
```

Если каталог ещё не является Git-репозиторием:

```bash
git init
```

Добавляем нужные файлы:

```bash
git add .
```

Создаём первый коммит:

```bash
git commit -m "Initial commit"
```

Проверяем его:

```bash
git log -1 --format=fuller
```

На этом этапе локальный репозиторий уже должен содержать именно то состояние, которое мы собираемся опубликовать.

## 8. Создаём готовый репозиторий на GitHub

Для публичного репозитория:

```bash
gh repo create FAEBFE/01-simple --public --source=. --remote=origin --push
```

Для приватного:

```bash
gh repo create FAEBFE/01-simple --private --source=. --remote=origin --push
```

Здесь:

```text
--public / --private
    задаёт видимость репозитория

--source=.
    указывает текущий локальный Git-репозиторий
```

```text
--remote=origin
    создаёт remote с именем origin
```

```text
--push
    сразу отправляет существующую локальную Git-историю на GitHub
```

Таким образом, команда не просто создаёт пустой репозиторий на GitHub, а **создаёт его и сразу публикует в нём локальное содержимое**.

Отдельно выполнять:

```bash
git remote add origin ...
```

или:

```bash
git push
```

после этой команды не требуется.

## 9. Проверяем созданный репозиторий

Проверяем remote:

```bash
git remote -v
```

Ожидаем:

```text
origin  git@github.com:FAEBFE/01-simple.git (fetch)
origin  git@github.com:FAEBFE/01-simple.git (push)
```

Проверяем локальное состояние:

```bash
git status
```

Проверяем репозиторий на GitHub:

```bash
gh repo view FAEBFE/01-simple
```

Проверяем коммиты, опубликованные на GitHub:

```bash
gh api repos/FAEBFE/01-simple/commits \
  --jq '.[] | "\(.sha[0:7])  \(.commit.author.name)  \(.commit.author.email)"'
```

В результате должен быть наш первоначальный коммит с правильными данными автора.

## 10. Проверяем авторство коммита

Локально:

```bash
git log -1 --format=fuller
```

На GitHub:

```bash
gh api repos/FAEBFE/01-simple/commits \
  --jq '.[] | "\(.sha[0:7])  \(.commit.author.name)  \(.commit.author.email)"'
```

Проверяем, что GitHub связал коммит с нужным аккаунтом.

Таким образом проверяются сразу две вещи:

```text
push прошёл
        ↓
репозиторий создан и содержимое опубликовано

author/email определились правильно
        ↓
коммит принадлежит нужному GitHub-аккаунту
```

## 11. Если это был тестовый репозиторий — удаляем его

После завершения проверки тестовый репозиторий можно удалить:

```bash
gh repo delete FAEBFE/01-simple
```

CLI попросит подтвердить:

```text
? Type FAEBFE/01-simple to confirm deletion:
```

Вводим:

```text
FAEBFE/01-simple
```

Если `gh` сообщает:

```text
This API operation needs the "delete_repo" scope.
```

добавляем разрешение:

```bash
gh auth refresh -h github.com -s delete_repo
```

GitHub покажет одноразовый код **в терминале** и адрес:

```text
https://github.com/login/device
```

Открываем адрес в браузере и вводим код.

Код не приходит по электронной почте.

После получения разрешения повторяем:

```bash
gh repo delete FAEBFE/01-simple
```

Проверяем удаление:

```bash
gh repo view FAEBFE/01-simple
```

Репозиторий должен быть не найден.

Важно: это удаляет **репозиторий на GitHub**, но не локальный `.git`. Если каталог использовался исключительно для эксперимента и локальная история больше не нужна, её можно удалить отдельно:

```bash
rm -rf .git
```

Это уже операция с локальным тестовым каталогом, а не с GitHub.

## Итог

После настройки создание готового GitHub-репозитория из существующего локального проекта сводится к одной команде.

Публичный:

```bash
gh repo create OWNER/REPOSITORY --public --source=. --remote=origin --push
```

Приватный:

```bash
gh repo create OWNER/REPOSITORY --private --source=. --remote=origin --push
```

`--push` здесь принципиален: без него GitHub-репозиторий создаётся, но локальная Git-история в него не отправляется.

Перед выполнением команды нужно убедиться, что локальный Git-репозиторий уже содержит именно те файлы и коммиты, которые должны попасть на GitHub.