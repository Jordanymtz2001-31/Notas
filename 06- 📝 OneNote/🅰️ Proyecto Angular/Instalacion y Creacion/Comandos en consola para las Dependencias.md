# 🚀 Guía de Comandos de Consola y Configuración en Angular

## 💻 1. Instalación Global del Entorno

> [!IMPORTANT] **Requisito previo**
> Asegúrate de tener instalado Node.js en tu sistema antes de ejecutar estos comandos en tu terminal.

* **Instalar Angular CLI:** Instala de forma global la interfaz de línea de comandos de Angular.
  ```bash
  npm install -g @angular/cli
  ```
* **Instalar TypeScript:** Instala de forma global el compilador y tipado de TypeScript.
  ```bash
  npm install -g typescript
  ```
* **Crear un nuevo Proyecto:** Genera la estructura base de un nuevo espacio de trabajo.
  ```bash
  ng new NombreDelProyecto
  ```

---

## 📦 2. Dependencias y Librerías Externas (Dentro del Proyecto)

Ejecuta estos comandos parándote en la **raíz del proyecto** para agregar utilidades de diseño, alertas y librerías del sistema:

* **Bootstrap:** Framework de estilos clásico.
  ```bash
  npm install bootstrap
  ```
* **Popper.js:** Motor de posicionamiento. Instálalo **solo si usas componentes de Bootstrap** como dropdowns, tooltips o popovers.
  ```bash
  npm install @popperjs/core
  ```
* **Tailwind CSS (V4):** Framework de utilidades CSS de bajo nivel.
  ```bash
  npm install tailwindcss @tailwindcss/postcss postcss --force
  ```
  * 🌐 *Ref: Consulta los pasos de configuración de archivos en la [Documentación Oficial de Tailwind para Angular](https://tailwindcss.com/docs/installation/framework-guides/angular).*
* **SweetAlert2:** Librería para renderizar estilos de alertas dinámicas y modales estéticos.
  ```bash
  npm install sweetalert2
  ```
* **Zone.js:** Librería del núcleo de Angular encargada de la detección y ejecución de cambios asíncronos en el DOM.
  ```bash
  npm i zone.js
  ```

---

## 🛠️ 3. Generación de Módulos, Componentes y Estructura

> [!TIP] **Abreviaturas del CLI**
> En los comandos de Angular, el flag **`g`** significa `generate`, **`c`** significa `component`, **`s`** significa `service` y **`--skip-tests`** evita la creación del archivo de pruebas unitarias `.spec.ts` para agilizar el desarrollo.

* **Componentes de Negocio:** Genera vistas organizadas por módulos.
  ```bash
  ng g c Componente/listar --skip-tests
  ```
* **Componente Compartido (Navbar):** Ideal para crear la barra de navegación global y renderizarla únicamente en el archivo HTML principal de la aplicación (`app.component.html`).
  ```bash
  ng g c Shared/navbar --skip-tests
  ```
  * 📍 *Ejemplo práctico: ENUCOM / SEMANA 4 / AUTHBasica*
* **Servicios de Microservicios:** Crea puentes de comunicación independientes para mapear cada microservicio del Backend hacia el Frontend.
  ```bash
  ng g s Servidor/servidor --skip-tests
  ```
* **Servicios de Autenticación (Opcional):** Administra el flujo de inicio de sesión de forma aislada.
  ```bash
  ng g s Auth/servidorAuth --skip-tests
  ```
* **Clases y Entidades:** Estructura moldes de TypeScript para mapear y tipar los modelos de datos que provienen de las APIs del Backend.
  ```bash
  ng g class Entidad/Nombre --skip-tests
  ```

---

## 🔒 4. Seguridad, Guardianes e Interceptores Nativos

* **Crear un Interceptor de Red:** Intercepta peticiones HTTP para adjuntar headers globales de sesión o interceptar credenciales.
  ```bash
  ng g i core/interceptors/auth
  ```
  * *Nota alternativa:* También puedes generar un servicio regular con fines de interceptación genérica usando `ng g s Auth/interceptor`.
* **Crear un Guardián de Rutas:** Protege páginas específicas bloqueando el acceso a usuarios que no estén autenticados en el sistema.
  ```bash
  ng g g core/guards/auth --functional
  ```

> [!tip] **¿Por qué utilizar el flag `--functional`?**
> En versiones anteriores de Angular, los *guards* se manejaban como clases complejas con mucha carga de código estructural. Al usar el flag `--functional` (estándar nativo a partir de Angular 17+), el framework genera una **función flecha simple** mucho más limpia y rápida de mantener.

### ⚙️ Menú de Configuración del Guardian (`ng g g`)
Al lanzar el comando, la CLI te desplegará una lista interactiva de opciones:

```text
Which type of guard would you like to create?  
❯ ◉ CanActivate  
  ◯ CanActivateChild  
  ◯ CanDeactivate  
  ◯ CanMatch
```

* **Elección:** Seleccionamos **`CanActivate`** (la primera opción) debido a que si el usuario no cuenta con los permisos necesarios o tokens válidos en la app, el guardián devolverá un valor `false` y **bloqueará inmediatamente el acceso** a esa página específica.

---

## 🚀 5. Ciclo de Servidor local

* **Levantar Servidor de Desarrollo:** Compila el proyecto en memoria y arranca el entorno local.
  ```bash
  ng serve -o
  ```
  * *💡 Tip:* El flag **`-o`** (`--open`) le indica a Angular que abra de forma automática tu navegador predeterminado en la dirección `http://localhost:4200` una vez termine de compilar.
