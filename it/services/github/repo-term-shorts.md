# Создание GitHub-репозитория и подключение через терминал

Экспериментальный минималистичный вариант: основная настройка выполняется блоками команд, которые можно вставлять в терминал целиком.

## Цель

Создать локальный Git-репозиторий, создать для него публичный репозиторий на GitHub и сразу отправить первый коммит.

При этом Git должен использовать правильные имя и email, а SSH — правильный GitHub-аккаунт.

## 1. Настройка Git и проверка SSH

Если GitHub CLI уже авторизован, весь блок можно выполнить целиком:

```bash
git config --global user.name "FAEBFE"
git config --global user.email "336804604+FAEBFE@users.noreply.github.com"

git config --global --unset core.sshCommand 2>/dev/null || true

gh api user --jq '{login,id}'

ssh -T git@github.com
```

В результате:

- `gh api user` должен показать аккаунт `FAEBFE`;
    
- `ssh -T` должен подтвердить аутентификацию этого же аккаунта.
    

`core.sshCommand` здесь удаляется специально: если он был задан глобально, Git мог использовать другой SSH-ключ вместо обычного `~/.ssh/config`.

## 2. Создание репозитория

Создаём каталог, инициализируем Git, создаём первый коммит и одновременно создаём репозиторий на GitHub:

```bash
cd ~/mdbd/pub/01-simple

git init

printf '# 01-simple\n' > README.md

git add README.md
git commit -m "Initial commit"

gh repo create FAEBFE/01-simple \
  --public \
  --source=. \
  --remote=origin \
  --push
```

Здесь нет отдельной команды для создания `main`: используется текущее имя ветки, созданное `git init`.

## 3. Проверка результата

Основные проверки можно выполнить одним блоком:

```bash
printf '\n--- Git ---\n'
git status
git remote -v
git log -1 --format=fuller

printf '\n--- GitHub ---\n'
gh repo view FAEBFE/01-simple

printf '\n--- SSH ---\n'
ssh -T git@github.com
```

### Что смотреть

**`git status`**

Должно быть:

```text
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

Если Git создал другую основную ветку, название в выводе будет соответствующим.

**`git remote -v`**

Должен быть SSH-адрес:

```text
git@github.com:FAEBFE/01-simple.git
```

**`git log -1 --format=fuller`**

Проверяем:

- `Author`;
    
- `Commit`;
    
- имя `FAEBFE`;
    
- правильный GitHub noreply email.
    

**`gh repo view`**

Проверяем, что репозиторий действительно создан на GitHub и доступен.

**`ssh -T`**

Ожидаем подтверждение:

```text
Hi FAEBFE! You've successfully authenticated, but GitHub does not provide shell access.
```

## 4. Проверка атрибуции коммита

GitHub определяет автора коммита по данным, записанным непосредственно в коммит, в частности по email.

Проверить это можно через API:

```bash
gh api repos/FAEBFE/01-simple/commits/$(git rev-parse HEAD) \
  --jq '{
    author: .commit.author,
    committer: .commit.committer,
    github_author: .author.login,
    github_committer: .committer.login
  }'
```

Здесь проверяем сразу три вещи:

1. какой `name` записан в коммит;
    
2. какой `email` записан в коммит;
    
3. какой аккаунт GitHub распознал как автора и коммиттера.
    

## Что в итоге связано между собой

```text
Git
│
├── user.name
├── user.email
│       │
│       └── записываются в commit
│
└── SSH
        │
        └── определяет GitHub-аккаунт
            для push / pull
```

Поэтому это две разные настройки:

- **Git identity** отвечает за данные внутри коммита и его атрибуцию;
    
- **SSH authentication** отвечает за аккаунт, через который GitHub принимает операции с репозиторием.
    

Изменение `user.email` не исправляет уже созданные коммиты. Оно влияет только на новые коммиты.

## Полезные команды

Текущая Git identity:

```bash
git config --global --get user.name
git config --global --get user.email
```

Текущий SSH-конфиг:

```bash
ssh -G github.com | grep -E '^(user|identityfile|identitiesonly) '
```

Проверка GitHub CLI:

```bash
gh api user --jq '{login,id}'
```

Проверка SSH:

```bash
ssh -T git@github.com
```

Проверка remote:

```bash
git remote -v
```

Проверка последнего коммита:

```bash
git log -1 --format=fuller
```