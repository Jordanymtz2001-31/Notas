# 🐍 Python Core y Programación Orientada a Objetos (POO)

Este apunte centraliza los conceptos avanzados de Python, estructuras de datos, patrones de diseño bajo POO y los principios fundamentales de SOLID para desarrollo de software.

---

## 🏛️ Fundamentos de la Programación Orientada a Objetos (POO)

> [!info] **¿Qué es la POO?**
> Es un paradigma de programación que organiza el código estructurándolo en base a **Objetos**. Cada objeto encapsula sus propios datos (atributos) y comportamientos (métodos), permitiendo modelar de manera fiel las entidades del mundo real.

---

## ⚡ Conceptos Avanzados de Python

> [!info] 🚀 Funciones Lambda (Anónimas)
> Son ideales para escribir funciones cortas, temporales y resolver lógicas en una sola línea de código. Suelen combinarse con funciones de orden superior como `map()`, `filter()` y `reduce()`.

> [!code] 📦 Gestión de Argumentos Dinámicos
> * `*args` ➡️ Captura un número variable ($n$) de argumentos posicionales como una **Tupla**.
> * `**kwargs` ➡️ Captura argumentos condicionales o nombrados como un **Diccionario** (Clave/Valor).

> [!faq]+ 🛠️ ¿Qué es un Decorador?
> Es una función que recibe otra función como parámetro, **modifica o extiende su comportamiento** y la devuelve, todo esto sin alterar el código fuente original de la función empaquetada.

---

## 📊 Estructuras de Datos Nativas

> [!abstract] **Análisis de Colecciones**
> En Python, las estructuras básicas permiten aplicar algoritmos de recorridos, búsquedas, ordenamiento, filtrado y acumulación de forma clara y eficiente.

| Estructura | ¿Es Mutable? | Características Clave |
| :--- | :---: | :--- |
| **Listas** | ✅ SÍ | Colección ordenada de elementos de cualquier tipo. Puede cambiar de tamaño dinámicamente. |
| **Tuplas** | ❌ NO | Colección ordenada e **inmutable**. Funciona como una lista fija y protegida en memoria. |
| **Arrays** | ✅ SÍ | Colección mutable, pero **obliga** a que todos sus elementos sean del mismo tipo de dato. |
| **Sets** | ✅ SÍ | Colección **no ordenada y sin elementos duplicados**. Ideal para verificar pertenencia. |
| **Diccionarios** | ✅ SÍ | Colección mutable estructurada en pares `Clave: Valor`, donde las claves actúan como campos de búsqueda rápidos. |

> [!tip] 📏 Regla Práctica de Elección
> * **Usa Lista:** Si el orden de inserción importa y los datos van a cambiar constantemente.
> * **Usa Tupla:** Si necesitas asegurar que los datos no se modifiquen durante la ejecución.
> * **Usa Set:** Si necesitas eliminar duplicados de forma masiva o validar si un elemento existe.
> * **Usa Dict:** Si necesitas realizar búsquedas ultrarrápidas indexadas por una clave única.

### ⚖️ Operadores de Comparación: `is` vs `==`
* `==` ➡️ Compara **valores**. Evalúa si el contenido (números, cadenas) es igual.
* `is` ➡️ Compara **identidad**. Evalúa si ambas variables apuntan exactamente al mismo objeto en la memoria RAM.

---

## 🧱 Pilares de la Programación Orientada a Objetos (POO)

> [!important] **Constructor (`__init__`)**
> Es el método especial encargado de inicializar los atributos de un objeto en el momento exacto de su creación. Se ejecuta de forma automática al instanciar la clase.

### 1. 🔍 Abstracción
Consiste en aislar y mostrar únicamente las características esenciales de un objeto (forma general), ocultando los detalles complejos de la implementación. En código se expresa mediante Clases Abstractas e Interfaces.

> [!faq]+ 🧩 Clases Abstractas, Interfaces y ABC (Abstract Base Classes)
> * **Clase Abstracta (Superclase):** Una plantilla incompleta que define comportamiento común para que sus subclases lo hereden. **No se puede instanciar directamente.**
> * **Métodos Abstractos:** Se declaran en la superclase usando el decorador `@abstractmethod`. No tienen cuerpo y **obligan** a las subclases a implementarlos con su propia lógica.
> * **Módulo `abc`:** Es la librería nativa de Python utilizada para crear clases abstractas.
> * **Interfaces:** En Python no existen de forma nativa; se simulan con clases 100% abstractas (módulo `abc`). Funcionan como un contrato profesional para obligar a clases muy distintas a implementar los mismos métodos.
> * *Referencias: Repaso_PYTHON (Level 2) \| Enucom 2 (Práctica 13)*

### 2. 🔒 Encapsulamiento
Protección del estado interno de un objeto restringiendo el acceso directo a sus atributos. En Python se maneja mediante convenciones de guiones bajos y decoradores.

> [!code] 🎛️ Convenciones de Acceso y Propiedades
> * `self.atributo` ➡️ **Público:** Accesible desde cualquier lugar del código.
> * `self._atributo` ➡️ **Protegido:** Aviso de uso interno. Indica que debe usarse con cuidado fuera de la clase.
> * `self.__atributo` ➡️ **Privado:** Activa el *Name Mangling* para dificultar su acceso directo fuera de la clase.
> * `@property` ➡️ Decorador utilizado para obtener de forma segura el valor de un atributo (Getter).
> * `@atributo.setter` ➡️ Decorador utilizado para validar, modificar o asignar un nuevo valor al atributo (Setter).
> * *Referencias: Repaso_PYTHON (Level 2) \| Enucom 2 (Práctica 2)*

### 3. 🧬 Herencia
Permite crear una clase nueva (Hija / Subclase) basada en una existente (Padre / Superclase), heredando y reutilizando todos sus atributos y métodos.
* **Uso de `super()`:** La palabra reservada `super().__init__()` se utiliza para invocar al constructor, métodos o atributos de la clase padre directamente desde la clase hija.
* *Referencias: Repaso_PYTHON (Level 2) \| Enucom 2 (Práctica 8)*

### 4. 🎭 Polimorfismo
Es la capacidad de que diferentes objetos respondan al llamado de un mismo método o acción, pero ejecutando comportamientos distintos según su tipo.

> [!abstract] 🔄 Tipos de Polimorfismo
> * **SobreEscritura (Overriding):** Redefinir un método heredado de la clase padre dentro de la clase hija, manteniendo exactamente la misma firma (nombre y parámetros) para cambiar su comportamiento. *(Común en Python)*.
> * **SobreCarga (Overloading):** Crear múltiples métodos con el mismo nombre pero diferentes parámetros en la misma clase. **Nota técnica:** Python nativo no soporta la sobrecarga de métodos tradicional; se simula usando argumentos por defecto (`None`) o parámetros dinámicos (`*args`).
> * **De Parámetro (Genéricos):** El mismo método o estructura funciona para cualquier tipo de dato (ej: un tipo genérico `List[T]` mapea Clientes, Productos o Pedidos por igual).
> * *Referencias: Repaso_PYTHON (Level 2) \| Enucom 2 (Práctica 15)*

---

## 🧬 Relaciones entre Objetos y Tipos de Métodos

### 🏗️ Composición vs. Agregación
* **Composición ("Tiene un" / Relación Fuerte):** Una clase contiene objetos de otra clase como componentes vitales. Tienen un ciclo de vida acoplado: **Si el objeto Padre muere, los objetos Hijos mueren con él.** (Ej: Una nota es parte de un cuaderno).
* **Agregación (Relación Débil):** El objeto secundario existe independientemente y sobrevive sin el objeto principal. (Ej: Un programador se agrega a un `Equipo`. Si el equipo se desintegra, el programador sigue existiendo independiente).

### ⚙️ Tipos de Métodos en Clases
1. **Método de Instancia:** Recibe `self` como primer parámetro. Puede acceder y modificar tanto los atributos del objeto concreto como los estáticos de la clase.
2. **Método de Clase:** Usa el decorador `@classmethod` y recibe `cls` como primer parámetro. Modifica o accede al estado de la clase global, no del objeto individual.
3. **Método Estático:** Usa el decorador `@staticmethod`. No recibe ni `self` ni `cls`. Funciona como una función utilitaria aislada que no altera el estado de la clase ni del objeto, pero pertenece al contexto de la clase.
4. **Métodos Concretos:** Métodos tradicionales dentro de clases abstractas que sí tienen su código completamente implementado y listo para ser heredado o ejecutado directamente.

---

## 📐 Los 5 Principios SOLID

Son las reglas de diseño fundamentales para escribir software limpio, mantenible, desacoplado y altamente escalable en POO.

* **S - Single Responsibility (Responsabilidad Única):** Una clase debe tener una, y solo una, razón para cambiar. No mezcles lógica de negocio, conexiones a bases de datos y generación de reportes en el mismo archivo.
* **O - Open/Closed (Abierto/Cerrado):** El software debe estar abierto para extender su comportamiento, pero cerrado para su modificación. Agrega nuevas funcionalidades creando nuevas clases en lugar de romper el código que ya funciona.
* **L - Liskov Substitution (Sustitución de Liskov):** Las subclases deben poder reemplazar por completo a sus clases padre sin alterar el comportamiento correcto del programa.
* **I - Interface Segregation (Segregación de Interfaces):** Es mejor tener muchas interfaces específicas que una sola interfaz grande y genérica. No obligues a una clase a implementar métodos que no necesita usar.
* **D - Dependency Inversion (Inversión de Dependencias):** Depende de abstracciones (interfaces o clases abstractas), no de implementaciones concretas. Desacopla tus módulos inyectando las dependencias.

---

## 💬 Respuestas de Alto Impacto para Entrevistas

> [!faq]+ 🗫 ¿Has aplicado los principios SOLID en tus proyectos de desarrollo?
> **Sí, constantemente.** 
> * **En entornos con Spring Boot:** El uso de las anotaciones `@Service` y `@Repository` junto a las Interfaces de Java implementa de forma nativa los principios de **Responsabilidad Única (SRP)** e **Inversión de Dependencias (DIP)** mediante la inyección de dependencias.
> * **En entornos con Django Monolítico:** Debido a la arquitectura nativa MVT (Model-View-Template) de Django, se prioriza la velocidad de desarrollo acoplando ciertos componentes. Sin embargo, en arquitecturas modernas de **Microservicios (FastAPI / Spring Boot)**, la implementación manual de SOLID es una práctica obligatoria y recomendada para mantener los servicios completamente desacoplados y resilientes.

---

## 🗫 Respuestas Estratégicas para Entrevistas Técnicas

> [!faq]+ 🟩 ¿Por qué la POO es la opción estándar en el desarrollo de software empresarial?
> Porque en proyectos medianos y grandes, la POO aumenta drásticamente la **escalabilidad y mantenibilidad** del software. Permite la **reutilización de código** de forma segura, reduce la redundancia y facilita el trabajo en equipo al aislar los componentes del sistema en módulos independientes.

> [!faq]+ 🟩 ¿Por qué decides utilizar tú la POO en tus proyectos diarios?
> La utilizo principalmente para **organizar el código y garantizar que sea mantenible** a largo plazo. Me permite separar responsabilidades de forma limpia dentro de clases específicas y reutilizar comportamientos complejos mediante el uso estratégico de la herencia y las interfaces, evitando el código espagueti.

> [!faq]+ 🟩 ¿Cómo equilibras el uso de principios de diseño (como SOLID) en diferentes frameworks de Python?
> * **En Django Monolítico:** Prefiero priorizar la velocidad de desarrollo y el pragmatismo que ofrece el patrón nativo **MVT (Model-View-Template)** sobre la aplicación de un diseño SOLID puro, respetando la filosofía del framework.
> * **En Microservicios (Django REST / FastAPI):** El enfoque cambia por completo. Aquí sí implemento capas de **Services manuales** para desacoplar la lógica de negocio de los controladores, garantizando el principio de **Responsabilidad Única (SRP)** y manteniendo los servicios altamente mantenibles, testeables y desacoplados.
