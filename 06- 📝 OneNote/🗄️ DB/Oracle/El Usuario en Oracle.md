# El Usuario en Oracle

## ¿Cómo se conoce la creación de "bases de datos" en Oracle?

En la **práctica común** en Oracle, cuando se habla de "crear una base de datos" para una aplicación, esto suele referirse a **crear un usuario** dentro de la instancia de Oracle.

> [!info] **Explicación práctica**
> Por eso, en muchos entornos se dice: *"se crea la base de datos de la aplicación"* cuando realmente se está creando un **usuario (USER)** en Oracle.

## Diferencia entre Usuario y Base de Datos

* **Base de datos (instancia/BD):** En Oracle, la base de datos es la **instancia completa** (procesos + archivos de datos). Se crea normalmente con **DBCA** (Database Configuration Assistant) o durante la instalación. Una instancia suele contener muchas aplicaciones.
* **Usuario (USER):** En Oracle, un **usuario equivale a un esquema** por defecto. Al crear un usuario, este tiene su propio espacio para crear tablas, vistas, índices, etc. Es lo que habitualmente se crea por aplicación.

**Conclusión:** Un usuario no es una base de datos completa. Un usuario crea y gestiona objetos dentro de su **esquema**, y vive dentro de **una única instancia/BD de Oracle**.

## Creación básica de un usuario

```sql
CREATE USER nombre_usuario IDENTIFIED BY contraseña;
GRANT CONNECT, RESOURCE TO nombre_usuario;
```

> [!tip] **Nota**
> Para aplicaciones, conviene otorgar solo los privilegios necesarios (`CONNECT`, `CREATE TABLE`, etc.), siguiendo el principio de mínimos privilegios.

## Conexión desde DBeaver

Desde DBeaver, para conexiones a Oracle se pueden usar usuarios de aplicación o administrativos (`SYSTEM`, `SYS`). Al conectar con un **usuario de aplicación**, ese usuario verá únicamente su esquema (y lo que tenga permisos para consultar).