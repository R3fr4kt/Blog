# Blog


## Project Overview

This lab demonstrates a full attack lifecycle against a Linux-based server hosting a vulnerable WordPress instance. The engagement covered reconnaissance, credential compromise, remote code execution, and privilege escalation to root.

The objective was to simulate a realistic penetration testing scenario following a structured and methodical approach.

---

## Key Actions Performed

### 1. Surface Enumeration

* Full TCP port scan identified exposed services (SSH, HTTP, SMB).
* Service and version detection confirmed a WordPress installation.
* Web directory fuzzing revealed administrative endpoints.

### 2. Credential Attack

* User enumeration performed via WPScan.
* Controlled password attack using a common wordlist.
* Valid credentials successfully obtained:

  `kwheel : cutiepie1`

This highlighted weak password policy and insufficient brute-force protection.

### 3. Remote Code Execution

* Initial public exploit was evaluated and discarded due to instability.
* Alternative exploitation path identified using `wp_crop_rce`.
* Authenticated RCE achieved via Metasploit.
* Shell access obtained and stabilized for post-exploitation.

### 4. Privilege Escalation

* System enumeration revealed a misconfigured privileged binary.
* Environment variable manipulation allowed execution with elevated privileges.
* Full root compromise achieved.

---

## Skills Demonstrated

* Structured reconnaissance methodology
* Web application enumeration (WordPress)
* Credential attacks and risk validation
* Exploit evaluation and decision-making
* Metasploit usage in controlled environments
* Linux privilege escalation techniques
* Shell stabilization and post-exploitation workflow

---

## Professional Value

This lab reflects practical offensive security capabilities aligned with real-world penetration testing engagements:

* Analytical decision-making (avoiding unstable exploits)
* Efficient attack path identification
* Understanding of Linux privilege models
* Ability to chain multiple vulnerabilities into full compromise

---

# GitHub About Section (Short Version)

Offensive Security practitioner documenting hands-on labs and real-world attack simulations.

Focused on structured methodology, exploit analysis, and Linux privilege escalation.

Demonstrating practical skills in enumeration, credential attacks, RCE exploitation, and full system compromise.


