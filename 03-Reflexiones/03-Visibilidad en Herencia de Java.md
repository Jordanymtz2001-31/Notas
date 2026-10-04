---
tipo: reflexion
estatus: ✅ Resuelta
categoria: "☕ Java → Acceso y Herencia"
fecha_registro: "2026-10-04"
fecha_resolucion: "2026-10-04"
fuente_origen: "JLS §6.6 y §8.4.8.3"
---

# Visibilidad en Herencia de Java

> [!info] ❓ **La Duda / Reflexión**
> Al heredar de una clase, ¿puedo cambiar la visibilidad de un método?
> Por ejemplo: si el padre declara un método `public`, ¿puedo sobreescribirlo como `protected`?
> Y más en general: ¿existe un orden de visibilidad de más cerrada a más abierta?

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Hoy aprendí que al hacer herencia de clases, los modificadores de acceso no se pueden "cerrar". Si heredo un método `public`, no puedo colocarle `protected`.
*   **Sospecha inicial:** Creo que debe existir un orden de visibilidad y que al sobreescribir no se permite reducir el alcance, solo conservarlo o ampliarlo.

---

## 🔍 Investigación y Respuestas
### Fuentes Consultadas
*   [x] Documentación oficial: JLS (Java Language Specification), Java SE 21
    *   §6.6 Access Control — [docs.oracle.com](https://docs.oracle.com/javase/specs/jls/se21/html/jls-6.html#jls-6.6)
    *   §8.4.8.3 Requirements in Overriding and Hiding — [docs.oracle.com](https://docs.oracle.com/javase/specs/jls/se21/html/jls-8.html#jls-8.4.8.3)
*   [x] Verificación directa contra la especificación antes de afirmar el hecho técnico.

### Orden de visibilidad (de más cerrada a más abierta)

Según **JLS §6.6.1**, los cuatro niveles de acceso en clases son:

| Modificador | Alcance |
| :--- | :--- |
| `private` | Solo dentro de la misma clase |
| *(sin modificador)* / package access | Solo dentro del mismo paquete |
| `protected` | Mismo paquete + subclases (incluso en otros paquetes) |
| `public` | En cualquier lugar |

> [!tip] **Package access**
> Cuando un miembro no lleva ningún modificador de acceso, se dice que tiene *package access*: solo es visible dentro del paquete donde fue declarado.

---

### Regla al sobreescribir (override) y ocultar (hide)

**JLS §8.4.8.3** establece:

> [!abstract] **Regla de visibilidad en herencia**
> The access modifier of an overriding or hiding method must provide **at least as much access** as the overridden or hidden method.

Traducción: el método que sobreescribes debe tener **igual o más visibilidad**, nunca menos.

| Si el método del padre es... | El hijo debe ser... |
| :--- | :--- |
| `public` | `public` (obligatorio) |
| `protected` | `protected` **o** `public` |
| package access | **no** `private` (puede ser package, `protected` o `public`) |

```java
class Padre {
    public void saludo() { }      // public
    protected void detalle() { }  // protected
    void interno() { }            // package access
}

class Hijo extends Padre {
    public void saludo() { }      // ✅ OK: se conserva public
    // protected void detalle() { }  // ❌ ERROR: cerraría la visibilidad
    public void detalle() { }      // ✅ OK: se amplía a public
    public void interno() { }      // ✅ OK: se amplía
    // private void interno() { }   // ❌ ERROR: cerraría a private
}
```

---

### Matiz importante: los métodos `private`

**JLS §8.4.8.3** añade:

> [!warning] **`private` no se hereda técnicamente**
> Note that a `private` method cannot be overridden or hidden in the technical sense of those terms.

Los métodos `private` **no son heredados**. Una subclase puede declarar un método con la misma firma sin ninguna restricción de visibilidad, porque no es un *override* real: es un método nuevo e independiente.

```java
class Padre {
    private void secreto() { }
}

class Hijo extends Padre {
    private void secreto() { }  // ✅ OK: no es override, es un método nuevo
    public void otro() { }
}
```

---

## 🚀 Acciones a tomar (Next Actions)
- [x] Verificar la regla contra el JLS antes de darla por cierta.
- [x] Registrar la conclusión en esta nota.
- [ ] Practicar con un ejemplo compilable en IntelliJ para ver el error de compilación en vivo.
- [ ] Revisar si en `06- 📝 OneNote` conviene enlazar este concepto desde la sección de Java.
