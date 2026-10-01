
## ☁️ Publicación de Imágenes en Docker Hub (Push Workflow)

Este proceso es indispensable para empaquetar tus imágenes locales y subirlas al servidor remoto de Docker Hub, permitiendo que tu equipo las descargue o que se desplieguen en entornos de producción.

> [!important] 🔑 Paso 1: Autenticación en el Servidor
> Antes de realizar cualquier operación, debes iniciar sesión de forma segura en tu terminal de Linux con tus credenciales de Docker Hub:
> ```bash
> docker login
> ```

> [!todo] 🏗️ Paso 2: Construcción Manual (Si no usas Compose)
> Si no has generado la imagen mediante tu orquestador, créala directamente desde la raíz del microservicio donde se encuentra tu `Dockerfile`:
> ```bash
> docker build -t nombre-de-tu-imagen .
> ```
> * **Nota:** No olvides incluir el punto `.` al final, el cual le indica a Docker que use el contexto del directorio actual.

> [!warning] 📌 Paso 3: Etiquetado Obligatorio (Docker Tag)
> **Este paso es el más crítico.** Para que Docker sepa exactamente a qué cuenta y repositorio web debe enviar los archivos, debes renombrar tu imagen local siguiendo un formato estricto:
> ```bash
> docker tag nombre-de-tu-imagen tu-usuario-docker/nombre-del-repo:latest
> ```
> 
> > [!abstract] 🔍 Desglose del Formato Estándar: `usuario/nombre-repositorio:etiqueta`
> > * **`tu-usuario-docker`:** Tu nombre de usuario real y único en la plataforma de Docker Hub.
> > * **`nombre-del-repo`:** El nombre de destino que le asignaste (o quieres que tenga) el repositorio en la web.
> > * **`latest`:** La etiqueta (*tag*) que indica que es la versión estable más reciente de tu microservicio.

> [!success] 🚀 Paso 4: Subir la Imagen al Registro Remoto
> Una vez etiquetada correctamente, envía la imagen a la nube utilizando el comando de empuje:
> ```bash
> docker push tu-usuario-docker/nombre-del-repo:latest
> ```
