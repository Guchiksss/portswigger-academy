# User role controlled by request parameter

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / Access Control  
**Level:** Apprentice  
**Date:** 14.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0af000ae046dd2e480acbcb2005700a6.web-security-academy.net/

---

## 1. Objective
Delete the user 'carlos' through the admin panel, which checks the user's role via Cookie.

## 2. Reconnaissance
- Checked robots.txt - not present
- Checked /admin - actually exists, but access is restricted
- Can log in using the provided credentials wiener:peter

## 3. Exploitation
1. Logged into the website as wiener:peter
2. Used Burp Suite Proxy with Intercept On to access /admin
3. In the intercepted request in the Request panel, changed the Cookie parameter from admin=False to admin=True and gained access to the admin panel
4. After gaining access to the panel, deleted the user carlos using the same method - by changing the Cookie parameter in the request

**Payload:**
```text
Cookie: Admin=false → Cookie: Admin=true
```

**Why it worked:**  
The developers left the parameter checking the user's role in the Cookie of the HTTP request, which is easy to intercept and modify.

## 4. Result
Access to the admin panel obtained, user deleted, lab solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
Storing the user role flag in a Cookie is a bad practice, as Cookies are easily forged on the client side.

**How to fix it:**
1. Implement server-side validation: for each request to /admin, the server should check the user's role in its database or session, rather than trusting the Cookie value
2. Use secure authentication mechanisms (e.g., server-side sessions) where the user's role is stored and verified exclusively on the server
