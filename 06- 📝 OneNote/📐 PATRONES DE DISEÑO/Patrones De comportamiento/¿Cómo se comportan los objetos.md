# 📐 Patrones de Diseño (Parte 2): Observer, Strategy y Template

## 🅰️ 1. Angular (TypeScript)

### 🔹 Observer
* **Funcionamiento:** Es el núcleo del desarrollo Frontend en Angular. Su función principal es **notificar automáticamente los cambios de estado** a los componentes que estén escuchando.
* **Implementaciones nativas:**
  * **`RxJS Observables`** → Flujos de datos asíncronos basados en eventos a los que te suscribes.
  * **`Signals`** → El nuevo sistema de reactividad fina de Angular para rastrear cambios de estado de forma eficiente.

### 🎮 Strategy
* **Funcionamiento:** Permite cambiar el comportamiento o la forma de procesar los datos de manera dinámica según el contexto.
* **Implementación nativa:**
  * **`RxJS operators`** → Permiten alterar dinámicamente cómo viaja y se transforma la información en un mismo flujo de datos (`map`, `filter`, `switchMap`, etc.).

### 📑 Template Method
* **Funcionamiento:** Define la estructura esqueleto de un algoritmo o comportamiento, delegando la renderización o pasos finales a la lógica interna de la vista.
* **Implementaciones nativas (Directivas de Control):**
  * **`*ngIf`** → Condiciona la estructura del DOM según el estado.
  * **`*ngFor`** → Define el esqueleto de repetición para renderizar listas dinámicas.

---

## 🍃 2. Spring (Java)

### 🔹 Observer
* **Funcionamiento:** Actúa como un **escuchador de eventos (Event Listener)** dentro del ecosistema de la aplicación.
* **Implementación nativa:**
  * **`@EventListener`** → Despega métodos automáticamente cuando un evento específico es publicado en el contexto de Spring, desacoplando los módulos.

### 🎮 Strategy
* **Funcionamiento:** Consiste en **inyectar comportamiento** y poder cambiarlo de forma dinámica en tiempo de ejecución. 

> [!example] **Analogía de Desarrollo**
> Es como el sistema de un videojuego donde el personaje tiene diferentes armas disponibles y puede cambiar de una a otra (comportamiento) dinámicamente según la situación.

* **Implementaciones nativas:**
  * **`@Scheduled`** → Permite definir diferentes estrategias de ejecución de tareas en el tiempo (cron, fixedDelay, etc.).
  * **`@ConditionalOnProperty`** → Intercambia dinámicamente qué Bean (estrategia de código) se va a inyectar basándose en las propiedades del archivo de configuración.

### 📑 Template Method
* **Funcionamiento:** Abstrae los pasos repetitivos y la configuración pesada de una arquitectura, exponiendo un esqueleto limpio para que el desarrollador solo maneje la lógica cruda.
* **Implementación nativa:**
  * **`JdbcTemplate`** → Se encarga de abrir conexiones, manejar excepciones y cerrar recursos de la BD en SQL, dejando que tú solo escribas el query y el mapeo.

---

## 🐍 3. Django (Python)

### 📑 Template Method
* **Funcionamiento:** Define la estructura base de cómo debe responder el servidor HTTP a un recurso, permitiendo heredar y sobreescribir métodos específicos.
* **Implementación nativa:**
  * **`Class-based Views` (Vistas Basadas en Clases)** → Django ya tiene el esqueleto listo de cómo listar, crear o borrar (`ListView`, `CreateView`). Tú solo heredas de ellas y configuras el modelo o el template HTML.

### 🔹 Observer
* **Funcionamiento:** Permite que ciertas piezas de código de la aplicación sean notificadas cuando ocurren acciones específicas en otras partes del sistema.
* **Implementación nativa:**
  * **`Signals` (Señales)** → El despachador global de Django. Permite que "receptores" escuchen eventos del ORM (como `post_save` o `pre_delete`) para ejecutar lógica secundaria automáticamente.

### 🎮 Strategy
* **Funcionamiento:** Permite definir un algoritmo como una serie de pasos intercambiables por los que debe cruzar una petición antes de llegar a su destino final.
* **Implementación nativa:**
  * **`Middleware stack`** → Una lista ordenada de clases en `settings.py` que procesan las peticiones y respuestas globales. Puedes quitar, poner o reordenar middlewares (estrategias de seguridad, sesión, CORS) de forma dinámica sin tocar las vistas.
