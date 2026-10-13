# User ID Controlled by Request Parameter with Unpredictable User IDs

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / Access Control  
**Level:** Apprentice  
**Date:** 14.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0af000ae046dd2e480acbcb2005700a6.web-security-academy.net/

---

## 1. Objective
The site has a horizontal privilege escalation vulnerability on the user account page. The task is to find the GUID of `carlos` and obtain his API Key.

## 2. Reconnaissance
- Visited the blog site, browsed through posts and looked for mentions of `carlos` among commenters — found nothing.
- Logged into the site; noticed that the personal account page displays your API Key (understood where to look for it).
- Started reading posts again and found `carlos` among the authors.
- Navigated to his post page and found his GUID in the URL.

## 3. Exploitation
1. Used IDOR (replaced my own GUID with `carlos`'s GUID in the personal account URL).

**Payload:**
```text
userId=4fc6c347-fe1c-4c27-a045-ad30c9170262
```

**Why it worked:**  
The site exposed another user's GUID on the author's post page, which made it possible to perform an IDOR attack.

## 4. Result
Gained access to `carlos`'s account, obtained his API Key, lab solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
A complex GUID does not protect against IDOR if the site itself exposes it in public access, especially when a horizontal privilege escalation vulnerability exists.

**How to fix it:**
1. For every request to a resource, the server must verify at the backend level that the requested ID belongs to the currently authorized user.
2. Do not expose internal user identifiers (GUID, ID) in publicly accessible places: page URLs, metadata, API responses.
