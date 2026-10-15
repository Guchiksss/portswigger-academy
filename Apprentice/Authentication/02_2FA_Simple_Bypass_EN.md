# 2FA Simple Bypass

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / Authentication  
**Level:** Apprentice  
**Date:** 16.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0abe00ed0306b44b81d125de004d0068.web-security-academy.net/

---

## 1. Objective
Gain access to Carlos's account by bypassing the two-factor authentication (2FA) mechanism.

## 2. Reconnaissance
- The following credentials were provided: `wiener:peter`, `carlos:montoya`.
- Logged in with `wiener:peter`, the site required entering a verification code sent to the email.
- After entering the code, I was redirected to the account page.

## 3. Exploitation
- Initiated login with the victim's credentials: `carlos:montoya`.
- On the page requiring the verification code, I manually changed the URL in the address bar.
- Access to Carlos's account was obtained without entering the 2FA code.

**Payload:**
```text
URL manipulation: /login2 -> /my-account
```

**Why it worked:**  
The site's logic flaw is that after entering the correct login and password, the site considers the user authorized. However, it still requires two-factor authentication, which can be easily bypassed by directly navigating to a protected endpoint.

## 4. Result
Access to the account was obtained, the lab is solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
In this case, 2FA only protects the initial log in page, but does not protect the rest of the authenticated application.

**How to protect:**
1. The server must check the "2FA passed" flag in the session on every request to protected pages, not just during the initial login.
2. After entering the password, the session must transition to a "pending 2FA" state, and only after a successful code entry — to a "fully authenticated" state.
