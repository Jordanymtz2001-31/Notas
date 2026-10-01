---
tipo: duda-investigacion
estatus: FInalizado
categoria: Patron de Diseño
fecha_registro: 2026-09-16
fecha_resolucion: 2026-09-16
fuente_origen:
---

> [!info] ❓ **La Duda / Reflexión**
> Escribe aquí la pregunta o el pensamiento inicial de forma clara.
> 1. ¿Cuales son los objetos Inmutables en java y spring?
> 2. 
> 3. 
> 

---

# 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Me realizaron esa pregunta en una entrevista
*   **Sospecha inicial:** La verdad es que nosabia que era.

---

# 🔍 Investigación y Respuestas
### Fuentes Consultadas
* [x] Buscar en Google / Documentación Oficial: ✅ 2026-09-16
* [x] Preguntar a IA / Foros: ✅ 2026-09-16

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
>
>
>## 📋 Catálogo de Objetos Inmutables (Java & Spring)
>
>En una entrevista técnica, los objetos inmutables (aquellos cuyo estado no cambia tras su creación) se clasifican en:
>
>### 1. En Java Puro (Nativos)
>- **`String`:** El objeto inmutable por excelencia en Java. Cualquier modificación genera un nuevo espacio en el String Pool.
>- **Clases Wrapper:** `Integer`, `Long`, `Boolean`, `Double`, etc. Carecen de métodos modificadores.
>- **Java `record`:** Clases restrictivas introducidas en Java 16 ideales para transportar datos puros de forma inmutable.
>- **`Optional`:** Las instancias contenedoras de Optional son inmutables por definición.
>- **Colecciones Inmutables:** Estructuras devueltas por `List.of()`, `Map.of()` o procesadas a través del método `.toList()` de la API de Streams.
>
>### 2. En el Entorno de Spring Boot
>- **Tus DTOs (cuando se diseñan como `record`):** Los objetos de transferencia de datos que procesas en tus controladores. 
>- **Componentes Singleton (Inyección por Constructor):** Los Beans de Spring (`@Service`, `@Controller`) se vuelven inmutables en su estructura cuando declaras sus dependencias como `private final` e inyectadas por constructor, impidiendo mutaciones en caliente durante la ejecución del servidor.
>- **Configuraciones Protegidas:** Mapeos de propiedades de configuración que usan `@ConfigurationProperties` combinados con `@ConstructorBinding` (inmutabilidad en los parámetros de arranque).


---

# 🚀 Acciones a tomar (Next Actions)
- [ ] 
