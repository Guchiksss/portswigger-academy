# Unprotected Admin Functionality (Continued)

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / Access Control  
**Level:** Apprentice  
**Date:** 13.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0ac1004c041404c483ce0ad700fb007c.web-security-academy.net/

---

## 1. Objective
Delete the user `carlos` by finding and exploiting an unprotected administrative panel, whose URL is hidden on the page.

## 2. Reconnaissance
- Opened the main page of the website.
- Opened DevTools to analyze the client-side code.

## 3. Exploitation
1. Inspected the page source code and found an inline JavaScript (inline script).
2. Discovered the code line that dynamically creates a link to the admin panel: `adminPanelTag.setAttribute('href', '/admin-xxxxxx');`
3. Copied this random URL (for example, `/admin-fbzb19`).
4. Navigated to the found URL in the browser.
5. Deleted the user `carlos` from the management panel.

**Payload/URL:**

```text
/admin-fbzb19
```

**Why it worked:**  
The developers used "security by obscurity" — they tried to hide the admin panel behind an unpredictable URL. But this URL was stored in client-side code (inline JavaScript), which is visible to every user. Anyone can open the page source and find it.

## 4. Result
Access to the administrative panel was obtained, `carlos` was deleted, and the flag was received.

## 5. Key Takeaway & Remediation

**What I learned:**  
Security by obscurity is the same as leaving the door to your house open. You should not hide an unpredictable URL in client-side code, because all client-side code is visible to the user.

**How to fix it:**
1. Require a specific access level for the URL, or allow requests only from a whitelist of IP addresses.
2. Never hide sensitive data in client-side code.
