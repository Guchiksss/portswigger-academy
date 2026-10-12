# Unprotected Admin Functionality

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / Access Control  
**Level:** Apprentice  
**Date:** October 13, 2026  
**Status:** ✅ Solved  
**Lab Link:** https://0ade00fd037bfa1f8ae72bef009a00be.web-security-academy.net/

---

## 1. Objective
Delete the user `carlos` by finding and exploiting an unprotected administrative panel.

## 2. Reconnaissance
- Opened the main page of the website
- Checked for `robots.txt` - if present, analyze it

## 3. Exploitation
1. Found the address of the unprotected admin panel in `robots.txt`
2. Accessed the unprotected admin panel
3. Deleted the user "carlos"

**Payload/URL:**
```text
/robots.txt // /administrator-panel
```
## 4. Result
After navigating to `/administrator-panel`, a user management panel opened. Deleted the user `carlos`, after which the lab was successfully completed and the flag was obtained.
