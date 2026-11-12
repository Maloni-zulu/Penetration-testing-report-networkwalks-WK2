# 🔍 Penetration Testing Report: Footprinting, Network Scanning & Vulnerability Assessment

![Kali Linux](https://shields.io)
![Nmap](https://shields.io)
![Focus](https://shields.io)

---

## 1. 📋 Project Information

| Field | Details |
| :--- | :--- |
| **Author** | Maloni Zulu |
| **Role** | Cybersecurity Intern |
| **Organization** | Networkwalks |
| **Program** | Cybersecurity Internship |
| **Batch** | B084 |
| **Week** | Week 02 |
| **Date** | 12 November 2026 |
| **Project** | Footprinting, Network Scanning and Vulnerability Assessment |
| **Primary Target** | `networkwalks.com` |
| **Secondary Target** | `scanme.nmap.org` |
| **Operating System** | Kali Linux |
| **Host Machine** | Windows 11 |

---

## 2. ⚠️ Liability Disclaimer

> **Note:** This project was conducted strictly for educational and cybersecurity training purposes.

* Testing activities were restricted to systems and resources for which authorization was available or that were explicitly designated for security testing.
* The Nmap test target, `scanme.nmap.org`, was used for authorized scanning practice.
* **No exploitation or unauthorized access was attempted.**
* The findings documented in this report are based on observations made during the assessment.
* Any recommendations are intended to improve security awareness and defensive practices.

*Unauthorized access to computer systems may violate applicable laws. All security testing should be conducted within an approved scope.*

---

## 3. 📖 Introduction

This report documents the practical activities carried out during **Week 2** of my Cybersecurity Internship at **Networkwalks**. The exercise focused on three core areas of cybersecurity:

1. **Footprinting and Reconnaissance**
2. **Network Scanning and Service Discovery**
3. **Vulnerability Assessment and Security Analysis**

Various tools available in Kali Linux were utilized to gather domain intelligence, examine publicly exposed technologies, inspect DNS records, identify web security mechanisms, and discover active network services. Additionally, **Nmap** and **Zenmap** were used to analyze open ports, detect service versions, and visualize network topologies.

The primary goal was not merely to execute security tools, but to interpret their output and evaluate potential security implications.

---

## 4. 🎯 Objectives

* Understand the concepts of footprinting and passive/active reconnaissance.
* Gather publicly available information about a target domain.
* Identify domain registration and DNS ownership details.
* Fingerprint web technologies and server configurations.
* Inspect HTTP response headers for security controls.
* Detect the presence of a Web Application Firewall (WAF).
* Enumerate DNS records (`NS`, `MX`, `TXT`, `SPF`, etc.).
* Identify active hosts and open network ports.
* Perform service and version detection using Nmap.
* Utilize Zenmap's graphical scanning and topology mapping features.
* Document findings and provide actionable security recommendations.

---

## 5. 🛠️ Tools & Technologies Used

| Tool / Technology | Purpose |
| :--- | :--- |
| **Kali Linux** | Primary penetration testing operating system |
| **WHOIS** | Domain registration and ownership lookup |
| **WhatWeb** | Website technology and CMS fingerprinting |
| **Nslookup** | DNS resolution and IP address identification |
| **cURL** | HTTP response header inspection |
| **WAFW00F** | Web Application Firewall detection |
| **DNSRecon** | Comprehensive DNS enumeration |
| **Nmap** | Network discovery, port scanning, and service detection |
| **Zenmap** | Graphical user interface (GUI) and topology viewer for Nmap |
| **Windows 11** | Host operating system |
| **Oracle VirtualBox** | Hypervisor / virtualization environment |

---

## 6. 🖥️ Lab Configuration

| Component | Configuration |
| :--- | :--- |
| **Host Operating System** | Windows 11 Pro |
| **Virtualization Software** | Oracle VirtualBox 7.2 |
| **Attacker Machine** | Kali Linux |
| **Allocated RAM** | 8 GB |
| **Processor** | Intel Core i5 |
| **Network Environment** | Virtual Network (NAT / Bridged) |
| **Primary Target** | `networkwalks.com` |
| **Secondary Target** | `scanme.nmap.org` |
| **Secondary Target IP** | `45.33.32.156` |

---

## 7. 🕵️ Footprinting and Reconnaissance

Footprinting is the process of gathering foundational intelligence about a target prior to conducting deeper security assessments. Below are the tools, commands, and observations from this phase.

### 7.1 WHOIS Lookup
* **Tool:** `WHOIS`
* **Purpose:** Retrieve publicly available domain registration information, registrar details, registration/expiry dates, and name servers.
* **Command:**
  ```bash
  whois networkwalks.com
  ```
* **Findings:**
  * Domain registration details, creation dates, and expiry dates were retrieved.
  * Name server records were collected.
  * Registrant contact details were privacy-protected.
  * **HostGator** hosting infrastructure was identified from the records.
* **Security Relevance:** Attackers use WHOIS intelligence to map domain ownership, hosting providers, and potential social engineering vectors.
* **Evidence:** <img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/8b11511b-a6f1-40b3-b640-ecf01e6350d8" />


---

### 7.2 WhatWeb – Technology Fingerprinting
* **Tool:** `WhatWeb`
* **Purpose:** Identify underlying web technologies, frameworks, and CMS versions.
* **Commands:**
  ```bash
  whatweb networkwalks.com
  whatweb -v networkwalks.com
  ```
* **Findings:**
  * Identified **WordPress** (`v7.1.4`) as the Content Management System.
  * Detected **WP Download Manager** plugin (`v3.3.76`).
  * Collected additional web server and HTTP header metadata.
* **Security Relevance:** Exposed CMS and plugin versions allow security analysts (and attackers) to cross-reference software against known CVE advisories.
* **Evidence:** <img width="1366" height="255" alt="image" src="https://github.com/user-attachments/assets/ea02f2f8-95a7-48f4-88f7-9ffe09ef9b90" />


---

### 7.3 Nslookup – DNS Resolution
* **Tool:** `Nslookup`
* **Purpose:** Resolve the target domain name into its corresponding IPv4/IPv6 address.
* **Command:**
  ```bash
  nslookup networkwalks.com
  ```
* **Findings:**
  * Successfully resolved `networkwalks.com` to its hosting IP address via the configured DNS resolver.
* **Security Relevance:** Identifying the host IP helps determine whether the site sits behind a CDN/proxy or exposes its origin server directly.
* **Evidence:** <img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/f0be2f34-c3db-4774-b0f0-00b6ae44d057" />


---

### 7.4 cURL – HTTP Header Inspection
* **Tool:** `cURL`
* **Purpose:** Inspect HTTP response headers returned by the web server.
* **Command:**
  ```bash
  curl -I https://networkwalks.com
  ```
* **Findings:**
  * Server returned an `HTTP 200 OK` status over an established HTTPS connection.
  * Inspected `Content-Type`, server banners, and caching headers.
* **Security Relevance:** Reviewing headers helps verify whether critical defensive headers (`Content-Security-Policy`, `Strict-Transport-Security`, `X-Content-Type-Options`) are properly enforced.
* **Evidence:** <img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/8fb135e0-8115-4e30-a728-2624591676b8" />


---

### 7.5 WAFW00F – Web Application Firewall Detection
* **Tool:** `WAFW00F`
* **Purpose:** Detect whether a Web Application Firewall is protecting the target.
* **Commands:**
  ```bash
  wafw00f networkwalks.com
  wafw00f https://networkwalks.com
  ```
* **Findings:**
  * Detected **ModSecurity (SpiderLabs)** WAF protecting the web application.
* **Security Relevance:** Identifying a WAF clarifies what defensive filtering is in place against common web attacks (e.g., SQLi, XSS) and helps explain rate-limiting or blocked requests during testing.
* **Evidence:** <img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/f0e03907-e832-48b4-ac8e-a8d35e82b164" />


---

### 7.6 DNSRecon – DNS Enumeration
* **Tool:** `DNSRecon`
* **Purpose:** Enumerate DNS records associated with the target domain.
* **Command:**
  ```bash
  dnsrecon -d networkwalks.com
  ```
* **Findings:**
  * Enumerated `NS`, `MX`, `TXT`, and `SPF` records.
  * Reported **BIND 9.16.23** in the scan output.
  * Identified **cPanel-related** DNS records.
* **Security Relevance:** Overly verbose or misconfigured DNS records can reveal internal mail routing, third-party services, and management interfaces (like cPanel).
* **Evidence:** <img width="1366" height="340" alt="image" src="https://github.com/user-attachments/assets/7c99943e-2de8-425f-8f6c-8d7b76ae1e59" />


---

### 7.7 Nmap – Port Scanning and Service Detection
* **Tool:** `Nmap`
* **Purpose:** Discover open ports, running services, software versions, and OS fingerprints.
* **Commands:**
  ```bash
  nmap -sV networkwalks.com
  nmap -T4 -A -v networkwalks.com
  nmap -sV -T4 scanme.nmap.org
  ```
* **Findings:**
  * *[Paste your open ports, service versions, and scan observations for `networkwalks.com` and `scanme.nmap.org` here]*
* **Security Relevance:** Unnecessary open ports and exposed service banners increase the attack surface and can aid attackers in identifying outdated software.
* **Evidence:** <img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/e5859657-588c-4793-a073-0e0f00024369" />


---

---

## 8. 🗺️ Zenmap – Graphical Network Scanning

Zenmap provides a graphical interface for Nmap, making it easier to examine scan results, host information, network topology, and exposed services. For this exercise, `scanme.nmap.org` (the official Nmap test target) was scanned.

* **Target:** `scanme.nmap.org`
* **IP Address:** `45.33.32.156`
* **Scan Profile:** Intense Scan
* **Command:**
  ```bash
  nmap -T4 -A -v scanme.nmap.org
  ```

### 8.1 Zenmap Intense Scan
* **Purpose:** Perform detailed network scanning involving service detection, operating system detection, and traceroute.
* **Findings:**
  * The target was identified as live.
  * Service detection and operating system detection were attempted/performed.
  * Traceroute information was collected.
  * Open ports and associated services were examined.
* **Evidence:** <img width="1333" height="717" alt="image" src="https://github.com/user-attachments/assets/633ad4e5-45ea-4548-a932-6689573d9cb5" />


---

### 8.2 Ports and Hosts View
* **Purpose:** Examine discovered hosts, open ports, and associated services through Zenmap's graphical interface.
* **Findings:**
  * Multiple open ports and associated services were displayed.
  * Port information was organized for easier examination and review of exposed services.
* **Evidence:** <img width="1348" height="711" alt="image" src="https://github.com/user-attachments/assets/12268e14-9024-4a2b-b770-a9ba12ce1f97" />


---

### 8.3 Network Topology View
* **Purpose:** Visualize the network path between the scanning machine and the target.
* **Findings:**
  * The topology view displayed the relationship between the scanning machine and target.
  * Network hops were visualized, providing a clearer understanding of the route to the target.
* **Evidence:** <img width="1346" height="717" alt="image" src="https://github.com/user-attachments/assets/82c06fd2-5055-454a-b647-cf469fef0de7" />


---

### 8.4 Hosts Viewer
* **Purpose:** Examine discovered host information and operating system detection results.
* **Findings:**
  * Target host information and open port details were presented in a centralized interface.
  * **Linux** was reported as the operating system match.
* **Evidence:** <img width="1346" height="736" alt="image" src="https://github.com/user-attachments/assets/6ce029ba-a85f-4aff-9bf5-d5a3238e7858" />


---

## 9. 🔧 Errors Encountered and Solutions

During the practical exercises, several technical challenges were encountered and resolved:

| Error / Challenge | Possible Cause | Solution |
| :--- | :--- | :--- |
| **DNSRecon command not found** | Tool was not installed | Installed DNSRecon using APT (`sudo apt install dnsrecon`) |
| **WHOIS timeout** | Network delay or server response delay | Retried the command after checking connectivity |
| **cURL connection delay** | Slow network or HTTPS handshake delay | Checked connectivity and retried |
| **WAFW00F scan delay** | Multiple HTTP requests during detection | Allowed the scan to complete |
| **Nmap scan delay** | Extensive scanning and network filtering | Reviewed scan options and allowed completion |
| **Zenmap scan delay** | Intense Scan performs several detection activities | Waited for the scan to finish |

> **Lesson Learned:** Troubleshooting is an important part of penetration testing. Understanding why a tool fails makes it easier to resolve problems and obtain reliable results.

---

## 10. 📊 Risk Analysis and Security Observations

The following table summarizes the security observations made during the assessment:

| No. | Observation | Potential Security Implication | Suggested Priority |
| :--- | :--- | :--- | :--- |
| **1** | Public WHOIS information | Can support infrastructure reconnaissance | 🟢 Low |
| **2** | Web technology fingerprinting | May reveal software requiring security updates | 🟡 Medium |
| **3** | Domain IP address identification | Provides information about hosting infrastructure | 🟢 Low |
| **4** | HTTP response headers | May reveal configuration details or missing security controls | 🟡 Medium |
| **5** | WAF technology identification | Reveals information about defensive architecture | 🟢 Low |
| **6** | DNS records exposed | Can assist infrastructure enumeration | 🟡 Medium |
| **7** | HTTP and HTTPS ports exposed | Increases the externally accessible attack surface | 🟡 Medium |
| **8** | Multiple services identified on `scanme.nmap.org` | Provides information about exposed services on the test target | 🔵 Informational |
| **9** | Operating system fingerprinting | May help identify platform-specific attack opportunities | 🟢 Low |

> **Risk Assessment Note:** These observations represent potential security considerations, not confirmed vulnerabilities. No exploitation or vulnerability verification was performed. The risk priorities are preliminary and should be validated through further authorized assessment.

---

## 11. 🛡️ Security Recommendations

Based on the observations, the following defensive recommendations are proposed:

* **Domain Information:** Review publicly available registration information and enable domain privacy where appropriate.
* **Software Updates:** Keep WordPress, plugins, and other web components updated.
* **Infrastructure Protection:** Apply appropriate network controls to protect the origin server.
* **HTTP Security:** Review HTTP response headers and implement appropriate security headers.
* **WAF Configuration:** Maintain updated ModSecurity rulesets and regularly review security logs.
* **DNS Security:** Regularly audit DNS records and remove obsolete entries.
* **Port Management:** Disable unnecessary services and restrict access to sensitive ports.
* **Operating System Security:** Maintain current security patches and apply appropriate host firewall policies.

---

## 12. 💡 Lessons Learned

This practical exercise improved my understanding of several cybersecurity concepts:

1. **Footprinting and Reconnaissance:** Gathered information about a target using different reconnaissance tools.
2. **Domain Intelligence:** Gained practical experience retrieving domain registration and DNS information using `WHOIS` and `DNSRecon`.
3. **Technology Fingerprinting:** Learned how `WhatWeb` identifies website technologies and software components.
4. **HTTP Analysis:** Understood how to inspect HTTP response headers using `cURL`.
5. **Web Application Firewall Detection:** Gained experience identifying WAF technologies using `WAFW00F`.
6. **Network Scanning:** Learned how `Nmap` identifies active hosts, open ports, and running services.
7. **Network Visualization:** Explored `Zenmap`'s graphical features, including Ports/Hosts, Topology, and Hosts Viewer.
8. **Technical Documentation:** Learned the importance of documenting commands, evidence, findings, and recommendations in a structured penetration testing report.

---

## 13. 🏁 Conclusion

Week 2 of my Cybersecurity Internship at **Networkwalks** provided an opportunity to gain practical experience in footprinting, reconnaissance, network scanning, and security analysis.

Throughout the exercise, I worked with several Kali Linux tools, including **WHOIS**, **WhatWeb**, **Nslookup**, **cURL**, **WAFW00F**, **DNSRecon**, and **Nmap**. I also used **Zenmap** to perform graphical network scanning against the official Nmap test target, `scanme.nmap.org`.

One important takeaway was that reconnaissance plays a significant role in understanding an organization's digital infrastructure before deeper security assessments are conducted. I also learned that identifying an open port, exposed technology, or publicly available information does not automatically mean a vulnerability exists—proper validation and contextual analysis are necessary before assigning security risks.

Overall, this exercise strengthened my practical understanding of reconnaissance methodologies, network scanning, security observations, and professional penetration testing documentation.

---

## 14. 📚 References

* **Networkwalks:** [https://networkwalks.com](https://networkwalks.com)
* **Nmap Official Website:** [https://nmap.org](https://nmap.org)
* **Nmap Scanme Test Target:** [https://nmap.org](https://nmap.org)
* **Kali Linux Tools:** [https://kali.org](https://kali.org)
* **WHOIS Documentation:** [https://kali.orgwhois/](https://kali.orgwhois/)
* **WhatWeb Documentation:** [https://kali.orgwhatweb/](https://kali.orgwhatweb/)
* **DNSRecon Documentation:** [https://kali.orgdnsrecon/](https://kali.orgdnsrecon/)
* **WAFW00F Documentation:** [https://kali.orgwafw00f/](https://kali.orgwafw00f/)
* **Nmap Documentation:** [https://nmap.orgbook/](https://nmap.orgbook/)
* **cURL Documentation:** [https://curl.se](https://curl.se)

---

**Author:** Maloni zulu
*Cybersecurity Intern — Networkwalks (Batch B084)*
