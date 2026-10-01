# 🐍 Preguntas de Django para Entrevistas

> [!info] **¿Qué es Django?**
> Es un kit de herramientas de Python que agiliza el desarrollo de proyectos web de forma rápida y limpia.

---

## 🧰 ¿Qué trae Django?

> [!abstract] **Kit de Herramientas Incluido**
> 1. CRUD básico.
> 2. Plantillas.
> 3. Manejo de formularios.
> 4. Gestión de usuarios con permisos basados en roles.
> 5. ORM.
> 6. Test.

> [!success] **Beneficios**
> 1. Velocidad de desarrollo con menos código.
> 2. Seguridad para autenticaciones de roles de usuarios y contraseñas seguras.
> 3. Escalable.

---

## ⚔️ Comparativas

### 🐍 Django vs Flask

| Aspecto | Django | Flask |
| :--- | :--- | :--- |
| **Enfoque** | Framework completo y opinado | Ligero y flexible |
| **Ideal para** | Proyectos grandes y estructurados | APIs pequeñas o componentes modulares |
| **Componentes** | ORM, admin, auth integrados | Mínimos, extensible con librerías |

### 🐍 Django vs Django Rest Framework

> [!faq]+ **Django (base)**
> Es el framework general de Python para hacer aplicaciones web (monolitos o microservicios). Se pueden hacer APIs pero es todo manual, más tiempo y código.

> [!faq]+ **Django REST Framework (DRF)**
> Es un complemento de Django pensado específicamente para crear APIs REST (JSON). Es lo que se usa en arquitecturas de microservicios y cuando el cliente es otro framework de frontend o servicio.

> [!tip] **Ventajas de DRF**
> * 🔥 ViewSets (como `@RestController` en Spring Boot)
> * 🔥 Serializers (JSON automático)
> * 🔥 Routers (URLs automáticas)
> * 🔥 Paginación automática
> * 🔥 Filtros/search automáticos
> * 🔥 JWT/OAuth integrado
> * 🔥 Documentación API automática

---

## 🏗️ Arquitectura y Patrones

### 🏛️ MVC vs MTV

> [!faq]+ **MVC (Model-View-Controller)**
> Arquitectura dividida en 3:
> * **Modelo:** Comunicación con la base de datos.
> * **Controlador:** Intermedio que procesa los datos.
> * **Vista:** Interfaz donde se muestra la información.

> [!faq]+ **MTV (Model-View-Template)**
> Arquitectura de Django dividida en 3:
> * **Modelo:** Comunicación con la base de datos.
> * **Vista:** Funciona como el controlador, procesa los datos.
> * **Template (Plantilla):** Es la vista que muestra los datos al usuario.

### 🗂️ Relaciones de Bases de Datos

> [!faq]+ **Monolito**
> No se usan microservicios. Todo está en una base de datos con sus tablas y se tienen que hacer las llaves Foráneas para las relaciones.

> [!faq]+ **Microservicios**
> Las tablas están en su propia base de datos, independientemente si es el mismo gestor o no. No hay llaves Foráneas. Para hacer las relaciones, los microservicios se consumen entre sí a través de protocolos HTTP y URLs.

---

## 🧪 ORM y Migraciones

> [!info] **¿Qué es el ORM?**
> El ORM en Django es la capa que te permite trabajar con la base de datos usando objetos de Python en lugar de escribir SQL directamente. Django convierte tus modelos en tablas y tus consultas en operaciones más naturales dentro del código.

> [!code] **Mapeo y Tabla**
> * **Mapeo:** `class Modelo(models.Model)`
> * **Tabla:** `class Meta: db_table = "tabla"`

> [!tip] **Migraciones**
> Para que el ORM funcione, se necesitan hacer las migraciones:
> 1. `python manage.py makemigrations` ← GENERA migración
> 2. `python manage.py migrate` ← APLICA a BD

---

## 🔧 Request y Serialización

### 📨 ¿Qué es `request` y para qué?

> [!abstract] **Request contiene diferentes tipos de peticiones que hace el cliente al microservicio:**
> * `request.method` → GET, POST, PUT, DELETE
> * `request.GET` → `?nombre=Fido`
> * `request.data` → `{"nombre": "Fido"}` (JSON)
> * `request.user` → Usuario autenticado
> * `request.auth` → Token JWT

### 🔄 Serialización y Deserialización

> [!info] **Serialización**
> Proceso de convertir un objeto a bytes. Esto ocurre cuando queremos obtener (GET) información. Los datos se transmiten a través de APIs en formato JSON o XML, y se guardan en Bases de datos.

> [!info] **Deserialización**
> Proceso inverso: toma los bytes y reconstruye el objeto a datos originales. Pasa cuando guardamos datos o enviamos a la base de datos (POST) en formato JSON.

> [!success] **Importancia**
> * **¿Por qué sirve?** Comunica el Backend con el Frontend de forma automática, segura y validada. Es el puente entre la base de datos y las APIs JSON.
> * **Positivo:** Validaciones automáticas, conversión a JSON automática, menos código, más rápido.
> * **Negativo (sin ello):** En el código tendríamos que hacerlo todo manual, mucho código, más tiempo.

---

## 🔗 API, Endpoints y Serializers

### 🌐 ¿Qué es una API?

> [!info]
> Es un conjunto de EndPoints de un microservicio que serán consumidos por un cliente (Angular) u otros servicios.

### 📍 ¿Qué es un Endpoint?

> [!info]
> Es una URL específica dentro de una API que representa un recurso o una acción. Por ejemplo `/api/inventario/` es un endpoint que puede responder a un GET para listar productos o a un POST para agregar uno nuevo.

### 🔀 APIView vs ViewSet

> [!faq]+ **APIView**
> Son EndPoints personalizados manuales, como uno de Detalles_Venta.

> [!faq]+ **ViewSet**
> Son los EndPoints que nos da en automático DRF (CRUD básico).

### 📦 Serializer vs ModelSerializer

> [!faq]+ **`serializers.Serializer`**
> Para serializar tendríamos que colocar los campos manualmente campo por campo, es más tardado.

> [!faq]+ **`serializers.ModelSerializer`**
> Podemos mandar todos los campos en automático, con seleccionar todos. Mucho más fácil y rápido.

### 🧪 TestCase vs APITestCase

| Aspecto | TestCase (Django) | APITestCase (DRF) |
| :--- | :--- | :--- |
| **Cliente HTTP** | `django.test.Client` | `rest_framework.test.APIClient` |
| **Responses** | HTML por defecto | **JSON** por defecto |
| **Transacciones DB** | 2 `atomic()` | 1 `atomic()` (más lento) |
| **Velocidad** | ⚡ Más rápido | 🐌 Más lento |
| **Uso memoria** | Menos | Más |

---

## 🌐 Web Services: SOAP vs REST

> [!info] **¿Qué es un Web Service?**
> Es una forma de comunicación entre aplicaciones o dispositivos a través de la red, que permite el intercambio de datos usando protocolos y formatos estándar (HTTP como transporte y XML/JSON como formatos de mensaje), de forma independiente del lenguaje de programación o la plataforma.

### 📋 SOAP

> [!faq]+ **SOAP**
> Es más estructurado, tiene reglas más estrictas y utiliza XML para la comunicación.

### 🔗 REST

> [!faq]+ **REST (Teoría)**
> Es la teoría de un diseño arquitectónico con 6 principios para crear APIs web. Aprovecha los métodos HTTP para la comunicación, es más fácil y rápido de usar.

> [!abstract] **Los 6 Principios REST**
> 1. **Sin estado:** Cada petición es independiente. El servidor no guarda estados de clientes. Se utiliza JWT para contener usuarios y permisos.
> 2. **Cliente-Servidor:** Separación clara entre ambos.
> 3. **Cacheable:** Las respuestas se pueden cachear temporalmente en el navegador, proxy o servidor.
> 4. **Interfaz Uniforme:** URLs + Métodos HTTP estándar.
> 5. **Sistema en capas:** Cliente → API Gateway → Servidor → DB.
> 6. **HATEOAS (Opcional):** Incluir URLs dentro del JSON para funcionar como mapa de navegación.

> [!faq]+ **REST API**
> Implementa la teoría REST pero no usa HATEOAS. Utiliza la configuración de `ModelViewSet` para ser más sencilla.

> [!faq]+ **RESTful API**
> Implementa todos los principios de la teoría REST, incluido HATEOAS (más código).

---

## 🛡️ Seguridad y Autenticación

### 🔐 Métodos de Seguridad

> [!abstract] **5 formas de implementar autenticación:**
> 1. **JWT:** Estándar para autenticación stateless. Se genera un token cuando el usuario inicia sesión y viaja en cada petición para verificar su identidad.
> 2. **Token Authentication:** Django genera un token único por usuario en BD.
> 3. **Basic Authentication:** Solo compara usuario y contraseña, es inseguro.
> 4. **Session Authentication:** Django crea sesión en BD, solo sirve para navegadores, no APIs móviles.
> 5. **OAuth2:** Genera un token que uno mismo valida. Se usa cuando inicia sesión en redes sociales.

> [!important] **Para Microservicios el mejor es: JWT**
> * ✅ Stateless → No guarda sesión en BD
> * ✅ Escalable → 1000 servidores sin problemas
> * ✅ Rápido → Verifica firma (sin BD)
> * ✅ Seguro → Firma criptográfica + expira
> * ✅ Roles → claims (admin, vet, cliente)
> * ✅ Refresh → Tokens largos para re-login

### 🛡️ ¿Cómo proteges una API?

> [!success]
> Con autenticación JWT, validación de roles, control de permisos, CORS bien configurado y validación de entrada para evitar datos inválidos o maliciosos.

---

## 🧪 Excepciones y Manejo de Errores

> [!warning] **Excepciones**
> Son errores en tiempo de ejecución que interrumpen el flujo del código. Se manejan de diferentes formas dependiendo del tipo de error.

> [!abstract] **Estrategias de Manejo**
> * **Manejo de permisos (APIs):** Implemento un `custom_exception_handler` global en `settings.py` que intercepta excepciones como `PermissionDenied` (403) y `NotAuthenticated` (401), personalizando la respuesta con el status correspondiente.
> * **Depuración rápida:** Capturo datos con `print()` para verificar si los estoy recibiendo correctamente.
> * **Lógica de negocio en Views:** Uso `try-except` para manejar errores específicos:
>   - `ObjectDoesNotExist` cuando un registro no se encuentra en la BD.
>   - `ValueError` cuando los datos recibidos tienen un formato incorrecto.
> * **Errores en formularios:** Django los captura automáticamente a través de su sistema de validación con `form.is_valid()`.
> * **`raise`:** Lanzar una excepción manualmente cuando detectas una condición inválida.
> * **`logging`:** En producción se usa el módulo `logging` de Python para registrar errores en archivos o servicios externos sin que el usuario los vea.

---

## 🔌 Conectividad: WSGI y ASGI

> [!info] **¿Qué es WSGI?**
> Es una interfaz estándar para la comunicación entre servidores y aplicaciones web. Nginx (servidor web) redirige a la aplicación, y WSGI convierte las rutas HTTP a formato Python.

> [!abstract] **Ejemplo: Flujo en un Hotel**
> 1. Cliente (Angular): "Quiero ver cliente ID 1"
> 2. Hotel (Nginx): "Recepcionista, atiende esto → Puerto 8001"
> 3. Recepcionista WSGI: "¿Habitación 205? → Llamo ahí"
> 4. Habitación 205 (ClienteViewSet): "Aquí datos del cliente Juan"
> 5. Recepcionista WSGI: "Señor, aquí tiene la info de Juan" (JSON)

> [!important] **WSGI vs ASGI**
> * **WSGI:** Más lento, peticiones una a la vez. Es como una camioneta con carga.
> * **ASGI:** Más rápido, 100 peticiones a la vez (chats, streaming). Es como un tráiler.

---

## 🔗 Inyección de Dependencias

> [!info] **¿Qué es?**
> Consiste en suministrar a una clase las dependencias (como repositorios o servicios) que necesita para funcionar, en lugar de crearlas. Mantiene el código limpio, modular, aumentando la flexibilidad, el mantenimiento y la estabilidad.

> [!tip] **En Django**
> * **Django puro:** No usa el modelo estándar de DRF. No tiene inyección de dependencias automáticas, es 100% manual (Crearlas, Inyectarlas y Controlarlas).
> * **Django Rest:** Usa el modelo de DRF que ayuda a automatizar procesos (CRUD básico, Serializado, URLs con Router). No se crean ni se inyectan las dependencias, pero sí tenemos el control.

---

## 🧪 Pruebas

> [!info] **Filosofía de Pruebas**
> "Me gusta probar la lógica de negocio aislada con unit tests, y después validar endpoints o flujos completos con pruebas de integración."

### 🧪 Pruebas Django vs Postman

| Aspecto | Tests Django | Postman |
| :--- | :--- | :--- |
| **Propósito** | Automatización + CI/CD | Pruebas manuales/ad-hoc |
| **Velocidad** | Segundos (100s tests) | Minutos (1 petición) |
| **Repetición** | Automática ilimitada | Manual cada vez |
| **Base de datos** | DB temporal limpia | DB real |
| **CI/CD** | ✅ Integra GitHub Actions | ❌ Manual |
| **Cobertura** | Lógica + edge cases | Solo endpoints felices |

> [!tip] **Recomendación Práctica**
> * **90% Test Django:** Toda lógica crítica, autenticación, permisos, validaciones.
> * **10% Postman:** Propósito inicial, compartir con equipo frontend.

> [!important] **Tip**
> Los tests Django te salvan cuando refactorizas. Postman solo confirma "HOY FUNCIONA".
> * **Django Nativo:** Se usa la clase `TestCase`.
> * **Django RestFramework:** Se usa `APITestCase` para endpoints más avanzados.

---

## 🗫 Respuestas Estratégicas para Entrevistas

### 🟩 ¿Qué es la diferencia entre REST y microservicios?

> [!faq]+ **Respuesta**
> REST es un estilo de comunicación entre servicios mediante HTTP. Los microservicios son una arquitectura donde cada funcionalidad del sistema vive en un servicio independiente. En Polihules y Textiles usé APIs REST para conectar el Backend con Angular, y en ambos casos los servicios estaban separados por responsabilidad, lo que técnicamente ya es una arquitectura de microservicios.

### 🟩 ¿Cómo separaste tus servicios?

> [!faq]+ **Respuesta**
> En Textiles separé los servicios por responsabilidad de negocio. Tenía un servicio para gestión de inventarios, otro para control de lotes y otro para generación de reportes. Cada uno exponía sus endpoints REST de forma independiente y Angular los consumía según el módulo que necesitara. La idea era que si algo fallaba en reportes, no afectara el control de inventarios.

### 🟩 ¿Qué problema tuviste al implementar microservicios?

> [!faq]+ **Respuesta**
> El principal reto fue mantener la consistencia de datos entre servicios, por ejemplo cuando un lote se actualizaba en inventarios, el módulo de reportes tenía que reflejar ese cambio en tiempo real. Lo manejé sincronizando a través de las APIs REST. Una arquitectura de microservicios pura con mensajería asíncrona como Kafka o RabbitMQ es algo que me interesa profundizar.

### 🟩 ¿Cómo manejas las transacciones en microservicios?

> [!faq]+ **Respuesta**
> En mis proyectos manejé transacciones dentro de un mismo servicio usando el ORM de Django, que permite hacer rollback automático si algo falla. Para transacciones entre servicios distintos, lo manejé verificando el estado de cada operación y en caso de error ejecutando una operación compensatoria manualmente. Sé que a mayor escala existe el patrón Saga para manejar transacciones distribuidas, que es algo que quiero profundizar.

### 🟩 ¿Puedes explicar el flujo completo de una API?

> [!faq]+ **Respuesta**
> El usuario hace una acción en el frontend (presiona un botón para consultar inventario). Angular genera una petición HTTP con el método correcto (GET) hacia un endpoint específico del backend Django. Esa petición llega al router de Django que la dirige al view o viewset correspondiente. Ahí se ejecuta la lógica de negocio, se consulta la base de datos mediante el ORM, y se construye la respuesta. Django serializa esa respuesta a JSON y la devuelve con un código HTTP (200 si fue exitoso). Angular recibe el JSON, lo procesa y actualiza la interfaz. Si hubo un error, el backend devuelve un código como 400 o 500 y el frontend lo maneja mostrando un mensaje al usuario.

### 🟩 ¿Qué puedes aportarnos que otros candidatos no puedan?

> [!faq]+ **Respuesta**
> Me manejo bien con su stack porque tengo experiencia real con Django, Angular y bases de datos relacionales. Pero lo que creo que me diferencia es que también tengo experiencia en Java con Spring Boot, lo que me da una visión más amplia de arquitecturas backend. Además he trabajado en industrias distintas, textiles y automotriz, lo que me enseñó a adaptarme rápido a contextos de negocio diferentes. Y tengo resultados concretos: en Textiles redujimos un 40% el tiempo de control de inventarios. No solo conozco el stack, sé usarlo para generar impacto real.

### 🟩 ¿Por qué quieres este puesto?

> [!faq]+ **Respuesta**
> Me interesa este puesto porque el stack que manejan, Python y Django con frontend en Angular, es exactamente donde tengo experiencia real y resultados concretos. No tendría que adaptarme al stack, podría enfocarme en entender el negocio y aportar rápido. En los primeros tres meses me enfocaría en entender bien la arquitectura existente, identificar áreas de mejora en rendimiento o procesos, y demostrar que puedo tomar responsabilidades sin necesitar mucha supervisión.

### 🟩 ¿Has dado soporte a sistemas en producción?

> [!faq]+ **Respuesta**
> Sí, en ambas empresas. En Textiles monitoreaba los procesos automatizados y corregía errores en los scripts de alertas.

### 🟩 ¿Alguna vez cometiste un error en un proyecto? ¿Cómo lo manejaste?

> [!faq]+ **Respuesta**
> En Textiles, en una actualización del módulo de reportes, un cambio en los modelos de Django afectó una consulta que no tenía cubierta en pruebas. El reporte llegó a producción con datos incorrectos por unas horas que afectó a los supervisores de turno, así que había urgencia. Lo detecté revisando logs, revertí el cambio, corregí la consulta y agregué pruebas para ese caso específico. Desde entonces soy más cuidadoso con las migraciones y reviso siempre los datos de salida antes de liberar.

### 🟩 ¿Cuál es la diferencia entre autenticación y autorización?

> [!faq]+ **Respuesta**
> La autenticación es identificarse y validarse como usuario registrado en la base de datos para poder entrar a la plataforma. La autorización es, después de autenticarse, poder realizar acciones que solo ciertos usuarios autenticados puedan realizar. En Textiles manejé roles de usuario, donde los operarios tenían permisos de lectura y escritura, pero solo los administradores podían editar o eliminar registros, esto con JWT. Una vez autenticado el usuario, el token cargaba su rol y Django verificaba ese rol en cada endpoint para permitir o denegar la acción.

### 🟩 ¿Qué proyectos harías en Python puro sin framework?

> [!faq]+ **Respuesta**
> Python puro lo usaría para scripts de automatización y procesamiento de datos, donde no necesitas la estructura completa de un framework. En Textiles por ejemplo, los scripts de alertas de stock y generación de reportes los hice en Python puro porque eran procesos independientes que solo necesitaban conectarse a la base de datos y ejecutar lógica. Para una aplicación web completa siempre elegiría Django o FastAPI porque ya resuelven problemas como ruteo, autenticación y serialización que no tiene sentido construir desde cero.

### 🟩 ¿Cuál fue el resultado principal de automatizar los procesos operativos?

> [!faq]+ **Respuesta**
> Se redujo en un 40% el tiempo dedicado al control de inventarios.
