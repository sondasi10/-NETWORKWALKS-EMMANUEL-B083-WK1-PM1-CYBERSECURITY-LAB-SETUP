# 🔐 Penetration Testing Report — Footprinting & Network Scanning

**Program/Batch:** B083 – Networkwalks Cybersecurity Internship
**Pentester:** Sondasi
**Date:** 17 September 2026
**Modules Completed:** W2-PM1 (Multiple Kali Tools) · W2-PM5 (Zenmap Scanning)
**Client/Target:** networkwalks.com (written permission secured) · Own local LAN network
**Permission Secured:** ✅ Yes
**Phases Covered:** Reconnaissance & Footprinting · Scanning & Network Discovery

📄 [Download the full formatted report (PDF)](W2-PM-FINAL_Sondasi_B083.pdf)

---

## ⚠️ Liability Disclaimer

These activities were performed only on systems/devices where written permission was secured, or on devices I own myself. All material here is for education and research purposes only. Unauthorised access is a crime in most countries even when nothing is damaged — the instructor, authors, and Networkwalks are not responsible for any misuse of this information.

---

## 📖 Introduction

This report covers footprinting the `networkwalks.com` domain using multiple Kali Linux tools (W2-PM1), and scanning a local network with Zenmap (W2-PM5). Together, these show how an attacker moves from gathering public information to mapping live hosts on a network. This is the Week 2 submission of an ongoing Networkwalks internship.

Every step below includes the exact command used, the result observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux & Windows | Operating systems used for reconnaissance and scanning |
| WHOIS | Find domain registration details (owner, dates, name servers) |
| WhatWeb | Fingerprint web technologies (server, CMS, plugins, IP) |
| Nslookup | Resolve the domain name to its IP address using DNS |
| Curl -I | Read the HTTP response headers of the website |
| Wafw00f | Detect whether a Web Application Firewall protects the site |
| DNSRecon | Enumerate DNS records (SOA, NS, SRV) |
| Zenmap (Nmap GUI) | Scan the local subnet to find live hosts, IPs and MAC addresses |

---

## 🔍 Activities Performed

### 4.1 Footprinting & Reconnaissance

Reconnaissance was performed against `networkwalks.com` using six Kali Linux tools.

**WHOIS** (`whois networkwalks.com`) — Domain registered with GoDaddy.com, LLC; created 6 Nov 2019, expiring 6 Nov 2027. Registry locks in place (client Delete/Renew/Transfer/Update Prohibited), DNSSEC unsigned. Name servers: `NS6135.HOSTGATOR.COM`, `NS6136.HOSTGATOR.COM`.

**WhatWeb** (`whatweb networkwalks.com`) — Identified WordPress 7.1, WP Download Manager 3.3.58, server IP `192.232.216.135`, Apache, Bootstrap 7.1, jQuery 3.7.1, exposed contact email (`info@networkwalks.com`), and a 301 redirect from HTTP to HTTPS.

**Nslookup** (`nslookup networkswalks.com`) — Run against a mistyped hostname (extra "s"), returned `NXDOMAIN`. The correct IP (`192.232.216.135`) was confirmed via WhatWeb/Curl instead. **Lesson:** a single-character typo is enough to produce a misleading "no record" result.

**Curl** (`curl -I https://networkwalks.com`) — Returned `HTTP/2 200` from Apache, set a `__wpdm_client` cookie, exposed the WordPress REST API (`/wp-json/`, including `/wp-json/wp/v2/pages/53`), a Permissions-Policy header referencing Google/Cloudflare/hCaptcha, and cache headers revealing the hosting stack.

**Wafw00f** (`wafw00f networkwalks.com`) — Identified **ModSecurity (SpiderLabs)** as the active WAF, detected after 2 requests.

**DNSRecon** (`dnsrecon -d networkwalk.com`) — Run against a mistyped domain (missing the final "s"). Enumeration completed and returned SOA/NS records pointing to `ns35/ns36.domaincontrol.com` (GoDaddy) rather than the HostGator servers seen in the real WHOIS record — no SRV records found. This confirms the query resolved a *different* domain, reinforcing the same lesson from Nslookup: **always verify exact target spelling and cross-check results against a second source.**

### 4.2 Network Scanning with Zenmap

Subnet `10.0.3.2/24` was scanned using the **Ping scan** profile (`nmap -sn 10.0.3.2/24`). Completed in 11.64 seconds, identifying 3 live hosts out of 256 addresses:

| Host | Status | MAC Address |
|---|---|---|
| 10.0.3.2 | Up | 52:54:00:12:35:00 (QEMU virtual NIC) |
| 10.0.3.3 | Up | 52:54:00:12:35:00 (QEMU virtual NIC) |
| 10.0.3.15 | Up | localhost / scanning host — no MAC reported |

The Zenmap **Topology** view confirmed the same three hosts connected to the localhost node on a fisheye ring diagram.

---

## 📊 Risk Analysis / Impact

| # | Risk / Finding | Evidence | Potential Impact | Risk Level |
|---|---|---|---|---|
| 1 | Web technology & version exposed | WhatWeb identified WordPress 7.1 & WP Download Manager 3.3.58 | Attackers may target known CMS/plugin vulnerabilities | 🟡 Medium |
| 2 | Server IP address identifiable | WhatWeb + Curl both resolved 192.232.216.135 | Reveals hosting location/provider | 🟢 Low |
| 3 | HTTP technical info exposed | Curl revealed headers + `/wp-json/` REST endpoint | Assists technology/API fingerprinting | 🟢 Low |
| 4 | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals security architecture details | 🟢 Low |
| 5 | DNS infrastructure exposed | DNSRecon returned SOA/NS records | Helps build infrastructure profile | 🟡 Medium |
| 6 | Domain registration details exposed | WHOIS returned registrar, dates, name servers | Supports social-engineering/mapping | 🟢 Low |
| 7 | Multiple live hosts visible on LAN | Zenmap found 3 hosts w/ MAC addresses | Unknown/unauthorized devices may be present | 🟡 Medium |

*These are observations from information-gathering exercises, not confirmed vulnerabilities — no exploitation was performed.*

---

## ✅ Recommendations

1. **Review publicly exposed technology info** — periodically audit visible CMS/plugin versions.
2. **Keep software updated** — WordPress core & plugins should follow current advisories.
3. **Review HTTP headers** — confirm `/wp-json/` and cache headers disclose nothing unnecessary.
4. **Review DNS/WHOIS records regularly** — check exposed registration details.
5. **Properly configure and monitor the WAF** — keep ModSecurity tuned and active.
6. **Perform regular internal network discovery** — maintain an up-to-date device inventory.
7. **Investigate unknown devices** — verify any unexpected host found during scans.
8. **Double-check target spelling during recon** — a single typo can silently redirect a query; always cross-check.
9. **Maintain network documentation** — keep topology and device records current.
10. **Perform security testing with authorisation** — only test systems/networks with explicit permission.

---

## 🏁 Conclusion

Week 2 covered footprinting, reconnaissance, and network scanning. Six Kali Linux tools were used to profile `networkwalks.com` — WHOIS for registration data, WhatWeb for technology fingerprinting, Nslookup for DNS resolution, Curl for HTTP headers, Wafw00f for WAF detection, and DNSRecon for DNS enumeration. The mistyped Nslookup/DNSRecon queries were a valuable hands-on lesson in why exact spelling and cross-checking matter during recon.

Zenmap was then used to scan a local subnet, discovering 3 live hosts with IP/MAC addresses and confirming them via a topology diagram.

Overall, this reinforced that solid information gathering — done carefully, documented clearly, and always within an authorised scope — is a foundational cybersecurity skill.

---

## 📸 Evidence

**1. WHOIS lookup** — `whois networkwalks.com`
![WHOIS lookup](evidence/whois.png)

**2. WhatWeb technology fingerprint** — `whatweb networkwalks.com`
![WhatWeb fingerprint](evidence/whatweb.png)

**3. Nslookup query (NXDOMAIN due to typo)** — `nslookup networkswalks.com`
![Nslookup query](evidence/nslookup.png)

**4. HTTP headers via Curl** — `curl -I https://networkwalks.com`
![Curl headers](evidence/curl.png)

**5. WAF detection via Wafw00f** — `wafw00f networkwalks.com`
![Wafw00f WAF detection](evidence/wafw00f.png)

**6. DNS enumeration via DNSRecon** — `dnsrecon -d networkwalk.com`
![DNSRecon enumeration](evidence/dnsrecon.png)

**7. Zenmap ping scan of local subnet** — `nmap -sn 10.0.3.2/24`
![Zenmap ping scan](evidence/zenmap.png)

**8. Zenmap network topology (fisheye view)**
![Zenmap topology](evidence/topology.png)

---

**Author:** Sondasi — Cybersecurity Professional, Batch B083
**Program:** Networkwalks Cybersecurity Internship | Week 02
