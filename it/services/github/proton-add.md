# GitHub не присылает письмо на Proton Mail

При добавлении Proton Mail в аккаунт GitHub столкнулся с проблемой: **письмо с кодом подтверждения от GitHub не приходило**.

При этом другой Proton-ящик нормально получал письма от GitHub.

## Что помогло

В Proton Mail нужно было **добавить адрес электронной почты для восстановления (recovery email)**.

После добавления recovery email повторно запросил письмо подтверждения в GitHub:

**GitHub → Settings → Emails → Resend verification email**

После этого письмо с кодом пришло на Proton Mail.

### Последовательность

1. Открыть **Proton Mail → Settings**.
    
2. Добавить **Recovery email**.
    
3. Подтвердить адрес восстановления.
    
4. Вернуться в GitHub.
    
5. Открыть **Settings → Emails**.
    
6. Нажать **Resend verification email**.
    
7. Получить код на Proton Mail и подтвердить адрес.
    

Таким образом, если GitHub не присылает письмо подтверждения на Proton Mail, но на другой почтовый ящик GitHub письма приходят, стоит проверить наличие **адреса электронной почты для восстановления Proton Account**.

Официальная документация Proton:

[Recovery methods — Proton Account](https://proton.me/support/recovery-methods?utm_source=chatgpt.com)

Официальная документация GitHub:

[Verifying your email address — GitHub Docs](https://docs.github.com/en/account-and-profile/how-tos/email-preferences/verifying-your-email-address?utm_source=chatgpt.com)