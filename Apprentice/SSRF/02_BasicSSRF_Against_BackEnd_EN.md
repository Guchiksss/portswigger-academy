# Basic SSRF against Another Back-End System

**Platform:** PortSwigger Web Security Academy  
**Category:** Web / SSRF  
**Level:** Apprentice  
**Date:** 21.10.2026  
**Status:** ✅ Solved  
**Lab Link:** https://0aad000f03c7453580b75d2f00eb008f.web-security-academy.net/

---

## 1. Objective
Using the product stock check feature on the store website, scan the internal `192.168.0.X` network (port 8080) for an admin panel to delete 'carlos'.

## 2. Reconnaissance
1. Went to the website, navigated to the first available product page, and discovered the 'Check stock' button there.
2. Intercepted this request using Burp Suite Interceptor.
3. In the request, I saw that `stockApi` accepts a full URL, which means the site is vulnerable to SSRF.

## 3. Exploitation
1. Sent the request to Intruder.
2. Changed `stockApi` to `http://192.168.0.X:8080/admin`.
3. Placed payload markers around 'X' in the URL.
4. In Payloads, added numbers from 0 to 255 (simple list).
5. Launched a sniper attack.
6. As a result of iterating through IP addresses, I discovered an anomalously long response with the payload 122.
7. In Interceptor, I changed `stockApi` to `http://192.168.0.122:8080/admin` and sent the request.
8. The stock check system returned the HTML of the admin panel to me.
9. In the admin panel, I clicked the delete button for 'carlos' and intercepted this request via Interceptor to get the endpoint of this action on the back-end (`admin/delete?username=carlos`).
10. Returned to the initial product page, intercepted the stock check request, and replaced `stockApi` with the 'carlos' deletion endpoint (`http://192.168.0.122:8080/admin/delete?username=carlos`).

**Payloads:**
```text
Attack 1 (scanning): Sniper + numbers from 0 to 255 in the last IP octet
Attack 2 (deletion): Direct request to http://192.168.0.122:8080/admin/delete?username=carlos
```

**Why it worked:**  
The website has an SSRF vulnerability in the stock check functionality, which makes direct requests via URL without validation. This allowed:
1. To scan the internal `192.168.0.0/24` network by substituting different IP addresses into the `stockApi` parameter.
2. To discover a live host with an admin panel on port 8080 by the anomalous response length.
3. To directly target the user deletion API endpoint, bypassing the need to interactively use the web interface (since SSRF returns raw HTML, not a rendered page).

## 4. Result
Gained access to the internal network and the admin panel, the lab is solved.

## 5. Key Takeaway & Remediation

**What I learned:**  
SSRF can be used not only to access localhost, but also to scan internal networks. An attacker can iterate through IP addresses in internal subnets and find hidden services (admin panels, databases, internal APIs) that are inaccessible from the external network.

**How to protect:**
1. Validate and whitelist URLs in the `stockApi` parameter: forbid not only localhost, but also internal IP ranges.
2. Implement protection at the network firewall level: block outgoing requests from the application to internal networks.
