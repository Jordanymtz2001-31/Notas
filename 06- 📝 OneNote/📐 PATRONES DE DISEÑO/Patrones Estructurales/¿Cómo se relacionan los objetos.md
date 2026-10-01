# 📐 Patrones de Diseño (Parte 3): Decorator, Proxy, Facade y Module

## 🍃 1. Spring (Java) 

### 🎨 Decorator
* **Funcionamiento:** Permite añadirle responsabilidades o modificar el comportamiento de un objeto, método o clase de forma dinámica sin alterar su código fuente original.
* **Implementaciones nativas (Anotaciones):**
  * **`@Transactional`** → Envuelve al método para inyectarle comportamiento de transacciones de base de datos (commit/rollback) automáticamente.
  * **`@PreAuthorize`** → Decora el método para condicionar su ejecución a que el usuario cumpla con ciertos roles o permisos antes de entrar.

### 🔌 Proxy
* **Funcionamiento:** Actúa como un intermediario o sustituto de otro objeto para controlar el acceso a él, interceptando las llamadas antes de que lleguen al objeto real.
* **Implementación nativa:**
  * **`Spring AOP Proxies`** → Herramienta central de la Programación Orientada a Aspectos (AOP). Spring crea proxies dinámicos alrededor de tus Beans para inyectar lógica secundaria (como seguridad, logs o transacciones) de manera invisible.

### 🏛️ Facade (Fachada)
* **Funcionamiento:** Proporciona una interfaz unificada y simplificada frente a un subsistema o conjunto de clases complejas, ocultando la dificultad técnica detrás de una capa limpia.
* **Implementaciones nativas:**
  * **`@Service`** → Funciona como fachada de la lógica de negocio, agrupando llamadas a múltiples repositorios y servicios externos en métodos sencillos.
  * **`@Repository`** → Abstrae toda la complejidad del acceso a datos y mapeo de la base de datos bajo métodos simples como `save()` o `findById()`.

---

## 🐍 2. Django (Python)

### 🎨 Decorator
* **Funcionamiento:** Extiende o altera el comportamiento de funciones o vistas web de manera limpia sin modificar el código interno de la función.
* **Implementaciones nativas:**
  * **`@login_required`** → Intercepta la petición hacia una vista para verificar si el usuario ha iniciado sesión; si no es así, lo redirige al login.
  * **`@permission_required`** → Envuelve la vista para validar de forma automática si el usuario cuenta con el permiso específico de la base de datos antes de permitir la ejecución.

### 🔌 Proxy
* **Funcionamiento:** Controla y optimiza el acceso a un recurso pesado o remoto, difiriendo su carga real hasta el momento exacto en que se necesita.
* **Implementación nativa:**
  * **`QuerySet lazy loading` (Carga Perezosa)** → El ORM no realiza la consulta SQL a la base de datos cuando defines el QuerySet; actúa como un proxy. La petición a la BD se ejecuta únicamente cuando intentas iterar, evaluar o leer los datos en el código de Python.

### 🏛️ Facade (Fachada)
* **Funcionamiento:** Esconde las complejidades de un sistema complejo (como un motor de base de datos relacional) mediante una capa de código intuitiva.
* **Implementación nativa:**
  * **`ORM abstraction`** → El ORM completo de Django funciona como una gran fachada. Te permite buscar, filtrar o insertar datos usando métodos puros de Python (`Model.objects.filter()`) sin que tengas que lidiar con la sintaxis manual de consultas SQL ni conexiones a bases de datos.

---

## 🅰️ 3. Angular (TypeScript)

### 🎨 Decorator
* **Funcionamiento:** Anota y modifica clases o propiedades en tiempo de diseño, permitiendo que Angular les inyecte metadatos y configure su comportamiento en el framework.
* **Implementaciones nativas:**
  * **`@Component`** → Transforma una clase estándar en un componente web, asociándolo a un template HTML y estilos CSS.
  * **`@Injectable`** → Declara que una clase (servicio) puede ser utilizada por el sistema de inyección de dependencias de Angular.
  * **`@Pipe`** → Define herramientas para transformar visualmente datos en los templates sin modificar el valor original en TypeScript.

### 🔌 Proxy
* **Funcionamiento:** Intercepta y manipula el flujo de las peticiones o respuestas que viajan entre la aplicación y el exterior.
* **Implementación nativa:**
  * **`Angular Interceptor` (HttpInterceptor)** → Actúa como un proxy de red. Captura todas las peticiones salientes (`HttpRequest`) y respuestas entrantes (`HttpResponse`) para añadir tokens JWT globales, manejar códigos de error o activar pantallas de carga de forma centralizada.

### 🏛️ Facade (Fachada)
* **Funcionamiento:** Oculta la complejidad del manejo de estados globales masivos, centralizando la interacción en una sola API limpia para los componentes.
* **Implementación nativa:**
  * **`NgRx Effects`** → Actúa como una fachada sobre los flujos de datos asíncronos y efectos secundarios en la arquitectura Redux de Angular, permitiendo que los componentes solo disparen acciones sencillas sin conocer la complejidad del store.

### 📦 Module
* **Funcionamiento:** Agrupa componentes, directivas, pipes y servicios relacionados en bloques funcionales aislados para organizar el código y permitir la carga perezosa (Lazy Loading).
* **Implementaciones nativas:**
  * **`ES6 Modules`** → El sistema estándar de JavaScript/TypeScript para importar y exportar archivos mediante `import` y `export`.
  * **`Angular Modules` (NgModule)** → El sistema clásico de Angular para declarar el ecosistema de dependencias y visibilidad de un módulo de la aplicación.
