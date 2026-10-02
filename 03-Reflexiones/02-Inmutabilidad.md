---
tipo: duda-investigacion
estatus: FInalizado
categoria: Pilares de Software
fecha_registro: 2026-09-15
fecha_resolucion: 2026-09-15
fuente_origen: "[[002-Gestion de ALumnos]]"
---

# Inmutabilidad

> [!info] ❓ **La Duda / Reflexión**
> Escribe aquí la pregunta o el pensamiento inicial de forma clara.
> 1. ¿Que es Inmutabilidad?
> 2. ¿Donde se ve en Spring Boot?
> 3. ¿Por que es importante?
> 

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** AL tener luna entrevista con NSGular me dieron reto de que tengo que estudiar mas esa parte
*   **Sospecha inicial:** Solo se que el objeto no debe de ser modificable.

---

## 🔍 Investigación y Respuestas
### Fuentes Consultadas
* [x] Buscar en Google / Documentación Oficial: ✅ 2026-09-15
* [x] Preguntar a IA / Foros: ✅ 2026-09-15

 
 > [!abstract] Resumen de Inmutabilidad
> La **inmutabilidad** es uno de los pilares más importantes en el desarrollo de software moderno y la arquitectura de microservicios.
>
> En términos sencillos, un objeto es **inmutable** si **su estado no puede ser modificado después de haber sido creado**. Si necesitas cambiar algo, no modificas el objeto original; en su lugar, creas una copia nueva con el dato actualizado.

### ¿Por qué es vital la Inmutabilidad?
- **Seguridad en Hilos (Thread-Safety):** Spring Boot maneja por defecto sus componentes como *Singletons* (una sola instancia compartida por miles de usuarios al mismo tiempo). Si un objeto es inmutable, múltiples usuarios pueden leerlo a la vez por internet sin el riesgo de que un usuario altere los datos del otro. Evita condiciones de carrera catastróficas.
- **Efectos Secundarios Cero:** Garantiza que si pasas un objeto como parámetro a un método o a otro servicio, ese método no va a alterar tus datos originales a tus espaldas.
- **Facilidad para hacer Tests:** En tus pruebas con `jqwik` y `Mockito`, es muchísimo más fácil predecir y comparar el comportamiento de objetos cuyos valores están blindados y no cambian mágicamente a mitad del flujo.

---

### ¿Dónde se implementa en Spring Boot?

#### A. En los DTOs (Data Transfer Objects)
- Los datos que viajan por la red (JSONs de entrada y salida) deben ser inmutables. Una vez que recibes un `POST` con los datos de un alumno, esos datos son una "fotografía" del momento; no deberían cambiar mientras se procesan.
- **⚠️ Clarificación Importante:** Los **DTOs** (creados como `record`) son **100% Inmutables**; no tienen métodos `set()`. No debes confundirlos con las **Entidades/Modelos de la Base de Datos**, que sí son **Mutables** (tienen setters y getters tradicionales) porque Hibernate los necesita para actualizar las tablas. Y al crear los objetos junto con el metodo Builder en base a los dtos se vuelven con doble candado de seguridad. 
#### **¿Cuál es el chiste de la inmutabilidad si necesito hacer cambios?** 
- Modificacion en Dtos(Inmutable).
Para "modificar" un objeto inmutable, se utiliza el **Patrón Builder**. En lugar de alterar el objeto existente, el Builder actúa como un constructor temporal que clona los datos anteriores, aplica el cambio y, al ejecutar `.build()`, sella un objeto **completamente nuevo** en memoria.

```java
// No modificamos al alumno1 (sigue intacto), creamos al alumno2 con la edad cambiada
AlumnoDTO alumno2 = alumno1.toBuilder().edad(26).build();
```

- Modificacion en Entidad 
	Lo anterior aplica para objetos inmutables(dtos), en el caso de actualizaciones directas hacia la base de datos se usa la entidad mutables del modelo que recuperamos de la base de datos.
	
```java
profesor.setNombre(dto.nombre());
profesor.setApellidoP(dto.apellidoP());
profesor.setApellidoM(dto.apellidoM());
profesor.setEdad(dto.edad());
profesor.setEspecialidad(dto.especialidad());

Profesor updated = profesorRepository.save(profesor);
return toDTO(updated);
```

- **Getters (o métodos de lectura):** Se usan en todo el proyecto para lectura pura de datos. *(Nota: En los Java Records no se usa la palabra "get", se lee directo con el nombre del método: `alumnoDTO.nombre()`)* .
- **Setters:** Modifican el objeto original directamente en la memoria RAM. **Su uso se reserva exclusivamente para las `@Entity` de JPA/Hibernate** donde la mutabilidad es obligatoria para sincronizar los cambios con las bases de datos.

#### B. En la Inyección de Dependencias (Componentes de Spring)
- Tus servicios (`@Service`) y controladores (`@Controller`) deben tener sus dependencias protegidas para que ningún proceso en caliente pueda reemplazar tu repositorio o cliente web por otra instancia.

#### C. En la Configuración (`@ConfigurationProperties`)
- Los archivos de propiedades (`application.properties`) que lee Spring al arrancar deben cargarse de forma inmutable para evitar que el código altere contraseñas o URLs de bases de datos en tiempo de ejecución.

---

### ¿Cómo se implementa? (Código Profesional)

#### 🛠️ Caso 1: DTOs Inmutables con Java Records
- Desde Java 16+, la mejor forma absoluta de crear DTOs inmutables es usando **`record`**. Un record automáticamente define todos sus campos como `private final`, crea un constructor con todos los campos y elimina los métodos `set()`.

#### 🛠️ Caso 2: Inyección de Dependencias Inmutable (Por Constructor)
- La inyección por atributos (`@Autowired`) es aceptada en tests, pero **en producción está desaconsejada**. El estándar profesional exige **Inyección por Constructor combinado con la palabra clave `final`** .

```java
@Service
public class AlumnoServiceImpl implements AlumnoService {

    // final garantiza que una vez asignado el repositorio al arrancar, NADIE pueda cambiarlo
    private final AlumnoRepository alumnoRepository;
    private final ProfesorClient profesorClient;

    // Spring Boot inyecta de forma segura mediante el constructor
    public AlumnoServiceImpl(AlumnoRepository alumnoRepository, ProfesorClient profesorClient) {
        this.alumnoRepository = alumnoRepository;
        this.profesorClient = profesorClient;
    }
}
```

> [!tip] Tip de Productividad
> Si usas Lombok, puedes ahorrarte escribir el constructor manualmente usando la anotación `@RequiredArgsConstructor` arriba de la clase, la cual genera automáticamente el constructor para todos los campos marcados como `final`.

---

### 🛑 ¿Dónde NO se usa la Inmutabilidad? (La Excepción)
Las **Entidades de Base de Datos (`@Entity` de JPA/Hibernate)** son la gran excepción a la regla de inmutabilidad en Spring Boot.
Debido a cómo funciona Hibernate internamente para sincronizar los cambios con la base de datos (el ciclo de vida del *Entity Manager*), las entidades **necesitan ser mutables**. Por eso en tus entidades `Profesor` o `Alumno` de JPA sigues utilizando métodos `.setNombre()` convencionales o el patrón `@Builder` mutable para poder actualizar registros mediante un `repository.save()`.

---

### ¿Tiene relación el método `Stream` con la inmutabilidad?
**Sí, una relación total y absoluta.** la API de Streams de Java (`.stream()`) fue diseñada bajo la filosofía de la **Programación Funcional**, la cual tiene como regla número uno la **inmutabilidad**.

Opera como una tubería que procesa colecciones de forma inmutable: transforma los elementos en vuelo sin tocar jamás la lista original, devolviendo siempre una estructura de datos nueva y aislada.

```java
List<Long> ids = creados.stream()
    .map(p -> p.id())
    .toList();
```

El método `stream()` cumple rigurosamente con dos principios inmutables:
- **No modifica la lista original:** La lista `creados` se mantiene exactamente igual antes y después del stream. Ningún elemento dentro de ella fue alterado, ordenado o eliminado.
- **Produce una estructura nueva e independiente:** El método `.toList()` (en versiones de Java moderno) devuelve una lista **completamente nueva** que, además, es **inmutable por defecto**. Si intentas hacer un `.add()` posterior, el sistema arrojará un error.

#### 🍎 Ejemplo Analógico: Mutabilidad vs Inmutabilidad
Imagina que tienes una caja con 3 manzanas rojas (Lista Original):

- **Si usas Setters (Mutabilidad):** Tomas la manzana número 1 de la caja original y le pintas encima con un marcador verde (`manzana.setColor("Verde")`) . Modificaste tu caja original; la manzana roja dejó de existir.
- **Si usas Streams (Inmutabilidad):** Pasas las manzanas por una banda transportadora (`.stream()`) . La banda genera una réplica exacta de cada manzana pero en color verde (`.map(...)`) y las deposita en una **caja totalmente nueva** (`.toList()`) . Al final del proceso, conservas tu caja original con sus manzanas rojas intactas , y obtienes una nueva caja reluciente con manzanas verdes.

#### **3 casos reales y más comunes** donde usarás Streams en tu código de producción

1.1 Convertir una Lista de Entidades a una Lista de DTOs (Mapeo)

- Este es el uso número uno en Spring Boot. Cuando consultas la base de datos, el repositorio te regresa una lista de Entidades (`List<Alumno>`). Por buenas prácticas de seguridad e inmutabilidad, no puedes exponer la entidad directa al controlador; tienes que transformarla en `List<AlumnoDTO>`.
```java
@Override
public List<AlumnoDTO> findAll() {
    List<Alumno> alumnos = alumnoRepository.findAll();
    
    // Usamos el stream para transformar la lista de entidades a DTOs
    return alumnos.stream()
            .map(alumno -> toDTO(alumno)) // Transforma cada Alumno en AlumnoDTO
            .toList(); // Devuelve una lista nueva e inmutable
}

```

1.2 Filtrar información según reglas de negocio (`.filter()`)
- Imagina que el jefe de proyecto te pide un endpoint que devuelva únicamente los alumnos que son **mayores de edad** (edad >= 18) y cuyo profesor asignado sea el ID `10`. En lugar de hacer ciclos `for` con un montón de `if` anidados, usas un filtro inmutable.
 ```java
 public List<AlumnoDTO> findAlumnosMayoresDeEdad(Long profesorId) {
    List<Alumno> todosLosAlumnos = alumnoRepository.findAll();

    return todosLosAlumnos.stream()
            .filter(alumno -> alumno.getEdad() >= 18)        // Filtro 1: Solo mayores de edad
            .filter(alumno -> alumno.getProfesorId().equals(profesorId)) // Filtro 2: Solo de ese profesor
            .map(this::toDTO)                                // Los transformamos a DTO
            .toList();
}

 ```

1.3 Buscar un elemento específico dentro de una lista (`.findFirst()`)
- A veces mandas a traer una lista de configuraciones o datos de otro microservicio mediante Feign y necesitas extraer **un solo registro** que cumpla con una condición muy específica. Si no lo encuentra, debes lanzar una excepción personalizada.
```java
public AlumnoDTO findAlumnoPorEmailEspecifico(String emailBuscar) {
    List<Alumno> alumnos = alumnoRepository.findAll();

    return alumnos.stream()
            .filter(alumno -> alumno.getEmail().equalsIgnoreCase(emailBuscar))
            .map(this::toDTO)
            .findFirst() // Intenta atrapar al primero que cumpla la condición
            .orElseThrow(() -> new AlumnoNotFoundException("No se encontró el alumno con email: " + emailBuscar));
}

```

>[!tip]  En Spring Boot, los **Streams** son el motor principal de la capa de **Servicio (`@Service`)**. Se usan para:
>
>1. **Mapear:** Transformar colecciones de Entidades a DTOs usando `.map()`.
>2. **Filtrar:** Descartar datos que no cumplan con reglas de negocio usando `.filter()`.
>3. **Buscar:** Extraer elementos únicos usando `.findFirst()` combinado con `.orElseThrow()`.  
>    Todo esto sin mutar la lista original traída por el repositorio.

 ---

## 🚀 Acciones a tomar (Next Actions)
- [ ] 
