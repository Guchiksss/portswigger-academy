# Basic SSRF against the Local Server

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / SSRF  
**Level:** Apprentice  
**Date:** 16.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0a450089046082bf839eeb0300020050.web-security-academy.net/

---

## 1. Objective
Gain access to the internal admin panel on localhost and delete the user `carlos`.

## 2. Reconnaissance
- Accessed the online store and opened the page of the first available product.
- Discovered the 'Check stock' button, which sends a request to an internal back-end system.
- Intercepted this request using Burp Suite (Intercept) and replaced the value of the `stockedAPI` parameter with `http://localhost/admin`.
- Instead of the stock check result, the response returned the HTML of the admin panel containing the functionality to delete the user 'carlos'.
- When attempting to delete the user through the interface, received an authorization error: the request originated from my browser, not from the server itself.
- Intercepted and analyzed the deletion request, discovering the back-end endpoint responsible for this action.

## 3. Exploitation
1. In the intercepted request, replaced the value of the `stockedAPI` parameter with the full path to the deletion endpoint: `http://localhost/admin/delete?username=carlos`.
2. Sent the modified request. The server executed the operation on its own behalf, the user was deleted, and the lab was solved.

**Payload:**
```text
http://localhost/admin/delete?username=carlos
```

**Why it worked:**  
The application implicitly trusts requests originating from localhost. The server itself made the request to the internal admin panel, which does not require authentication for local connections, so the deletion operation was executed with administrator privileges.

## 4. Result
Gained access to the internal admin panel via SSRF and deleted the user `carlos`. Lab solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
SSRF allows an attacker to force the server into making requests to internal resources (localhost, internal networks) that are inaccessible from the outside. Internal interfaces often implicitly trust local requests and do not require authentication. When exploiting SSRF, it is more effective to call API endpoints directly rather than interactively using the web interface, since browser cookies are not forwarded in the server's internal requests.

**How to fix it:**
1. Validate and whitelist the URLs the application is allowed to access: block `localhost`, `127.0.0.1`, and internal IP ranges (10.x.x.x, 192.168.x.x).
2. Require authentication and authorization on the admin panel and internal APIs even for requests originating from localhost.
