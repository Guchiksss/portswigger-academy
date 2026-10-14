# User ID Controlled by Request Parameter with Password Disclosure

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / Access Control  
**Level:** Apprentice  
**Date:** 14.10.2026  
**Status:** ✅ Solved  
**Lab Link:** [Insert your lab link here]

---

## 1. Objective
Gain access to the admin panel and delete the user `carlos` by exploiting an IDOR vulnerability to disclose the administrator's password.

## 2. Reconnaissance
- Checked `robots.txt` — not present.
- Checked `/admin` — the page exists, but access is restricted. Noticed the word "administrator" mentioned on the access denied page.
- Logged in with the provided credentials `wiener:peter`.
- Discovered the `userid` parameter in the URL of the personal account page.
- Gained access to `carlos`'s account by changing the `userid` and found his password exposed in a masked field.
- Attempted to use `carlos`'s credentials to access `/admin` — failed.

## 3. Exploitation
1. Navigated to the personal account page and identified the `userid` parameter in the URL.
2. Changed `userid=wiener` to `userid=administrator`.
3. Successfully gained access to the administrator's account page.
4. Used browser DevTools (Inspect Element) to reveal and extract the administrator's password from the masked `type="password"` input field.
5. Logged out and logged back in as the administrator using the extracted password.
6. Gained full access to the `/admin` panel.
7. Deleted the user `carlos`.

**Payload:**
```text
userid=wiener → userid=administrator
```

**Why it worked:**  
The application has an IDOR vulnerability that allows changing the `userid` parameter in the URL to access other users' accounts without proper backend authorization checks. Additionally, it improperly exposes passwords on the client side.

## 4. Result
Obtained the administrator's password, gained access to the admin panel, and successfully deleted the user `carlos`. Lab solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
A horizontal vulnerability (IDOR) can become a stepping stone for vertical privilege escalation if the attacker manages to access an administrator's account. Furthermore, displaying passwords in masked fields on the client side creates a false sense of security and is a critical design flaw.

**How to fix it:**
1. Implement server-side validation: verify access rights to accounts on the backend for every request, rather than trusting the `userid` parameter provided in the URL.
2. Never display user passwords on the personal account page, even in a masked form. Passwords must be stored exclusively in a hashed format in the database and never sent to the client side.
