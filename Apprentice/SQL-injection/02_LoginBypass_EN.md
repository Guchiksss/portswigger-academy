# SQL Injection - Login Bypass

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / SQL Injection  
**Level:** Apprentice  
**Date:** 27.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0aeb007c03c2a2f782ac83f000fa0000.web-security-academy.net/

---

## 1. Objective
Need to log in as 'administrator'

## 2. Reconnaissance
- Accessed the site and navigated to the 'My account' tab

## 3. Exploitation

1. Entered "administrator'--" in the login field, and entered random characters in the password field

**Payloads:**
```text
Username: administrator'--
Password: 123
```

**Why it worked:**  
Knowing how the SQL query that checks authorization on the site works, I commented out the password field and it was not checked

## 4. Result
Access to administrator obtained, lab solved

## 5. Key Takeaway

**What I learned:**  
You can inject SQL code directly through user input
