# Penetration Testing & System Security Assessment Report

## Executive Summary
This report documents a controlled security assessment conducted in an isolated laboratory environment. The objective of this exercise is to demonstrate the full lifecycle of a penetration test—from initial reconnaissance to remediation—and highlight defensive strategies for system hardening.

---

## 1. Penetration Testing Methodology

Penetration testing follows a structured, multi-phase framework (such as PTES or NIST SP 800-115) to systematically assess target security controls.

Reconnaissance ──> Scanning ──> Exploitation ──> Post-Exploitation ──> Reporting

* **Phase 1: Reconnaissance (Information Gathering):** Collecting technical intelligence about the target environment (e.g., domain names, IP ranges, public technology stacks) without directly interacting aggressively with systems.
* **Phase 2: Scanning & Enumeration:** Identifying active hosts, open ports, running network services, and potential system vulnerabilities using automated discovery tools.
* **Phase 3: Exploitation:** Safely testing identified vulnerabilities to demonstrate potential security risks and determine if unauthorized access can be achieved.
* **Phase 4: Post-Exploitation:** Assessing the potential impact of a compromise by determining privilege levels, analyzing host configurations, and mapping sensitive data exposure.
* **Phase 5: Reporting:** Documenting technical findings, risk levels, proof-of-concept evidence, and actionable remediation guidelines for system administrators.

---

## 2. Exploitation Phase (Metasploit Laboratory Verification)

### Step 2.1: Target Scanning & Service Identification
* **Objective:** Identify exposed services on the target system (`192.168.56.102`).
* **Evidence:**
  > ![Screenshot 01: Nmap Scan Output](screenshots/01_nmap_scan.png)  
  > *Target scanning and open service identification output.*

### Step 2.2: Vulnerability Exploitation & Session Access
* **Objective:** Verify vulnerability status using the Metasploit Framework.
* **Evidence:**
  > ![Screenshot 02: Metasploit Module Execution](screenshots/02_exploit_execution.png)  
  > *Module execution parameters and session initiation status.*

### Step 2.3: Post-Exploitation Information Gathering
* **Objective:** Collect basic host identification details to confirm session access.
* **Evidence:**
  > ![Screenshot 03: Post-Exploitation Output](screenshots/03_sysinfo_hashdump.png)  
  > *Post-exploitation host system information (`sysinfo`) or hash listings (`hashdump`).*

---

## 3. Password Security & Authentication Attacks

### Step 3.1: Service Credential Testing (Hydra)
* **Objective:** Assess SSH password strength using dictionary-based authentication testing.
* **Evidence:**
  > ![Screenshot 04: Hydra Brute Force Output](screenshots/04_hydra_ssh.png)  
  > *Hydra SSH authentication testing results.*

### Step 3.2: Password Hash Auditing (John the Ripper)
* **Objective:** Demonstrate offline password recovery against weak stored password hashes.
* **Evidence:**
  > ![Screenshot 05: John the Ripper Crack Output](screenshots/05_john_hashes.png)  
  > *John the Ripper processing `hashes.txt` and recovering password.*

---

## 4. Social Engineering Awareness & Simulation

### Step 4.1: Web Interface Simulation Setup
* **Objective:** Demonstrate how credential harvest interfaces function in social engineering campaigns.
* **Evidence:**
  > ![Screenshot 06: SET Web Server Running](screenshots/06_set_webserver.png)  
  > *Social-Engineer Toolkit (SET) web server listener active.*

### Step 4.2: Phishing Detection & User Awareness Training Guide

To train organization members to identify and report phishing attempts, security awareness programs should emphasize the following key indicators:

| Indicator Category | Detection Red Flag | Verification Practice |
| :--- | :--- | :--- |
| **Sender Identity** | Lookalike domains (e.g., `micros0ft.com` or `github-support.net`) | Inspect full email headers and match domain names precisely. |
| **Embedded Links** | Mismatched URLs (hovering reveals a destination different from displayed text) | Hover over links before clicking; never enter credentials on non-standard URLs. |
| **Urgency / Threat** | High-pressure tone ("Account suspended in 24 hours!") | Contact internal IT support directly via verified channels rather than replying. |
| **Authentication** | Missing Multi-Factor Authentication (MFA) prompts | Enable hardware-based FIDO2/WebAuthn or TOTP authenticator apps on all accounts. |

---

## 5. Malware Analysis Fundamentals
Understanding host-level threats requires combining static and dynamic analysis methodologies in isolated environments.

### Static Analysis vs. Dynamic Analysis

   ┌─────────────────────────┐
                │ Malware Analysis Methods│
                └────────────┬────────────┘
                             │
       ┌─────────────────────┴─────────────────────┐
       ▼                                           ▼
┌──────────────────────┐                    ┌──────────────────────┐
│   Static Analysis    │                     │   Dynamic Analysis   │
├──────────────────────┤                    ├──────────────────────┤
│ • Examine code without│                    │ • Execute sample in  │
│   executing binary   │                    │   isolated sandbox   │
│ • Inspect headers,    │                    │ • Monitor runtime    │
│   hashes, & strings  │                    │   system API calls   │
│ • Reverse engineering│                    │ • Observe network &  │
│   via disassemblers  │                    │   file modifications │
└──────────────────────┘                    └──────────────────────┘

* **Static Analysis:** Inspects binary structure without execution. Analyzes file hashes (SHA-256), imported DLL functions, embedded strings, and header structures using tools like PEStudio or `strings`.
* **Dynamic Analysis:** Executes the binary inside a sandboxed environment (e.g., Cuckoo Sandbox, ANY.RUN) while recording real-time behavior: process creation, registry modifications, network connections, and file drops.

### Analysis Sandbox Evidence
> ![Screenshot 07: Static and Dynamic Sample Analysis](screenshots/07_malware_sandbox.png)  
> *Static sample metadata analysis or sandbox dynamic execution logs.*

---

## 6. System Hardening & Mitigation Strategies

Implementing proactive defense-in-depth measures significantly reduces system vulnerability surface areas.

### 6.1 Patch Management
* **Action:** Apply vendor security updates regularly (`sudo apt update && sudo apt upgrade -y`).
* **Impact:** Eliminates known public vulnerabilities (such as legacy service exploits).

### 6.2 Network Security & Firewall Configuration
* **Action:** Restrict ingress and egress traffic using host-based firewalls (e.g., `ufw` or `iptables`).
* **Example Rules:**
  * Block unauthorized incoming port access: `sudo ufw default deny incoming`
  * Allow explicit management ports only: `sudo ufw allow proto tcp from 192.168.56.0/24 to any port 22`

### 6.3 Disabling Unnecessary Services
* **Action:** Audit running background services and terminate non-essential daemons.
* **Impact:** Minimizes potential attack vectors by reducing open network listeners (`systemctl disable <service_name>`).

### Hardening Verification Evidence
> ![Screenshot 08: Firewall Configuration Output](screenshots/08_ufw_status.png)  
> *Active host firewall status verifying restricted ports (`ufw status verbose`).*
Understanding host-level threats requires combining static and dynamic analysis methodologies in isolated environments.

### Static Analysis vs. Dynamic Analysis
