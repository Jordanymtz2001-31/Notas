# 📊 Base de Datos y Lenguaje SQL (Guía de Preparación)

## 🗄️ 1. Conceptos Fundamentales de Datos

### 🔹 Definiciones Clave
* **Base de Datos:** Forma organizada de almacenar información mediante tablas, consultas, vistas, etc., facilitando su recuperación.
* **DBMS (Database Management System):** El sistema gestor que controla la creación, mantenimiento y uso de la base de datos (MySQL, PostgreSQL, Oracle).
* **SQL:** Lenguaje de Consulta Estructurado estándar para bases de datos relacionales.
* **SQL / PLSQL:** Evolución procedimental de SQL. Permite crear variables, bucles y lógica similar a la Programación Orientada a Objetos para diseñar Procedimientos Almacenados.
* **Tabla:** Entidad que representa un objeto del negocio. Contiene filas (registros) y columnas (campos).
* **Alias:** Segundo nombre temporal que se asigna a una tabla o columna para simplificar las uniones y la lectura de la consulta.

### 📐 Diseño y Estructuración de Bases de Datos

> [!NOTE] **Decisiones de Diseño según Cardinalidad**
> Define la relación numérica exacta entre los registros de dos tablas:
> * **1:1 (Uno a Uno):** Un registro se asocia exclusivamente con otro. *Ejemplo: Usuario ↔ Perfil.*
> * **1:N (Uno a Muchos):** Un registro de la Tabla A se relaciona con varios de la Tabla B. *Ejemplo: Usuario ↔ Pedidos.*
> * **N:N (Muchos a Muchos):** Múltiples registros de ambas tablas se relacionan entre sí. Requiere una tabla intermedia. *Ejemplo: Usuarios ↔ Roles.*

* **Clave Principal (Primary Key):** Identificador único y obligatorio para cada registro de una tabla.
* **Llave Foránea (Foreign Key):** Columna que apunta a la clave primaria de otra tabla para enlazar la información.
* **Integridad Referencial:** Regla del gestor que impide borrar o alterar registros de una tabla si existen datos que dependen de ellos en otra tabla.
* **Subconsulta:** Una consulta anidada dentro de otra. El gestor ejecuta primero la consulta interna y utiliza su resultado para filtrar la externa, uniendo datos relacionados.

---

## 🧼 2. Normalización, Redundancia y Tipos de BD

### ⚠️ El Reto de la Redundancia
La **redundancia** implica que la información se está repitiendo innecesariamente en más de una tabla, lo que puede provocar inconsistencias en los datos. Para solucionarlo se aplican técnicas de diseño:

* **Normalización:** Consiste en organizar los campos y separar tablas grandes en tablas más pequeñas para reducir la redundancia y asegurar la dependencia lógica de los datos.
  * **1era Forma Normal (1FN):** Establece que cada celda debe contener un único valor atómico (prohibido guardar listas), no deben existir grupos de datos repetidos y se requiere una clave primaria única.
  * *Caso Real:* "En *Polihules* normalicé la base de datos porque el sistema sufría de alta redundancia y falta de integridad referencial."
* **Desnormalización:** Técnica inversa que reduce el nivel de normalización para simplificar el acceso a los datos.
  * *Estrategia:* En sistemas empresariales con un volumen muy alto de lecturas (consultas), a veces conviene desnormalizar tablas específicas para acelerar el rendimiento y evitar JOINS costosos.

### 📊 OLTP vs. OLAP

| Característica | Base de Datos OLTP | Base de Datos OLAP |
| :--- | :--- | :--- |
| **Enfoque Principal** | Operaciones transaccionales rápidas. | Análisis masivo de datos para negocio. |
| **Operaciones Comunes** | `INSERT`, `UPDATE`, `DELETE`. | `SELECT` complejos y agregaciones. |
| **Volumen de Información** | Bajo por transacción (datos actuales). | Altísimo (históricos de la empresa). |
| **Objetivo** | Mantener el día a día operativo. | Facilitar la toma de decisiones. |

---

## 🛠️ 3. Restricciones, Transacciones y Modificación

### 🛑 Restricciones (Constraints)
Reglas a nivel de base de datos que dictan qué tipo de datos tienen permitido ingresar a las columnas:
1. **Restricción de Clave Primaria (PRIMARY KEY):** Identificador único, no nulo.
2. **Restricción de Clave Foránea (FOREIGN KEY):** Mantiene la integridad con otra tabla.
3. **Restricción No Nula (NOT NULL):** Obliga a que el campo contenga un valor.
4. **Restricción Única (UNIQUE):** Impide valores duplicados en esa columna (ej. correos).
5. **Restricción por Defecto (DEFAULT):** Asigna un valor automático si no se envía ninguno.
6. **Constraint CHECK:** Regla de validación lógica que evalúa una condición (`TRUE`/`FALSE`). Si la condición no se cumple, el gestor lanza una excepción y ejecuta un *rollback* automático.

### 💳 Transacciones (`ACID`)
Es una unidad lógica de trabajo que agrupa una o más operaciones SQL bajo el principio de **"todo o nada"**. Garantiza la consistencia del sistema.
* **Commit:** Confirma y consolida los cambios en el disco de manera permanente.
* **Rollback:** Deshace absolutamente todas las operaciones realizadas desde el inicio de la transacción si ocurre un fallo.

> [!example] **Ejemplo Clave: Transferencia Bancaria**
> Si se retira dinero de la Cuenta A y el sistema falla antes de depositarlo en la Cuenta B, la transacción ejecuta un **Rollback** automático para revertir ambas acciones, asegurando que el dinero no se pierda.

### 🗑️ Métodos de Eliminación de Datos

| Comando | Tipo de Operación | Alcance del Borrado | Permite WHERE | Rendimiento |
| :--- | :--- | :--- | :--- | :--- |
| **`DELETE`** | DML | Borra filas específicas o selectivas. | ✅ Sí | 🐌 Más lento (registra fila por fila). |
| **`TRUNCATE`** | DDL | Vacía la tabla por completo (mantiene estructura).| ❌ No | ⚡ Rápido (libera el espacio de golpe). |
| **`DROP`** | DDL | Elimina la tabla por completo del sistema. | ❌ No | ⚡ Instantáneo (destruye estructura y datos). |

---

## ⚙️ 4. Automatización y Programación en la BD

### ⚡ Automatizaciones del Gestor
* **JOBS:** Tareas programadas ejecutadas de forma automática por el servicio del gestor (ej. SQL Server Agent). Se usan para Respaldos (`Backup`), limpiezas automáticas de logs o procesamiento de reportes diarios sin intervención humana.
* **Cursor:** Mecanismo equivalente a un ciclo `FOR` en programación; recorre e itera fila por fila un conjunto de registros para evaluar o transformar campos específicos.
* **Vistas:** Consultas guardadas en el servidor que se comportan como tablas virtuales. Se utilizan principalmente por motivos de seguridad, permitiendo limitar qué columnas o datos sensibles puede ver un usuario.

### ⚙️ Lógica: Funciones, Procedimientos y Triggers

* **Funciones:** Diseñadas para realizar cálculos específicos y están **obligadas a retornar un valor**. *(Ejemplos: `SUM`, `COUNT`, `MIN`, `DATE`).*
* **Procedimientos Almacenados:** Bloques de código reutilizables que se ejecutan en la BD. Permiten parámetros de entrada/salida (no obligatorios) y realizan operaciones complejas (`INSERT`, `UPDATE`, `DELETE`). Su uso principal es centralizar la lógica y aumentar la **seguridad** del sistema.
* **Disparadores (Triggers):** Procedimientos automáticos que se activan antes (`BEFORE`) o después (`AFTER`) de un evento en la base de datos. Se dividen según su disparador:
  * **DDL Triggers:** Se activan ante cambios de estructura (`CREATE`, `ALTER`, `DROP`).
  * **DML Triggers:** Se activan ante manipulación de datos (`INSERT`, `UPDATE`, `DELETE`).
  * *Caso Real:* "En *Polihules* utilicé triggers en Oracle para que, al actualizarse el inventario, el sistema recalculara automáticamente el stock disponible sin depender del backend, reduciendo la carga de red y los errores humanos."

#### ⚖️ Procedimiento Almacenado vs. Trigger
* **Trigger:** Se ejecuta de forma **automática** por un evento. Su uso común es para **auditoría, logs de cambios o validaciones estrictas**.
* **Procedimiento:** Se ejecuta de forma **manual** invocándolo con parámetros. Su uso común es para la **lógica de negocio completa, reportes o migraciones de datos**.

---

## 🔍 5. Uniones (JOINS) y Filtrado Avanzado

### 🤝 Tipos de Uniones (JOINS)
* **CROSS JOIN:** Multiplicación cartesiana. Une todas las filas de la primera tabla con todas las de la segunda, devolviendo todas las combinaciones posibles.
* **INNER JOIN:** Devuelve únicamente los registros donde exista una coincidencia exacta entre ambas tablas según la condición de unión.
* **LEFT JOIN:** Devuelve todos los registros de la tabla izquierda, junto con los registros que coincidan de la tabla derecha. Si no hay coincidencia, llena los campos de la derecha con `NULL`.
* **RIGHT JOIN:** Devuelve todos los registros de la tabla derecha, junto con los registros que coincidan de la tabla izquierda. Si no hay coincidencia, llena los campos de la izquierda con `NULL`.

### ⚖️ Diferencia entre WHERE y HAVING
La diferencia principal radica en el momento en que se aplican los filtros durante el flujo de ejecución de la consulta.

* **`WHERE`:**
  * Filtra filas individuales antes de realizar cualquier agrupación.
  * Se procesa **antes** de la cláusula `GROUP BY`.
  * ❌ No permite el uso de funciones de agregación (`SUM`, `COUNT`, `AVG`).
* **`HAVING`:**
  * Filtra los grupos resultantes generados por la agrupación.
  * Se procesa **después** de la cláusula `GROUP BY`.
  * ✅ Permite usar funciones de agregación para filtrar los resultados basados en dichos cálculos matemáticos.

### ⚖️ Diferencia entre GROUP BY y ORDER BY
* **`GROUP BY`:** Junta registros con valores idénticos en una columna específica para aplicarles funciones matemáticas (`SUM`, `COUNT`, `AVG`) y consolidar la información en una sola fila resumen por grupo.
* **`ORDER BY`:** Modifica la presentación estética del resultado final, ordenando las filas de manera ascendente (`ASC`) o descendente (`DESC`) según las columnas indicadas.

#### 📝 Ejemplo Visual de GROUP BY:
Si tenemos la tabla **Pedidos**:

| Producto | Cantidad |
| :--- | :--- |
| Manzana | 10 |
| Pera | 5 |
| Manzana | 2 |
| Pera | 8 |

Al aplicar la consulta con agregación:

```sql
SELECT Producto, SUM(Cantidad) 
FROM Pedidos 
GROUP BY Producto;
```

El resultado consolidado será:

* **Manzana:** 12
* **Pera:** 13

---

## 🚀 6. Consultas de Nivel Técnico (Respuestas para Entrevistas)

### 📈 ¿Cómo sacarías el segundo salario más alto de una tabla?

> [!TIP] **Enfoque 1: Usando Subconsultas (Estándar ANSI SQL)**
> Busca el salario máximo que sea estrictamente menor al salario máximo absoluto de la tabla.
```sql
SELECT MAX(salario) 
FROM Empleados 
WHERE salario < (SELECT MAX(salario) FROM Empleados);
```

> [!TIP] **Enfoque 2: Usando LIMIT y OFFSET (PostgreSQL / MySQL)**
> Ordena de forma descendente, elimina los duplicados con `DISTINCT`, salta el primer registro (`OFFSET 1`) y toma el siguiente (`LIMIT 1`).
```sql
SELECT DISTINCT salario 
FROM Empleados 
ORDER BY salario DESC 
LIMIT 1 OFFSET 1;
```

### 🧹 ¿Cómo limpias un reporte o cómo eliminas datos repetidos en la BD?
"Para limpiar datos repetidos existen dos escenarios comunes dependiendo de si es una consulta de reporte o una limpieza definitiva en las tablas de la BD:

1. **Para un reporte (Solo lectura):** 
   Utilizo la cláusula **`DISTINCT`** al inicio del `SELECT` para indicarle al motor que ignore las filas completamente duplicadas en el resultado visual, o aplico un **`GROUP BY`** si necesito consolidar métricas de esos datos.

2. **Para limpiar la Base de Datos (Eliminar duplicados reales):** 
   Si la tabla cuenta con una clave primaria o una columna de identidad, utilizo una **CTE (Common Table Expression)** combinada con la función de ventana **`ROW_NUMBER()`**, la cual particiona los datos por la columna duplicada y les asigna un número secuencial (1, 2, 3...).

```sql
WITH CTE_Duplicados AS (
    SELECT id, columna_repetida,
           ROW_NUMBER() OVER (PARTITION BY columna_repetida ORDER BY id) AS fila_num
    FROM MiTabla
)
DELETE FROM MiTabla 
WHERE id IN (SELECT id FROM CTE_Duplicados WHERE fila_num > 1);
```

*💡 **Explicación:** Este método conserva únicamente el primer registro original (`fila_num = 1`) y elimina de forma segura todas las repeticiones del sistema mediante una sola transacción.*
