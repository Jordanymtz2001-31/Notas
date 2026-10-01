# 🌿 Flujo de Trabajo en Equipo con Git (Git Workflow)

Este apunte detalla el proceso estándar para colaborar en un repositorio de forma segura con un equipo de desarrollo, protegiendo las ramas principales.

---

## 🏗️ Fase 1: Clonar e Iniciar el Día

> [!info] 1. Descargar el Proyecto
> Cuando te unes al equipo, te compartirán la URL del repositorio central (GitHub/GitLab). Usa este comando para bajarlo a tu computadora por primera vez:
> ```bash
> git clone <URL_DEL_REPOSITORIO>
> ```

> [!important] 2. Sincronizar antes de empezar a programar
> Antes de tocar el código para una nueva tarea, **siempre** debes actualizar tu rama principal local para tener los últimos cambios que tus compañeros subieron a la nube.
> ```bash
> git checkout develop   # O 'main' según use tu equipo
> git pull
> ```
> * **¿Qué hace `git pull`?** Hace dos tareas automáticas: baja los cambios remotos (`git fetch`) y los fusiona de inmediato con tu repositorio local (`git merge`).

---

## 💻 Fase 2: Desarrollo de la Tarea

> [!tip] 3. Crear una Rama de Trabajo
> **Nunca trabajes directamente sobre main o develop.** Crea una rama independiente para tu tarea (llamadas *feature branches*).
> ```bash
> git checkout -b feature/NOMBRE-TAREA   # Crea la rama y te mueve a ella
> git branch                             # Verifica en qué rama estás parado
> ```

> [!example] 4. Guardar Avances (Commits)
> Mientras editas código y corres tus pruebas unitarias con *jqwik*, ve guardando tus fotos de progreso (commits) de manera organizada:
> ```bash
> git status                             # Visualiza qué archivos modificaste
> git add .                              # Agrega todos los cambios al área de preparación
> git commit -m "feat: Implemento endpoint de login con validaciones"
> ```

---

## 🔀 Fase 3: Integración del Código (Estrategias del Equipo)

Cuando terminas tu tarea, llega el momento de fusionar tu código con la rama del equipo (`develop` o `main`). Aquí tienes las dos opciones comunes que aplican las empresas:

### 🥇 Opción 1: Mediante Pull Request / Code Review (La más segura y común)
*Esta opción la controla el Líder Técnico o tus compañeros mediante una interfaz web.*

> [!faq]+ Paso a paso: Opción 1
> 1. **Sube tu rama al servidor remoto:**
>    ```bash
>    git push -u origin feature/NOMBRE-TAREA
>    ```
> 2. **Crea un Pull Request (PR) o Merge Request (MR):** 
>    Entras a GitHub/GitLab y abres el PR hacia `develop`. Aquí esperas a que tus compañeros validen y aprueben tu código. El líder del equipo será quien presione el botón de fusionar.
> 3. **Limpia tu entorno local después de la fusión:**
>    Una vez que el líder aceptó tu código en la nube, regresas a tu terminal en Linux para limpiar las ramas viejas:
>    ```bash
>    git checkout develop                  # Regresas a la rama principal
>    git pull                              # Traes el merge que hizo el líder
>    git branch -d feature/NOMBRE-TAREA    # Eliminas tu rama local ya fusionada
>    ```
> 4. **(Opcional) Borrar la rama remota si la plataforma no lo hizo:**
>    ```bash
>    git push origin --delete feature/NOMBRE-TAREA
>    ```

---

### 🥈 Opción 2: Fusión Manual Local (Para equipos pequeños o proyectos propios)
*En esta opción, tú mismo eres el encargado de resolver conflictos y subir la rama principal ya fusionada.*

> [!faq]+ Paso a paso: Opción 2
> 1. **Asegúrate de respaldar tu rama antes:**
>    ```bash
>    git checkout feature/NOMBRE-TAREA
>    git add .
>    git commit -m "feat: Implemento alta de clientes"
>    git push -u origin feature/NOMBRE-TAREA
>    ```
> 2. **Pásate a la rama principal y actualízala:**
>    ```bash
>    git checkout develop
>    git pull
>    ```
> 3. **Realiza la fusión local:**
>    ```bash
>    git merge feature/NOMBRE-TAREA
>    ```
>    *Si surgen conflictos de código con el trabajo de otros compañeros, los resuelves en tu editor, guardas y haces un commit del merge.*
> 4. **Sube la rama principal actualizada al servidor remoto:**
>    ```bash
>    git push origin develop
>    ```


- TOKEN:  <tu_PAT_aquí>