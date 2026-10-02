---
name: revision-notas
description: Revisa notas de un minibaúl de este vault Obsidian en 4 pasadas (estructura, ortografía, exactitud técnica, sintaxis Markdown/callouts) y entrega un reporte de cambios. Usar cuando el usuario pida revisar, corregir, auditar o mejorar notas, o pida corregir un minibaúl completo.
license: MIT
metadata:
  idioma: es-MX
  alcance: vault-obsidian
---

# Revisión de notas de un minibaúl

Procedimiento reproducible para auditar las notas de un subárbol del vault. Cada sesión aplica los
mismos criterios, en el mismo orden, para que el resultado sea comparable entre ejecuciones.

## Cuándo usar esta skill

Úsala cuando pidan revisar, corregir, auditar o depurar notas de un minibaúl: "revísame el
minibaúl de Docker", "corrige mis notas de Java", "revisa que estas notas estén bien".

**No la uses** para crear una nota nueva. Para eso está la skill `nueva-nota`, que elige carpeta,
plantilla y número; aquí solo se revisa lo que ya existe.

## Entradas

Antes de revisar, identifica:

1. **Qué notas entran.** Carpeta o conjunto explícito. Si piden "el minibaúl", son todas las `.md`
   del subdirectorio, recursivamente. Lista los archivos antes de empezar.
2. **Qué reglas aplican.** El `AGENTS.md` de la raíz del vault más el `AGENTS.md` de la carpeta
   específica. Si no existe el de la carpeta, dedúcelo de las notas vecinas y menciona que es una
   inferencia.
3. **El alcance de la corrección.** Si el usuario no lo dijo, pregunta antes de editar:
   - Solo ortografía y estructura, o también contenido técnico.
   - Aplicar los cambios o solo reportar.

## Las 4 pasadas

Ejecútalas en orden. No mezcles pasadas: cada una detecta una clase de defecto distinto, y el
orden evita que un arreglo estructural mueva las líneas y te haga perder el punto.

### Pasada 1 — Estructura

Ramas de código primero, porque un defecto de estructura desplaza todos los números de línea.

- **Fences balanceados.** Cuenta las líneas cuyo fence abre o cierra con ```` ``` ````. Debe salir un
  número par. Si sale impar, busca el último fence abierto sin pareja: **todo lo que viene después
  queda dentro del bloque de código**. Es el defecto más destructivo; repórtalo primero.
- **Un solo H1.** Un segundo `#` significa que el autor estaba escribiendo una "continuación".
  Se convierte en `---` seguido de la siguiente sección numerada.
- **Numeración correlativa de H2.** Extrae el número de cada `##` y compara contra la secuencia
  esperada. Reporta duplicados y huecos.
- **Nivel de encabezado correcto.** Un subtema no numerado que usa `##` debería ser `###`.
- **Separadores `---`** entre bloques temáticos mayores.

### Pasada 2 — Ortografía

Solo ortografía, tildes y puntuación. **No toques el estilo de redacción**: la prosa de estas notas
es deliberada y ya funciona.

- Tildes faltantes o de más, y `¿?` / `¡!` invertidos.
- Palabras pegadas (`opautomatizar`), capitalizaciones en medio (`SPring`), palabras sin tilde.
- Concordancia de número y género: *"Existe varias pruebas"*, *"Los cuales son importante"*.
- Sustantivos técnicos mal escritos. Las palabras que no son español no se corrigen:
  `getAllById`, `EntityManager`, `findById`.

### Pasada 3 — Exactitud técnica

Esta pasada es la de mayor valor y la que más necesita criterio. **Corrige solo lo que puedas
confirmar; no edites lo discutible.**

Regla de oro: si un enunciado es claramente falso o desactualizado y la corrección es inequívoca,
corrígelo. Si es una cuestión de opinión, de versión o de estilo técnico, **no lo toques** y repórtalo
en la sección de hallazgos.

**Antes de dar una corrección por buena, búscala en la fuente oficial y cítala:** JLS para Java,
documentación de Spring para Spring Boot, MDN para web, PostgreSQL para SQL, los docs de la
herramienta para lo demás. Un blog anónimo no sirve como prueba. Si no encuentras la fuente, no
corrijas: repórtalo como `Sin verificar: ...` y deja la decisión al usuario.

- Orden de preferencia: **documentación oficial → libro técnico → tutorial → blog**.
- Si la fuente oficial contradice algo ya escrito en la nota, repórtalo con la ruta de la nota y
  **no edites hasta que el usuario lo apruebe**.
- Si contradice lo que dijo el usuario en la conversación, repórtalo y que él decida. Nada de
  arreglar en silencio.

Puntos que ya se han detectado en este vault, para no volver a tropezar:

- `@Override` **no es obligatorio** en Java. Es una recomendación fuerte; sí es obligatorio en C# y
  TypeScript. Afirmar lo contrario es un error que rebaten en entrevista.
- Tomcat y Netty **no se sustituyen automáticamente**. Spring MVC usa Tomcat; Spring WebFlux usa
  Reactor Netty porque es otro starter. Cambiar de motor en MVC requiere excluir
  `spring-boot-starter-tomcat` y agregar Jetty o Undertow.
- `spring.jpa.hibernate.ddl-auto` en producción: el estándar es **`validate`** + Flyway o
  Liquibase. `none` solo le dice a Hibernate que no haga nada y no valida el esquema.
- `CrudRepository` no se limita a `save`, `findById` y `deleteById`. También `findAll`, `count`,
  `existsById`, `saveAll`, `deleteAll`.
- Un **status HTTP 404 no es una excepción**: es una respuesta válida. No se captura en un `catch`.
  En el `catch` caen timeouts y fallos de conexión; el 404 se detecta leyendo la respuesta.
- Una excepción `catch` de tipo concreto no se alcanza si el código nunca la lanza.
- En general, cuando notes "automáticamente", "siempre" u "obligatoriamente", verifícalo: casi
  siempre hay una excepción o es una recomendación, no una regla del lenguaje.

### Pasada 4 — Sintaxis Markdown y callouts

- **Callouts con tipo válido**, en minúscula. Válidos: `note`, `abstract`, `info`, `todo`, `tip`,
  `success`, `question`, `warning`, `failure`, `danger`, `bug`, `example`, `quote`.
- **No confundir alias con tipo inventado.** `important`, `hint` y `tldr` son alias válidos de `tip`
  en Obsidian, así que `[!important]` en minúscula **sí es válido** y no debe "corregirse" a
  `warning`. Los tipos inventados (`hotel`, `peligro`) sí se renderizan como cita plana sin color
  ni plegado.
- **Callout bien cerrado**: toda línea de continuación empieza con `>`. Si una línea se queda sin
  `>`, el texto se sale del callout.
- **Título de callout en negrita** cuando la nota usa ese estilo de forma consistente.
- **Lenguaje declarado** en cada fence de código.
- **Wikilinks resueltos.** Verifica que el archivo de destino exista antes de dar el enlace por
  bueno. Reporta los rotos sin arreglarlos de golpe: crea la nota es decisión del usuario.
- **Tablas bien formadas**: mismo número de columnas por fila y separador `| :--- |`.

## Reporte

Cierra con dos tablas. Nada de prosa adicional.

**Cambios aplicados** — una fila por corrección concreta:

| Nota | Línea | Tipo | Antes | Después |
| :--- | :--- | :--- | :--- | :--- |
| `Preguntas de Spring…` | 70 | técnica | Netty reemplaza automáticamente a Tomcat | … |

**Hallazgos sin aplicar** — lo dudoso, desactualizable o de decisión del usuario:

| Nota | Línea | Hallazgo | Por qué no se tocó |
| :--- | :--- | :--- | :--- |
| `Preguntas de Spring…` | 243 | Compara Servlet 2.5 vs 3.0 | Desactualizado para Spring Boot 3.x, pero es tema del usuario |

Tipos de hallazgo válidos: `desactualizado`, `discutible`, `decisión del usuario`, `no verificado`.

## Reglas de la revisión

- **No borres ni sobrescribas** una nota sin confirmación. Si una pasada exige reescribir más del
  30% de una nota, párate y pregunta.
- **Respeta el estilo existente** al corregir. Cambia la línea que está mal, no la sección entera.
- **Una pasada, un archivo.** Termina un archivo antes de pasar al siguiente, para que el reporte
  refleje el estado real.
- Si una corrección revela que dos reglas del `AGENTS.md` están desactualizadas, menciónalo
  al final para que las actualice.
- Nunca escribas en `.trash/` ni edites `.obsidian/` sin preguntar.
