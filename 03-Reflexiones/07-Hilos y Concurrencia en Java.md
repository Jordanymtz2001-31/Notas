---
tipo: reflexion
estatus: 🔄 Pendiente
categoria: "☕ Java → Concurrencia"
fecha_registro: "2026-10-19"
fecha_resolucion:
fuente_origen: "Oracle Java SE 21 API (Thread, Thread.State, Runnable)"
---

# 07-Hilos y Concurrencia en Java

> [!info] ❓ **La Duda / Reflexión**
> ¿Qué diferencia hay entre proceso e hilo, entre concurrencia y paralelismo, y cómo se modela el ciclo de vida de los hilos en Java?

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Ver video "THREADS (Hilos) y CONCURRENCIA en Java" (TodoCode).
*   **Sospecha inicial:** Entendía hilos como procesos; pensaba que "concurrencia = al mismo tiempo". Tras la verificación con la API oficial, hacía falta corregir matices y dejar las definiciones claras.

---

## 🔍 Investigación y Respuestas

### Procesos vs Hilos
> [!abstract] Diferencia clave
> Un **proceso** es una instancia de un programa en ejecución, con su propio espacio de memoria aislado. Los **hilos** viven dentro de un proceso: comparten memoria, son más ligeros y permiten ejecución concurrente dentro del mismo programa.

### Concurrencia vs Paralelismo
> [!abstract] Nuance importante
> **Concurrencia** = gestionar varias tareas *a la vez* (intercalando su ejecución). **Paralelismo** = ejecutarlas *realmente al mismo tiempo* (varios núcleos). En un solo núcleo puede haber concurrencia, pero no paralelismo.

### Formas de crear hilos en Java
Java permite dos formas principales:

1. **Extender `Thread`**
   ```java
   public class MiHilo extends Thread {
       @Override
       public void run() {
           System.out.println("Hilo: " + Thread.currentThread().getName());
       }
   }
   // Uso
   new MiHilo().start();
   ```

2. **Implementar `Runnable` (recomendada)**
   ```java
   public class Tarea implements Runnable {
       @Override
       public void run() {
           System.out.println("Tarea: " + Thread.currentThread().getName());
       }
   }
   // Uso
   new Thread(new Tarea()).start();
   ```

> [!tip] **Recomendación**
> Implementar `Runnable` evita el problema de **herencia única** de Java, permite implementar múltiples interfaces y facilita compartir **la misma instancia** entre varios hilos (ideal para tareas cooperativas).

### Diferencia entre `start()` y `run()`
*   `start()`: solicita al JVM que cree y programe un nuevo hilo de ejecución. El `run()` se ejecutará en ese nuevo hilo.
*   `run()`: si se llama directamente, se ejecuta **en el hilo actual** (no crea hilo nuevo). Es un error común pensar que invocar `run()` lanza otro hilo.

### Ciclo de vida de un hilo (Thread.State)
`java.lang.Thread.State` (Java SE 21) define **6 estados**:

| Estado | Descripción |
|---|---|
| `NEW` | Hilo creado pero aún no iniciado (`start()` no llamado). |
| `RUNNABLE` | En ejecución en la JVM; puede estar esperando recursos del SO (CPU). |
| `BLOCKED` | Bloqueado esperando adquirir un monitor lock (entrada/reentrada a bloque sincronizado). |
| `WAITING` | Esperando indefinidamente a que otro hilo realice una acción (`Object.wait()` sin timeout, `Thread.join()` sin timeout, `LockSupport.park()`). |
| `TIMED_WAITING` | Esperando con tiempo límite (`Thread.sleep(long)`, `Object.wait(long)`, `Thread.join(long)`, `LockSupport.parkNanos/parkUntil`). |
| `TERMINATED` | Ejecución finalizada (el método `run()` ha terminado o se salió). |

> [!note] **Aclaración sobre simplificaciones**
> En algunos blogs antiguos (ej. 2012) se agrupan estados: "en pausa" suele cubrir `BLOCKED`, `WAITING` y `TIMED_WAITING`. En Java moderno son **6 estados distintos**.

### Ejemplo: compartir misma instancia con Runnable
```java
public class Contador implements Runnable {
    private int n = 0;
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " -> " + ++n);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Contador tarea = new Contador();
        new Thread(tarea, "Hilo-1").start();
        new Thread(tarea, "Hilo-2").start(); // comparten instancia
    }
}
```

### Más allá de lo básico
*   **`ExecutorService`**: gestiona pools de hilos de forma más eficiente.
*   **Hilos virtuales (Java 21)**: permiten escalar muchas tareas concurrentes con menor sobrecarga (ideal para I/O intensivo). Es lo recomendable en aplicaciones modernas para concurrencia estructurada.

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
> Los hilos son unidades de ejecución dentro de un proceso (comparten memoria), no procesos independientes. La concurrencia organiza tareas para que avancen alternando; el paralelismo las ejecuta simultáneamente. Java modela el ciclo con 6 estados (`NEW/RUNNABLE/BLOCKED/WAITING/TIMED_WAITING/TERMINATED`). La forma preferida para crear hilos es **implementar `Runnable`** (evita herencia única, permite reutilizar la misma instancia entre hilos) y **nunca llamar `run()` directamente**: siempre usar `start()` para crear un hilo nuevo.

---

## 🚀 Acciones a tomar (Next Actions)
- [ ] Practicar con ejemplos compartiendo instancia Runnable y viendo estados con `Thread.getState()`.
- [ ] Explorar `ExecutorService` y hilos virtuales en Java 21 para casos reales.