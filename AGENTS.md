# Vault Obsidian — Reglas Generales

Este archivo es para agentes de IA (opencode, Claude Code, Cursor). No es una nota del vault.
Contiene las convenciones que aplican a **todas** las carpetas. Las reglas específicas de una
carpeta concreta viven en el `AGENTS.md` de esa carpeta, que tiene prioridad sobre este.

## Identidad del vault

- Es un **vault de Obsidian**personal de estudio, en español.
- Es un repositorio git con **auto-commit y push automáticos cada 15 minutos** (plugin `obsidian-git`),
  remoto `origin → github.com/Jordanymtz2001-31/Notas.git`. **No lances `git push` a mano**: espera a
  que lo haga el plugin. No hay build, lint ni tests que correr.
- **No todo se sube:** `.gitignore` deja fuera `.trash/`, los `.json` de interfaz de Obsidian y la
  media pesada (`.mp4`, `.mp3`, `.mov`, …). GitHub no es un backup completo del vault.
- La carpeta `.opencode/` y este archivo son configuración del agente, no contenido del vault.

## Estructura

| Carpeta | Contenido |
| :--- | :--- |
| `01- 🏗️ Proyecto/` | Proyectos reales. Un subdirectorio por proyecto. |
| `02- 📓 Diario/` | Planeación diaria (`YYYY-MM-DD.md`) y bitácora (`NN-Título.md`). |
| `03-Reflexiones/` | Dudas y reflexiones, **plano** (el tema va en `categoria:`, no en subcarpetas). Notas `NN-Título.md`. |
| `04-Plantillas/` | Plantillas base. **No editar las plantillas, crear notas a partir de ellas.** |
| `05-Contenido_TikTok/` | Guiones y listas de contenido. |
| `06- 📝 OneNote/` | Archivo histórico importado de OneNote. 2 niveles: `<Tema emoji>/<Subtema>/<Nota>.md`. |
| `.trash/` | Papelera de Obsidian. **Prohibido leer, escribir o restaurar aquí.** |

## Idioma

- Todo el contenido en **español**, con tildes, `¿?` y `¡!` correctos.
- Los identificadores técnicos van en inglés o tal cual los da la API: `findById()`, `spring.jpa.hibernate.ddl-auto`, `try-catch`.
- Nombres de archivo y títulos en español.
- Nunca traduzcas un nombre de clase, anotación o comando. `try-catch` no es `intentar-atrapar`.

## Frontmatter (YAML)

- **`06- 📝 OneNote/`: NUNCA usar frontmatter.** Verificado: 0 de 24 notas lo tienen. Empezar directo en `# Título`.
- `01- 🏗️ Proyecto/` y `03-Reflexiones/`: sí lo usan. Mantener el que ya tenga la nota.
- `02- 📓 Diario/`: normalmente sin frontmatter; la fecha va en el H1 como `# 📅 Planeación: YYYY-MM-DD`.
- No agregar propiedades que no estén declaradas en `.obsidian/types.json` (principalmente `tags`, `aliases`, y las `fecha_*`).

## Enlaces

- Wikilinks con `[[NombreDeLaNota]]`, **sin ruta**, solo el nombre de archivo. Obsidian resuelve por nombre.
- Un wikilink **debe apuntar a una nota que exista**. Antes de crear `[[X]]`, verifica que `X.md` esté en el vault.
- Si la nota de destino no existe pero el contenido está en otro lado, enlaza a la nota real en vez de crear una vacía.
- Las referencias a ejercicios o prácticas van con el formato `📍 *==> Ref: <Ejercicio> - Semana N - Enucom N*` en cursiva.
- Si un detalle pertenece a `03-Reflexiones/`, en la nota que enlaza pon solo el `[[enlace]]` y una línea de contexto. No dupliques el desarrollo.

## Callouts

Formato con título en negrita. El título va justo después de `] ` y **todas las líneas del bloque
empiezan con `>`**. El `>` va al **inicio** de cada línea, nunca al final del título: si cierras la
línea de título con `>`, ese `>` se queda dentro del título y se ve al renderizar.

```markdown
> [!tip] **Título en negrita**
> Contenido del callout.
```

Tipos válidos, **siempre en minúscula**: `note`, `abstract`, `info`, `todo`, `tip`, `success`,
`question`, `warning`, `failure`, `danger`, `bug`, `example`, `quote`.

- **Minúscula siempre.** `> [!NOTE]`, `> [!WARNING]`, `> [!IMPORTANT]` funcionan en Obsidian, pero
  no son idiomáticos en este vault. Es cuestión de estilo, no de validez.
- **Alias** (equivalen al tipo base; usa el tipo base salvo que la nota ya lo use así):
  `abstract` → `summary`, `tldr` · `tip` → `hint`, `important` · `success` → `check`, `done` ·
  `question` → `help`, `faq` · `warning` → `caution`, `attention` · `failure` → `fail`, `missing` ·
  `danger` → `error` · `quote` → `cite`.
- **Tipos no listados** (`hotel`, `peligro`, `code`, …) **caen al estilo de `note`**, según la
  fuente oficial: *"any unsupported type defaults to the `note` type"*. No rompen, pero no tienen
  ni el color ni el icono propios del tipo que creías elegir.
- **Inválido del todo:** usar mayúsculas en el tipo (`[!TIP]`) sí funciona en Obsidian, pero no es
  idiomático aquí. Una etiqueta en vez de un tipo va **después**: `> [!info] Alias`.
- Todo bloque de líneas que sigue al `>` inicial debe llevar `>` al inicio. Si una línea se queda
  sin `>`, el callout se corta y el texto se sale del bloque.

## Tareas

Sintaxis del plugin **Tasks** (`.obsidian/plugins/obsidian-tasks-plugin`):

- Consulta embebida: ```` ```tasks ```` con `not done`, `due today`, `due tomorrow`, `due before today`.
- Fecha límite: `📅 2026-08-24`. Fecha de creación: `(@2026-10-01 17:00)`.
- Marcada: `- [x] Tarea ✅ 2026-08-21`.
- Pendiente suelto sin fecha: `- [ ] Idea #backlog`.

## Formato general

- Separar bloques temáticos mayores con `---`.
- **Listas con `*`**, no con `-`. Es el estilo del vault.
- Negrita en el **término clave** al definir un concepto: `**Inyección de Dependencias:** ...`.
- Todo bloque de código con el **lenguaje declarado** en el fence: ```` ```java ````, ```` ```bash ````,
  ```` ```properties ````, ```` ```text ````, ```` ```mermaid ````. Nunca fence sin lenguaje.
- **Todo fence abierto debe cerrarse.** Antes de terminar una nota, cuenta los fences: debe ser un
  número par. Un fence sin cerrar se traga todo el contenido que se agregue después.
- Títulos de sección con emoji representativo del tema: `## 🏗️ Construcción`, `## 🔐 Seguridad`.

## Creación de notas

**Nunca crees una nota sin proponer primero el destino y la plantilla en una sola pregunta.**
Primero identifica de qué se trata, arma la propuesta concreta (carpeta + plantilla + nombre) y
espera el sí del usuario. No preguntes en dos pasos.

| Lo que es | Carpeta | Plantilla | Nombre |
| :--- | :--- | :--- | :--- |
| **Duda** / "no entiendo X" / concepto que quiero entender | `03-Reflexiones/` | `Dudas y Reflexiones.md` | `NN-Título.md` |
| **Reflexión** / desarrollo de un tema | `03-Reflexiones/` | `Dudas y Reflexiones.md` | `NN-Título.md` |
| **Material de consulta** de una tecnología: comandos, pasos, diferencias, teoría | `06- 📝 OneNote/<Tema>/<Subtema>/` | — nota suelta | `<Título>.md` |
| **Nuevo proyecto** | `01- 🏗️ Proyecto/` | `Proyecto.md` | `NNN-Título.md` |
| **Planeación** de un día concreto | `02- 📓 Diario/` | `Diario.md` | `YYYY-MM-DD.md` |
| **Tema suelto** del diario (bitácora, comandos, listas) | `02- 📓 Diario/` | — nota suelta | `NN-Título.md` |
| "Lo estudio después", sin haber empezado | **No se crea nota** | tarea en `02- 📓 Diario/11-Backlog.md` con `#backlog` | — |

- La decisión es por la **naturaleza del contenido**, no por el tema. El mismo tema puede vivir en
  dos carpetas: una explica, la otra desarrolla tu experiencia. Ver `## Vinculación Reflexiones y OneNote`.
- **El número `NN` es el siguiente libre de esa carpeta.** Se calcula listando la carpeta, tomando
  el prefijo numérico más alto y sumando 1. Con dos dígitos en `03-Reflexiones/` y
  `02- 📓 Diario/`, con tres en `01- 🏗️ Proyecto/`.
- **No se renumeran** las notas existentes: cada lote conserva su prefijo.
- Duda y reflexión comparten plantilla; solo cambia la propiedad `tipo`
  (`duda-investigacion` / `reflexion`).
- **Nunca** crear notas en la raíz del vault ni dentro de `04-Plantillas/`.

## Vinculación Reflexiones y OneNote

`06- 📝 OneNote` es el archivo de referencia **por tecnología**; `03-Reflexiones/` es la
investigación **personal** con pregunta, fuentes y conclusión. Un concepto puede estar en ambas, y
en ese caso **una enlaza a la otra sin duplicar el desarrollo**.

* **En OneNote → Reflexiones:** junto al concepto, una línea dentro de un callout:

  ```markdown
  > [!tip] **Primary Key**
  > Definición corta, a modo de recordatorio.
  > Para más detalles de mi experiencia revisa [[03-Primary Key]]
  ```

* **En Reflexiones → OneNote:** en el frontmatter, con las propiedades que ya trae la plantilla:

  ```yaml
  categoria: "🗄️ DB → SQL"
  fuente_origen: "[[Preguntas de SQL para entrevistas]]"
  ```

* `categoria` es el **tema** de OneNote donde encaja. `03-Reflexiones/` es **plano**: el tema va en
  `categoria`, nunca en subcarpetas.
* Si el tema **no tiene carpeta** en OneNote, **pregunta si crearla**. No la crees por tu cuenta.
* Si la nota de Reflexión ya está resuelta, marcar `estatus` en el frontmatter.

## Verificación de afirmaciones

Antes de **corregir al usuario** o de **escribir un hecho técnico** en una nota, contrástalo con la
fuente oficial y cita esa fuente.

* **Fuente oficial primero:** JLS (Java), documentación de Spring, MDN (web), PostgreSQL, RFC,
  docs de la herramienta. Después libros o tutoriales. Un blog de desconocido no sirve como prueba.
* Si no puedes verificarlo, dilo explícitamente: `Sin verificar: ...`. Nunca lo presentes como cierto.
* **Alcance: solo hechos técnicos.** Se verifica cuando (a) vas a corregir algo que dijo el usuario,
  (b) vas a escribir un hecho técnico en una nota de estudio, o (c) el usuario afirma un hecho
  técnico. No verifiques cada frase de la conversación.
* Si la fuente contradice al usuario, **repórtalo y deja que él decida**. Nunca lo arregles en silencio.
* Si contradice algo ya escrito en una nota, repórtalo como hallazgo con la ruta de la nota,
  sin editar hasta que lo apruebe.

## Reglas de edición

- **Preguntar antes de renombrar o mover una nota.** El vault tiene `alwaysUpdateLinks: true`, así que
  Obsidian actualiza los enlaces, pero un enlace actualizado puede apuntar a otra nota si el nombre
  se repite.
- **No borrar ni sobrescribir** una nota existente sin confirmación explícita.
- No crear notas directamente en la raíz del vault. Cada nota va en su carpeta temática.
- No tocar `.obsidian/` ni `.trash/` sin preguntar. El plugin `obsidian-icon-folder` depende de
  `.obsidian/icons`, y esos iconos no se editan a mano.
- Al agregar contenido a una nota existente, respetar el estilo y la estructura que ya tiene. No
  reformatear secciones que no tengan que ver con el cambio pedido.
