# Tproot
## 📌 Information

| Field | Value |
|------|------|
| Platform | DockerLabs |
| Difficulty | Very Easy |
| OS | Linux |
| Category | Privilege Escalation |

## 🚀 Despliegue de la máquina

<img width="1920" height="921" alt="Screenshot_2026-08-02_15_31_06" src="https://github.com/user-attachments/assets/10e27fc3-a465-4989-8064-7e8abdf175e8" />

<img width="1920" height="921" alt="Screenshot_2026-08-02_15_31_25" src="https://github.com/user-attachments/assets/1bbaf963-d9a3-45f2-b9d1-9b8f8207578c" />


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
nmap -sV -Pn -A -Pn -T4 [ip]
```

### Findings

- FTP
- HTTP

---

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
