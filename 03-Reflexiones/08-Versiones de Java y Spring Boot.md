---
tipo: reflexion
estatus: ✅ Resuelta
categoria: "☕ Java → Versiones"
fecha_registro: "2026-10-19"
fecha_resolucion: "2026-10-19"
fuente_origen: "Conversación Perplexity + Oracle Java SE Support Roadmap + Spring Boot 4.0 Release Notes"
---

# 08-Versiones de Java y Spring Boot

> [!info] ❓ **La Duda / Reflexión**
> ¿Cuáles son las versiones de Java y Spring Boot más importantes a conocer para trabajo y entrevistas? ¿Qué cambió en cada una y cómo afecta la compatibilidad entre ellas?

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Tarea de documentar versiones de Java y Spring Boot (LTS) y sus cambios más relevantes.
*   **Sospecha inicial:** Conocer solo la versión no basta — hay que entender compatibilidad (qué versión de Java necesita cada Spring Boot), no confundir conceptos y saber qué distinciones evitar en entrevistas.

---

## 🔍 Investigación y Respuestas

### 1. Diferencia entre Java, Spring Framework y Spring Boot

> [!abstract] Capas distintas, no alternativas
> * **Java:** lenguaje, bibliotecas estándar y plataforma de ejecución (JVM).
> * **Spring Framework:** framework base (inyección de dependencias, web, integración, etc.).
> * **Spring Boot:** facilita crear y ejecutar aplicaciones Spring (autoconfiguración, dependencias coordinadas, convenciones). **Se apoya en Spring Framework, no lo sustituye.**

*Ejemplo típico:* "Java 17 + Spring Boot 3 + Spring Framework 6" — son **capas** del mismo proyecto (Boot 3 se construye sobre Spring Framework 6 y requiere Java 17 mínimo).

### 2. Versiones LTS de Java y cambios clave

| Versión | Año (lanzamiento) | Cambio principal que conviene asociar |
|---|---|---|
| **Java 8 (LTS)** | Marzo 2014 | **Lambdas y Streams**: procesar colecciones declarativamente (filtrar/map/reducir). Base muy presente en sistemas existentes. |
| **Java 11 (LTS)** | Septiembre 2018 | Punto LTS tras Java 8. `var` apareció en **Java 10**, no en 11 (11 permite `var` en parámetros de lambdas). |
| **Java 17 (LTS)** | Septiembre 2021 | Java "moderno": **Text Blocks** (Java 15), **Switch expressions** (Java 14), **Records** (Java 16), **Pattern Matching para `instanceof`** (Java 16), **Clases selladas (sealed classes)** (Java 17). |
| **Java 21 (LTS)** | Septiembre 2023 | **Virtual Threads** (concurrencia ligera, estilo bloqueante), **Pattern Matching para `switch`** (definitivo en 21) y mejoras de legibilidad. |
| **Java 25 (LTS)** | Septiembre 2025 | Nueva LTS: **Scoped Values**, simplificaciones (`main` de instancia, archivos fuente compactos), mejoras en AOT, rendimiento y diagnóstico. *Structured Concurrency* aún preview en 25. |

> [!note] **Nota**
> Las fechas corresponden al lanzamiento inicial (no parches). 8, 11, 17, 21 y 25 son LTS. El siguiente LTS previsto es **Java 29 (Septiembre 2027)** (Oracle Java SE Support Roadmap).

**Ejemplos breves:**

```java
// Streams (Java 8)
List<String> nombres = usuarios.stream()
    .filter(Usuario::isActivo)
    .map(Usuario::getNombre)
    .toList();

// var (Java 10)
var mensaje = "Hola"; // inferido a String

// Record (Java 16, disponible desde 17)
public record UsuarioResponse(Long id, String nombre) {}

// Pattern matching con instanceof (Java 16)
if (obj instanceof String texto) {
    System.out.println(texto.toUpperCase());
}

// Pattern matching con switch (Java 21)
static String describir(Object dato) {
    return switch (dato) {
        case Integer n -> "Número: " + n;
        case String s  -> "Texto: " + s;
        default        -> "Otro tipo";
    };
}
```

### 3. Versiones de Spring Boot y cambios clave

| Versión | Año | Cambio principal |
|---|---|---|
| **Spring Boot 2.0** | Febrero 2018 | Inicio de rama 2.x. Configuraciones basadas en herencia aún comunes (`WebSecurityConfigurerAdapter`). |
| **Spring Boot 2.7** | 2022 | Última rama 2.x (puente de migración). El estilo de Security empezó a moverse hacia beans (`SecurityFilterChain`). Soporte OSS 2.7.18 finalizó 23/nov/2023. |
| **Spring Boot 3.0** | Noviembre 2022 | **Salto de generación**: **Java 17 mínimo**, **Spring Framework 6**, **migración Java EE → Jakarta EE** (`javax.*` → `jakarta.*`), soporte para **imágenes nativas** con GraalVM (Spring Native sustituido). |
| **Spring Boot 3.2** | Noviembre 2023 | Integra **Virtual Threads** (Java 21) (`spring.threads.virtual.enabled=true`), **RestClient**, y mejoras de observabilidad/CRaC. |
| **Spring Boot 4.0** | Noviembre 2025 | Se apoya en **Spring Framework 7**. Cambios relevantes: **modularización**, **Jackson 3 por defecto** (Jackson 2 deprecado con transición temporal), enfoque en **null-safety** (JSpecify), evolución de APIs/HTTP Service Clients. |

**Ejemplos breves:**

```text
# Virtual Threads (requiere Java 21 + Boot 3.2+)
spring.threads.virtual.enabled=true
```

```java
// Jakarta EE (Boot 3+)
import jakarta.persistence.Entity;
import jakarta.validation.Valid;
// Antes (Boot 2.x): javax.persistence.*, javax.validation.*
```

```java
// Security moderno (SecurityFilterChain) en lugar de heredar WebSecurityConfigurerAdapter
@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.authorizeHttpRequests(auth -> auth
        .requestMatchers("/public/**").permitAll()
        .anyRequest().authenticated());
    return http.build();
}
```

### 4. Compatibilidad entre Java y Spring Boot (práctica)

| Spring Boot | Java mínimo/recomendado |
|---|---|
| **2.0 – 2.6** | Java 8+ (recomendado 8/11) |
| **2.7.x** | Java 8+ (ideal para migrar desde 2.7 → 3.0) |
| **3.0.x – 3.1.x** | **Java 17+** (no compatible con 8/11) |
| **3.2+** | **Java 17+**; aprovecha mejor **Java 21** (Virtual Threads) |
| **4.0+** | **Java 17+** (se basa en Spring Framework 7; Boot 4 apunta a ecosistema moderno; Java 21+ recomendable según casos de uso) |

> [!tip] **Regla práctica**
> Si migras **Boot 2.7 → Boot 3.0**, **obligas también actualización de JDK a Java 17+** (además del cambio `javax`→`jakarta`).

### 5. Distinciones comunes que evitan errores en entrevistas

| Afirmación imprecisa | Forma correcta |
|---|---|
| *"var apareció en Java 11"* | Apareció en **Java 10**. Disponible con Java 11. |
| *"Los records aparecieron en Java 17"* | Quedaron definitivos en **Java 16**. Están disponibles en Java 17. |
| *"Spring Boot 3 creó `SecurityFilterChain`"* | El cambio hacia configuración por beans pertenece a **Spring Security** (5.7+); Boot 3 lo adoptó como estilo recomendado. |
| *"Java 21 activa virtual threads en cualquier Boot"* | Necesita integración compatible: **Spring Boot 3.2+** integra su uso con los componentes del framework. |
| *"Todos los `javax` cambian a `jakarta`"* | Solo cambian los paquetes de **APIs migradas a Jakarta EE** (JPA, Validation, Servlet, etc.), no todas las dependencias indiscriminadamente. |
| *"Boot 4 exige Jackson 3 sin alternativa"* | **Jackson 3 es el predeterminado**. Boot 4.0 documenta una **transición temporal** con soporte a Jackson 2 (deprecado). |

### 6. Uso real (JetBrains 2025)

La encuesta global de JetBrains 2025 (múltiples respuestas) reportó:
**Java 21 (40%)**, **Java 17 (39%)**, **Java 8 (31%)**, **Java 11 (17%)**.

> [!note] **Importante**
> No suman 100% (varias versiones en uso). Esto justifica **estudiar versiones antiguas** (legado), pero **no confunde "se sigue usando" con "conviene elegirla para un proyecto nuevo"**. Para proyectos nuevos, **Java 17 o 21** + **Spring Boot 3.2+** son opciones sólidas.

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
> Java, Spring Framework y Spring Boot son capas distintas. Las LTS clave: 8 (lambdas/streams), 11 (punto de migración), 17 (records, sealed, text blocks), 21 (virtual threads) y 25 (scoped values). Spring Boot 3.0 exige **Java 17 + Jakarta EE**; Boot 3.2 aprovecha **Java 21** con virtual threads; Boot 4.0 se apoya en **Spring Framework 7** con **Jackson 3** por defecto. Lo esencial para entrevistas: **compatibilidad Boot↔Java**, distinguir qué característica nació en qué versión (evitar atribuir `var`/`records` al año erróneo) y saber que el salto `javax→jakarta` es propio de Boot 3/Jakarta EE.

---

## 🚀 Acciones a tomar (Next Actions)
- [ ] Dominar **Java 17 + Java 21** antes de profundizar en Java 25/Boot 4.
- [ ] Practicar migración conceptual Boot 2.7 → Boot 3.0 (javax→jakarta + SecurityFilterChain + Java 17).
- [ ] Repasar la tabla de confusiones comunes para respuestas precisas en entrevistas.