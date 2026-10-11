# traversal vulnerability: File path traversal, simple case 

**Платформа:** PortSwigger Web Security Academy  
**Категория:** [Web / File Path Traversal]  
**Уровень:** Apprentice 
**Дата:** [12.10.2026]  
**Статус:** ✅ Solved
**Ссылка:** https://0ab300e503417255807880e800600029.web-security-academy.net/

---

## 1. Objective
Получить содержимое файла /etc/passwd

## 2. Reconnaissance
- Попал на сайт онлайн магазина, увидел множество товаров
- У каждого товара свой productId
- Надо использовать уязвимость обхода пути

## 3. Exploitation
1. Открыл burpsuite
2. В proxy в браузере открыл эту страницу товара с сайта (intercept on)
3. Отправил запрос с изображением товара в repeater
4. В repeater изменил GET с filename=21 на ../../../etc/passwd

**Payload**
```text
../../../etc/passwd

 **Почему это сработало:**
  Последовательность ../ возвращает на уровень выше вплоть до корневой папки где находится нужный файл

## 4. Result
Сервер вернул содержимое файла /etc/passwd в ответе. Лабораторная работа успешно пройдена, флаг получен.
