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

Luego verifique la conectividad

<img width="1920" height="921" alt="Screenshot_2026-08-12_22_35_50" src="https://github.com/user-attachments/assets/4fa5e699-10e5-42e0-bb24-897ac8acd187" />

---

## 2. Nmap

<img width="1920" height="921" alt="Screenshot_2026-08-12_22_56_18" src="https://github.com/user-attachments/assets/462f7f73-e86e-4493-b328-044034889495" />
nmap nmap -sV -sC -sS -A -T5 192.168.100.213

puertos y servicios descubiertos.

<img width="563" height="373" alt="image" src="https://github.com/user-attachments/assets/954ecd15-41da-4c29-9e4e-1cc8a81b24b7" />

* 21/tcp → FTP vsftpd 2.3.4
* 22/tcp → SSH OpenSSH 4.7p1
* 23/tcp → Telnet Linux telnetd
* 80/tcp → HTTP Apache httpd 2.2.8
* 139/tcp → SMB smbd 3.X 

---

# 🌐Enumeracion web🌐

http://192.168.100.213 no encontramos nada en la wed con la ip

<img width="1920" height="921" alt="Screenshot_2026-08-12_22_59_56" src="https://github.com/user-attachments/assets/d9c3c360-1ec3-4701-af7c-d723fe711425" />


### Utilizamos la herramienta Gobuster para descubrir Directorios ocultos 

<img width="1920" height="695" alt="Screenshot_2026-08-12_23_28_23" src="https://github.com/user-attachments/assets/c05588ae-64fd-4561-801b-0bf66fa65eb1" />


bash
gobuster dir -u http://192.168.100.213 -w /usr/share/wordlists/dirb/common.txt -x php,txt,js,py,html

Encontramos directorios oculos 
dav
phpMyAdmin
test
twiki
pero dentro de ellos no habia nada importante 


# 💣 Exploitation

Con la herramienda searchsploit y la vercion de la vulneravilidad podemos encontrar exploits disponibles

bash
searchsploit  vsftpd 2.3.4

<img width="1920" height="921" alt="Screenshot_2026-08-12_23_43_49" src="https://github.com/user-attachments/assets/d40d42ca-56e2-4225-9d74-d2f9522eecf7" />

ENcontramos dos exploit uno en paython y otro en metasploitable
yo desidi hacerlo con el de python manualmente 

CON la herramienta 
searchsploit -m
descargamos el exploit

<img width="1920" height="921" alt="Screenshot_2026-08-12_23_47_49" src="https://github.com/user-attachments/assets/fbbb95e0-b2d4-4fd3-b6fb-9eb0da4dc6d3" />

searchsploit -m unix/remote/49757.py


Con la herramienta python ejecutamos el exploit para obtener la shell reversa 


<img width="1920" height="921" alt="Screenshot_2026-08-12_23_49_09" src="https://github.com/user-attachments/assets/ec030c1a-6160-4840-b7a1-40251decb638" />

python3 49757.py 192.168.100.213
python3 el nombre del exploit en el caso mio era 49757.py y la ip victima



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

