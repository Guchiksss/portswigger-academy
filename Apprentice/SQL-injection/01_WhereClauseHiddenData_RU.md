# SQL Injection - WHERE Clause to Retrieve Hidden Data

**Платформа:** PortSwigger Web Security Academy  
**Категория:** Web / SQL Injection  
**Уровень:** Apprentice  
**Дата:** 27.10.2026  
**Статус:** ✅ Solved  
**Ссылка:** https://0ad2005704a00be080518a4100da0024.web-security-academy.net/

---

## 1. Objective
Дан сайт с SQLi уязвимостью и пояснение как работает SQL с категориями на сайте (SELECT * FROM products WHERE category = 'Gifts' AND released = 1), необходимо увидеть скрытые товары (1 или более)

## 2. Reconnaissance
- Зашел на сайт и увидел раздел с выбором категорий

## 3. Exploitation

1. Перешел в категорию 'Accessories'
2. Добавил в URL "'+OR+1=1--"
3. Сайт показал все товары 


**Payloads:**
```text
Исходный URL: https://0ad2005704a00be080518a4100da0024.web-security-academy.net/filter?category=Accessories
Инъекция: '+OR+1=1--
Измененный URL: 'https://0ad2005704a00be080518a4100da0024.web-security-academy.net/filter?category=Accessories%27+OR+1=1--'
```

**Почему это сработало:**  
Зная как выглядел исходный SQL-код, добавил кавычку после значение параметра 'category=Accessories' и саму инъекцию (OR 1=1 - для того, чтобы инъекция сработала в любом случае, '--' - для того, чтобы параметр 'released=1' не был учтен (сделал его комментарием))

## 4. Result
Все товары были показаны, включая скрытые, лабораторная работа решена

## 5. Key Takeaway & Remediation

**Что я узнал:**  
У незащищенных сайтов можно получить доступ к БД, через GET-запрос, просто изменив URL, SQL-инъекция позволяет вставить произвольный SQL-код в запрос приложения, обойдя логику фильтрации
