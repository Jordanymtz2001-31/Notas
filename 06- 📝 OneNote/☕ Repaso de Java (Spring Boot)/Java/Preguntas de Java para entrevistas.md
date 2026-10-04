# ☕ Programación Orientada a Objetos (POO) y Estructuras en Java

## 🏢 1. El Paradigma de la POO en el Mundo Empresarial

### 🔹 Definición Fundacional
La **Programación Orientada a Objetos** es un paradigma que organiza el código basándose en **objetos**. Cada objeto encapsula **datos (atributos)** y **comportamientos (métodos)** para modelar una entidad del mundo real o conceptual. Comparte exactamente el mismo fundamento teórico que en Python, variando únicamente en las restricciones de su sintaxis.

### 💼 ¿Por qué se utiliza en las empresas?
Es la opción predilecta en arquitecturas corporativas debido a que incrementa drásticamente la **escalabilidad** del software, facilita un desarrollo altamente **mantenible** y promueve la **reutilización de código**.

> [!example] **Caso de Uso Real en Plataformas**
> "Cuando manejamos múltiples usuarios ligados a una plataforma, para evitar definir de forma redundante los mismos atributos y acciones en cada flujo del sistema, creamos una **Clase Base** unificada que represente la entidad global de usuario y de ahí estructuramos el negocio."

---

## 🗃️ 2. Estructuras de Datos y Elementos del Lenguaje

### 📊 Colecciones y Tipos de Datos (Estructuras)

| Estructura en Java   | Estado de Mutabilidad             | Tipo de Contenido y Propósito                                                                                                           |
| :------------------- | :-------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------- |
| **`ArrayList`**      | 🔓 **Mutable** (Modificable)      | Colección dinámica y redimensionable de elementos de cualquier tipo de dato.                                                            |
| **`Record`**         | 🔒 **Inmutable** (No modificable) | Introducido para representar una lista o estructura fija de datos que solo transporta información sin alterar su estado.                |
| **`HashMap` / Maps** | 🔓 **Mutable** (Diccionario)      | Colección estructurada en pares **Clave-Valor**, donde la clave actúa como el campo indexado y el valor guarda el dato correspondiente. |
|                      |                                   |                                                                                                                                         |

### 🅰️ Objetos Inmutables Nativos de Java
En Java nativo existen unos cuantos y en Spring Boot otros; para más detalles consulta [[02-Objetos Inmutables en Java y Spring]].

### 🧩 ¿Qué es Optional?
- **`Optional<T>` es una CLASE** (específicamente una clase final contenedora) que pertenece al paquete `java.util`.
- Es una clase introducida en Java 8 que funciona como una **caja o envoltura (wrapper)**. Puede estar llena (contener un objeto) o vacía (reemplazando al peligroso `null`) . Su objetivo es obligar al programador a validar si el dato existe antes de usarlo, eliminando el famoso error `NullPointerException`.
- Para usar un `Optional`, estás obligado a llamar a sus **métodos** (como `.ofNullable()`, `.isPresent()`, `.orElseThrow()`)

### 🔍 Operadores, Anotaciones y Funciones

* **Diferencia entre `==` y `.equals()`:**
  * **`==`** → Valida la **identidad** del objeto en memoria RAM. Compara si ambas variables apuntan exactamente a la misma referencia física. Sin embargo, también se puede comparar con tipos primitivos, es decir con variables que no pertenecen al objeto.
  * **`.equals()`** → Compara la **igualdad de los valores** internos del objeto, ya sean cadenas de texto (`String`) o datos numéricos.
* **Funciones Lambda:** Ideales para implementar funciones muy cortas y temporales. A diferencia de Python, en Java las lambdas son conocidas como funciones anónimas avanzadas que **permiten escribir código en múltiples líneas** utilizando llaves `{}`.
* **Anotaciones:** A diferencia de los decoradores de Python que extienden dinámicamente las funciones en ejecución, las anotaciones de Java actúan como metadatos que **le indican al compilador cómo se debe comportar** un objeto o método en tiempo de compilación o ejecución.

---

## 🏛️ 3. Moldes, Inicialización y Estado

* **Clase:** Funciona como el molde, plano o plantilla que define la estructura conceptual de nuestros objetos (qué atributos tendrán y qué métodos podrán ejecutar).
* **Objeto:** Es la **instancia concreta** y física creada a partir de una clase al ejecutarse el programa. 
* **Constructor:** Método especial de una clase encargado de inicializar los atributos del objeto. Se ejecuta en automático al instanciar la clase con la palabra clave `new`.

> [!important] **¿Por qué es indispensable el Constructor?**
> Garantiza y asegura que los objetos "nazcan" con datos válidos, correctos y listos para trabajar, blindando el sistema contra errores de valores nulos (`NullPointerException`) y haciendo el código más claro.

### 🔢 Valores por defecto de los atributos

Cuando declaras un atributo y **no lo inicializas**, la JVM le asigna un valor por defecto según su tipo. Esto aplica **tanto a los atributos de instancia como a los estáticos**: lo que cambia es el *cuándo* y el *dónde*, no el *si*.

| Clase de atributo | ¿Valor por defecto? | Cuándo ocurre | Dónde vive |
| :--- | :--- | :--- | :--- |
| **Atributo de instancia** | Sí | Al crear **cada** objeto con `new` | Uno por objeto |
| **Atributo estático (`static`)** | Sí | **Una sola vez**, al inicializar la clase | **Compartido** por todos los objetos |
| **Variable local o parámetro** | **No** | Hay que asignarla, o **error de compilación** | Solo dentro del método |

Los valores concretos dependen del tipo declarado:

| Tipo | Valor por defecto |
| :--- | :--- |
| `boolean` | `false` |
| `int`, `short`, `byte`, `long` | `0` |
| `float`, `double` | `0.0` |
| `char` | `'\u0000'` |
| Referencias (`String`, objetos, arrays) | `null` |

> [!warning] **La trampa de `static`**
> `static int contador;` **sí** vale `0`, no "vale lo que hubiera en memoria". Lo que hace `static` es que ese `0` se asigne **una única vez al inicializar la clase** y luego se comparta entre todos los objetos que usen esa clase. Declarar `static int contador = 0;` no cambia el valor inicial, pero deja la intención explícita y evita la pregunta en el código.
>
> Donde de verdad **no** existe valor por defecto son las **variables locales** y los **parámetros de método**: ahí el compilador te obliga a inicializar, y si no lo haces el programa **no compila**.
>
> Y hay un tercer caso: un atributo declarado `final` **nunca** recibe valor por defecto. Hay que inicializarlo en la declaración, en el constructor (si es de instancia) o en un bloque `static` (si es estático).

```java
class Contador {
    int instancias;            // por objeto  -> 0
    static int total;          // por clase   -> 0, asignado una vez
    static final int MAXIMO = 100;  // final: obligatorio inicializar

    void ejemplo(int received) {
        int acumulado;         // ⚠️ error de compilación: sin valor por defecto
        int copia = received;  // el parámetro sí hay que asignarlo antes de usarlo
    }
}
```

> [!important] **Frase para la entrevista**
> "En Java todos los atributos reciben un valor por defecto según su tipo, sean de instancia o estáticos. Lo que cambia es el momento: el de instancia se inicializa en cada objeto al usar `new`, y el estático se inicializa una sola vez al cargar la clase y se comparte entre todos los objetos. Lo que sí exige inicialización explícita son las variables locales y los parámetros, y los atributos `final`."

---

## 📐 4. Los 4 Pilares de la Programación Orientada a Objetos

### 📋 A. Abstracción
Consiste en **mostrar únicamente lo esencial** de un objeto hacia el exterior (el "qué hace" de forma general) y ocultar los detalles innecesarios de su implementación (el "cómo lo hace"). En el código se materializa a través de Clases Abstractas e Interfaces que expresan puros conceptos del dominio.

#### Superclases Abstractas (`abstract`)
* Son **plantillas incompletas** que definen comportamientos comunes generales para que sus subclases los hereden e implementen.
* **No se pueden instanciar directamente** para crear objetos; su única existencia es servir de base para la herencia.
* Pueden contener **Métodos Abstractos** (declarados sin cuerpo/código en la superclase que obligan a las subclases a sobreescribirlos con su propia lógica).
* 📍 *==> Ref: Ejercicio 6 - Semana 1 - Enucom 2*

### 🔒 B. Encapsulamiento
Es el mecanismo utilizado para proteger el estado interno de un objeto, **restringiendo el acceso directo a sus atributos**. 
* En Java se implementa declarando los atributos como **`private`**.
* Para interactuar con ellos, se exponen métodos públicos conocidos como **Getters** (para leer) y **Setters** (para modificar y validar los datos).

### 🌿 C. Herencia
Permite crear una clase nueva (Subclase/Hija) basada en una clase existente (Superclase/Padre), **reutilizando y extendiendo** todos sus atributos y métodos no privados. Se utiliza para modelar relaciones jerárquicas en común.
* **`extends`** → Palabra clave de Java utilizada para heredar los componentes de la superclase.
* 📍 *==> Ref: Ejercicio 5 - Semana 1 - Enucom 3*

### 🎭 D. Polimorfismo
Es la capacidad que permite que **diferentes objetos respondan a un mismo método o mensaje, pero ejecutando comportamientos totalmente distintos** según su clase.
* 📍 *==> Ref: Ejercicio 7 - Semana 1 - Enucom 3*

> [!info] **Los 3 Tipos de Polimorfismo**
> 1. **Sobrecarga (Overload):** Crear múltiples métodos con el **mismo nombre pero diferentes parámetros** (en tipo, cantidad u orden) dentro de la misma clase. *Ejemplo clásico: Definir un constructor con parámetros y otro vacío.* Se usa para dar múltiples formas de invocar una acción.
> 2. **Sobreescritura (Override):** Redefinir un método de la clase padre dentro de la clase hija, manteniendo exactamente la **misma firma** (nombre y parámetros). En Java es una **recomendación fuerte** marcar el método con la anotación **`@Override`**, para que el compilador verifique la correcta vinculación y detecte errores de firma. ⚠️ Ojo: **no es obligatorio** en Java como sí lo es en C# o TypeScript; el método funciona igual sin la anotación.
> 3. **De Parámetro (Genéricos):** Permite que un mismo método o estructura procese cualquier tipo de objeto. *Ejemplo: Una estructura `List<T>` funciona de manera idéntica ya sea que contenga objetos `Cliente`, `Producto` o `Pedido`.*

---

## 🤝 5. Relaciones entre Objetos y Control Interno

### 📐 Composición vs. Agregación

* **Composición ("Tiene un" estricto):** Una clase contiene objetos de otras clases como atributos esenciales. **Existe una dependencia de vida total**: si la clase "padre" se destruye, los objetos "hijos" mueren con ella. El objeto es parte intrínseca del otro y no puede existir de forma aislada.
* *Ejemplo Clave:* Un coche está formado por un motor. Si el coche deja de existir, el motor también lo hace en este modelo básico de propiedad exclusiva.
```java
class Motor {
    public void encender() {
        System.out.println("El motor está encendido.");
    }
}

class Coche {
    private Motor motor; // Composición: El coche "tiene un" motor

    public Coche() {
        this.motor = new Motor(); // El motor se crea con el coche
    }

    public void arrancar() {
        motor.encender();
        System.out.println("El coche está en movimiento.");
    }
}

public class Main {
    public static void main(String[] args) {
        Coche miCoche = new Coche();
        miCoche.arrancar();
    }
}

```

* **Agregación ("Tiene un" débil):** El objeto "hijo" tiene una existencia independiente y puede sobrevivir sin el objeto "padre". 
  * *Ejemplo Clave:* La clase principal (**Pedido**) contiene una referencia a otra clase (**Producto**), pero sus ciclos de vida son independientes. Si eliminas el pedido, los productos siguen existiendo en tu sistema (no se destruyen).

### 🔑 Palabras Clave de Control de Instancia
* **`this`:** Apunta de forma directa a la instancia actual de la clase ("Yo mismo"). Se usa para diferenciar atributos de la clase de parámetros locales con el mismo nombre.
* **`super`:** Hace referencia directa a la clase padre. Se utiliza para invocar los métodos o atributos heredados, o para mandar a llamar al constructor de la superclase en la primera línea de la subclase.

---

## 🎛️ 6. Interfaces, Modificadores y Métodos de Clase

### 📜 El Contrato de las Interfaces (`interface`)
Es un **contrato profesional 100% abstracto** que define firmas de métodos sin implementar. Permite que clases completamente distintas y sin parentesco de herencia implementen los mismos comportamientos, respetando principios SOLID como *Open/Closed* e *Inversión de Dependencias*.
* ❌ **No se pueden crear instancias (`new`) directamente de una interfaz.**
* **¿Por qué usarla?** Define comportamientos comunes entre clases distintas, facilita el polimorfismo a gran escala, modulariza el código y nos obliga a **programar hacia abstracciones, no hacia implementaciones concretas**.
* 📍 *==> Ref: Ejercicio 8 - Semana 1 - Enucom 3*

### 🔒 Modificadores de Acceso
Palabras clave que deciden qué partes del código quedan expuestas y cuáles protegidas bajo las reglas del encapsulamiento:
* **`public`:** El elemento es accesible desde cualquier clase de la aplicación.
* **`protected`:** Accesible dentro de la misma clase, por clases del mismo paquete y por cualquier subclase (hija) mediante herencia.
* **`default` (Sin palabra clave):** El elemento es accesible única y exclusivamente por clases que pertenezcan al **mismo paquete**.
* **`private`:** Restricción total. El elemento es accesible únicamente dentro de las llaves de la misma clase.

> [!tip] **Visibilidad al heredar y sobreescribir**
> Al sobreescribir un método **no se puede cerrar** su visibilidad: solo conservarla o ampliarla. Si el padre es `public`, el hijo debe serlo también. Para más detalles revisa [[03-Visibilidad en Herencia de Java]].

### 🎛️ Métodos de Instancia vs. Métodos Estáticos
*📍 ==> Ref: Práctica 15 - Semana 1 - Enucom 2*

* **Método de Instancia:** Recibe la referencia del objeto como parámetro implícito. Tiene acceso total tanto a los atributos del objeto (instancia) como a las variables estáticas globales de la clase. Requiere crear un objeto con `new` para poder ser invocado.
* **Método Estático (`static`):** Pertenece directamente a la estructura de la clase, **no al objeto**. No puede acceder a ningún atributo de instancia, pero sí interactúa con atributos estáticos. Se puede invocar directamente usando el nombre de la clase sin necesidad de instanciar un objeto.

---

## 🎛️ 7. Lógica Funcional: Métodos Concretos

### 🔹 Definición y Alcance
A diferencia de los métodos abstractos (que carecen de cuerpo), los **Métodos Concretos** poseen su propia lógica completamente implementada y se ejecutan tal cual están escritos en la clase. 

> [!info] **Características Operativas**
> * Tienen acceso total a los atributos de instancia de la clase.
> * Las subclases heredan estos métodos mediante la relación de herencia (`extends`) y pueden utilizarlos de manera directa, sin importar si la clase base es abstracta o regular.

---

## ⚠️ 8. Gestión de Errores: Arquitectura de Excepciones

### 🔹 El Bloque Estructural `try-catch-finally`
Las excepciones son errores anómalos que ocurren en tiempo de ejecución e interrumpen el flujo normal del código. Java implementa un mecanismo estructurado para capturarlos y evitar que la aplicación se caiga:

* **`try`:** Envuelve el bloque de código propenso a fallar.
* **`catch`:** Captura el tipo de excepción específico y define la lógica de mitigación o el mensaje de error.
* **`finally`:** Bloque de cierre que **siempre se ejecuta**, independientemente de si ocurrió una excepción o no. Es ideal para liberar recursos críticos (cerrar conexiones a BD o archivos).

### ⚖️ Categorías de Excepciones: Checked vs. Unchecked

| Característica | Excepciones Verificadas (Checked) | Excepciones No Verificadas (Unchecked) |
| :--- | :--- | :--- |
| **Obligación del Compilador**| ⚠️ **Obligatorio** manejarlas. El código no compila si no se controlan explícitamente. | 🔓 **Opcional**. El compilador no exige su captura en tiempo de diseño. |
| **Origen Común** | Errores por factores externos al control del código (red, bases de datos, archivos). | Errores de lógica en la programación o fallos internos del flujo. |
| **Mecanismo** | Se capturan con `try-catch` o se delegan con la palabra clave `throws`. | Se mitigan validando el código o usando capturas opcionales. |
| **Ejemplos Comunes** | `IOException`, `SQLException`. | `NullPointerException`, `ArithmeticException`. |

### 🛠️ Flujo de Lanzamiento: `throw` vs. `throws`
* **`throws`:** Se coloca en la **firma del método** para advertir al compilador y a otros desarrolladores qué tipo de excepciones es capaz de disparar dicho método.
* **`throw`:** Se coloca dentro del **cuerpo del método** para disparar u originar efectivamente la instancia de la excepción cuando se detecta una condición inválida.
#### 📝 Ejemplo Práctico de Validación:
```java
public void validar(int id) throws IllegalArgumentException {  
    if (id < 0) {  
        // Lanza efectivamente la excepción interrumpiendo el flujo
        throw new IllegalArgumentException("El ID no puede ser negativo");  
    }  
}
```

### Más detalles sobre la implementación de Excepciones Reales
* *Podemos ver los detalles en [[02-Manejo de Excepciones en Spring Boot]] donde explico las diferencias de cada uno, cuando y donde cada uno.*
---

## 🗃️ 9. Estructuras de Datos Avanzadas (Collections Framework)

Para optimizar el rendimiento de un sistema, la elección de la estructura de datos debe basarse en el tipo de operación más frecuente:

* **ArrayList (`¿Acceder por Índice?`):** Almacena elementos en un arreglo contiguo en memoria. Es extremadamente rápido para leer datos por su posición, pero es **lento al agregar o eliminar elementos** en posiciones intermedias porque obliga a reordenar el resto del arreglo.
* **LinkedList (`¿Insertar/Eliminar al Inicio?`):** Lista doblemente enlazada donde cada elemento apunta al siguiente y al anterior. Es ideal para insertar o borrar datos al inicio o al final de la colección en tiempo constante ($O(1)$), por lo que suele usarse en arquitecturas especiales de colas o pilas.
* **HashMap (`¿Buscar por Clave?`):** Estructura indexada en pares Clave-Valor. Es la opción más utilizada para búsquedas directas de alta velocidad, ya que recupera la información de forma casi instantánea mediante el cálculo de un código Hash.
* **HashSet (`¿Unicidad de Datos?`):** Colección que **ignora de forma automática los elementos repetidos**. Si intentas agregar exactamente el mismo valor dos veces, el conjunto lo detecta y solo conservará una única instancia en memoria.

---

## 📐 10. Los 5 Principios SOLID

Son las reglas esenciales de diseño de software orientadas a objetos para construir sistemas limpios, desacoplados, altamente mantenibles y escalables.

* **S - Single Responsibility (Responsabilidad Única):** Una clase debe tener una sola razón para cambiar. No se deben mezclar lógicas dispares (como negocio, persistencia y reportes) en un mismo archivo.
* **O - Open/Closed (Abierto/Cerrado):** El software debe estar abierto para extender su comportamiento, pero cerrado para modificar su código fuente original. Permite añadir nuevas funciones sin alterar lo que ya funciona.
* **L - Liskov Substitution (Sustitución de Liskov):** Las subclases deben poder reemplazar por completo a sus clases padre sin alterar la integridad ni romper el comportamiento del programa.
* **I - Interface Segregation (Segregación de Interfaces):** Es mejor tener muchas interfaces pequeñas y especializadas que una sola interfaz gigante. No se debe obligar a una clase a implementar métodos que no necesita utilizar.
* **D - Dependency Inversion (Inversión de Dependencias):** Los módulos de alto nivel no deben depender de módulos de bajo nivel; ambos deben depender de abstracciones (interfaces). Programar hacia abstracciones, no hacia implementaciones concretas.

---

## 💼 11. Aplicación Práctica de SOLID en Spring (Casos de Éxito)

> [!tip] **Uso de este bloque en Entrevistas Técnicas**
> Defender estos puntos demuestra experiencia real aplicando arquitectura limpia sobre el framework de Spring.

### 🔹 SRP (Single Responsibility) en la Arquitectura por Capas
En mis desarrollos con Spring, la separación estricta de componentes asegura que cada clase tenga un único motivo de cambio:
* **`Controller`:** Se encarga única y exclusivamente del transporte HTTP (mapear peticiones, validar formatos de entrada y retornar códigos de estado como 200, 400 o 500).
* **`Service`:** Concentra exclusivamente las **reglas y la lógica de negocio** pura de la empresa. No sabe nada de SQL ni de protocolos HTTP.
* **`Repository`:** Maneja exclusivamente el acceso, persistencia y comunicación con la base de datos relacional.

### 🔹 OCP (Open/Closed) en el Cálculo de Precios Dinámicos
* **Problema Común (Mal diseño):** Tener un método con múltiples bloques condicionales `if-else` o `switch` para evaluar si el precio de un producto es Normal, con Descuento, de Temporada, etc. Cada estrategia nueva obligaría a modificar y arriesgar el código existente.
* **Solución Aplicada (Buen diseño):** Definir una interfaz base o clase abstracta (`EstrategiaPrecio`) con un método abstracto `calcular()`. A partir de ahí, se heredan clases concretas aisladas: `PrecioNormal`, `PrecioDescuento` y `PrecioBuenFin`. Si el negocio requiere un nuevo tipo de precio a futuro, simplemente se añade una nueva clase **sin tocar ni modificar las clases anteriores**, manteniendo el sistema cerrado a modificaciones pero abierto a extensiones.

### 🔹 ISP (Interface Segregation) en Repositorios Especializados
* **Problema Común (Mal diseño):** Crear una interfaz masiva llamada `OrdenesRepository` que contenga decenas de métodos de reportería, auditoría, facturación y CRUD tradicional. Si un servicio de facturación inyecta esta interfaz, quedará acoplado a métodos que no le corresponden, y cualquier cambio en auditoría romperá el servicio de facturas.
* **Solución Aplicada (Buen diseño):** Dividir las interfaces por tareas atómicas y específicas. De este modo, las clases de servicios mandan a llamar únicamente a la interfaz exacta que van a utilizar, evitando dependencias e impactos colaterales innecesarios en el sistema.

### 🔹 DIP (Dependency Inversion) mediante la Inyección de Beans
* **Implementación:** Las clases de la capa de negocio (`Services`) nunca se acoplan ni dependen de las clases concretas de infraestructura (como una implementación directa de `JpaRepository` o una base de datos específica). 
* **Mecanismo:** El `Service` declara su dependencia apuntando a una **interfaz o abstracción**, y es el contenedor de inversión de control de Spring el encargado de inyectar dinámicamente la implementación correspondiente en tiempo de ejecución.
