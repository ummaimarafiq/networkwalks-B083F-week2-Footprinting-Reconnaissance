# Networkwalks B083F – Week 2 Footprinting & Reconnaissance Report

**Program:** Cybersecurity & Ethical Hacking Internship – Networkwalks  
**Batch:** B083F  
**Author:** Ummaima Rafique  
**Date:** October 2026

---

## 1. Executive Summary

This report documents the footprinting and reconnaissance activities performed during Week 2 of my internship. The objective was to gather publicly available information about a target organization without directly interacting with its systems.

All activities were performed under the Networkwalks Letter of Authorization (NW-LOA-B083F-001). The scope was limited to networkwalks.com (owned by Networkwalks) and my own local network.

Key findings include the domain registrar (GoDaddy), hosting provider (HostGator), web technologies (WordPress 7.1.2), Web Application Firewall (ModSecurity), and a detailed DNS footprint.

---

## 2. Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Domain registration details |
| WhatWeb | Web technology fingerprinting |
| nslookup | DNS resolution |
| curl -I | HTTP header analysis |
| wafw00f | WAF detection |
| dnsrecon | DNS record enumeration |
| Zenmap | Local network scanning |
| theHarvester | Email and subdomain harvesting |
| GHDB | Google Hacking Database dorks |

---

## 3. Activities Performed

### 3.1 WHOIS – Domain Registration

**Command:** `whois networkwalks.com`

Result: Registrar GoDaddy, name servers on HostGator.

![WHOIS](01%20whois.png)

### 3.2 WhatWeb – Technology Fingerprinting

**Command:** `whatweb networkwalks.com`

Result: WordPress 7.1.2, WP Download Manager 3.3.58, Apache server, IP 192.232.216.135.

![WhatWeb](02%20whatweb.png)

### 3.3 nslookup – DNS Resolution

**Command:** `nslookup networkwalks.com`

Result: Domain resolves to 192.232.216.135.

![nslookup](03%20nslookup.png)

### 3.4 curl -I – HTTP Headers

**Command:** `curl -I https://networkwalks.com`

Result: HTTP/2 200, Apache server, WordPress REST API endpoint /wp-json/ exposed.

![curl](04%20curl.png)

### 3.5 wafw00f – WAF Detection

**Command:** `wafw00f networkwalks.com`

Result: Site is behind ModSecurity (SpiderLabs) WAF.

![wafw00f](05%20wafw00f.png)

### 3.6 dnsrecon – DNS Enumeration

**Command:** `dnsrecon -d networkwalks.com`

Result: NS, MX, TXT (SPF), SRV records enumerated. Bind 9.16.23.

![dnsrecon](06%20dnsrecon.png)

### 3.7 theHarvester – Email Harvesting

**Command 1:** `theHarvester -d microsoft.com -l 1000 -b baidu`

Result: 2 emails and 15 hosts found.

![theHarvester Baidu](07%20harvester%20baidu.png)

**Command 2:** `theHarvester -d microsoft.com -l 50 -b all`

Result: Additional sources queried (many required API keys).

![theHarvester All](08%20harvester%20all.png)

### 3.8 Zenmap – Network Scanning

**Step 1 – Local IP:** `ipconfig` → 192.168.100.8 / 255.255.255.0

![ipconfig](09%20ipconfig.png)

**Step 2 – Ping scan of 192.168.100.0/24**

Result: 2 live hosts:
- 192.168.100.1 (Huawei router) – MAC: 3C:67:8C:4B:74:B0
- 192.168.100.8 (my PC)

![Zenmap Scan](10%20zenmap%20scan.png)

**Step 3 – Topology generated**

![Zenmap Topology](11%20zenmap%20topology.png)

### 3.9 GHDB – Google Hacking Database

**Camera dork:** `intitle:"webcamXP" inurl:8080`

![GHDB Camera Dork](12%20ghdb%20cam%20.png)

**Math PDF dork:** `intitle:index.of "parent directory" mathematics pdf`

![GHDB Math Dork](13%20ghdb%20math%20dork.png)

Open directory listing found: pegasso.zapto.org/DOCS-TECH/Math/

---

## 4. Maltego – Attempted, Blocked

Maltego CE registration does not list Pakistan in the country dropdown. Only CaseFile (which does not support transforms) was accessible. This module will be revisited.

---

## 5. Problems Encountered & Solutions

| Problem | Solution |
|---|---|
| Maltego country block | Country dropdown didn't include Pakistan. Documented as skipped. |
| theHarvester API errors | Expected — many sources need paid API keys. Free sources still worked. |
| Zenmap missing local host | Nmap doesn't show the scanning machine in its own ARP scan. |

---

## 6. Conclusion & Lessons Learned

- Passive reconnaissance reveals a huge amount of information without touching the target.
- Web technology fingerprinting leads directly to known vulnerability databases.
- DNS records expose hosting providers and mail servers.
- Google dorks turn search engines into powerful recon tools.
- theHarvester aggregates dozens of public sources into one profile.
- Local network scanning reveals unauthorized devices.
- Every command and output must be documented for a proper security report.

---

## 7. Evidence

All screenshots are in the root of this repository.

---

*Prepared for the Networkwalks Cybersecurity Internship (Batch B083F).*
