# 🛠️ IT Support Troubleshooting Guides & SOPs

A collection of step-by-step troubleshooting procedures for common web hosting, DNS, SSL, and end-user connectivity issues. Written to standardize issue resolution and provide clear explanations for non-technical users.

---

## 📋 Guide 1: Troubleshooting Website Down / DNS Resolution Issues

### 1. Symptoms
* User reports their website is not loading (`ERR_NAME_NOT_RESOLVED` or `DNS_PROBE_FINISHED_NXDOMAIN`).
* Website loads on mobile data, but fails on the office or home Wi-Fi.
* Email delivery fails after a recent domain or hosting migration.

### 2. Step-by-Step Diagnosis
1. **Verify if the issue is local or global:**
   * Test the site on an alternate network (e.g., mobile data vs. Wi-Fi) or check global DNS propagation using tools like `dnschecker.org`.
2. **Check Domain Status & Expiry:**
   * Run a `WHOIS` lookup to confirm the domain is active and not expired or suspended (`clientHold` / `serverHold`).
3. **Inspect Nameservers & DNS Records:**
   * Verify the domain points to the correct authoritative nameservers.
   * Check the **A Record** (points root `@` and `www` to the correct web server IPv4 address).
   * Check **MX Records** if email is affected.
4. **Clear Local DNS Cache (If DNS records are already accurate):**
   * **Windows:** Open Command Prompt and run `ipconfig /flushdns`
   * **macOS:** Open Terminal and run `sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder`
   * **Browser:** Clear browser cache or test in an Incognito/Private window.

### 3. Resolution & Customer Communication
* **If A-Record or Nameservers were outdated:** Update the DNS zone in cPanel/WHM or the domain registrar, inform the user that propagation typically takes 1–4 hours (up to 24 hours globally), and follow up once confirmed live.
* **If local cache was stale:** Walk the user through flushing DNS or restarting their router, then verify the site loads cleanly.

