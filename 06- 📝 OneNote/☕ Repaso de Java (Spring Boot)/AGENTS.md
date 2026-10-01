# ☕ Repaso de Java (Spring Boot) — Reglas de la carpeta

Reglas del agente para este subtree. Hereda todo lo de `/AGENTS.md` (raíz del vault); aquí solo
se define lo propio de las notas de repaso de entrevistas técnicas.

## Propósito de la carpeta

Notas de **repaso para entrevistas técnicas** en Java y Spring Boot, escritas para poder leerlas en
voz alta y responder. No es documentación de referencia ni un tutorial paso a paso: es un guion de
entrevista. El lector ya conoce la teoría; la nota le sirve para recordar la frase exacta, el
ejemplo y la analogía.

Estructura de 2 niveles:

| Carpeta | Nota |
| :--- | :--- |
| `Java/` | `Preguntas de Java para entrevistas.md` — POO, herencia, polimorfismo, excepciones, Collections, SOLID. |
| `Spring Boot/` | `Preguntas de Spring para entrevistas.md` — IoC, autoconfiguración, MVC, REST, Servlets, JPA/Hibernate, seguridad. |
| `Spring Boot/` | `TEST.md` — estrategias y herramientas de testing. |

## Esquema de encabezados

```markdown
# <emoji> Título de la nota          ← UNO solo por nota

## <emoji> N. Sección                ← N correlativo, sin saltos ni repeticiones
Texto de la sección.

### <emoji> Subtítulo                ← detalle dentro de la sección N
```

- **Un solo H1 por nota.** Cuando el tema crece y quieres continuar, **no** agregues otro `#`.
  Cierra con `---` y abre la siguiente sección numerada.
- La numeración de `##` es **correlativa de 1 a N** y nunca se repite. Si insertas una sección en
  medio, renumera las siguientes. Cuando el tema crece dentro de una nota, **renumera desde el
  punto de inserción hacia abajo**: es el defecto estructural más frecuente de estas notas.
- Un subtema que no merece número va como `###`, nunca como `##` suelto.
- Cada H2 y H3 lleva un emoji representativo del contenido.
- Antes de cada `##` numerado, un separador `---`.

### Antes de terminar, verifica

- [ ] Un solo H1.
- [ ] `##` numerados 1..N, sin duplicados ni huecos.
- [ ] Número par de fences de código (todos cerrados).
- [ ] Todo callout con `> [!tipo] **Título**` y continuación con `>`.
- [ ] Ningún `>*` ni `>-`: son bullets rotos y no se renderizan como lista.
- [ ] Ningún placeholder tipo `[INDEX]`, `[PENDIENTE]` o `[TODO]`.
- [ ] Línea en blanco entre un párrafo y el encabezado que le sigue.

## Callouts

Tipo en **minúscula**. Los que se usan en esta carpeta:

| Tipo | Para qué |
| :--- | :--- |
| `[!note]` | Documento interno, avance de estudio, contexto personal. |
| `[!abstract]` | Resumen para no releer todo el bloque. |
| `[!tip]` | Consejo de estrategia en la entrevista. |
| `[!info]` | Explicación conceptual, servlets, analogías. |
| `[!important]` | Alias válido de `tip`. Para "esto sí te lo van a preguntar". |
| `[!warning]` | Riesgo real (ej. `ddl-auto` en producción). |
| `[!question]` | Pregunta que te puedes hacer en la entrevista. |
| `[!example]` | Ejemplo concreto o analogía. |
| `[!success]` | Resumen de la estrategia aplicada. |

Formato obligatorio, con `>` en la primera línea para continuar:

```markdown
> [!tip] **Título en negrita**
> Contenido del callout.
> - Lista interna del callout.
```

`[!IMPORTANT]` en mayúscula es válido pero no idiomático: escribe el tipo en minúscula. Lo único que se
renderiza como cita plana, sin color ni plegado, son los **tipos inventados**: `hotel`, `peligro`.
Para una etiqueta en vez de un tipo, va **después**: `> [!info] Alias`.

## Referencias y enlaces

- Ejercicio o práctica de la escuela:

  ```markdown
  📍 *==> Ref: Ejercicio 7 - Semana 1 - Enucom 3*
  ```

- Experiencia real en un proyecto (Polihules, Textiles): entrecomillada, en primera persona, como
  respuesta hablada. Ej. *"En *Textiles* lo implementé para blindar los endpoints del backend…"*

- Detalle técnico que ya vive en `03-Reflexiones/`: solo el enlace y una línea de por qué importa.
  No repitas el desarrollo.

  ```markdown
  Para más detalles revisa [[02-Manejo de Excepciones en Spring Boot]].
  ```

## Tablas comparativas

Úsalas siempre que compares dos o más cosas. Es el recurso más eficiente para una entrevista:

```markdown
| Característica | Opción A | Opción B |
| :--- | :--- | :--- |
| **Cuándo usarla** | ... | ... |
```

Ya existen para: Spring vs Spring Boot, Checked vs Unchecked, `CrudRepository` vs `JpaRepository`,
Monolito vs Microservicios, Tomcat vs Netty, REST API vs RESTful API.

## Analogías

Las analogías son el recurso más valioso de estas notas: hacen que un concepto se memorice. Cuando
un concepto sea abstracto, inclúyela y ponle nombre explícito.

Ejemplos ya en uso, para no repetirlas ni contradecirlas:

- **Hotel y habitaciones** → Servlets, `DispatcherServlet`, `@RestController`.
- **Fórmula 1** → ORM vs JPA vs Hibernate (concepto / reglamento / auto).
- **Paciente vs medicina** → CRUD (`JpaRepository`) vs acceso de bajo nivel a la BD (`EntityManager`).
- **Cascada de filtros** → arquitectura por capas, el cliente no conoce al servidor final.

Escribe la analogía como un bloque con callout, no como prosa suelta.

## Código

- **Siempre declara el lenguaje** en el fence: ```` ```java ````, ```` ```bash ````,
  ```` ```properties ````, ```` ```text ````, ```` ```mermaid ````.
- Ejemplos de Java completos y compilables, con clase `Main` si son ejecutables.
- Usa diagramas `mermaid` para jerarquías de herencia, flujos de petición y relaciones entre capas.
- Comenta solo lo que no se deduce leyendo el código.
- **Todo fence abierto debe cerrarse.** Un fence sin cerrar se traga todo el contenido posterior.

## Estilo de redacción

- Prosa explicativa, no viñetas sueltas. Las listas son para enumerar, no para desarrollar.
- **Negrita en el término clave** al definir: `**Inyección de Dependencias:** …`.
- Marca los errores comunes con `❌`, las buenas prácticas con `✅`, los riesgos con `⚠️`.
- Números de complejidad: `$O(1)`, `$O(n)$`.
- Mermaid cuando la jerarquía de clases o el flujo de una petición no se entiende en prosa.

## Reglas de edición específicas

- Estas notas son densas y ya están escritas. Al corregir, **cambia solo lo que esté en el alcance
  pedido**: no reescribas frases enteras, no cambies el orden de las secciones, no unifiques el
  estilo de redacción que ya funciona.
- Ortografía y tildes: **sí se corrigen siempre**, es una regla dura.
- Correcciones técnicas: solo las confirmadas. Si detectas algo dudoso o desactualizable, **no lo
  edites**: repórtalo como hallazgo para que decida.
- Al agregar una sección nueva, mantén la numeración correlativa y el emoji del estilo.
- Si una nota queda muy larga, **no la partas sin preguntar**. Por ahora la preferencia es
  que todo el tema Java quede en `Preguntas de Java…` y todo el tema Spring en
  `Preguntas de Spring…`.
