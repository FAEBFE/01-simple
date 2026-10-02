# GitHub: создание и первичная настройка аккаунта

## Цель

Создать личный аккаунт GitHub и сразу привести его к минимально необходимому рабочему состоянию:

- основной email — Proton Mail;
    
- резервный email — Gmail;
    
- выбран username;
    
- основной email подтверждён;
    
- email скрыт от публичного отображения;
    
- включена двухфакторная аутентификация;
    
- создан и сохранён passkey;
    
- сохранены recovery codes.

[[proton-add|GitHub не присылает письмо на Proton Mail]]

---

## 1. Подготовить основной email

Рекомендуется создать отдельный адрес Proton Mail:

[Proton Mail — регистрация](https://account.proton.me/signup)

Использовать этот адрес как основной email GitHub.

Для резервного адреса использовать Gmail:

[Gmail — регистрация](https://accounts.google.com/signup)

---

## 2. Создать аккаунт GitHub

Открыть:

[GitHub — создание аккаунта](https://github.com/signup)

На первом шаге указать:

```text
Email → Proton Mail
```

Продолжить регистрацию.

---

## 3. Выбрать username

На следующем шаге указать:

```text
Username → желаемое имя аккаунта
```

Username обязателен.

Он используется, в частности:

**в адресе профиля:**

```text
https://github.com/USERNAME
```

**в SSH-адресах репозиториев:**

```text
git@github.com:USERNAME/REPOSITORY.git
```

**в стандартном адресе пользовательского сайта GitHub Pages:**

```text
https://USERNAME.github.io
```

Подробнее о создании сайта GitHub Pages:

GitHub Pages — пользовательский сайт

Поэтому username лучше выбрать сразу осознанно.

**Username и отображаемое имя — разные вещи.**

После регистрации отображаемое имя можно изменить отдельно в профиле.

---

## 4. Задать уникальный пароль

Создать уникальный пароль.

Не использовать пароль от Proton Mail, Gmail или других сервисов.

Сохранить его, например, в Bitwarden.

GitHub рекомендует использовать уникальный пароль длиной не менее 15 символов.

[GitHub — требования к паролям](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-strong-password)

---

## 5. Подтвердить основной email

Открыть Proton Mail и найти письмо от GitHub.

В письме получить код подтверждения и ввести его на странице GitHub.

Проверить состояние:

[GitHub → Email settings](https://github.com/settings/emails)

Основной адрес должен быть подтверждён.

---

## 6. Добавить резервный email

Открыть:

[GitHub → Email settings](https://github.com/settings/emails)

В разделе **Add email address** добавить Gmail.

Подтвердить адрес кодом из письма GitHub.

Итог:

```text
Proton Mail — основной
Gmail      — резервный
```

---

## 7. Сделать Proton основным

На странице:

[GitHub → Email settings](https://github.com/settings/emails)

В разделе **Primary email address** выбрать Proton Mail.

Сохранить.

---

## 8. Скрыть email

На той же странице:

[GitHub → Email settings](https://github.com/settings/emails)

Включить:

```text
Keep my email addresses private
```

Проверить также:

```text
Block command line pushes that expose my email
```

Оставить эту настройку включённой.

Для GitHub-коммитов использовать предоставляемый GitHub `noreply`-адрес.

---

## 9. Проверить отображаемое имя

Открыть:

[GitHub → Profile settings](https://github.com/settings/profile)

Проверить поле **Name**.

Username менять здесь нельзя.

Если отображаемое имя не нужно — оставить поле **Name** пустым.

Итоговое различие:

```text
Username → постоянный идентификатор аккаунта
Name     → отображаемое имя профиля
```

---

## 10. Включить двухфакторную аутентификацию

Открыть:

[GitHub → Password and authentication](https://github.com/settings/security)

В разделе **Two-factor authentication** выбрать:

```text
Enable two-factor authentication
```

Настроить TOTP-аутентификацию.

После настройки проверить, что 2FA включена.

---

## 11. Создать passkey

На той же странице:

[GitHub → Password and authentication](https://github.com/settings/security)

В разделе **Passkeys** выбрать добавление нового passkey.

Создать passkey через Bitwarden.

Сохранить passkey в Bitwarden.

После создания проверить, что он появился в списке passkeys GitHub.

---

## 12. Сохранить recovery codes

При настройке 2FA GitHub выдаст recovery codes.

Выбрать:

```text
Download recovery codes
```

Сохранить их в безопасном месте.

Не помещать recovery codes в Git-репозитории.

---

## 13. Проверить итоговое состояние

Открыть:

[GitHub → Email settings](https://github.com/settings/emails)

Проверить:

```text
[✓] Proton Mail добавлен
[✓] Proton Mail подтверждён
[✓] Proton Mail — Primary
[✓] Gmail добавлен
[✓] Gmail подтверждён
[✓] Keep my email addresses private
[✓] Block command line pushes that expose my email
```

Открыть:

[GitHub → Profile settings](https://github.com/settings/profile)

Проверить:

```text
[✓] Username выбран
[✓] Name настроен или оставлен пустым
```

Открыть:

[GitHub → Password and authentication](https://github.com/settings/security)

Проверить:

```text
[✓] 2FA включена
[✓] Passkey создан
[✓] Passkey сохранён в Bitwarden
[✓] Recovery codes сохранены
```

---

## Результат

Получен личный GitHub-аккаунт:

```text
GitHub
├── username
├── Proton Mail — основной
├── Gmail — резервный
├── email — скрыт
├── 2FA — включена
├── passkey — создан и сохранён в Bitwarden
└── recovery codes — сохранены
```

Следующий шаг:

**GitHub: настройка SSH-доступа**