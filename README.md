<p align="center"><img src="./img/n8n.png" width="600"  alt=" " /></p>
<h1 align="center"> n8n Guide </h1> 
<h4 align="right">Jul 25</h4>

<p>
  <img src="https://img.shields.io/badge/OS-Linux%20GNU-yellowgreen">
  <img src="https://img.shields.io/badge/OS-Windows%2011-blue">
  <img src="https://img.shields.io/badge/Hardware-Raspberry%20ver%204-red">
</p>

<br>

# Table of contents
- [Table of contents](#table-of-contents)
- [Summary](#summary)
- [Install from Docker image on Ubuntu 25.04 (locally)](#install-from-docker-image-on-ubuntu-2504-locally)
- [Troubleshooting](#troubleshooting)

<br>

# Summary
n8n es una herramienta de automatización de flujos de trabajo de código abierto que permite conectar y automatizar diferentes aplicaciones y servicios con muy poca o ninguna necesidad de programación. Se conecta con más de 200 servicios como Telegram, Hojas de Cálculo de Google, Discord, Gmail, MySQL, etc. Permite personalización avanzada mediante JavaScript. Se ejecuta fluidamente en Docker y es fácil de implementar en su servidor o VPS. Ofrece potentes integraciones con API y webhooks.

<br>

# Install from Docker image on Ubuntu 25.04 (locally)

1. Actualizar Ubuntu
```bash
apt update && apt upgrade -y
```
2. Instalar Docker Desktop en Ubunto <br>
Instalar Docker Engine & Docker Compose (LINK para instalar...)

3. Se requiere una ip estatica en el computador que servira de server

4. Crear carpeta principal
```bash
mkdir -p n8n-server/n8n-data
cd ..
```
> :memo: **Note:**  ```n8n-data``` es carpeta almacena toda la data utilizado por n8n

5. Permisos a la carpeta de trabajo
```bash
sudo chown -R 1000:1000 n8n-data //permiso de acceso para que pueda funcionar
sudo chmod -R 755 n8n-data //permiso de acceso para que pueda funcionar
```
con comando: 
```bash
ll
```
vamos a poder ver los archivos en el directorio con sus permisos. solo para verificar

6. Declarar variables de entorno como nombre usuario, contraseña, hub y URL. Crear archivo .env
```bash
nano .env
```
Copiar: 
```bash
### .env
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=[Your-User]
N8N_BASIC_AUTH_PASSWORD=[Your-Password]
N8N_HOST= [You-IP-Server]
N8N_PORT=5678
N8N_PROTOCOL=http
N8N_SECURE_COOKIE=false  # Temporarily disable secure cookie to avoid HTTP access issues
NODE_ENV=production
```
> :memo: **Note:** cambiar ```N8N_HOST= ```por la IP estática, ... REVISAR



5. Creamos el docker-compose.yml:
```bash
nano docker-compose.yml
```
Copiar:
```bash
### YML 
services:
  n8n:
    image: n8nio/n8n
    restart: always
    container_name: n8n-server  # Custom container name
    ports:
      - "5678:5678"
    env_file:
      - .env
    volumes:
      - ./n8n-data:/home/node/.n8n
```

```Explanation of Configuration:```

***container_name:*** Names the container n8n-server (feel free to change it). <br>
***env_file:*** Loads environment variables from the .env file. <br>
***volumes:*** Stores n8n data (including SQLite) on the host machine. <br>



6. Iniciar el sistema
dentro de la carpeta n8n-server
```bash
docker compose up -d 
```
 empezara correr, la primera vez tarda mas!

7. Verificar que esta corriendo
```bash
docker ps
```

> :memo: **Note:* Si queremos detener o iniciar el contenedor n8n
estando dentro de la carpeta
```bash
docker compose down && docker compose up
```

8. Permiter el puerto en el firewall (solo si esta habilitado el UFW)
```bash
sudo ufw status // ver status
sudo ufw allow 5678 // permitir el puerto de n8n en el firewall
``` 

9. Abrir Tu dominio desde el browser 
```bash
<ip-static-server>:5678
```
Inicia sesión con:<br>
Usuario: admin <br>
Contraseña: (Tu contraseña)

10. Ver logs si algo falla
```bash
docker logs -f n8n-server
```


<br>

# Troubleshooting
> :warning: **Warning:**

<br>

---
Copyright &copy; 2022 [carjavi](https://github.com/carjavi). <br>
```www.instintodigital.net``` <br>
carjavi@hotmail.com <br>
<p align="center">
    <a href="https://instintodigital.net/" target="_blank"><img src="./img/developer.png" height="100" alt="www.instintodigital.net"></a>
</p>


