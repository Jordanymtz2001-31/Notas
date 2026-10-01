---
tipo: duda-investigacion
estatus: FInalizado
categoria: Excepciones
fecha_registro: 2026-08-31
fecha_resolucion: 2026-08-31
fuente_origen: Con la IA de Google
---

> [!info] ❓ **La Duda / Reflexión**
> Escribe aquí la pregunta o el pensamiento inicial de forma clara.
> 1. ¿Como se utilizan los manejadores de excepciones en spring Boot?
> 2. ¿throw, throws, try-catch-finally, GlobalExceptionHandler se reemplazan o es lo mismo?
> 3. ¿Se utilizan siempre o depende del proyecto o caso?
> 4. ¿Cuando usarlos y para que sirven cada uno?

---

# 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Estaba repasando mis notas de [[Preguntas de Java para entrevistas]] y en los puntos de Excepciones me acorde de mi proyecto de Repaso [[002-Gestion de ALumnos]] en el cual implemente Manejador de excepciones y quise investigar al respecto.
*   **Sospecha inicial:**
	*Pense que eran para lo mismo o que se reemplazaban*

---

# 🔍 Investigación y Respuestas
### Fuentes Consultadas
* [x] Buscar en Google / Documentación Oficial: ✅ 2026-08-31
* [x] Preguntar a IA / Foros: ✅ 2026-08-31

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
> # 📌 Resumen de Aprendizaje: Manejo de Excepciones en Spring Boot
> 
> ---
> ## 🛠️ Conceptos Clave: ¿Para qué sirve cada uno?
> No todos los mecanismos sirven para lo mismo. En una arquitectura limpia, se complementan en lugar de sustituirse:
> * **`throw` (El Lanzador):** Es una **acción inmediata**. Se utiliza dentro del cuerpo de un método (por ejemplo, en un bloque `if`) para detener el flujo actual y avisar que se rompió una regla de negocio. Su función es *crear el problema*, no solucionarlo. 
> * **`throws` (La Advertencia):** Es una **declaración en la firma del método** (`public void miMetodo() throws Exception`). Obliga al compilador de Java a exigir un control estricto mediante un bloque `try-catch` a cualquiera que intente usar ese método. Se utiliza para advertir sobre fallos técnicos inevitables (como la caída de una base de datos o un archivo inexistente). 
> * **`try-catch-finally` (La Solución Local):** Se utiliza únicamente cuando se tiene un **plan de respaldo inmediato** para solucionar un error técnico en el mismo instante en que ocurre (por ejemplo, si falla un servidor de base de datos principal, el `catch` reconecta inmediatamente a uno secundario). Si no se puede solucionar el problema ahí mismo, es mejor no usarlo. 
> * **`GlobalExceptionHandler` (La Red de Seguridad):** Es un componente centralizado en la capa web (`@ControllerAdvice`). Su función es interceptar cualquier error que haya escalado desde las capas inferiores y transformarlo en una respuesta HTTP limpia, estandarizada y segura para el usuario final (por ejemplo, un JSON con código de estado `404` o `400`), evitando mostrar código interno (*Stacktrace*). 
> ---
>
> ## 📐 Arquitectura Recomendada en Spring Boot 
> Para mantener el código limpio y desacoplado, la regla de oro en Spring Boot es **combinar `throw` con el `GlobalExceptionHandler` y evitar el uso de `throws`**.
> ### Distribución por Capas: 
> 1. **Capa de Servicios (`@Service`):** 
> * Aquí es donde se aplican las validaciones de negocio. 
> * Si un dato está duplicado o un recurso no existe, se dispara directamente un **`throw new MiExcepcionPersonalizadaException()`**. 
> * **Buenas prácticas:** Las excepciones personalizadas deben heredar de **`RuntimeException`** (excepciones no comprobadas). De esta forma, Java no te obliga a poner la palabra `throws` en la firma de tus métodos, manteniendo el código limpio. 
> 1. **Capa de Controladores (`@RestController`):** 
> * No debe contener bloques `try-catch` para capturar errores de negocio. El controlador solo debe preocuparse por recibir la petición y delegar la lógica al servicio. 
> 1. **Manejador Global (`GlobalExceptionHandler`):** 
> * Se coloca en la periferia de la aplicación (capa Web) usando la anotación `@ControllerAdvice`. 
> * Es el encargado de atrapar las excepciones lanzadas por el servicio y construir la respuesta HTTP final que verá el cliente. 
> ---
> ## 💡 Conclusión para el Proyecto
> El diseño de tu lógica es **impecable y sigue los estándares de la industria**. El uso de métodos funcionales como `.orElseThrow()` y cortes de flujo con `throw new ProfesorDuplicadoException()` en tu servicio permiten que el código de negocio sea legible y que la responsabilidad de dar formato al error recaiga completamente en tu **GlobalExceptionHandler**.



---

# 🚀 Acciones a tomar (Next Actions)
- [x] 8:00 Revisar si el proyecto de [[002-Gestion de ALumnos]] Esta compliendo con estos manejadores de Excepciones para entender mejro el proyecto, de lo contrario mejorarlo. 📅 2026-09-07 ✅ 2026-09-07
