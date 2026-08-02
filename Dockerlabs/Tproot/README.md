# Tproot

## 📌 Information

| Field | Value |
|------|------|
| Platform | DockerLabs |
| Difficulty | Easy |
| OS | Linux |
| Category | Privilege Escalation |

---

# 🎯 Objective

Obtain root access.

---

# 🛠 Tools

- Nmap
- Netcat
- Searchsploit

---

# 🔍 Enumeration

## Nmap

```bash
nmap -sCV TARGET_IP
```

### Findings

- FTP
- SSH
- HTTP

---

## Gobuster

```bash
gobuster dir -u http://TARGET -w wordlist.txt
```

### Findings

/admin

/uploads

---

# 🚀 Exploitation

Describe every step.

Explain WHY.

Do not only paste commands.

---

# 🔑 Privilege Escalation

Explain the vulnerability.

Show evidence.

---

# 📸 Screenshots

```
images/

01-nmap.png

02-gobuster.png

03-shell.png

04-root.png
```

---

# 📖 Lessons Learned

- Enumeration is the most important phase.
- Check service versions.
- Search for public exploits.
- Validate permissions.

---

# 🏁 Flag

```
ROOT
```
