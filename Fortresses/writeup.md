# 🛡️ HTB Machine Writeup — DummyBox

## 📌 Overview
- **Platform:** Hack The Box  
- **Difficulty:** Medium  
- **Category:** Web / Privilege Escalation  
- **Points:** 30  

---

## 🔎 Reconnaissance

### 🔹 Nmap Scan

```bash
nmap -sC -sV -T4 10.10.10.10
```

Port 22 — SSH

Port 80 — HTTP (Apache 2.4.x)

Port 8009 — AJP


gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt


