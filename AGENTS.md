# Vault Obsidian — Reglas Generales

Este archivo es para agentes de IA (opencode, Claude Code, Cursor). No es una nota del vault.
Contiene las convenciones que aplican a **todas** las carpetas. Las reglas específicas de una
carpeta concreta viven en el `AGENTS.md` de esa carpeta, que tiene prioridad sobre este.

## Identidad del vault

- Es un **vault de Obsidian**personal de estudio, en español.
- No es un repositorio git. No hay build, lint ni tests que correr.
- La carpeta `.opencode/` y este archivo son configuración del agente, no contenido del vault.

## Estructura

| Carpeta | Contenido |
| :--- | :--- |
| `01- 🏗️ Proyecto/` | Proyectos reales. Un subdirectorio por proyecto. |
| `02- 📓 Diario/` | Planeación diaria y bitácora. Notas numeradas `NN-Título.md`. |
| `03-Reflexiones/` | Dudas técnicas puntuales. Notas numeradas `NN-Título.md`. |
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

Formato con título en negrita, y `>` al final de la primera línea para continuar:

```markdown
> [!tip] **Título en negrita**
> Contenido del callout.
```

Tipos válidos, **siempre en minúscula**: `note`, `abstract`, `info`, `todo`, `tip`, `success`,
`question`, `warning`, `failure`, `danger`, `bug`, `example`, `quote`.

- **Minúscula siempre.** `> [!NOTE]`, `> [!WARNING]`, `> [!IMPORTANT]` funcionan en Obsidian, pero
  no son idiomáticos en este vault. Es cuestión de estilo, no de validez.
- `important`, `hint` y `tldr` son **alias válidos** de `tip`, pero no están en la lista de arriba.
  Antes de un alias, prefiere `tip` o `warning`.
- **Inválidos:** cualquier tipo inventado (`hotel`, `peligro`, `importante`). Se renderizan como cita
  plana, sin color ni plegado. Una etiqueta en vez de un tipo va **después**: `> [!info] Alias`.
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
