---
tipo: duda-investigacion
estatus: ✅ Resuelta
categoria: "☕ Java → Estructuras de Datos"
fecha_registro: "2026-10-14"
fecha_resolucion: "2026-10-14"
fuente_origen: "Oracle Java SE 21 API (Stack, Queue, Deque)"
---

# Pilas y Colas en Java

> [!info] ❓ **La Duda / Reflexión**
> ¿Qué son exactamente las pilas (Stack) y las colas (Queue) en Java?
> ¿Cuándo usar cada una y qué estructuras (`Stack`, `ArrayDeque`, `Queue`, `LinkedList`) son las adecuadas?
> En la navegación web, ¿son pilas o colas las que se usan para "Atrás" y "Adelante"?

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Vi un video que hablaba sobre pilas y colas y sentí que necesitaba comprenderlo bien.
*   **Sospecha inicial:** Pila = LIFO (último en entrar, primero en salir), Cola = FIFO (primero en entrar, primero en salir). Pensaba que la navegación web usaba pila para atrás y cola para adelante.

---

## 🔍 Investigación y Respuestas

### Fuentes Consultadas
*   [x] Documentación oficial: Oracle Java SE 21 API
    *   [`Stack`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Stack.html)
    *   [`Queue`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Queue.html)
    *   [`Deque`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Deque.html)
*   [x] Verificación directa contra la especificación antes de afirmar el hecho técnico.

### Conceptos básicos

| Estructura | Acrónimo | Orden | Operaciones típicas | Analogía |
| :--- | :--- | :--- | :--- | :--- |
| **Pila (Stack)** | LIFO | *Last In, First Out* | `push` (apilar), `pop` (desapilar), `peek` (ver tope) | Una pila de platos: el último que pones, es el primero que quitas. |
| **Cola (Queue)** | FIFO | *First In, First Out* | `offer`/`add` (encolar), `poll`/`remove` (desencolar), `peek`/`element` (ver cabeza) | Cola para pagar en un supermercado: el primero que llega, es el primero en ser atendido. |

---

### Corrección: Navegación web (Atrás/Adelante)

Mi pensamiento inicial tenía un error: **la navegación web no usa pilas + colas, sino DOS PILAS**.

| Acción | Qué ocurre |
| :--- | :--- |
| **Navegar a una página nueva** | Se guarda la página actual en la **pila "Atrás"** y se **limpia completamente** la **pila "Adelante"**. |
| **Pulsar "Atrás"** | Se saca la página de la **pila Atrás** (LIFO) y se mete en la **pila Adelante**. |
| **Pulsar "Adelante"** | Se saca la página de la **pila Adelante** (LIFO) y se mete en la **pila Atrás**. |

> [!important] **Idea clave**
> Tanto "Atrás" como "Adelante" funcionan con **pila (LIFO)**. Cuando navegas a una página **nueva** después de haber retrocedido, **se borra toda la pila de "Adelante"** (no se desencola elemento a elemento). Esto es lo que hace que "ya no puedas avanzar a las páginas que estaban ahí" después de tomar un nuevo camino de navegación.

La impresora sí es el ejemplo clásico correcto de **cola (FIFO)**.

---

### ¿En qué estructuras de datos se aplican en Java?

| Estructura Java | Tipo | Uso recomendado | Notas |
| :--- | :--- | :--- | :--- |
| **`java.util.Stack<E>`** | Clase (legada, hereda de `Vector`) | ❌ No recomendado | Ofrece `push/pop/peek`. La doc oficial recomienda **`ArrayDeque`** en su lugar. |
| **`java.util.ArrayDeque<E>`** | Clase (implementa `Deque`) | ✅ **Recomendado para pila** | Rápido, sin sincronización. Para usar como pila: `push()` (añade al inicio), `pop()` (quita del inicio), `peek()` (mira inicio). |
| **`java.util.Deque<E>`** | Interfaz | ✅ Recomendado | *Double-ended queue*: sirve **como pila (LIFO) o como cola (FIFO)**. |
| **`java.util.Queue<E>`** | Interfaz | ✅ Recomendado para cola | Implementaciones: `ArrayDeque`, `LinkedList`, `PriorityQueue`. Para concurrencia: `BlockingQueue`, `ConcurrentLinkedQueue`. |
| **`java.util.LinkedList<E>`** | Clase (implementa `List`, `Deque`, `Queue`) | ✅ Válido para cola | También puede usarse como cola/pila, pero `ArrayDeque` suele ser más eficiente para apilar/desapilar sin nodos adicionales en muchos casos. |
| **`java.util.ArrayList<E>`** | Clase (implementa `List`) | ❌ No recomendado | Aunque puedes simular pila (añadir/quitar al final) o cola, **no está diseñado para eso**. Usa las estructuras específicas. |

---

### Métodos: dos estilos en `Queue`

La interfaz `Queue` ofrece **dos variantes** para cada operación:

| Operación | Variante que lanza excepción | Variante que devuelve valor especial | Significado |
| :--- | :--- | :--- | :--- |
| **Insertar (encolar)** | `add(e)` | `offer(e)` | `offer` devuelve `false` si no hay espacio (útil en colas con límite). |
| **Eliminar (desencolar)** | `remove()` | `poll()` | `poll()` devuelve `null` si la cola está vacía (evita excepción). |
| **Examinar (ver cabeza)** | `element()` | `peek()` | `peek()` devuelve `null` si la cola está vacía. |

---

### Código de ejemplo

```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    public static void main(String[] args) {
        // PILA (LIFO) - Forma moderna (recomendada)
        Deque<String> pila = new ArrayDeque<>();
        pila.push("A"); // apilar
        pila.push("B");
        pila.push("C");
        System.out.println("Tope: " + pila.peek()); // C
        System.out.println("Pop: " + pila.pop());   // C (LIFO)
        System.out.println("Pop: " + pila.pop());   // B
        System.out.println("Pop: " + pila.pop());   // A

        System.out.println("---");

        // COLA (FIFO) - Usando Queue + ArrayDeque
        Queue<String> cola = new ArrayDeque<>();
        cola.offer("Primero");
        cola.offer("Segundo");
        cola.offer("Tercero");
        System.out.println("Cabeza: " + cola.peek());  // Primero
        System.out.println("Poll: " + cola.poll());    // Primero (FIFO)
        System.out.println("Poll: " + cola.poll());    // Segundo
        System.out.println("Poll: " + cola.poll());    // Tercero

        System.out.println("---");

        // COLA también con LinkedList (válido)
        Queue<String> cola2 = new LinkedList<>();
        cola2.offer("1");
        cola2.offer("2");
        System.out.println(cola2.poll()); // 1
        System.out.println(cola2.poll()); // 2
    }
}
```



---

### Relación con otras estructuras de datos

Es importante distinguir entre **ser** una pila/cola y **usarse para implementar o recorrer** otras estructuras.

| Concepto | ¿Es pila/cola? | Explicación |
| :--- | :--- | :--- |
| **Arrays (`[]`)** | ❌ No | Son estructuras de acceso aleatorio (por índice). Sin embargo, `ArrayDeque` **implementa** pilas y colas **usando un array circular** por debajo. |
| **`ArrayList`/`List`** | ❌ No | No son FIFO/LIFO nativos. Aunque puedes simular pila/cola (añadir/quitar al final), no es su propósito. Usa `ArrayDeque` o `LinkedList` para cola/pila. |
| **`Set` / `HashSet` / `TreeSet`** | ❌ No | Trabajan con unicidad de elementos, sin orden FIFO/LIFO definido. |
| **`Map` / `HashMap` / `TreeMap`** | ❌ No | Pares clave-valor, no son secuencias lineales. |
| **Árboles (`Tree`, `BinaryTree`)** | ❌ No | Estructuras jerárquicas. **Sí se usan pilas/colas en sus recorridos:** DFS (Depth-First Search) usa **pila** (recursiva o explícita), BFS (Breadth-First Search) usa **cola** (nivel por nivel). |
| **Grafos** | ❌ No | Igual que árboles: BFS usa **cola** para recorrer por niveles; DFS usa **pila** (o recursividad). El grafo no es pila/cola, el algoritmo las emplea como herramienta. |

> [!tip] **Regla mnemotécnica**
> *DFS = Deep (profundidad) → usa **Stack (Pila)***  
> *BFS = Breadth (amplitud) → usa **Queue (Cola)***

> [!warning] **Recomendación oficial**
> Para usar una **pila**, prefiere **`ArrayDeque`** sobre `Stack`. `Stack` hereda de `Vector` (sincronizado y legado). `ArrayDeque` es más ligero y moderno.

---

## 🚀 Acciones a tomar (Next Actions)
- [x] Verificar conceptos y estructuras contra la API oficial de Java SE 21.
- [x] Registrar la conclusión en esta nota.
- [x] Enlazar desde `Preguntas de Java para entrevistas.md` (sección 9: Collections Framework).
- [ ] Practicar con ejercicios de pila/cola (invertir cadena con pila, simular impresora con cola).
