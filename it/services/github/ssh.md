# GitHub: настройка SSH-доступа

## Цель

Настроить SSH-доступ к GitHub на Linux, чтобы Git работал с репозиториями через SSH без ввода логина и пароля GitHub.

Итоговая проверка:

```bash
ssh -T git@github.com
```

В ответ должно появиться:

```text
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

GitHub прямо указывает эту команду как проверку успешной SSH-аутентификации.

---

## 1. Проверить, установлен ли SSH

```bash
ssh -V
```

Если команда существует, получаем версию OpenSSH.

---

## 2. Проверить существующие SSH-ключи

```bash
ls -la ~/.ssh
```

Ищем пары файлов вида:

```text
id_ed25519
id_ed25519.pub
```

или:

```text
id_rsa
id_rsa.pub
```

Если подходящий ключ уже есть — **не создавать новый без необходимости**.

GitHub рекомендует сначала проверить существующие ключи, а уже затем создавать новый.

---

## 3. Проверить, работает ли уже доступ к GitHub

```bash
ssh -T git@github.com
```

Если получено:

```text
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

то SSH-доступ уже настроен.

**На этом остановиться.**

---

## 4. Если ключа нет — создать Ed25519

```bash
ssh-keygen -t ed25519 -C "EMAIL"
```

Вместо `EMAIL` указать адрес, связанный с GitHub.

На вопрос:

```text
Enter file in which to save the key (/home/USER/.ssh/id_ed25519):
```

нажать `Enter`.

Затем задать парольную фразу ключа.

GitHub рекомендует Ed25519 для нового ключа.

После создания должны появиться:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Закрытый ключ `id_ed25519` **никому не передавать**.

---

## 5. Запустить ssh-agent

```bash
eval "$(ssh-agent -s)"
```

Проверить:

```bash
ssh-add -l
```

Если ключ ещё не добавлен:

```bash
ssh-add ~/.ssh/id_ed25519
```

GitHub использует `ssh-agent` для управления ключами и их парольными фразами.

---

## 6. Скопировать публичный ключ

```bash
cat ~/.ssh/id_ed25519.pub
```

Скопировать **всю одну строку**, начинающуюся примерно так:

```text
ssh-ed25519 AAAA...
```

Не копировать содержимое закрытого ключа:

```text
~/.ssh/id_ed25519
```

---

## 7. Добавить ключ в GitHub

Открыть настройки SSH-ключей GitHub:

[GitHub → Settings → SSH and GPG keys](https://github.com/settings/keys?utm_source=chatgpt.com)

Выбрать:

**New SSH key**

Заполнить:

- **Title** — понятное имя компьютера, например `Arch`
    
- **Key type** — `Authentication Key`
    
- **Key** — содержимое `~/.ssh/id_ed25519.pub`
    

Сохранить ключ.

GitHub требует добавить публичный ключ в аккаунт, прежде чем использовать его для SSH-аутентификации Git-операций.

---

## 8. Проверить SSH-доступ

```bash
ssh -T git@github.com
```

При первом подключении SSH может спросить:

```text
Are you sure you want to continue connecting (yes/no)?
```

Проверить fingerprint GitHub и подтвердить:

```text
yes
```

При успешной аутентификации GitHub сообщает:

```text
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

Это нормальный результат: GitHub не предоставляет интерактивный shell через этот SSH-адрес.

---

## 9. Проверить Git remote

В каталоге репозитория:

```bash
git remote -v
```

Если используется HTTPS:

```text
origin  https://github.com/USER/REPOSITORY.git (fetch)
origin  https://github.com/USER/REPOSITORY.git (push)
```

перевести `origin` на SSH:

```bash
git remote set-url origin git@github.com:USER/REPOSITORY.git
```

Проверить:

```bash
git remote -v
```

Должно быть:

```text
origin  git@github.com:USER/REPOSITORY.git (fetch)
origin  git@github.com:USER/REPOSITORY.git (push)
```

---

## 10. Проверить работу Git

```bash
git fetch
```

или:

```bash
git pull
```

Если операция выполняется без запроса пароля GitHub, SSH-доступ работает.

---

## Итог

Должны выполняться:

```bash
ssh -T git@github.com
```

и в репозитории:

```bash
git remote -v
```

с SSH-адресом вида:

```text
git@github.com:USER/REPOSITORY.git
```

После этого Git использует SSH для доступа к GitHub.