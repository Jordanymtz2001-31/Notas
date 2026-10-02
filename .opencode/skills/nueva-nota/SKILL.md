---
name: nueva-nota
description: Crea una nota nueva en el vault Obsidian con el destino y la plantilla correctos. Usar cuando el usuario pida crear, apuntar, guardar, registrar o anotar una duda, reflexión, concepto, proyecto, planeación, bitácora o "una nota sobre X", o cuando diga "créame una nota", "apúntalo", "ponlo en el diario", "esto lo estudio después". Identifica la carpeta, propone UNA sola pregunta, aplica la plantilla y reporta la ruta final.
license: MIT
metadata:
  idioma: es-MX
  alcance: vault-obsidian
---

# Crear una nota nueva en el vault

Procedimiento para crear una nota en la carpeta correcta, con la plantilla correcta y sin notas
huérfanas. La regla de oro: **nunca escribir un archivo antes de que el usuario confirme destino.**

## Cuándo usar esta skill

Úsala cuando pidan crear, apuntar o registrar algo del vault: "créame una nota sobre X",
"apúntalo", "ponlo en el diario", "esto lo guardo como duda", "empezaremos un proyecto de Y",
"anota que mañana haré Z".

**No la uses** para revisar notas existentes — eso es `revision-notas`.

## Paso 1 — Clasificar por naturaleza, no por tema

El tema no decide la carpeta: el mismo tema puede vivir en dos sitios. Decide por **qué clase de
contenido escribes**:

| El contenido es | Carpeta | Plantilla | Formato de nombre |
| :--- | :--- | :--- | :--- |
| Pregunta o concepto que quiero entender mejor (duda) | `03-Reflexiones/` | `Dudas y Reflexiones.md` | `NN-Título.md` |
| Desarrollo o reflexión de un tema ya estudiado | `03-Reflexiones/` | `Dudas y Reflexiones.md` | `NN-Título.md` |
| Material de consulta de una tecnología: comandos, pasos, diferencias, teoría | `06- 📝 OneNote/<Tema>/<Subtema>/` | — nota suelta | `<Título>.md` |
| Proyecto nuevo con objetivo y tareas | `01- 🏗️ Proyecto/` | `Proyecto.md` | `NNN-Título.md` |
| Planeación de un día concreto | `02- 📓 Diario/` | `Diario.md` | `YYYY-MM-DD.md` |
| Tema suelto del diario: bitácora, comandos, listas | `02- 📓 Diario/` | — nota suelta | `NN-Título.md` |
| Idea que todavía no empiezo | **No se crea** | tarea en `02- 📓 Diario/11-Backlog.md` con `#backlog` | — |

Trucos de clasificación:

* Si la frase suena a **"¿por qué pasa esto?"** o **"no entiendo X"** → duda → `03-Reflexiones/`.
* Si suena a **"así se hace X"** con pasos o comandos → material de consulta → `06- 📝 OneNote/`.
* Si suena a **"voy a hacer X"** con entregables → proyecto → `01- 🏗️ Proyecto/`.
* Si es **"mañana haré X"** → planeación → `02- 📓 Diario/`.
* Si es **"algún día estudiaré X"** → no crees la nota. Añade la tarea a `11-Backlog.md` con
  `#backlog`. Es la opción que más evita notas vacías.

## Paso 2 — Calcular el número siguiente

1. Lista la carpeta de destino.
2. Toma todos los prefijos numéricos iniciales de los `.md` (no de las subcarpetas).
3. Siguiente = máximo + 1, **con el mismo número de cifras** que el patrón de esa carpeta:

| Carpeta | Cifras | Ejemplo de existentes | Siguiente |
| :--- | :--- | :--- | :--- |
| `02- 📓 Diario/` | 2 | `11-Backlog.md`, `14-Comandos…` | `15-` |
| `03-Reflexiones/` | 2 | `02-Inmutabilidad.md`, `01-10.…` | `03-` |
| `01- 🏗️ Proyecto/` | 3 | `007-…md` | `008-` |

**No renumeres las notas existentes.** Cada lote conserva su prefijo aunque quede desordenado.

## Paso 3 — Una sola pregunta, con la propuesta armada

Junta carpeta, plantilla y nombre en **una única pregunta**. El usuario confirma con un clic o
corrige. No preguntes primero el tipo y luego la carpeta.

Si la nota va a `03-Reflexiones/`, incluye en la misma pregunta el campo `categoria:` (ver Paso 5).

Si el tema no tiene carpeta en `06- 📝 OneNote/`, **pregunta si crearla**. No la crees por tu cuenta.

Si el usuario duda entre duda y reflexión: son la misma plantilla, solo cambia `tipo`. No insistas.

## Paso 4 — Crear el archivo

1. Copia la plantilla desde `04-Plantillas/`. **Nunca edites la plantilla.**
2. Sustituye `{{title}}` por el título y `{{date}}` por la fecha de hoy en `YYYY-MM-DD`.
3. Rellena el frontmatter y no agregues propiedades que no estén en `.obsidian/types.json`
   (`tags`, `aliases`, `fecha_*`, `tipo`, `estatus`, `categoria`, `fuente_origen`).
4. `06- 📝 OneNote/` es la única carpeta **sin frontmatter**: empieza en `# Título`.
5. Nunca en la raíz del vault ni dentro de `04-Plantillas/`.

## Paso 5 — Puente con OneNote si aplica

Solo si la nota es una duda o reflexión y pertenece a un tema de OneNote:

* **En el frontmatter de la nueva nota**, rellena con el tema real de OneNote:

  ```yaml
  categoria: "🗄️ DB → SQL"
  fuente_origen: "[[Preguntas de SQL para entrevistas]]"
  ```

* `03-Reflexiones/` es **plano**: el tema va en `categoria`, nunca en subcarpetas.

* **Opcional y solo si el usuario lo pide:** añade en la nota de OneNote, junto al concepto, una
  línea dentro de un callout que enlace de vuelta:

  ```markdown
  > [!tip] **Primary Key**
  > Definición corta, a modo de recordatorio.
  > Para más detalles de mi experiencia revisa [[03-Primary Key]]
  ```

  Nunca dupliques el desarrollo de la Reflexión dentro de OneNote.

## Paso 6 — Verificar antes de reportar

* [ ] Un solo `# ` H1 en la nota nueva.
* [ ] Fences en número par, todos con lenguaje declarado.
* [ ] Callouts en minúscula, con `> [!tipo] **Título**>` y cada línea de continuación con `>`.
* [ ] Ningún `{{title}}` ni `{{date}}` sin sustituir.
* [ ] Todo `[[wikilink]]` que hayas escrito apunta a un archivo que exista de verdad.
* [ ] La nota no quedó en la raíz ni en `04-Plantillas/`.
* [ ] Si creaste carpeta nueva en OneNote, fue porque el usuario lo aprobó.

## Paso 7 — Reportar

Cierra con la ruta final y el wikilink listo para pegar:

```text
✅ Nota creada: 03-Reflexiones/03-Primary Key.md
   Plantilla: 04-Plantillas/Dudas y Reflexiones.md
   categoria: 🗄️ DB → SQL
   Enlace: [[03-Primary Key]]
```

Si el usuario no pidió el puente con OneNote, dilo explícitamente para que decida si lo añade.
