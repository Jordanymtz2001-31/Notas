# 📅 Planeación: 2026-08-20

## ☀️ Foco Principal del Día
*¿Cuál es la única cosa que si logro hoy hará que el día valga la pena?*
- [x] Realizar las 5 Pruebas de Integracion de  [[07-Pruebas]] (@2026-08-27 08:00) ✅ 2026-08-31
## 📝 Tareas
- [x] 8:00 Investigar como implementar las pruebas 📅 2026-08-24 ✅ 2026-08-24
- [x] 8:00 Estudias el codigo de la primera prueba con ayuda de Google, realizar documentacion en el codigo fuente. 📅 2026-08-25 ✅ 2026-08-25
- [x] 8:00 Estudias el codigo de la segunda prueba, realizar la documentacion 📅 2026-08-26 ✅ 2026-08-26
- [x] 8:00 Realizar la 3 pruebas 📅 2026-08-27 ✅ 2026-08-28
- [x] 8:00 13 Pruebas de propiedad con jqwik 📅 2026-08-26 ✅ 2026-08-26

### 📆 Agenda de Hoy
```tasks
not done
due today
```

### ⏭️ Agenda de Mañana
```tasks
not done
due tomorrow
```

### ⚠️ Tareas Retrasadas (Atrasos)
```tasks
not done
due before today
```


> [!abstract] 💭 Reflexiones y Notas - Primera prueba de POST
> *Tomar encuenta que ambos se ejecutan dentro app*
> 1. Todos los tests del servicio-profesor
>cd app sh mvnw -pl servicio-profesor test
> 2. Un solo archivo de test 
>sh mvnw -pl servicio-profesor test -Dtest=ProfesorUpdateTest
> 3. Un solo método específico
>sh mvnw -pl servicio-profesor test Dtest=ProfesorUpdateTest#actualizacionPreservaIdentidadYIncorporaCambios
> 4. Ver output detallado (sin colapsar)
> sh mvnw -pl servicio-profesor test -Dtest=ProfesorUpdateTest -Dsurefire.useFile=false
> 
>-  Para estas pruebas sigue el mismo el mismo [[02-Patron AAA]] pero con un poco diferente.
>- No simulamos nada como en Mockito, estamos usando la base de datos real (en memoria) para meter datos aleatorios masivos.
>- Kiro me coloco @Transactional tanto en la clase como en el metodo pero investigue que basta con que solo este arriba de la clase, por que por defecto los metodos tambien se vuelven transaccionales automaticamente. No necesitamos replicar(redundante).

---

> [!abstract] 💭 Reflexiones y Notas - Segunda prueba de GET 
> - **Propósito:** Demostrar que el sistema procesa colecciones masivas sin errores.
>- Aplica los mismo 3 puntos de la primera prueba
>- Para este caso se usaron 2 metodos el cual depende de la operacion del test.                                                                                                          
> 
>		 1 Solo metodo: Se usa cuando probamos objetos independientes **uno a uno** (Ej: Guardar un profesor individual). `jqwik` sabe inyectar primitivos (`String`, `int`) nativamente campo por campo usando anotaciones como `@ForAll`.
>	 
>		 2 Métodos (Test + Fábrica): Se usa cuando el test requiere colecciones complejas como **estructuras de listas** (`List<ProfesorDTO>`). Como `jqwik` no sabe armar listas con objetos personalizados de forma nativa, requiere un método fábrica (`@Provide`) que defina las reglas de generación de la lista y sus elementos, para luego enviarla completa al método principal (`@Property`).
>	 
> - **Estándar Profesional:** Se coloca el método de prueba (`@Property`) en la parte superior porque documenta la intención del test, y los métodos generadores (`@Provide`) abajo como soporte utilitario.
> - **Advertencias de Nulidad en el IDE (Warnings Naranjas)**:
> 
> 		Origen: No es un error de lógica del código. Es un falso positivo del análisis estático del compilador (motor Eclipse subyacente en VS Code). Al usar la sintaxis abreviada de referencia a método (`ProfesorDTO::id`), el IDE se confunde con los metadatos de nulidad y asume que el objeto origen (`this`) podría estar desprotegido o ser `null`.
> - **Soluciones Profesionales:**
>
> 		Expresión Lambda Tradicional (`p -> p.id()`): Resuelve la ambigüedad del IDE al declarar explícitamente la variable de entrada del flujo.		    
> 
> 		Anotación `@SuppressWarnings("null")`: Instrucción directa al compilador para silenciar advertencias teóricas de nulidad dentro de un entorno controlado de pruebas.

---

> [!abstract] 💭 Reflexiones y Notas - Tercera prueba de PUT 
> - **Propósito:** Garantiza que al actualizar los datos de un profesor, el sistema **no le cambie el ID**. Si el profesor era el ID `7`, debe seguir siendo el ID `7`. No debe generar un registro nuevo ni duplicarse.
>- Aplica los mismo 3 puntos de la primera prueba
>- Se realizaron termino la prueba de PUT con exito. Es menos complejo que el segundo y facil de entender
>- Se ejecutaron los 3 test y ya fueron documentados sus comandos.

---

> [!abstract] 💭 Reflexiones y Notas - Cuarta prueba de DELETE 
> - **Propósito:** La operación de eliminación **no lanza ninguna excepción (el profesor existía)**. Al hacer un **GET** posterior sobre el mismo id lanza ProfesorNotFoundException (ausencia confirmada).
>- Aplica los mismo 3 puntos de la primera prueba

---

> [!abstract] 💭 Reflexiones y Notas - Quinta prueba de GET-Busqueda 
> - **Propósito:** Verifica que para cualquier id que no exista en la base de datos, las tres Operaciones(GET,PUT,DELETE) retornan 404
>- Aplica los mismo 3 puntos de la primera prueba

---

> [!abstract] 💭 Reflexiones y Notas - Capas que se usaron
> 1. Capa de Servicio (Service) Todos los tests inyectan el servicio con @Autowired. *Es el punto de entrada. Los tests no pasan por el Controller.*
> ```java
> @Autowired
private AlumnoService alumnoService;
>```
> 1. Capa de Repositorio (`Repository`) No se inyecta explícitamente, pero se ejercita de forma real a través del servicio. JPA ejecuta las queries contra H2.
> 2. Capa de Repositorio (`Repository`) No se inyecta explícitamente, pero se ejercita de forma real a través del servicio. JPA ejecuta las queries contra H2.
> 3. Capa de DTOs (AlumnoDTO / ProfesorDTO) Todos los tests operan con DTOs — los construyen como input y verifican los campos del DTO devuelto.
> 4. Capa de Excepciones (`ProfesorNotFoundException`, etc.) Tests como `ProfesorNotFoundTest` verifican que las excepciones del dominio se lanzan correctamente desde el servicio.
## 📌 Recordatorios Flotantes (Backlog)
*Espacio para ideas o pendientes sin fecha fija.*
- [ ] 📌
