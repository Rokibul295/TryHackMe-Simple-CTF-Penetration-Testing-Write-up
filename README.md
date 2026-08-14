# TryHackMe — Simple CTF

A penetration-testing write-up for the **Simple CTF** room on TryHackMe, covering network reconnaissance, web enumeration, vulnerability identification, initial access, and Linux privilege escalation.

## 🎯 Lab Information

* **Platform:** TryHackMe
* **Room:** Simple CTF
* **Target IP:** `10.49.144.45`
* **Attacker IP:** `10.49.184.156`
* **Difficulty:** Easy
* **Environment:** Intentionally vulnerable Linux machine

## 🛠️ Tools Used

* Nmap
* Gobuster
* cURL
* FTP
* SSH
* Searchsploit
* Linux enumeration tools

## 🔎 Attack Path

```text
Nmap Enumeration
      ↓
FTP / HTTP / SSH Discovery
      ↓
Web Enumeration
      ↓
CMS Made Simple 2.2.8
      ↓
CVE-2019-9053
      ↓
SQL Injection
      ↓
Credential Discovery
      ↓
SSH Access
      ↓
User Flag
      ↓
sudo Enumeration
      ↓
Vim Privilege Escalation
      ↓
Root Shell
      ↓
Root Flag
```

## 📌 Key Findings

### Open Ports

| Port | Service | Version       |
| ---- | ------- | ------------- |
| 21   | FTP     | vsftpd 3.0.3  |
| 80   | HTTP    | Apache 2.4.18 |
| 2222 | SSH     | OpenSSH 7.2p2 |

### Web Application

The `/simple/` directory exposed **CMS Made Simple 2.2.8**.

The application was found to be vulnerable to:

**CVE-2019-9053 — SQL Injection**

The vulnerability was used to obtain valid credentials, which were then used for SSH access.

### Privilege Escalation

After obtaining access as the `mitch` user, `sudo -l` revealed that **Vim** could be executed with elevated privileges.

Vim's shell execution functionality was leveraged to obtain a root shell.

## 🧪 Important Commands

### Full Port Scan

```bash
nmap -p- 10.49.144.45
```

### Service Enumeration

```bash
nmap -sC -sV 10.49.144.45
```

### Web Directory Enumeration

```bash
gobuster dir -u http://10.49.144.45/simple/ \
-w /usr/share/wordlists/dirb/common.txt \
-x php,txt,bak
```

### FTP Enumeration

```bash
nmap -p21 --script ftp-anon 10.49.144.45
```

### SSH Access

```bash
ssh -p 2222 mitch@10.49.144.45
```

### Privilege Enumeration

```bash
sudo -l
```

### Vim Privilege Escalation

```bash
sudo vim -c ':!/bin/bash'
```

## 🏁 Flags

* **User:** `G00d j0b, keep up!`
* **Root:** `W3ll d0n3. You made it!`

## 📚 Skills Demonstrated

* Network reconnaissance
* Full-port scanning
* Service enumeration
* FTP enumeration
* Web directory enumeration
* CMS fingerprinting
* CVE research
* SQL injection exploitation
* Credential discovery
* SSH authentication
* Linux privilege enumeration
* Sudo misconfiguration exploitation
* Privilege escalation

## ⚠️ Disclaimer

This write-up was created for educational purposes and was performed against the intentionally vulnerable **TryHackMe** lab environment. The techniques demonstrated should only be used on systems where you have explicit authorization to perform security testing.

## 👤 Author

**Rokibul Islam Labonno**

Cybersecurity / CTF Write-up
