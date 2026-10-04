---
tipo: duda-investigacion
estatus: ✅ Resuelta
categoria: "☕ Java → Formato de Fechas y Horas"
fecha_registro: "2026-10-04"
fecha_resolucion: "2026-10-04"
fuente_origen: "Oracle Java SE 21 API (DateTimeFormatter, FormatStyle)"
---

# DateTimeFormatter y FormatStyle

> [!info] ❓ **La Duda / Reflexión**
> ¿Cómo se usan `ofLocalizedDateTime(FormatStyle.SHORT)`, `ofLocalizedTime(FormatStyle.MEDIUM)` y `ofLocalizedTime(FormatStyle.SHORT)`?
> Solo sé que son para fechas y horas, pero no sé en qué situaciones aplica cada uno.
> ¿Se usan en Java puro, en Spring Boot o en otras tecnologías?

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Aparecieron estas fábricas de `DateTimeFormatter` al revisar cómo formatear fechas y horas, pero no quedó claro qué diferencia hay entre cada una ni cuándo conviene usarlas.
*   **Sospecha inicial:** Creo que el parámetro `FormatStyle` controla "cuánto detalle" se muestra (corto, medio, largo), y que el método decide si formateamos fecha, hora o ambas.

---

## 🔍 Investigación y Respuestas
### Fuentes Consultadas
*   [x] Documentación oficial: Oracle Java SE 21 API
    *   [`DateTimeFormatter`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/time/format/DateTimeFormatter.html)
    *   [`FormatStyle`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/time/format/FormatStyle.html)
*   [x] Verificación directa contra la especificación antes de afirmar el hecho técnico.

### Qué son realmente

No son "funciones para sacar la fecha": son **fábricas de formateadores**. Crean un `DateTimeFormatter` inmutable e hilo-seguro que sabe **cómo escribir** (o leer) fechas y horas según el **locale** del sistema. El patrón exacto lo decide Java, no el programador.

> [!abstract] **Idea clave**
> `ofLocalized*` = formato para **humanos**, adaptado al idioma/país del sistema.
> `ofPattern("dd/MM/yyyy")` o ISO = formato para **máquinas**, estable y predecible.

---

### Los 4 valores de `FormatStyle`

| Estilo | Descripción oficial | Ejemplo fecha (en_US) | Ejemplo hora (en_US) |
| :--- | :--- | :--- | :--- |
| `SHORT` | Typically numeric | `12/13/52` | `3:30 PM` |
| `MEDIUM` | With some detail | `Jan 12, 1952` | `3:30:42 PM` |
| `LONG` | Lots of detail | `January 12, 1952` | `3:30:42 PM EST` |
| `FULL` | Most detail | `Tuesday, April 12, 1952 AD` | `3:30:42 PM PST` |

---

### Las fábricas de `DateTimeFormatter`

| Método | Qué formatea | Ejemplo oficial del JLS |
| :--- | :--- | :--- |
| `ofLocalizedDate(style)` | Solo **fecha** | `'2011-12-03'` |
| `ofLocalizedTime(style)` | Solo **hora** | `'10:15:30'` |
| `ofLocalizedDateTime(style)` | Fecha + hora (mismo estilo) | `'3 Jun 2008 11:05:30'` |
| `ofLocalizedDateTime(dateStyle, timeStyle)` | Fecha + hora (estilos distintos) | `'3 Jun 2008 11:05'` |

> [!warning] **`FULL` y `LONG` en hora**
> La documentación oficial advierte que "typically require a time-zone": necesitas un `ZonedDateTime` o `withZone(ZoneId)`. Con `LocalTime` a secas puede fallar.

---

### Código de ejemplo

```java
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;

public class Main {
    public static void main(String[] args) {
        // Fecha + hora corta  ->  ej. "4/10/26, 15:30"
        DateTimeFormatter f1 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT);

        // Hora media con segundos  ->  ej. "3:30:42 PM"
        DateTimeFormatter f2 = DateTimeFormatter.ofLocalizedTime(FormatStyle.MEDIUM);

        // Hora corta sin segundos  ->  ej. "3:30 PM"
        DateTimeFormatter f3 = DateTimeFormatter.ofLocalizedTime(FormatStyle.SHORT);

        LocalDateTime ahora = LocalDateTime.now();
        System.out.println(ahora.format(f1));
        System.out.println(ahora.toLocalTime().format(f2));
        System.out.println(ahora.toLocalTime().format(f3));
    }
}
```

---

### ¿Java puro, Spring Boot u otras tecnologías?

*   **Java puro:** Sí. Pertenecen a `java.time.format` y están disponibles **desde Java 8**, sin dependencias extra.
*   **Spring Boot:** Se pueden usar, pero **no son lo habitual en APIs REST**. Spring y Jackson prefieren formatos fijos (`yyyy-MM-dd`, ISO) porque un endpoint no puede devolver formato distinto según el país de quien llame. Sí encajan en **UI de usuarios**, **correos** o **reportes** donde el público es de un país concreto.
*   **Otras tecnologías:** Angular/React tienen sus propios pipes o formatters (`date:'dd/MM/yyyy'`); no usan `DateTimeFormatter` de Java.

---

### Cuándo usar cada uno

| Situación | Uso típico |
| :--- | :--- |
| Tablas o listas compactas en UI | `SHORT` |
| Display general legible (default) | `MEDIUM` |
| Documentos formales, contratos | `LONG` |
| Texto legal o encabezados completos | `FULL` |
| APIs REST / logs / base de datos | **No** usar localized; usar `ofPattern` o ISO |

---

## 🚀 Acciones a tomar (Next Actions)
- [x] Verificar las fábricas y `FormatStyle` contra la API oficial de Java SE 21.
- [x] Registrar la conclusión en esta nota.
- [ ] Probar los 4 estilos en IntelliJ con `Locale.getDefault()` y comparar la salida.
- [ ] Revisar si en `06- 📝 OneNote` conviene enlazar este concepto desde la sección de Java.
