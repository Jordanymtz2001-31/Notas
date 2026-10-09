---
tipo: duda-investigacion
estatus: ✅ Resuelta
categoria: "☕ Java → Cadenas y Mutabilidad"
fecha_registro: "2026-10-04"
fecha_resolucion: "2026-10-04"
fuente_origen: "Oracle Java SE 21 API (StringBuilder, StringBuffer)"
---

# StringBuilder y StringBuffer

> [!info] ❓ **La Duda / Reflexión**
> Mi entendimiento inicial era: "con `StringBuilder` y `StringBuffer` podemos mutar datos primitivos u objetos, y los DTOs se validan como inmutables porque pasan por una interfaz".
> ¿Qué son realmente estas clases y para qué se usan?
> ¿En qué se diferencian entre sí y de `String`?

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Estaba pensando en cómo manipular texto al construir respuestas de una API (concatenaciones en bucles, logs, consultas dinámicas) y surgió la confusión entre `String` inmutable y estas dos clases "mutables".
*   **Sospecha inicial:** Creo que `StringBuilder`/`StringBuffer` sirven para construir cadenas grandes de forma eficiente sin crear muchos objetos `String` temporales por cada concatenación.

---

## 🔍 Investigación y Respuestas
### Fuentes Consultadas
*   [x] Documentación oficial: Oracle Java SE 21 API
    *   [`StringBuilder`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/StringBuilder.html)
    *   [`StringBuffer`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/StringBuffer.html)
*   [x] Verificación directa contra la especificación antes de afirmar el hecho técnico.

### Qué son realmente

Son **secuencias de caracteres mutables**. A diferencia de `String` (que una vez creado no cambia), estas clases mantienen un **buffer interno** (`char[]` o `byte[]`) que crece automáticamente conforme agregas contenido. La documentación oficial las describe así:

*   **`StringBuilder`:** *"A mutable sequence of characters"* — **no es thread-safe**.
*   **`StringBuffer`:** *"A thread-safe, mutable sequence of characters"* — todos sus métodos públicos están **sincronizados**.

> [!abstract] **Idea clave**
> `String` → inmutable, cada operación crea un objeto nuevo.
> `StringBuilder`/`StringBuffer` → mutables, se modifican en el mismo objeto sin crear copias.

---

### Las 3 correcciones a mi entendimiento inicial

| # | Afirmación inicial (❌) | Realidad (✅) |
| :--- | :--- | :--- |
| 1 | "mutan datos primitivos u objetos" | Solo construyen/modifican **cadenas de caracteres**. `append(int)` acepta un entero, pero lo **convierte a su representación en texto** y lo agrega al buffer: no hace mutables las variables primitivas. |
| 2 | "los DTOs se validan inmutables por una interfaz" | La inmutabilidad de un DTO (`record`, campos `final`, validación con `@Valid`) es un concepto de **diseño de capas**, ajeno a `StringBuilder`/`StringBuffer`. Son temas separados. |
| 3 | "crea temporalmente ese objeto y se lo asigna al original" | `append()` trabaja sobre un buffer interno con **capacity** que crece automáticamente; al final, `toString()` construye una **`String` nueva** a partir de ese buffer. No se "asigna al original": se crea una `String` resultante. |

---

### Diferencia clave entre ambos

| Característica | `StringBuilder` | `StringBuffer` |
| :--- | :--- | :--- |
| **Thread-safe** | ❌ No | ✅ Sí (`synchronized`) |
| **Desde** | Java 5 | Java 1.0 |
| **Rendimiento** | ⚡ Más rápido (sin overhead de sincronización) | Más lento (bloqueo de monitores) |
| **Recomendación oficial** | ✅ *"se recomienda preferirlo"* | Solo si **varios hilos** escriben en la misma instancia |

---

### API principal (ambas implementan `CharSequence`)

*   `append(Object)` — agrega la representación en texto de cualquier tipo.
*   `insert(int, Object)` — inserta en una posición dada.
*   `delete(int, int)` — elimina un rango de caracteres.
*   `replace(int, int, String)` — reemplaza un rango.
*   `toString()` — devuelve la `String` resultante (nueva instancia).

---

### Código de ejemplo

```java
public class Main {
    public static void main(String[] args) {
        // StringBuilder (recomendado para un solo hilo)
        StringBuilder sb = new StringBuilder("Hola");
        sb.append(' ').append("Mundo").append(' ').append(2026);
        // append(2026) convierte el int a "2026" y lo agrega como texto
        String resultado = sb.toString();  // "Hola Mundo 2026"
        System.out.println(resultado);

        // StringBuffer (thread-safe, para concurrencia)
        StringBuffer sf = new StringBuffer("ID-");
        sf.append(12345).insert(3, "USER-");  // "ID-USER-12345"
        System.out.println(sf.toString());
    }
}
```

> [!warning] **Cuándo NO usarlas**
> Para **una sola concatenación simple** (ej. `String msg = "Hola " + nombre;`), `String` es suficiente: el compilador de Java suele optimizar la concatenación `+` con `+` usando internamente un `StringBuilder`. Estas clases brillan en **bucles** o **múltiples concatenaciones acumulativas**, donde evitarían crear muchos objetos `String` temporales.

---

## 🚀 Acciones a tomar (Next Actions)
- [x] Verificar `StringBuilder` y `StringBuffer` contra la API oficial de Java SE 21.
- [x] Registrar la conclusión en esta nota.
- [x] Enlazar desde `Preguntas de Java para entrevistas.md` (sección 2).
- [ ] Probar en IntelliJ un bucle de 10 000 concatenaciones con `String +` vs `StringBuilder.append` y comparar tiempos.
