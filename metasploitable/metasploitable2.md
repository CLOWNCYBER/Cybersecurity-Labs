# Metasploitable 2

## 📌 Information

| Field      | Value                                             |
| ---------- | ------------------------------------------------- |
| Platform   | Local Lab                                         |
| Machine    | Metasploitable 2                                  |
| Difficulty | Beginner                                          |
| OS         | Linux                                             |
| Category   | Enumeration / Exploitation / Privilege Escalation |

---

# 🎯 Objectivo

Obtén acceso inicial a la máquina Metasploitable 2 e intenta obtener privilegios de administrado
---

# 🛠 herramientas

* Nmap
* Gobuster
* Netcat
* Searchsploit
* Metasploit Framework
* FTP
* SSH
* SMB

---

# 🔍 Enumeracion

## 1. Descubriendo la ip del objetivo

Primero, identifique la dirección IP de la máquina Metasploitable 2
lo hacemos con una herramienta llamada barrido.sh
<img width="1920" height="921" alt="Screenshot_2026-08-12_20_28_01" src="https://github.com/user-attachments/assets/ce361033-bb0f-49cf-a555-106458c5d399" />

```
Luego verifique la conectividad

<img width="1920" height="921" alt="Screenshot_2026-08-12_22_35_50" src="https://github.com/user-attachments/assets/13e6e6d0-c2ab-4496-ae03-0e540a7d678b" />


---

## 2. Nmap
nmap nmap -sV -sC -sS -A -T5 192.168.100.213

 puertos y servicios descubiertos.


* 21/tcp → FTP
* 22/tcp → SSH
* 23/tcp → Telnet
* 80/tcp → HTTP
* 139/tcp → SMB
<img width="1920" height="921" alt="Screenshot_2026-08-12_20_28_01" src="https://github.com/user-attachments/assets/ccf0ff7b-56ce-416e-a5d7-c16c3267b678" />

---

# 🌐Enumeracion web🌐

http://192.168.100.213
```

Check the website and look for interesting information.

### Gobuster

```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```

### Findings

Document interesting directories and files discovered.

### 📸 Web Enumeration

Drag your screenshot here.

---

# 📂 FTP Enumeration

If FTP is available, connect to the service:

```bash
ftp TARGET_IP
```

Check whether anonymous access is allowed.

```text
Username: anonymous
Password: anonymous
```

Document anything interesting discovered through FTP.

### 📸 FTP

Drag your screenshot here.

---

# 🔎 Service Enumeration

Investigate the services discovered by Nmap.

For each interesting service, document:

* Service
* Version
* Possible vulnerabilities
* Enumeration performed
* Interesting findings

Example:

```text
Service:
Version:
Potential vulnerability:
Enumeration:
Findings:
```

---

# 💣 Exploitation

Once a vulnerable service has been identified, research the vulnerability before attempting exploitation.

Search with:

```bash
searchsploit SERVICE VERSION
```

Or search through Metasploit:

```bash
msfconsole
```

```text
search SERVICE VERSION
```

Document:

1. What vulnerability was identified.
2. Why it is vulnerable.
3. Which exploit was selected.
4. How the exploit was configured.
5. What happened after exploitation.

### 📸 Exploitation

Drag your screenshot here.

---

# 🐚 Initial Access

Document how you obtained your first shell on the machine.

Example:

```bash
whoami
```

```bash
hostname
```

```bash
id
```

### 📸 Shell

Drag your screenshot here.

---

# 🔑 Privilege Escalation

Once initial access has been obtained, investigate whether the current user can escalate privileges.

Check the current privileges:

```bash
sudo -l
```

Check the operating system:

```bash
uname -a
```

Look for interesting SUID binaries:

```bash
find / -perm -4000 -type f 2>/dev/null
```

Document the privilege escalation technique discovered.

### 📸 Privilege Escalation

Drag your screenshot here.

---

# 👑 Root Access

Verify whether root access was obtained:

```bash
whoami
```

Expected result:

```text
root
```

### 📸 Root

Drag your screenshot here.

---

# 🧠 Lessons Learned

* Importance of complete port enumeration.
* Identifying service versions with Nmap.
* Enumerating vulnerable services.
* Searching for known vulnerabilities.
* Understanding the difference between initial access and privilege escalation.
* Verifying privileges after obtaining a shell.

---

# 🏁 Conclusion

Metasploitable 2 provided a vulnerable environment for practicing enumeration, vulnerability identification, exploitation and privilege escalation.

The main objective was to obtain unauthorized access within the isolated laboratory environment and ultimately attempt to obtain root privileges.

---

# ⚠️ Disclaimer

This laboratory was performed in an intentionally vulnerable environment for educational purposes.

All techniques documented here should only be used against systems for which explicit authorization has been obtained.

