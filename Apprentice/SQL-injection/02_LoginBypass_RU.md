# SQL Injection - Login Bypass

**Платформа:** PortSwigger Web Security Academy  
**Категория:** Web / SQL Injection  
**Уровень:** Apprentice  
**Дата:** 27.10.2026  
**Статус:** ✅ Solved  
**Ссылка:** https://0aeb007c03c2a2f782ac83f000fa0000.web-security-academy.net/

---

## 1. Objective
Необходимо залогиниться в 'administrator'

## 2. Reconnaissance
- Зашел на сайт и перешел во вкладку 'My account'

## 3. Exploitation

1. В поле с логином ввел "administrator'--", а в поле с паролем ввел случаные символы

**Payloads:**
```text
Username: administrator'--
Password: 123
```

**Почему это сработало:**  
Зная как работает SQL-код, проверяющий авторизацию на сайте, закомментировал поле пароля и оно не проверялось

## 4. Result
Получен доступ к administrator, лабораторная работа решена
## 5. Key Takeaway

**Что я узнал:**  
Можно внедрять SQL-код непосредственно через пользовательский ввод
