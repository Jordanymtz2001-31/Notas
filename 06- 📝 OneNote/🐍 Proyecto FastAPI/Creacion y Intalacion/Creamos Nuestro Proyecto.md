# ⚡ Inicialización y Estructura de Entorno con FastAPI

## 💻 1. Configuración del Entorno Virtual

Antes de realizar las instalaciones, es necesario aislar las dependencias del proyecto creando y activando un entorno virtual.

```bash
# 1. Crear la carpeta del proyecto y acceder a ella
mkdir mi_proyecto && cd mi_proyecto

# 2. Crear el entorno virtual (venv)
py -m venv venv

# 3. Activar el entorno virtual (En Windows)
venv\Scripts\activate
```

---

## 📦 2. Gestión de Dependencias e Instalación

Con el entorno virtual **activado**, instala los paquetes esenciales para levantar el microservicio y habilitar la comunicación asíncrona:

* **FastAPI:** El framework core de alto rendimiento para construir APIs.
  ```bash
  pip install fastapi
  ```
* **Uvicorn:** El servidor web ASGI de alto rendimiento que se encarga de ejecutar y arrancar tu aplicación en el backend.
  ```bash
  pip install uvicorn
  ```
  * *💡 Tip:* También puedes instalar ambos en un solo comando ejecutando `pip install fastapi uvicorn`.
* **HTTPX:** Librería moderna para realizar peticiones HTTP salientes hacia otras APIs o servidores externos.
  ```bash
  pip install httpx
  ```

> [!🚀] **Diferencia Clave: HTTPX vs. Requests**
> A diferencia de la librería tradicional `requests` (que es estrictamente sincrónica y bloqueante), **`httpx` es compatible con funciones asíncronas (`async/await`)**, lo que permite aprovechar al máximo la arquitectura concurrente y de alta velocidad nativa de FastAPI.

---

## 🏗️ 3. Estructura de Archivos Recomendada

Para mantener el código modular, limpio y escalable, las responsabilidades de la API se dividen en los siguientes archivos según el caso de uso:

| Archivo | Responsabilidad del Componente |
| :--- | :--- |
| **`main.py`** | El punto de entrada de la aplicación. Aquí se inicializa la app y se configuran middlewares, CORS y routers globales. |
| **`routers.py`**| Contiene los **endpoints** (las rutas HTTP) que exponen los recursos de la API hacia el cliente. |
| **`schemas.py`**| Define los esquemas de datos utilizando **Pydantic**. Se encarga de validar de forma estricta las entradas y salidas de datos que llegan o salen de la API. |
| **`services.py`**| Centraliza la **lógica de negocio** pura de la aplicación, separándola de las rutas de transporte HTTP. |
| **`models.py`** | Se encarga de modelar las tablas de la base de datos (generalmente con ORMs como SQLAlchemy o SQLModel). |

---

## 🚀 4. Instanciación y Ciclo del Servidor Local

### Creación de la Instancia Base
Dentro de tu archivo de arranque principal (`main.py`), debes inicializar el framework creando la instancia del servidor:

```python
from fastapi import FastAPI

# Inicialización del microservicio
app = FastAPI()
```
*📍 ==> Ref: API FastAPI - PRÁCTICAS*

### Comando para Levantar el Servidor
Para arrancar el microservicio localmente y ponerlo en modo de escucha, ejecuta en tu terminal:

```bash
uvicorn main:app --reload
```

> [!💡] **Desglose del comando Uvicorn**
> * **`main:app`** → Le indica al servidor que busque el archivo llamado `main.py` y ejecute la instancia declarada con el nombre `app`.
> * **`--reload`** → Activa el modo de desarrollo. El servidor monitorea tu código e **inyecta los cambios automáticamente** en tiempo real cada vez que guardas un archivo, eliminando la necesidad de reiniciar la terminal manualmente.
