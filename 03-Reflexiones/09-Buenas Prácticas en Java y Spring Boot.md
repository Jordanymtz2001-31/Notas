---
tipo: reflexion
estatus: ✅ Resuelta
categoria: "☕ Java → Buenas Prácticas"
fecha_registro: "2026-10-20"
fecha_resolucion: "2026-10-20"
fuente_origen: "Investigación propia + Spring Framework Docs + OWASP"
---

# 09-Buenas Prácticas en Java y Spring Boot

> [!info] ❓ **La Duda / Reflexión**
> ¿Cuáles son las buenas prácticas que de verdad importan en una API REST con Java y Spring Boot, y cuáles dependen del criterio y no de la costumbre?

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Tarea de documentar buenas prácticas de código limpio y estándares según el tipo de proyecto.
*   **Sospecha inicial:** La mayoría ya las aplico; falta **refinar el criterio** para no caer en "porque así venía el tutorial".

---

## 🔍 Investigación y Respuestas

### 1. La idea central: 4 cosas que proteges

> [!abstract] Criterio antes que recetas
> Cada práctica existe para proteger una de cuatro cosas: **los datos · la seguridad · la capacidad de cambiar el código · la posibilidad de detectar fallos**. Si no sabes qué riesgo resuelves, la práctica sobra.

### 2. Separar responsabilidades e inyección por constructor

```text
Controller → Service → Repository → Base de datos
```

*   **Controller:** recibe HTTP, valida la estructura de entrada, devuelve respuesta.
*   **Service:** ejecuta el caso de uso y las reglas de negocio.
*   **Repository:** consulta y persiste.

```java
@Service
public class PedidoService {
    private final PedidoRepository pedidoRepository;

    public PedidoService(PedidoRepository pedidoRepository) { // inyección por constructor
        this.pedidoRepository = pedidoRepository;
    }
}
```

> [!tip] **Por qué constructor** (Spring Framework Docs, DI)
> Las dependencias quedan **explícitas** y `final`. Puedes instanciar el servicio en una prueba **sin arrancar toda la aplicación**. Es la variante recomendada por Spring frente a la inyección por setter.

**Criterio:** separa por **responsabilidad real**, no por cumplir un número de carpetas. No crees interfaces ni capas que solo reenvían llamadas.

### 3. DTOs y validación en más de un nivel

```java
public record CrearPedidoRequest(
    @NotNull Long clienteId,
    @NotEmpty List<@Valid LineaPedidoRequest> lineas
) {}

public record LineaPedidoRequest(
    @NotNull Long productoId,
    @Positive int cantidad
) {}

@PostMapping
public ResponseEntity<?> crear(@Valid @RequestBody CrearPedidoRequest request) {
    // Delegar al caso de uso
}
```

| Nivel | Ejemplo | Qué protege |
| :--- | :--- | :--- |
| **Validación de entrada** | Cantidad > 0 | Formato y valores recibidos |
| **Regla de negocio** | El pedido solo admite productos disponibles | Coherencia del caso de uso |
| **Restricción de BD** | Correo único / FK válida | Integridad persistida, incluso con concurrencia |

> [!warning] **No confundas "JSON válido" con "operación permitida"**
> OWASP separa los **controles de entrada** de los **controles de autorización**. Nunca confíes en un `total` enviado por el frontend: **recálculalo en el servidor**.

### 4. Seguridad por operación y por recurso

Tres comprobaciones, no una:

*   **Autenticación:** quién es.
*   **Autorización:** qué puede hacer.
*   **Acceso al recurso:** si puede hacerlo sobre *ese* objeto concreto.

```text
GET /pedidos/825   ← autenticado y con rol CLIENTE, pero ¿es SU pedido?
```

> [!danger] **IDOR (acceso directo a objetos insegura)**
> Si el usuario cambia el ID y obtiene el pedido de otro, la ruta estaba autenticada pero **no autorizada por recurso**. Tener rol `CLIENTE` no demuestra propiedad del pedido. En JWT valida **firma, emisor, audiencia y vigencia**, no solo la firma.

**Criterio:** en microservicios, el gateway puede rechazar tráfico, pero **cada servicio protege sus operaciones**. No dependas solo del gateway.

### 5. Externalizar configuración y no guardar secretos

```properties
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}
```

Evita: credenciales en Git · recompilar para cambiar una URL · usar config de desarrollo en producción.

> [!warning] **Un perfil `prod` no protege secretos**
> Un perfil solo **selecciona** configuración. La protección depende de **dónde guardas** los valores y **quién** los lee. Nunca los pongas en URLs ni en logs.

### 6. Transacciones bien delimitadas

```java
@Transactional
public Pedido crearPedido(CrearPedidoRequest request) {
    // reglas + crear pedido + crear líneas + actualizar
    return pedido;
}
```

> [!warning] **Limitaciones de @Transactional** (Spring Framework Docs)
> *   **Self-invocation:** una llamada de un método a otro **dentro del mismo objeto** no activa la anotación del segundo (modo proxy).
> *   **Rollback por defecto** solo ante `RuntimeException` y `Error`. Las **checked exceptions NO** revierten salvo que configures `rollbackFor`.
> *   Si **capturas y ocultas** el error, impides el rollback.
> *   No revierte un pago ya hecho en una **API externa** ni cambios de **otro microservicio**.

### 7. Migraciones de esquema, no `ddl-auto`

```text
V1__crear_clientes.sql
V2__crear_pedidos.sql
V3__agregar_estado_pedido.sql
```

Versiona con **Flyway** o **Liquibase** (Boot integra ambas; usa **un solo** mecanismo).

> [!warning] **Nunca `ddl-auto=update` en producción**
> Sirve para desarrollo, no como estrategia de evolución. Las migraciones te dejan revisar y controlar cada cambio de forma reproducible. Un script versionado **no** equivale a poder recuperar los datos: los cambios destructivos necesitan respaldo y plan de recuperación.

### 8. Errores consistentes

Centraliza con `@RestControllerAdvice`; en Spring moderno usa **`ProblemDetail`** (RFC 9457).

```json
{
  "type": "about:blank",
  "title": "Pedido no encontrado",
  "status": 404,
  "detail": "No existe el pedido solicitado."
}
```

> [!tip] **Contrato claro, no filtraciones**
> El cliente recibe un formato predecible. OWASP recomienda que las respuestas de error **no revelen** trazas, SQL ni rutas internas. No conviertas todo en `200 OK` con `"success": false`, ni en un genérico sin registrar la causa internamente.

### 9. Probar el comportamiento, según el riesgo

| Tipo | Qué comprueba |
| :--- | :--- |
| **Unitaria** | Cálculos, decisiones, reglas de negocio |
| **Web** | Validación, respuestas HTTP, seguridad |
| **Persistencia** | Consultas, relaciones, restricciones |
| **Integración** | Que componentes y configuración funcionan juntos |

> [!tip] **No uses `@SpringBootTest` para todo**
> Una prueba que confirma que "Spring arranca" **no** demuestra que un usuario no pueda modificar pedidos ajenos. Si pruebas una regla pura, no necesitas cargar todo el contexto.

### 10. Observabilidad sin filtrar datos

Logs con contexto, **métricas**, **health checks** y trazabilidad (Spring Boot Actuator).

> [!warning] **Cuidado con lo que registras**
> No expongas endpoints administrativos sin control y **nunca** registres tokens, contraseñas ni datos sensibles. `System.out.println()` no es una estrategia operativa.

### 11. Dependencias compatibles y builds reproducibles

Usa el **parent/BOM** de Spring Boot: coordina versiones probadas en conjunto (Spring Framework, Jackson, servidores…).

**Criterio:** si sobrescribes una versión, trátalo como **decisión con pruebas**, no como arreglo inocuo en el `pom.xml`. En el repo: compilación y tests automáticos, revisión de cambios, actualizaciones controladas y **misma versión de Java** en desarrollo y build.

### 12. Llamadas externas: fallar de manera controlada

Define explícitamente **timeouts**, qué se reintenta, cuántos intentos y qué pasa si sigue caído.

> [!danger] **Reintentar no es gratis**
> Si un `POST` de cobro termina en timeout, **no sabes** si el proveedor lo procesó. Antes de reintentar necesitas **idempotencia** o consultar el resultado. No añadas reintentos a una operación con efectos secundarios sin entender cómo evitas duplicados.

### 13. Lo que NO es una "práctica obligatoria"

| No lo hagas por costumbre | Evalúa antes |
| :--- | :--- |
| Microservicios para cualquier app | ¿Necesitas despliegue/escalado/responsabilidades independientes? |
| Una interfaz para cada clase | ¿Hay un contrato útil o desacoplamiento real? |
| Streams para todo | ¿Mejora la lectura frente a un bucle? |
| Caché en cada consulta lenta | ¿Primero hay un problema de consulta, volumen o modelo? |
| Virtual threads asumiendo velocidad | ¿Qué recurso limita realmente la app? |
| Todos los patrones que conoces | ¿Qué problema concreta resuelve cada uno? |

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
> Las buenas prácticas en Java/Spring Boot no son recetas: protegen **datos, seguridad, evolutividad y diagnóstico**. Lo esencial: capas con responsabilidad real e **inyección por constructor**; **DTOs + validación en 3 niveles**; seguridad **por operación y por recurso** (no solo rol); config **externalizada** y secretos fuera del repo; **`@Transactional`** con sus límites (self-invocation y rollback de checked); **migraciones** en vez de `ddl-auto`; errores con **`ProblemDetail`**; pruebas por riesgo; **observabilidad** sin filtrar datos; **BOM** para dependencias; y llamadas externas con **timeouts e idempotencia**. Lo demás depende del proyecto: la meta es poder **explicar cada pieza**, no repetir "así venía el tutorial".

---

## ✅ Checklist mínimo antes de entregar una API

*   [ ] Responsabilidades separadas y dependencias explícitas.
*   [ ] Entrada externa validada y datos aceptados delimitados.
*   [ ] Permisos comprobados **sobre la operación y el recurso concreto**.
*   [ ] Sin secretos en el repositorio, URLs ni logs.
*   [ ] Transacciones cubren la unidad de trabajo correcta.
*   [ ] Cambios de esquema versionados.
*   [ ] Errores con formato consistente y sin detalles internos.
*   [ ] Pruebas de negocio, fallos y permisos.
*   [ ] Se puede diagnosticar un fallo sin depurar producción.
*   [ ] Dependencias y configuración compatibles.
*   [ ] Llamadas externas con límites y comportamiento de fallo definido.

---

## 🚀 Acciones a tomar (Next Actions)
- [ ] Revisar mis proyectos actuales contra este checklist.
- [ ] Practicar la distinción validación de entrada vs regla de negocio vs restricción de BD.
- [ ] Reforzar idempotencia/timeouts en integraciones externas.