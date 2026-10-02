---
tipo: duda-investigacion
estatus: 🔄 Pendiente
categoria: Comandos
fecha_registro: 2026-09-02
fecha_resolucion: 2026-09-02
fuente_origen: Con la IA de Google
---

> [!info] ❓ **La Duda / Reflexión**
> Escribe aquí la pregunta o el pensamiento inicial de forma clara.
> 1.  Colocare los comandos mas usados y que aun no recuerdo por completo de Fedora 44

---

# 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Se me olvida con el paso del tiempo, necesito registrarlo en caso de que no haya internet. 

---

# 🔍 Investigación y Respuestas
### Fuentes Consultadas
* [x] Buscar en Google / Documentación Oficial: ✅ 2026-09-02
*   [ ] Preguntar a IA / Foros: 

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
> # 🐧 Comandos Esenciales de Control y Gestión en Fedora 44
> **Diagnostico:**
> 1. `htop`
> 2. `btop`
> 3. `top`
> ## 🔄 Apagado y Reinicio Moderno (`systemctl`) 
> Esta es la sintaxis nativa y recomendada en las distribuciones actuales que utilizan *systemd*. 
> * **Reiniciar el equipo:** `systemctl reboot` 
> * **Apagar el equipo:** `systemctl poweroff` 
> * **Suspender el equipo:** `systemctl suspend` 
> * **Hibernar el equipo:** `systemctl hibernate`
>
>
> ---
> ## 📦 Gestión de Paquetes (`dnf5`)
> Fedora 44 utiliza **DNF5** por defecto, lo que ofrece búsquedas e instalaciones mucho más veloces.
> 
>* **Actualizar todo el sistema:** `sudo dnf update` 
>* **Buscar un paquete/programa:** `dnf search <nombre>` 
>* **Instalar un programa:** `sudo dnf install <nombre>` 
>* **Eliminar dependencias innecesarias:** `sudo dnf autoremove`
>
>
>---
>## ⚙️ Gestión de Servicios (`systemctl`) 
>Control de procesos y demonios en segundo plano.
>
>* **Iniciar un servicio:** `sudo systemctl start <servicio>` 
>* **Detener un servicio:** `sudo systemctl stop <servicio>` 
>* **Habilitar inicio automático con el sistema:** `sudo systemctl enable <servicio>` 
>* **Ver el estado actual:** `systemctl status <servicio>`
>
>
> ## 📦 Docker
> Control de procesos y dominios en segundo plano
>
> * Iniciar un servicio (##Oracle): `podman start nombre`
> * Detener un servicio: `podman stop nombre`
> * Ver el estado: `podman ps`
>
>
> ## 💠 Gestion de contraseñas 
> * Conexion para Oracle con usuario root (user: system, dataBase: FREE): 15963
> * Conexion para Oracle con usuario MLRB (dataBase; FREE): 

---

# 🚀 Acciones a tomar (Next Actions)
- [ ] 
