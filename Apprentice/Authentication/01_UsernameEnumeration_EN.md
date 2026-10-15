# Username Enumeration via Different Responses

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / Authentication  
**Level:** Apprentice  
**Date:** 16.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0a37009904496be280b4c13a009500f7.web-security-academy.net/

---

## 1. Objective
The objective is to brute-force a valid username and its corresponding password using the provided wordlists to gain unauthorized access to the application.

## 2. Reconnaissance
- Accessed the blog login page; the objective was clear without extensive reconnaissance. Noted that PortSwigger provided two specific wordlists for this task: `Candidate usernames` and `Candidate passwords`.

## 3. Exploitation

### Step 1: Username Enumeration
- Intercepted the `/login` POST request in Burp Suite with dummy credentials and sent it to Burp Intruder.
- Placed the `§` payload markers exclusively around the `username` parameter.
- Loaded the `Candidate usernames` wordlist into the Payload configuration and launched the attack.
- Upon completion, noticed that one response had a different length compared to the rest (3352 → 3354).
- Identified this anomalous response as the valid username (`app`).

### Step 2: Password Brute-force
- Removed the payload markers from the `username` parameter (setting it to the discovered valid username) and placed them around the `password` parameter.
- Updated the Payload configuration by loading the `Candidate passwords` wordlist and launched the second attack.
- Upon completion, identified an anomalous response length for one password attempt (e.g., 185 bytes vs. ~3000+ for failed attempts).
- Extracted the valid credentials and used them to log in on the target website.

**Payloads:**
```text
Attack 1 (username): Sniper + Candidate usernames wordlist
Attack 2 (password): Sniper + Candidate passwords wordlist
```

**Why it worked:**  
The application leaked information by responding differently to valid vs. invalid usernames. Furthermore, it returned a significantly different response length (or byte size) upon a successful password match, allowing for easy identification of the correct credentials.

## 4. Result
Successfully brute-forced the credentials and logged into the compromised account, solving the lab.

## 5. Key Takeaway & Remediation

**What I learned:**  
The server reacts differently to existing and non-existing usernames, allowing attackers to enumerate valid users. Additionally, the server leaks information during password brute-forcing, as the correct password can be identified by an anomalous response length or byte count.

**How to fix it:**
1. Use a generic error message (e.g., "Invalid username or password") for all authentication failures, ensuring the response time and content are identical.
2. Implement rate limiting, account lockout mechanisms, CAPTCHA after multiple failed attempts, and artificial delays between login attempts to mitigate brute-force attacks.
