# Cybersecurity Home Lab - Lab 2: Network Discovery with Nmap

## Overview

This lab builds upon the environment created in Lab 1 by performing network reconnaissance against the isolated Host-Only network. The objective was to identify live hosts, enumerate exposed services, identify operating systems, and assess potential vulnerabilities using Nmap. No exploitation was performed during this lab; the focus remained on reconnaissance and analysis.

## Objectives

- Discover live hosts.
- Perform TCP and UDP port scans.
- Identify service versions.
- Fingerprint the target operating system.
- Execute default and vulnerability NSE scripts.
- Analyze findings from an attacker's perspective.

## Lab Environment

| Component | Value |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Attacker | KALI-ATTACK (192.168.56.40) |
| Target | METASPLOITABLE2 (192.168.56.50) |
| Network | 192.168.56.0/24 Host-Only |

## Tools Used

- Nmap 7.99
- Kali GNU/Linux 2026.2

## Implementation Procedure

### 1. Host Discovery

A ping sweep (`nmap -sn`) identified active hosts on the Host-Only network before deeper enumeration.

**Screenshot:** `screenshots/host-discovery-ping.png`

**Findings**

- Five lab VMs were discovered.
- The VirtualBox Host-Only adapter (`192.168.56.1`) was identified.
- An unexpected response from `192.168.56.100` was investigated and determined to be the disabled VirtualBox DHCP service rather than an additional host.

---

### 2. TCP Connect Scan

A TCP Connect scan (`nmap -sT`) enumerated open TCP ports.

**Screenshot:** `screenshots/tcp-connect-metaspoitable.png`

**Analysis**

Twenty-three TCP ports were identified. Legacy services including FTP, Telnet, HTTP, SMB, MySQL, PostgreSQL, IRC, and Tomcat were exposed, indicating a deliberately insecure target.

---

### 3. Service Version Detection

Version detection (`nmap -sV`) identified software versions running on exposed services.

**Screenshot:** `screenshots/service-version-detection-metasploitable.png`

**Key Findings**

- vsFTPd 2.3.4
- Apache 2.2.8
- Samba 3.x
- UnrealIRCd
- Tomcat 5.5
- MySQL 5.0
- PostgreSQL 8.3

These outdated versions are commonly referenced in penetration testing exercises because publicly documented vulnerabilities exist.

---

### 4. SYN Scan

A SYN scan (`sudo nmap -sS`) demonstrated a stealthier reconnaissance technique.

**Screenshot:** `screenshots/syn-scan-metasploitable.png`

**Analysis**

Results closely matched the TCP Connect scan while completing more quickly and without establishing full TCP connections.

---

### 5. UDP Scan

A Top-20 UDP scan (`sudo nmap -sU --top-ports 20`) identified UDP services.

**Screenshot:** `screenshots/udp-scan-metaspointable.png`

**Findings**

Open or filtered UDP services included DNS (53), NetBIOS Name Service (137), and NetBIOS Datagram Service (138). Many common UDP ports were closed.

---

### 6. Operating System Detection

OS fingerprinting (`sudo nmap -O`) was performed.

**Screenshot:** `screenshots/os-detection-metasploitable.png`

**Analysis**

Nmap correctly identified the target as a Linux 2.6-based system, consistent with Metasploitable 2.

---

### 7. Default NSE Scripts

Default NSE scripts (`nmap -sC`) collected additional information.

**Screenshot(s):** `screenshots/default-nse-scan-metasploitable1.png` `screenshots/default-nse-scan-metasploitable2.png` `screenshots/default-nse-scan-metasploitable3.png`

**Highlights**

- Anonymous FTP enabled.
- Apache and Tomcat identified.
- Samba configuration enumerated.
- Database information exposed.
- HTTP titles and additional service metadata collected.

---

### 8. Vulnerability Enumeration

Vulnerability scripts (`nmap --script vuln`) searched for publicly known weaknesses.

**Screenshot(s):** `screenshots/vulnerability-script-scan-metasploitable1.png` ... `vulnerability-script-scan-metasploitable6.png` ... `screenshots/vulnerability-script-scan-metasploitable7.png` 

**Important Findings**

- CVE-2011-2523 (vsFTPd 2.3.4 Backdoor)
- UnrealIRCd Backdoor
- SSL POODLE
- Logjam
- Weak Diffie-Hellman
- CCS Injection
- Slowloris
- HTTP TRACE enabled

No exploitation was performed. Confirming exploitability is reserved for later labs.

## Validation

Reconnaissance successfully identified the intended target, exposed services, operating system, and numerous publicly documented vulnerabilities. Results aligned with the purpose of Metasploitable 2 as an intentionally vulnerable training system.

## Analysis

This lab demonstrates the importance of enumeration before exploitation. Each successive scan revealed additional intelligence beyond simple host discovery, allowing the attack surface to be prioritized without modifying the target system.

The most significant finding was the identification of the vulnerable vsFTPd 2.3.4 service, which will be revisited during the Metasploit lab.

## Troubleshooting

### Unexpected Host (192.168.56.100)

An additional host appeared during discovery scans.

**Investigation**

- Aggressive scan revealed no open ports.
- ARP inspection confirmed a VirtualBox MAC address.
- VBoxManage identified the address as the disabled Host-Only DHCP service.

**Resolution**

No action required. The address represents VirtualBox infrastructure rather than an additional VM.

## Lessons Learned

- Enumeration should always precede exploitation.
- Different Nmap scan types provide different levels of visibility and speed.
- Service version detection greatly increases reconnaissance value.
- NSE scripts rapidly enrich reconnaissance results.
- Vulnerability scan results require validation before exploitation.

## Future Improvements

- Correlate discovered services with CVEs.
- Compare Nmap results with Nessus.
- Exploit selected services using Metasploit in Lab 5.
- Map findings to MITRE ATT&CK.

## MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Reconnaissance | Active Scanning (T1595) |
| Discovery | Network Service Discovery (T1046) |

## Cybersecurity Lab Roadmap

| Lab | Topic | Status |
|------|-------|--------|
| 01 | Build the Cybersecurity Lab | ✅ Complete |
| 02 | Network Discovery with Nmap | ✅ Complete |
| 03 | Network Traffic Analysis with Wireshark | ⏳ Next |
| 04 | Vulnerability Scanning | ⏳ Planned |
| 05 | Metasploit Framework | ⏳ Planned |
| 06 | Password Attacks | ⏳ Planned |
| 07 | Web Application Security | ⏳ Planned |
| 08 | Windows Logging | ⏳ Planned |
| 09 | Wazuh SIEM | ⏳ Planned |
| 10 | Security Onion | ⏳ Planned |
| 11 | MITRE ATT&CK Mapping | ⏳ Planned |
| 12 | Detection Engineering | ⏳ Planned |
| 13 | Incident Response | ⏳ Planned |
| 14 | Active Directory | ⏳ Planned |
| 15 | Active Directory Attacks | ⏳ Planned |
| 16 | Active Directory Defense | ⏳ Planned |
| 17 | Azure Fundamentals | ⏳ Planned |
| 18 | Microsoft Sentinel | ⏳ Planned |