# 🛠️ IT Support & Security Troubleshooting Guides (SOPs)

A collection of step-by-step Standard Operating Procedures (SOPs) for common web hosting, DNS, SSL/TLS, email security, and end-user connectivity issues.

---

## 📋 Guide 1: Troubleshooting Website Down / DNS Resolution Issues

### 1. Symptoms
* User reports their website is not loading (`ERR_NAME_NOT_RESOLVED` or `DNS_PROBE_FINISHED_NXDOMAIN`).
* Website loads on mobile data, but fails on office or home Wi-Fi.
* Email delivery fails after a recent domain or hosting migration.

### 2. Step-by-Step Diagnosis
1. **Verify if the issue is local or global:** Test the site on an alternate network (mobile data vs. Wi-Fi) or check global DNS propagation using `dnschecker.org`.
2. **Check Domain Status & Expiry:** Run a `WHOIS` lookup to confirm the domain is active and not expired or suspended (`clientHold` / `serverHold`).
3. **Inspect Nameservers & DNS Records:**
   * Verify authoritative nameservers match the hosting provider.
   * Check the **A Record** (points `@` and `www` to the correct web server IPv4 address).
   * Check **MX Records** if email is affected.
4. **Clear Local DNS Cache:**
   * **Windows:** `ipconfig /flushdns`
   * **macOS:** `sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder`

### 3. Resolution & Customer Communication
* Update outdated DNS records in cPanel/WHM or the registrar, advise the user on propagation windows (1–4 hours typical, up to 24 hours globally), and confirm once resolved.

---

## 🛡️ Guide 2: Phishing Email & Suspected Account Compromise Triage

### 1. Symptoms
* User reports clicking a suspicious link, entering credentials on an unfamiliar page, or receiving spoofed emails.
* Unexpected password reset alerts or outbound spam sent from a user's mailbox.

### 2. Step-by-Step Containment & Diagnosis
1. **Immediate Containment (If credentials were entered):**
   * Force an immediate password reset using a strong, unique passphrase.
   * Revoke all active login sessions and sign out of all devices.
   * Verify and enforce **Multi-Factor Authentication (MFA/2FA)**.
2. **Inspect Mailbox & Forwarding Rules:**
   * Check email settings in webmail/cPanel or Microsoft 365 for unauthorized auto-forwarding rules or filters created by an attacker.
3. **Analyze Email Headers & Authentication Records:**
   * Inspect the raw email header (`Return-Path`, `Received` IP hops) to verify the true sender server.
   * Verify the domain's **SPF**, **DKIM**, and **DMARC** TXT records in DNS to reduce spoofing risks.

### 3. Resolution & User Guidance
* Remove malicious forwarding rules, educate the user on spotting mismatched sender domains and urgent credential requests, and document the incident details for escalation if broader network exposure is suspected.

---

## 🔒 Guide 3: SSL/TLS Certificate & HTTPS Warning Resolution

### 1. Symptoms
* Browser displays `Your connection is not private` (`NET::ERR_CERT_DATE_INVALID` or `NET::ERR_CERT_COMMON_NAME_INVALID`).
* Site loads over `http://` but fails or shows mixed-content warnings on `https://`.

### 2. Step-by-Step Diagnosis
1. **Inspect Certificate Details:** Click the browser padlock/security icon to check the **Expiration Date** and **Issued To (Common Name / SANs)** to ensure both `example.com` and `www.example.com` are covered.
2. **Verify DNS Pointing:** Ensure the domain's A record already points to the server before running AutoSSL / Let's Encrypt validation.
3. **Check HTTPS Enforcement:** Verify whether an `.htaccess` rule or server redirect is forcing HTTP traffic to HTTPS.

### 3. Resolution
* Renew or reissue the SSL/TLS certificate via cPanel/WHM AutoSSL, enable Force HTTPS Redirect, and test in an Incognito window to confirm a clean padlock status.
