# Networkwalks B083F – Week 2 Footprinting & Reconnaissance Report

**Program:** Cybersecurity & Ethical Hacking Internship – Networkwalks  
**Batch:** B083F  
**Author:** Ummaima Rafique  
**Date:** October 2026

---

## 1. Executive Summary

This report documents the footprinting and reconnaissance activities performed during Week 2 of my internship. The objective was to gather publicly available information about a target organization without directly interacting with its systems. This passive reconnaissance phase is critical for understanding a target's digital footprint and potential attack surface.

All activities were performed under the Networkwalks Letter of Authorization (NW-LOA-B083F-001). The scope was limited to `networkwalks.com` (owned by Networkwalks) and my own local network (LAN).

The assessment was successful. Key findings include the identification of the domain's registrar, hosting provider, web technologies (WordPress), the presence of a Web Application Firewall (ModSecurity), and a detailed DNS footprint. Local network scanning revealed live hosts and their MAC addresses.

---

## 2. Introduction

Reconnaissance is the first phase of ethical hacking. It involves collecting as much information as possible about a target using only public sources. This information is used to plan subsequent phases like scanning and exploitation. This report covers the practical application of several industry-standard tools to achieve this goal.

---

## 3. Tools Used

| Tool | Purpose |
|---|---|
| **Kali Linux** | Primary OS for reconnaissance tools |
| **WHOIS** | Domain registration and ownership details |
| **WhatWeb** | Web technology fingerprinting |
| **nslookup** | DNS resolution and IP discovery |
| **curl -I** | HTTP header analysis |
| **wafw00f** | Web Application Firewall detection |
| **dnsrecon** | Full DNS record enumeration |
| **Zenmap** | GUI for Nmap to scan local networks |
| **theHarvester** | Email and subdomain harvesting |
| **GHDB** | Google Hacking Database for dorking |

---

## 4. Activities Performed

### 4.1 Footprinting with Multiple Kali Tools (PM1)

#### WHOIS – Domain Registration Details
**Command:** `whois networkwalks.com`
**Findings:** The domain is registered with GoDaddy. The name servers point to HostGator, indicating the hosting provider.

![WHOIS Output](screenshots/01-whois.png)

#### WhatWeb – Web Technology Fingerprinting
**Command:** `whatweb networkwalks.com`
**Findings:** The site runs on WordPress 7.1.2 and uses the WP Download Manager plugin (version 3.3.58). This is valuable for identifying known vulnerabilities.

![WhatWeb Output](screenshots/02-whatweb.png)

#### nslookup – DNS Resolution
**Command:** `nslookup networkwalks.com`
**Findings:** The domain resolves to the IP address `192.232.216.135`.

![nslookup Output](screenshots/03-nslookup.png)

#### curl -I – HTTP Response Headers
**Command:** `curl -I https://networkwalks.com`
**Findings:** The server is Apache. The HTTP 200 OK response confirms the site is live. The headers expose the WordPress REST API endpoint (`/wp-json/`).

![curl Output](screenshots/04-curl.png)

#### wafw00f – WAF Detection
**Command:** `wafw00f networkwalks.com`
**Findings:** The site is protected by a Web Application Firewall: **ModSecurity (SpiderLabs)**. This is a crucial finding, as it indicates security controls are in place.

![wafw00f Output](screenshots/05-wafw00f.png)

#### dnsrecon – DNS Enumeration
**Command:** `dnsrecon -d networkwalks.com`
**Findings:** Enumerated NS, MX, TXT (SPF), and SRV records. Revealed the DNS software version (Bind 9.16.23) and cPanel mail discovery records.

![dnsrecon Output](screenshots/06-dnsrecon.png)

### 4.2 Google Hacking Database (GHDB) (PM2)

**Objective:** Use Google dorks to find exposed devices and open directories.

**Dork 1:** `intitle:"webcamXP" inurl:8080`
**Result:** Found several exposed webcam interfaces.

![GHDB Camera Dork](screenshots/12-ghdb-cam-dork.png)

**Dork 2:** `intitle:index.of "parent directory" mathematics pdf`
**Result:** Found open directory listings containing mathematics PDF files.

![GHDB Math Dork](screenshots/14-ghdb-math-dork.png)

*(A full table of 10 camera links and 10 math PDF listings is provided in the final report.)*

### 4.3 theHarvester – Email & Subdomain Harvesting (PM4)

**Command 1 (Baidu):** `theHarvester -d microsoft.com -l 1000 -b baidu`
**Findings:** Found 2 email addresses and 15 subdomains/hosts.

![theHarvester Baidu](screenshots/07-harvester-baidu.png)

**Command 2 (All Sources):** `theHarvester -d microsoft.com -l 50 -b all`
**Findings:** Aggregated results from multiple public sources. *(Many sources require API keys and returned errors, which is expected.)*

![theHarvester All](screenshots/08-harvester-all.png)

### 4.4 Network Scanning with Zenmap (PM5)

**Objective:** Scan the local network to identify live hosts.

**Step 1: Local IP Identification**
**Command:** `ipconfig` (on Windows)
**Findings:** Local IP is `192.168.100.8`, with a `/24` subnet mask.

![ipconfig](screenshots/09-ipconfig.png)

**Step 2: Zenmap Ping Scan**
**Target:** `192.168.100.0/24`
**Findings:** Scan identified **2 live hosts**:
- `192.168.100.1` (Huawei router) – MAC: `3C:67:8C:4B:74:B0`
- `192.168.100.8` (my PC)

![Zenmap Scan](screenshots/10-zenmap-scan.png)

**Step 3: Topology Generation**
Generated a network topology PDF and saved it to the desktop.

![Zenmap Topology](screenshots/11-zenmap-topology.png)

---

## 5. Problems Encountered & Solutions

| Problem | Solution |
|---|---|
| **Maltego Country Block** | Maltego CE registration does not list Pakistan in its country dropdown. The tool could not be activated. This module was documented as incomplete. |
| **theHarvester API Errors** | Many sources require API keys. TheHarvester returned `Missing API key` errors. Free sources (Baidu, DNS) still returned valid results. |
| **Zenmap Missing Local Host** | Nmap does not show the scanning machine in its own ARP scan by design. This is expected behavior. |

---

## 6. Conclusion & Lessons Learned

This week's practical exercises demonstrated the power of passive reconnaissance. Before any direct interaction, a significant amount of information about a target can be gathered. Key takeaways include:

- **Technology Fingerprinting:** Identifying CMS and plugins is a direct path to finding known vulnerabilities.
- **DNS Intelligence:** DNS records reveal critical infrastructure like mail servers and hosting providers.
- **OSINT Power:** Tools like theHarvester aggregate public data to build a comprehensive target profile.
- **Internal Visibility:** Local network scanning with Zenmap is essential for discovering unauthorized devices.
- **Documentation:** Every finding must be documented with the exact command used and a screenshot for evidence.

All activities were conducted ethically and within the authorized scope.

---

## 7. Evidence & Screenshots

All screenshots are stored in the `screenshots/` folder of this repository.

---

*This report was prepared for the Networkwalks Cybersecurity Internship (Batch B083F).*
