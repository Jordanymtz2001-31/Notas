# 📐 Patrones de Diseño: Spring vs. Django vs. Angular

## 🍃 1. Spring (Java)

### 🔹 Singleton
* **Funcionamiento:** Spring crea una **sola instancia por contexto** de aplicación y la reutiliza en todas las inyecciones.
* **Uso automático:** Se activa al definir `@Component`, `@Service`, `@Repository` o `@Controller`.

### 🏭 Factory Method
* **Funcionamiento:** Crea objetos de diferentes tipos según una entrada sin usar condicionales (`if-else` o `switch`) en la lógica de negocio. Se basa en métodos abstractos definidos en una `Interface`.

> [!TIP] **Regla Práctica de Elección**
> * **Usa la Fábrica:** Para centralizar y desacoplar el resto del sistema de los detalles de cómo se crean los objetos. Encapsula por completo la lógica de construcción compleja.
> * **Cuándo implementarlo:**
>   1. Tienes varias implementaciones de una interfaz y necesitas elegir una según un parámetro → *Ejemplo: `NotificationFactory` que crea `EmailNotifier`, `SmsNotifier`, `PushNotifier` según el canal.*
>   2. Requieres separar la creación del uso → La lógica de negocio no debe saber qué clase concreta se crea, solo interactúa con la interfaz.

### 🏛️ Abstract Factory
* **Funcionamiento:** Es una **fábrica de fábricas**. Permite crear familias de objetos relacionados sin especificar sus clases concretas. 

> [!example] **Estructura por Configuración (Ejemplo Local vs Cloud)**
> 1. Definir una interfaz común `ServicioFactory` con métodos para cada tipo de servicio.
> 2. Tener `ServicioLocalFactory` y `ServicioCloudFactory` como beans de Spring.
> 3. Inyectar la fábrica adecuada según el perfil activo (`dev`/`prod`, `local`/`cloud`).
> 
> * **Familia "Local":** `LocalPaymentService`, `LocalShippingService`, `LocalEmailService`.
> * **Familia "Cloud":** `CloudPaymentService`, `CloudShippingService`, `CloudEmailService`.

### 🧬 Prototype
* **Funcionamiento:** Su objetivo es crear nuevos objetos **copiando (clonando)** un objeto existente, en lugar de crearlos desde cero con un operador `new`.
* **En Spring:** Patrón clásico que combina el método `clone()` junto con el **scope prototype** para beans. Cada vez que se inyecta dicho bean, Spring genera una nueva instancia en lugar de reutilizar la misma como en Singleton.

---

## 🐍 2. Django (Python)

### 🔹 Singleton
* **Funcionamiento:** **No existe un concepto nativo** dentro del framework y no es común usarlo directamente de forma manual.
* **Uso habitual:** Se utiliza el módulo de configuraciones estáticas (**`settings.py`**) para manejar valores globales del sistema.

### 🏭 Factory
* **Funcionamiento:** Crea objetos de diferentes clases según un tipo o configuración, sin exponer la lógica de creación en todo el código.
* **En Django:** No está integrado en el núcleo del framework. Se implementa manualmente con librerías externas como **`factory_boy`** y su uso principal radica en las **pruebas unitarias y la generación de datos de prueba**.
* **Cuándo usar:** Cuando cuentas con varias implementaciones de un modelo o servicio y necesitas elegir una según el tipo (similar a las interfaces de Java), o cuando la creación de objetos es compleja y repetitiva.

### 🏛️ Abstract Factory
* **Funcionamiento:** Permite soportar e intercambiar **familias de objetos/servicios relacionados** según la configuración del entorno.

> [!example] **Estructura por Configuración (Ejemplo Pasarelas de Pago)**
> * **Familia "Stripe":** `StripePaymentGateway`, `StripeShippingCalculator`, `StripeEmailNotifier`.
> * **Familia "PayPal":** `PayPalPaymentGateway`, `PayPalShippingCalculator`, `PayPalEmailNotifier`.
> 
> * **Implementación:** Se define una interfaz `ServicioFactory` con métodos para cada servicio, se crean las clases concretas `StripeFactory` y `PayPalFactory`, y se elige cuál instanciar según las variables de entorno o la configuración en `settings.py`.

### 🧬 Prototype
* **Funcionamiento:** Crea nuevos objetos duplicando uno existente para omitir la lógica de construcción desde cero.
* **En Django:** Se utiliza la librería estándar mediante **`copy.deepcopy()`** o implementando un método personalizado `clonar()` directamente en las clases o modelos de la base de datos.
* **Cuándo usar:** 
  1. Modelos complejos con múltiples campos y relaciones donde requieres crear muchos registros semejantes. *Ejemplo: Productos con el mismo catálogo base pero distintos precios, tallas o colores.*
  2. Cuando necesitas clonar objetos directamente de la base de datos sin repetir toda la lógica de construcción manual.

---

## 🅰️ 3. Angular (TypeScript)

### 🔹 Singleton
* **Funcionamiento:** Es un servicio que se instancia **una sola vez a nivel de toda la aplicación** y se comparte entre todos los componentes que lo inyectan.

| Casos de Uso | Servicios Comunes | Propósito en Angular |
| :--- | :--- | :--- |
| **Estado Compartido** | `Service`<br>`AuthService`<br>`CartService` | Realizar peticiones al backend.<br>Manejar el usuario y su estado de autenticación.<br>Administrar el carrito de compras global. |
| **Datos Globales** | `ThemeService`<br>`LanguageService`<br>`ConfigService` | Controlar temas de color (claro/oscuro).<br>Gestionar el cambio de idioma en la app.<br>Almacenar variables de configuración del sistema. |
| **Optimizar Rendimiento**| `WebSocketService` | Mantener una única conexión activa en toda la app. |

### 🏭 Factory
* **Funcionamiento:** Encapsula la creación de objetos (servicios, modelos, componentes de interfaz de usuario) y **evita esparcir la lógica manual de usar `new`** en múltiples componentes a través de la inyección de dependencias. Se usa cuando la creación es compleja y quieres evitar la duplicación de código.

### 🏛️ Abstract Factory
* **Funcionamiento:** Permite crear familias de servicios relacionados abstrayendo las clases concretas según las variables de entorno de Angular.

> [!example] **Estructura por Entorno (Ejemplo Ambientación)**
> * **Familia "Dev":** `DevApiService`, `DevMockService`, `DevLoggerService`.
> * **Familia "Prod":** `ProdApiService`, `ProdRealService`, `ProdLoggerService`.
> 
> * **Implementación:** Se define la interfaz `ServicioFactory`, se desarrollan las clases `DevServicioFactory` y `ProdServicioFactory`, y se inyecta la fábrica que corresponda según el ambiente de ejecución actual.

### 🧬 Prototype
* **Funcionamiento:** Duplica un objeto existente para generar variantes en lugar de construirlo mediante una clase concreta.
* **En Angular:** Se define un método `clone()` en una interfaz o clase base, y las clases concretas lo implementan para retornar copias modificadas.
* **Cuándo usar:**
  1. Tienes **objetos complejos** que se crean a partir de una base común. *Ejemplo: Un producto prototipo con los datos base de catálogo del cual desprendes variantes con distintos precios, descuentos o colores.*
  2. Requieres clonar objetos sin acoplar el código o depender de sus clases concretas.
