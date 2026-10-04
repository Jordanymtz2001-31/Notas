# 🅰️ Estructura de Archivos y Componentes en Angular

## ⚙️ 1. Configuraciones y Control Global

### `angular.json`
* **Propósito:** Archivo de configuración global de la CLI de Angular.
* **Uso común:** Es el lugar **donde colocamos las rutas de los archivos CSS y JS** para integrar librerías de diseño de terceros como **Bootstrap**.

### `app.config.ts`
* **Propósito:** El **centro de control principal** de la aplicación.
* **Uso común:** Aquí se registran, configuran y activan las funcionalidades, enrutadores y servicios globales del framework de manera centralizada.

### `app.routes.ts`
* **Propósito:** Gestor del sistema de navegación por URL.
* **Uso común:** Define estrictamente la **vinculación entre las URLs y los componentes** que el navegador debe cargar cuando el usuario navega en la aplicación.

---

## 🏛️ 2. Lógica del Componente Raíz y Servicios

### `app.component.ts` (App.ts)
* **Propósito:** Clase controladora del componente raíz de la aplicación.
* **Uso común:** Aloja el **constructor** principal que se ejecuta al instanciar el componente y contiene los métodos lógicos para controlar la navegación manual o dinámica entre las distintas vistas y subcomponentes.

### `service.ts` (Servidor.ts)
* **Propósito:** Funciona como el **mensajero, puente y conexión** directa entre el Frontend y el Backend.

> [!info] **Responsabilidad del Servicio**
> Centraliza el consumo de la API. En este archivo se declaran e importan todos los **EndPoints (URLs) del Backend**, especificando sus métodos HTTP (`GET`, `POST`, `PUT`, `DELETE`) y el tipo de dato que espera recibir o enviar.

---

## 🔒 3. Módulo de Autenticación y Seguridad (Opcional)

> [!TIP] **Estructura de la Carpeta `/auth`**
> Bloque modular dedicado a blindar y gestionar la seguridad de las rutas y peticiones del sistema.

* **`auth/auth.service.ts` (ServidorAuth.ts):** Contiene y configura de manera aislada los métodos necesarios para el manejo de credenciales de usuario, inicio de sesión (Login) y almacenamiento del Token.
* **`auth/auth.interceptor.ts` (Interceptor.ts):** Alberga una función interceptora de red (Middleware) encargada de **capturar cada petición HTTP saliente** para inyectar automáticamente las cabeceras de Autenticación (como el Token JWT) antes de que viajen al servidor.
