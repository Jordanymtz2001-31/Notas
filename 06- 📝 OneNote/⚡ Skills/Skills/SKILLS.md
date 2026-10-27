# 🤖 Catálogo de Skills para Agentes de IA (Desarrollo y Arquitectura)

## 🎨 1. Frontend y Experiencia de Usuario

### 🖥️ `interface-design`
* **Propósito:** Provee pautas y heurísticas avanzadas para construir interfaces limpias, responsivas y altamente usables, enfocándose de manera prioritaria en **paneles de gestión, dashboards y módulos de administración**.
* **🎯 Ejemplo de Prompt:** *"Puedes ayudarme a revisar la usabilidad de la pantalla de inventarios en el frontend con ayuda de la skill `interface-design`."*

### 🌐 `webapp-testing`
* **Propósito:** Automatiza y estructura planes de prueba (*Testing*) diseñados para ejecutarse sobre el frontend simulando interacciones dentro de un **navegador web real** (E2E / End-to-End).

---

## ⚙️ 2. Backend, APIs y Buenas Prácticas

### 🐍 `django-expert`
* **Propósito:** Desbloquea un conocimiento profundo y especializado sobre el ecosistema de Django y Django REST Framework, aportando grandes ventajas de optimización, seguridad y rendimiento en la lógica del **Backend**.

### 🛣️ `api-design-principles`
* **Propósito:** Asegura que el diseño, nombrado de rutas, códigos de estado y payloads de tus **REST APIs** sigan rigurosamente las **buenas prácticas** internacionales de la industria (especialmente en entornos de Python).
* **🎯 Ejemplo de Prompt:** *"Revisa si la estructura de este microservicio cumple con las buenas prácticas apoyándote de la skill `api-design-principles`."*

### ⚠️ `error-handling-patterns`
* **Propósito:** Analiza, robustece y refactoriza el código implementando patrones avanzados de **manejo de excepciones y errores en el Backend** (como controladores globales o rollbacks eficientes).
* **🎯 Ejemplo de Prompt:** *"¿Puedes revisar si este bloque de código maneja correctamente los fallos utilizando la skill `error-handling-patterns`?"*

---

## 📈 3. Gestión de Cambios y Planificación

### ⏱️ `changelog-generator`
* **Propósito:** Automatiza la documentación técnica compilando un **registro histórico estructurado (Changelog)** con los cambios, adiciones o correcciones que has realizado durante una sesión de trabajo específica (organizado de forma diaria, semanal o por actualización de versión).
* **🎯 Ejemplo de Prompt:** *"Realiza un Changelog del día basándote en las refactorizaciones que completamos."*

### 🧠 `brainstorming`
* **Propósito:** Actúa como un consultor técnico para **planificar, modelar y estructurar el flujo lógico y la arquitectura** de una funcionalidad específica antes de tirar la primera línea de código.

> [!example] **Caso de Uso Común**
> Ideal para mapear casos de uso complejos y definir qué componentes se necesitan en el sistema.
> * **🎯 Ejemplo de Prompt:** *"Quiero crear la funcionalidad para agregar productos al carrito de compras, ¿me puedes ayudar a planificar el flujo completo usando la skill `brainstorming`?"*

---

## ⚡ 4. Skills instaladas en opencode (globales)

> [!info] **Estado al 2026-10-27**
> 10 skills instaladas en `~/.config/opencode/skills/` y `~/.agents/skills/` (rutas globales que opencode descubre; aplican a todos los proyectos).
> Las de origen externo se instalaron con el CLI oficial de skills.sh: `npx skills add <repo> --skill <nombre> --agent opencode -g -y`.
> opencode las carga al arrancar la sesión: hay que abrir una sesión nueva para verlas.

### 🔍 `find-skills`
* **Propósito:** Meta-skill: descubre e instala otras skills del ecosistema cuando aparece una tarea que requiere una capacidad especial ("¿hay una skill para X?").
* **Fuente:** `vercel-labs/skills` (oficial de Vercel) — 4.1M de instalaciones, auditorías Socket y Agent Trust Hub en Pass.
* **Ubicación:** `~/.agents/skills/`.

### 🧠 `brainstorming` *(instalada)*
* **Propósito:** Consultor obligatorio antes de cualquier trabajo creativo: descubre qué se quiere realmente antes de construir. Es la versión oficial de la skill descrita en la sección 3.
* **Fuente:** `obra/superpowers`.
* **Ubicación:** `~/.config/opencode/skills/`.

### 📝 `writing-plans`
* **Propósito:** Escribe planes de implementación estructurados antes de tareas de varios pasos que ya tienen requisitos definidos.
* **Fuente:** `obra/superpowers`.
* **Ubicación:** `~/.agents/skills/`.

### ▶️ `executing-plans`
* **Propósito:** Ejecuta un plan paso a paso con puntos de verificación en cada etapa. Diseñada para usarse en pareja con `writing-plans`.
* **Fuente:** `obra/superpowers`.
* **Ubicación:** `~/.agents/skills/`.

### 🐞 `systematic-debugging`
* **Propósito:** Depuración con método: observar → hipótesis → probar → verificar, antes de proponer cualquier arreglo. Evita ediciones al azar.
* **Fuente:** `obra/superpowers`.
* **Ubicación:** `~/.agents/skills/`.

### 🧪 `test-driven-development`
* **Propósito:** Ciclo **TDD**: escribir primero el test que falla, implementar lo mínimo que lo pase, verificar y refactorizar.
* **Fuente:** `obra/superpowers`.
* **Ubicación:** `~/.agents/skills/`.

### 🌐 `webapp-testing` *(instalada)*
* **Propósito:** Toolkit con Playwright para interactuar y probar aplicaciones web locales en un **navegador real**: verificar frontend, depurar comportamiento de UI, capturar pantallazas y ver logs. Es la versión oficial de la skill descrita en la sección 1.
* **Fuente:** `anthropics/skills` (oficial de Anthropic).
* **Ubicación:** `~/.agents/skills/`.

### 🎨 `frontend-design`
* **Propósito:** Guía de **diseño visual distintivo e intencional** al construir UI nueva o rediseñar una existente: paleta, tipografía y layout que no se lean como plantillas genéricas. Sustituye a `interface-design` del catálogo.
* **Fuente:** `anthropics/skills` (oficial de Anthropic).
* **Ubicación:** `~/.config/opencode/skills/`.

### ⚠️ `error-handling-patterns` *(creada para este entorno)*
* **Propósito:** Revisa y refuerza el **manejo de excepciones** del backend Spring Boot (`GlobalExceptionHandler`, excepciones custom), contrastando con la nota [[02-Manejo de Excepciones en Spring Boot]] del vault. Es una adaptación propia de la descripción de la sección 2.
* **Fuente:** creada en este entorno.
* **Ubicación:** `~/.config/opencode/skills/`.

### 📚 `buscar-en-vault`
* **Propósito:** Consulta de **solo lectura** del vault Obsidian: busca notas relacionadas con un tema, prioriza `03-Reflexiones/` y `06- 📝 OneNote/`, y devuelve hasta 5 con su ruta. Nunca escribe ni modifica el vault.
* **Fuente:** creada en este entorno.
* **Ubicación:** `~/.config/opencode/skills/`.

> [!note] **Skills del catálogo aún NO instaladas**
> `django-expert`, `api-design-principles` y `changelog-generator` siguen siendo solo descripciones de las secciones 1‑3; `interface-design` fue sustituida por `frontend-design`.
