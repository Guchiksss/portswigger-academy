# OS Command Injection, Simple Case

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / OS Command Injection  
**Level:** Apprentice  
**Date:** 27.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0a9d00b604d3c77c805be912002a00b6.web-security-academy.net/

---

## 1. Objective
The site has an OS command injection vulnerability in the product stock check component; the goal is to determine the username

## 2. Reconnaissance
- Accessed the site and found the required vulnerable component in the first available product

## 3. Exploitation
1. Intercepted the stock check request using Burp Interceptor
2. In this request, saw the line with the parameters 'productId' and 'storeId'
3. Added to the value of the 'productID' parameter a linux-shell command that determines the user 'whoami' using the separators '|'
4. In the response, my command was executed and the server returned the username — lab solved

**Payloads:**
```text
productId=4|whoami|&storeId=1
```

**Why it worked:**  
The shell split the string by the | separator and executed 'whoami' as a separate command.

## 4. Result
Username - 'peter-NaWIuq', lab solved

## 5. Key Takeaway & Remediation

**What I learned:**  
If an application substitutes parameter values into a shell command without validation, an attacker can execute arbitrary OS commands through separators

**How to fix it:**
1. Strict validation (whitelist)
