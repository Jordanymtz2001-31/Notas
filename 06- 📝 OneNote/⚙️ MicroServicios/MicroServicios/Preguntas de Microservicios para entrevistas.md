# ⚙️ Arquitectura de Microservicios

> [!info] **¿Qué son los Microservicios?**
> Es un estilo arquitectónico donde una aplicación se construye como un **conjunto de servicios pequeños e independientes**. Cada uno ejecuta su propia lógica de negocio, tiene su propio dominio y se comunica a través de la red.

---

## 🏛️ Monolito vs. Microservicios

### 📦 Arquitectura Monolítica
* **Concepto:** Todos los componentes y módulos están acoplados dentro de un único proyecto. Todos dependen de todos.
* **Base de Datos:** Comparten una única base de datos global (normalmente relacional).
* **Comunicación interna:** Las relaciones ocurren en la memoria mediante anotaciones de JPA/Hibernate como:
  `@ManyToOne`, `@OneToMany`, `@OneToOne`, `@ManyToMany`.

### 🧩 Arquitectura de Microservicios
* **Concepto:** Los módulos se rompen en piezas independientes que se despliegan por separado.
* **Base de Datos:** Cada microservicio tiene su propio dominio y **su propia base de datos** independiente.
* **Comunicación externa:** Se relacionan mediante peticiones de red utilizando **HTTP REST** (ej. vía **FeignClient** o *RestTemplate*) mapeando los registros mediante sus IDs.

> [!success] 📈 Características y Ventajas
> 1. **Identidad Clara:** Cada microservicio tiene su propio nombre para ser identificado.
> 2. **Despliegue Independiente:** Cada uno se puede escalar, actualizar o modificar sin afectar a los demás.
> 3. **Aislamiento de Fallos:** Si un servicio se cae, los demás pueden seguir funcionando con normalidad.
> 4. **Equipos Ágiles:** Permite trabajar con un mínimo equipo de trabajo enfocado por módulo.

> [!warning] 📉 Desventajas
> * **Alto consumo de memoria:** Ejecutar múltiples instancias y bases de datos por separado requiere más recursos de infraestructura.
> * **Complejidad de pruebas:** Los testeos y despliegues (pruebas de integración) se vuelven mucho más complicados al depender de la red.

---

## 💬 Modelos de Comunicación en la Red

> [!faq]+ ⏳ Comunicación Síncrona (Bloqueante)
> Es un modelo donde el emisor envía una petición directa al receptor a través de la red y **suspende su ejecución** (bloquea el hilo de procesamiento) hasta recibir una respuesta inmediata.
> * **Si el servidor destino se cae:** La llamada se corta inmediatamente. El emisor recibe un error de conexión directo y la aplicación cliente (ej. Angular) mostrará una falla en pantalla a menos que se implemente resiliencia.

> [!faq]+ ✉️ Comunicación Asíncrona (No Bloqueante)
> Es un modelo basado en eventos o mensajes. El emisor publica la información en un intermediario (**Message Broker**) y **continúa su ejecución de inmediato**, permitiendo que el receptor procese el mensaje a su propio ritmo.
> * **Si el servidor destino se cae:** El mensaje se queda guardado de forma segura (como un "check gris") en el servidor de mensajería. En cuanto el receptor revive, lee los mensajes acumulados y ejecuta las tareas pendientes sin perder datos.

---

## 🛠️ Herramientas de Conectividad (Clientes HTTP)

### 🔀 RestTemplate vs. OpenFeign
Ambas son librerías de Java que funcionan como clientes HTTP para conectar microservicios de forma síncrona. Sin embargo, sus enfoques son opuestos:
* **RestTemplate:** Enfoque *imperativo* (tienes que programar manualmente la petición y la ruta). **Está obsoleto.**
* **OpenFeign:** Enfoque *declarativo* (defines una interfaz de Java con anotaciones y Spring se encarga del resto). Es el recomendado para HTTP REST.

### 🌐 WebClient
* **Concepto:** Cliente HTTP moderno, reactivo y no bloqueante (módulo *Spring WebFlux*).
* **Propósito:** Es el **reemplazo oficial y definitivo** de *RestTemplate*. Soporta tanto la comunicación síncrona tradicional como la programación reactiva asíncrona de alto rendimiento.

---

## 🗺️ Patrones Arquitectónicos Esenciales

### 📖 Service Discovery & Registry (El Directorio)
* **El Problema:** En entornos modernos (como *Docker* o la nube), los contenedores cambian de dirección IP y puertos constantemente al reiniciarse. Configurar IPs fijas (*hardcoded*) en el `application.yml` romperá el sistema.
* **La Solución:** Un patrón que actúa como un directorio telefónico automatizado en tiempo real.

> [!abstract] 🔍 Componentes Clave:
> * **Service Registry:** La base de datos o "libro de matrícula" donde se almacena el listado con las IPs y puertos de los servicios activos.
> * **Service Discovery (Eureka Server):** El servicio que usa Spring Boot para que los microservicios se registren al encenderse (ej: *"Soy Pagos y estoy en la IP:Port X"*) y para que otros puedan buscar su ubicación usando solo su **nombre** comercial.

### 🔌 API Gateway (La Puerta de Entrada)
* **Concepto:** Actúa como un punto de acceso único o puerto de entrada centralizado para los clientes externos (como el frontend de Angular).
* **Función:** Enlaza internamente los puertos individuales de todos los microservicios y expone **un solo puerto unificado** para todo el sistema, gestionando el enrutamiento y la seguridad.

### 🛡️ Circuit Breakers & Resiliencia
* **Resiliencia:** Capacidad del sistema para seguir funcionando (aunque sea de forma degradada) cuando uno o más servicios fallan.
* **Circuit Breaker (Disyuntor):** Patrón que corta las peticiones hacia un servicio dañado para evitar un efecto dominó catastrófico en la aplicación.

> [!important] ⚙️ La Máquina de Estados del Circuit Breaker:
> 1. **🟢 Cerrado (Closed) — Estado Normal:** Todo funciona bien. Las peticiones HTTP fluyen normalmente del Microservicio A al B.
> 2. **🔴 Abierto (Open) — Estado de Falla:** Si el porcentaje de errores supera un límite (ej. 50% de fallas), el circuito se abre. Las llamadas se rechazan inmediatamente y se ejecuta una respuesta alternativa (**Fallback**) para darle tiempo al servicio caído de recuperarse.
> 3. **🟡 Semi-Abierto (Half-Open) — Estado de Prueba:** Pasado un tiempo, se permite el paso de unas pocas peticiones de prueba. Si tienen éxito, el circuito se vuelve a **Cerrar** (normalidad). Si fallan, se vuelve a **Abrir** y reinicia el temporizador.

---

## 🐋 Infraestructura: El rol de Docker
* **Concepto:** Automatiza por completo el despliegue de las aplicaciones. 
* **Flujo:** Permite crear una **Imagen** (la receta base) de cada uno de tus microservicios y empaquetarla en un **Contenedor**.
* **Ventaja:** Garantiza que tu sistema Spring Boot funcione exactamente igual en cualquier computadora o servidor sin necesidad de instalar manualmente versiones de Java, librerías o dependencias locales, acelerando los despliegues.
