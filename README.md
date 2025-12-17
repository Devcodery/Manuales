# 📚 Manuales y Documentación de Servicios Docker

En esta carpeta encontrarás **documentación técnica y guías paso a paso** para desplegar, configurar y administrar servicios autoalojados (*Self-Hosted*) utilizando Docker en Linux.

Todo el contenido está basado en mis propias pruebas y configuraciones en **Pop!_OS / Ubuntu Server**, por lo que los manuales son prácticos, claros y funcionales. ⚙️

---

## ⚠️ Nota importante sobre los archivos de configuración

Los archivos `docker-compose.yml` y configuraciones que verás en estos manuales **están modificados y personalizados** según mis necesidades específicas (rutas de carpetas, volúmenes LVM, redes internas, asignación de recursos, etc.).

Aunque son totalmente funcionales para seguir mis guías, te recomiendo consultar siempre la **documentación oficial** de cada servicio para obtener el archivo más actualizado o una versión "limpia" si deseas realizar una instalación desde cero.

### 🔗 Enlaces Oficiales y Referencias

| Servicio | Documentación / Docker Hub Oficial |
| :--- | :--- |
| **Portainer** | [docs.portainer.io](https://docs.portainer.io/sts/start/install/server/docker/linux) |
| **Caddy** | [hub.docker.com/_/caddy](https://hub.docker.com/_/caddy) |
| **WireGuard** | [linuxserver.io/wireguard](https://docs.linuxserver.io/images/docker-wireguard/) |
| **WG-Easy** | [github.com/wg-easy/wg-easy](https://github.com/wg-easy/wg-easy) |
| **Pi-hole** | [github.com/pi-hole/docker-pi-hole](https://github.com/pi-hole/docker-pi-hole) |
| **Immich** | [documentation.immich.app](https://documentation.immich.app/docs/install/docker-compose) |
| **Jellyfin** | [jellyfin.org/docs](https://jellyfin.org/docs/general/installation/container) |


---

## ⚙️ Servicios incluidos

🛡️ **WireGuard / WG-Easy** – VPN rápida, ligera y con interfaz gráfica.  
🕳️ **Pi-hole** – Bloqueador de anuncios y rastreadores a nivel de red (DNS).  
📸 **Immich** – Gestión de fotos y vídeos con IA (alternativa a Google Photos).  
🍿 **Jellyfin** – Servidor multimedia con transcodificación por hardware.  
📦 **Portainer** – Gestión visual de contenedores Docker.  
🌐 **Caddy** – Proxy Inverso con SSL automático.  

---

## 🔐 Seguridad y Redes

Para que estos servicios funcionen correctamente y de forma segura, es vital entender la diferencia entre el **Firewall del Servidor (UFW)** y el **Firewall del Router (NAT)**.

### 1. UFW (Uncomplicated Firewall) - Seguridad del Servidor
Es el muro de seguridad de tu sistema Linux. Decide qué tráfico entra a tu máquina desde tu propia red local.

* **¿Cómo funciona?** Por defecto bloquea todo el tráfico entrante. Debemos abrir ("allow") solo lo necesario.
* **Comando básico:** `sudo ufw allow [PUERTO]/[PROTOCOLO]`
* **Activar:** `sudo ufw enable`
    * *⚠️ **IMPORTANTE:** Asegúrate de permitir el puerto SSH antes de activar el firewall, o perderás el acceso remoto.*

### 2. Router (NAT / Port Forwarding) - Acceso desde Internet
Para acceder a tus servicios desde fuera de casa (4G/5G), debes configurar la tabla NAT de tu router.

* **Servicios Web (Caddy):** Abrir puertos **80** (TCP) y **443** (TCP/UDP) dirigidos a la IP de tu servidor.
* **VPN (WireGuard):** Abrir puerto **51820** (UDP).
* **Resto de servicios:** **NO** abrirlos.

> **💡 La regla de oro con Caddy:**
> Si utilizas **Caddy** como Proxy Inverso, **NO necesitas abrir en el router** los puertos de gestión de cada servicio (ej. el 8096 de Jellyfin, el 9000 de Portainer o el 2283 de Immich).
>
> Solo necesitas tener abiertos el **80** y el **443**. Caddy recibirá el tráfico y se encargará de enviarlo internamente al servicio correcto según el subdominio que escribas.

---

## 🛠️ Mantenimiento y Utilidades

### 🔄 Cómo actualizar un contenedor Docker
Cuando sale una nueva versión de un servicio, el proceso para actualizar en Docker Compose es el siguiente:

1.  Accede a la carpeta del servicio:
    ```bash
    cd ~/docker/nombre_servicio
    ```
2.  Descarga la nueva imagen:
    ```bash
    docker compose pull
    ```
3.  Recrea el contenedor con la nueva versión (sin perder datos):
    ```bash
    docker compose up -d
    ```
4.  (Opcional) Borrar imágenes antiguas para liberar espacio:
    ```bash
    docker image prune -f
    ```

### 💻 Mini-guía SSH (Secure Shell)
SSH es el protocolo que utilizamos para conectarnos y controlar el servidor Linux de forma remota (línea de comandos) desde otro ordenador.

* **Conexión básica:** `ssh usuario@ip-del-servidor`
* **Seguridad:** Es **CRÍTICO** no abrir el puerto 22 (SSH) directamente en el router hacia internet, ya que recibirás miles de intentos de ataque por fuerza bruta.
* **Recomendación:** Para administrar tu servidor desde fuera de casa, conéctate primero a tu **VPN (WireGuard)** y luego usa SSH usando la IP local, tal y como si estuvieras en tu habitación.

---

## ☕ Autor

🧑‍💻 **Devcodery** 💬 Apasionado por la ciberseguridad, los servidores y la automatización.  
🌍 [github.com/Devcodery](https://github.com/Devcodery)

> “Automatiza, documenta y nunca dejes de aprender.”  
> — Devcodery
                                        