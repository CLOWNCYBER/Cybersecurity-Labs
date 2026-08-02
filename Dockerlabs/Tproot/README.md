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
- Python3
---

# 🔍 Enumeration

## Nmap

```bash
nmap -sV -Pn -A -Pn -T4 [ip]
```
<img width="1920" height="921" alt="Screenshot_2026-08-02_15_58_22" src="https://github.com/user-attachments/assets/237bb3b3-b8cb-41cd-9b00-9f59d33635cd" />


### Findings

- FTP
- HTTP

---

# 🚀 Exploitation

explotamos la vulnerabilidad de forma manual sin utilizar metasploit

En FTP encontramos la vulnerobilidad vsftpd 2.3.4



NO encontramos nada importante en http
<img width="1920" height="921" alt="Screenshot_2026-08-02_15_53_13" src="https://github.com/user-attachments/assets/6225488f-5586-459d-9769-0dd7a0a3ce90" />

Usamos la herramienta searchsploit y el nombre de la vulnerabilidad

<img width="1920" height="921" alt="Screenshot_2026-08-02_16_01_28" src="https://github.com/user-attachments/assets/7323b7fb-9aac-4f26-832e-c9e36de6b375" />

Con searchsploit En contramos exploit disponibles 

Descargamos el exploit con searchsploit -m

<img width="1920" height="921" alt="Screenshot_2026-08-02_16_02_46" src="https://github.com/user-attachments/assets/3fb279de-1e1d-4d3a-a049-51cc57ebf907" />

Lo ejecutamos con python3 el nombre del exploit 49757.py [y la ip de la maquina victima]

<img width="1920" height="921" alt="Screenshot_2026-08-02_16_54_45" src="https://github.com/user-attachments/assets/d443b7fc-b65f-4e23-a8c5-41b61bc85231" />

Y obtenemos la reverse shell
Con script /dev/null -c bash Podemos cambiar el pron de la maquina

<img width="1920" height="921" alt="Screenshot_2026-08-02_16_55_49" src="https://github.com/user-attachments/assets/c1139121-e3f2-4d9d-a4dd-6cc02b8aa050" />

<img width="1920" height="921" alt="Screenshot_2026-08-02_16_56_44" src="https://github.com/user-attachments/assets/39197522-d3b3-4a47-856b-0a1fd0ae9b84" />


---

# 🔑 Privilege Escalation

No fue necesaria una escalada de privilegios, ya que el acceso inicial se obtuvo directamente como usuario `root`.

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
261fd3f32200f950f231816b4e9a0594
