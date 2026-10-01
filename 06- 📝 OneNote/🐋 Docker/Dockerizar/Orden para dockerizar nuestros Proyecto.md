# 🐋 Guía de Dockerización y Despliegue de Microservicios

Esta guía detalla el ciclo completo para empaquetar, comunicar y monitorear servicios independientes utilizando Docker, Docker Compose y Nginx como API Gateway.

---

## 🏗️ Flujo de Configuración (Paso a Paso)

> [!todo] **Paso 1: Congelar Dependencias (Backend)**
> Asegúrate de tener activado tu entorno virtual en la terminal y genera el archivo de requerimientos del servicio:
> ```bash
> pip freeze > requirements.txt
> ```

> [!todo] **Paso 2: Crear el Dockerfile**
> Diseña la "receta" de construcción dentro de la raíz de cada microservicio para empaquetar su entorno aislado.

> [!todo] **Paso 3: Configurar el Nginx en el API Gateway**
> Crea el archivo `nginx.conf` dentro del proyecto del **API_Gateway** para enrutar los puertos internos a un único punto de acceso.

> [!todo] **Paso 4: Crear el Orquestador**
> Define el archivo `docker-compose.yml` en la raíz de cada servicio para automatizar su construcción y dependencias.

> [!todo] **Paso 5: Ajustar Seguridad en Producción**
> En el archivo `settings.py` de tus servicios, agrega las IPs de los contenedores o del Gateway en la lista de `ALLOWED_HOSTS`.

---

## 🖥️ Secuencia de Comandos Esenciales

### Comandos Generales de Arranque
* **Construir imágenes automáticamente:** (Si tienes la directiva `build` en tu compose):
  ```bash
  docker-compose build
  ```
  *(De lo contrario, las imágenes se descargarán directamente desde Docker Hub).*
* **Construir y probar localmente (Viendo logs):**
  ```bash
  docker-compose up --build
  ```
* **Levantar en segundo plano (Background / Detached):**
  ```bash
  docker-compose up -d
  ```

### Gestión ante Modificaciones y Errores
* **Si modificaste el Dockerfile o código:** (Reconstruye un solo contenedor específico):
  ```bash
  docker-compose up --build nombre_del_servicio
  ```
* **Si solo modificaste el docker-compose.yml:** (Aplica cambios sin reconstruir):
  ```bash
  docker-compose up -d
  ```
* **Reconstrucción absoluta desde cero:**
  ```bash
  docker-compose down && docker-compose build && docker-compose up -d
  ```

### Monitoreo del Sistema
```bash
docker-compose ps         # Verifica el estado de los servicios del compose actual
docker ps                 # Ver TODOS los contenedores activos en el sistema Linux
docker-compose logs -f    # Seguir los logs de la consola en tiempo real
docker-compose down       # Detener y remover los contenedores de este proyecto
```

---

## 🌐 Gestión de Redes (Networking) y Despliegue en Secuencia

> [!warning] **Nota Importante de Seguridad**
> En entornos de producción, **no es recomendable** meter las bases de datos en la misma red del API Gateway. De lo contrario, quedarían expuestas innecesariamente hacia servicios externos.

### Ejemplo Práctico de Despliegue Manual
1. **Crear la red global (Solo se ejecuta una vez):**
   ```bash
   docker network create biblioteca_net
   ```
2. **Conectar manualmente un contenedor activo a la red:**
   ```bash
   docker network connect biblioteca_net nombre_contenedor
   ```
3. **Comprobar el estado de las redes en Linux:**
   ```bash
   docker network ls                             # Listar redes existentes
   docker network inspect biblioteca_net         # Verificar qué IPs tienen los contenedores en esta red
   ```

> [!tip] 💡 Solución Permanente Recomendada (Evita conexiones manuales)
> Agrega la red nativa al final de **cada** `docker-compose.yml` bajo la propiedad de tus servicios para que se enlacen solos al encender:
> ```yaml
> services:
>   libros_app:
>     # ... tu configuración de entorno
>     networks:
>       - biblioteca_net
> 
> networks:
>   biblioteca_net:
>     external: true # Indica que la red ya fue creada globalmente en el sistema
> ```

---

## ❓ Preguntas de Autoevaluación (Modo Estudio)

> [!faq]+ 🟩 ¿Puedo bajar un microservicio y que siga corriendo el Gateway?
> **SÍ, absolutamente.** Nginx no se va a caer si detienes un contenedor específico. Simplemente devolverá un código de estado `502 Bad Gateway` para esa ruta en particular, mientras las demás siguen respondiendo bien:
> * `curl http://localhost:8080/MS_LIBROS/` ➡️ Retorna **502** (si libros_app está abajo).
> * `curl http://localhost:8080/MS_USUARIOS/` ➡️ Retorna **200 OK** (si usuarios_app está arriba).

> [!faq]+ 🟩 ¿Puedo levantar solo un microservicio + el Gateway para hacer pruebas en Postman?
> **SÍ, exacto.** No estás obligado a encender toda la arquitectura de la empresa. Para probar desarrollo local puedes encender únicamente lo que necesitas en la terminal:
> 1. `docker-compose up -d ms_usuarios`
> 2. `docker-compose up -d api_gateway`

> [!faq]+ 🟩 ¿Si apago los contenedores, debo volver a conectarlos a la red a mano?
> **SÍ**, únicamente si estás usando el método de conexión manual (`docker network connect`). Por eso se recomienda aplicar la **Solución Permanente** en el archivo YAML para automatizar el arranque.
