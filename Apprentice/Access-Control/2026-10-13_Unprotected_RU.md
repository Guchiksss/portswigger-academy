# Unprotected Admin Functionality

**Платформа:** PortSwigger Web Security Academy  
**Категория:** Web / Access Control  
**Уровень:** Apprentice  
**Дата:** 13.10.2026
**Статус:** ✅ Solved  
**Ссылка:** https://0ade00fd037bfa1f8ae72bef009a00be.web-security-academy.net/

---

## 1. Objective
Удалить пользователя `carlos`, найдя и использовав незащищенную административную панель.

## 2. Reconnaissance
- Открыл главную страницу сайта
- Проверил наличие robots.txt - если есть - изучаю

## 3. Exploitation
1. В robots.txt нашел адрес незащищенной админки
2. Зашел на незащищенную админку
3. Удалил пользователя "carlos"


**Payload/URL:** 
```text
/robots.txt // /administrator-panel
```

## 4. Result
После перехода на `/administrator-panel` открылась панель управления пользователями. Удалил пользователя `carlos`, после чего лабораторная работа была успешно пройдена и флаг получен.
