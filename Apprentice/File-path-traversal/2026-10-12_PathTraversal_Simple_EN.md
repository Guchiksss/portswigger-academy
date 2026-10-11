# File Path Traversal: Simple Case

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / File Path Traversal  
**Level:** Apprentice  
**Date:** October 12, 2026  
**Status:** ✅ Solved  
**Lab Link:** https://0ab300e503417255807880e800600029.web-security-academy.net/

---

## 1. Objective
Retrieve the contents of the `/etc/passwd` file by exploiting a file path traversal vulnerability in the product image loading functionality.

## 2. Reconnaissance
- Accessed an online store website and saw multiple products.
- Each product has a unique `productId`.
- Product images are loaded via a `filename` parameter in the URL.
- Need to exploit a path traversal vulnerability to read system files.

## 3. Exploitation
1. Opened Burp Suite.
2. Enabled proxy intercept in the browser and navigated to a product page.
3. Sent the image request to Burp Repeater.
4. Modified the GET parameter from `filename=21` to `../../../etc/passwd`.

**Payload:** `../../../etc/passwd`

**Why it worked:**  
The `../` sequence navigates up one directory level in the file system. Three such sequences allow escaping from the product images directory to reach the root filesystem `/`, where the `/etc/passwd` file is located. The server did not validate user input.

## 4. Result
The server returned the contents of the `/etc/passwd` file in the response. Lab successfully completed, flag obtained.
