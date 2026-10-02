# GitHub: создание репозитория через веб-интерфейс

## Цель

Создать новый GitHub-репозиторий и задать его основные параметры.

## 1. Открыть создание репозитория

[GitHub — New repository](https://github.com/new?utm_source=chatgpt.com)

## 2. Выбрать владельца

В `Owner` выбрать:

```text
FAEBFE
```

## 3. Задать имя

В `Repository name` указать имя репозитория.

Имя используется в адресе:

```text
https://github.com/FAEBFE/REPOSITORY
```

Если репозиторий предназначен для GitHub Pages, проверить требования к имени:

GitHub Pages — пользовательский сайт

## 4. Добавить описание

В `Description` указать краткое назначение репозитория.

Поле необязательно.

## 5. Выбрать видимость

Выбрать:

```text
Public
```

или:

```text
Private
```

## 6. Определить начальное содержимое

### Если репозиторий создаётся полностью через GitHub

Можно включить:

```text
Add a README file
```

При необходимости выбрать `.gitignore` и `License`.

### Если существует локальная Git-папка

Например:

```text
~/mdbd/pub/FAEBFE.github.io
```

и в ней уже будет создан Git-репозиторий, README и первый commit, **не добавлять** README, `.gitignore` и License через веб-интерфейс.

Это позволяет избежать двух независимых начальных историй Git.

## 7. Создать репозиторий

Нажать:

```text
Create repository
```

## 8. Проверить результат

Проверить:

- Owner;
    
- Repository name;
    
- Visibility;
    
- Default branch;
    
- Files.
    

Для аккаунта `FAEBFE` репозиторий имеет вид:

```text
FAEBFE/REPOSITORY
```

## Полезные команды

После создания репозитория его можно открыть:

```bash
gh repo view FAEBFE/REPOSITORY --web
```

Проверить текущую авторизацию:

```bash
gh auth status
```

Проверить аккаунт, от имени которого работает `gh`:

```bash
gh api user --jq '.login'
```

Получить SSH-адрес репозитория:

```bash
gh repo view FAEBFE/REPOSITORY --json sshUrl -q .sshUrl
```

## Результат

Создан GitHub-репозиторий с заданными:

- owner;
    
- name;
    
- visibility;
    
- initial files;
    
- default branch.
    

Следующий материал:

GitHub: создание и подключение репозитория из терминала