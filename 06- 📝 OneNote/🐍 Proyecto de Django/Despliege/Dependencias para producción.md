# 🚀 Despliegue en Producción: Gunicorn, Archivos Estáticos y Almacenamiento en Django

## ⚙️ 1. Gunicorn (Green Unicorn)

### 🔹 Definición
Es un servidor HTTP para aplicaciones Python basado en la interfaz **WSGI**. En producción, funciona como el **motor principal** que ejecuta y mantiene vivo tu proyecto de Django, aislando la aplicación de la red externa.

```bash
pip install gunicorn
```

### 🔁 Flujo de Trabajo en Producción
```mermaid
graph LR
A[Cliente / Nginx] -->|Petición HTTP| B(Gunicorn)
B -->|Traduce y transfiere| C[Django App]
C -->|Respuesta de código| B
B -->|Devuelve respuesta| A
```

> [!🚀] **Ventajas Clave**
> * **Concurrencia Real:** Maneja múltiples procesos en paralelo (*workers*) para atender cientos de peticiones de forma simultánea.
> * **Optimización:** Está diseñado y optimizado para soportar tráfico real y producción estable, a diferencia del comando `runserver` (que es exclusivo de desarrollo y monohilo).

---

## 🎨 2. WhiteNoise (Servidor de Archivos Estáticos)

### 🔹 Definición
Es una librería que le permite a Django **servir sus propios archivos estáticos (CSS, JS, Imágenes)** directamente desde la misma aplicación, eliminando la obligación estricta de montar y configurar Nginx o un servidor extra solo para este propósito.

```bash
pip install whitenoise
```

> [!WARNING] **Desarrollo vs. Producción**
> En desarrollo, el comando `runserver` sirve la carpeta `/static/` automáticamente tras bambalinas. **En producción, Django nativo bloquea este comportamiento por motivos de seguridad y rendimiento.** WhiteNoise intercepta estas solicitudes integrándose en la lista de `MIDDLEWARE` de tu `settings.py` para hacer que las rutas estáticas funcionen de forma nativa desde el propio contenedor o servidor.

### 📦 El comando `collectstatic`
Para que WhiteNoise pueda servir los archivos, es obligatorio consolidarlos todos en un directorio unificado usando la terminal. Existen dos formas comunes de ejecutarlo:

* **Método Incremental:** Copia únicamente los archivos estáticos nuevos o que hayan sufrido modificaciones desde la última ejecución.
  ```bash
  python manage.py collectstatic
  ```
* **Método Limpio (Producción/CI-CD):** Borra por completo el directorio estático destino y vuelve a compilar y copiar todo desde cero, de forma automatizada y sin pedir confirmaciones en la terminal.
  ```bash
  python manage.py collectstatic --clear --noinput
  ```

---

## 🗄️ 3. Almacenamiento Persistente y Multimedia (Imágenes)

### 🔹 El reto de los archivos en la Base de Datos
Las bases de datos relacionales no están diseñadas para guardar archivos binarios pesados (como imágenes o PDFs) de forma directa en sus celdas; únicamente guardan el texto de la ruta o URL del archivo.

```bash
pip install django-storages boto3
```

* **`django-storages`:** Colección de backends de almacenamiento personalizados para Django.
* **`boto3`:** El SDK oficial de AWS para Python. Se utiliza comúnmente para conectar Django de manera segura con servicios de almacenamiento en la nube, como los *buckets* de **Amazon S3**, asegurando que los archivos multimedia de tus usuarios persistan incluso si los contenedores de tu backend se reinician o destruyen.

---

## 🎨 4. Personalización del Panel de Administración

### 🔹 Django Jazzmin
Es una librería de estilos que reemplaza la interfaz visual clásica y nativa del administrador de Django por un **template moderno, responsivo, altamente personalizable y fácil de configurar** basado en Bootstrap y AdminLTE.

```bash
pip install django-jazzmin
```

> [!IMPORTANT] **Paso obligatorio de Configuración**
> Una vez instalado mediante la terminal, debes agregarlo a tu lista de aplicaciones instaladas dentro de tu archivo de control global. Asegúrate de colocarlo **antes** del módulo de administración nativo de Django:
> ```python
> # settings.py
> INSTALLED_APPS = [
>     'jazzmin',  # Debe ir antes de django.contrib.admin
>     'django.contrib.admin',
>     ...
> ]
> ```
