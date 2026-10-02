# 2 tipos de usuarios en DBeaver

En DBeaver se distinguen **dos tipos** de usuarios, aunque cumplen funciones distintas.

## Conexión local en Fedora 44

Para una conexión local en Fedora 44:

* **Servidor local:** `localhost` o usar *Local Socket* (dependiendo del gestor: PostgreSQL soporta sockets locales).
* **Usuario administrador del sistema:** en este equipo (Fedora 44) se usa el **usuario administrador local**. Al conectar al motor, se usa el **usuario del gestor de BD**, no el usuario de Linux.
* **Credenciales:** guardar en el gestor de contraseñas de DBeaver solo si es seguro (máquina de desarrollo).
* **Servicio del gestor:** verificar que esté activo (`systemctl status <servicio>`) antes de conectar.

> [!info] **Nota de seguridad**
> En entornos de desarrollo se pueden usar credenciales locales. En producción, nunca guardar contraseñas sin cifrado y usar usuarios con permisos mínimos.

## Tipo 1: Usuarios para el programa (DBeaver)

Son los usuarios/espacios que gestionan **DBeaver como aplicación**:

* Gestionan el **espacio de trabajo** y proyectos.
* Guardan **conexiones**, favoritos y configuraciones.
* Definen preferencias de interfaz y drivers.
* **No son usuarios del gestor de base de datos.** No autentican contra PostgreSQL/MySQL/Oracle, sino contra la configuración local de la aplicación.

## Tipo 2: Usuarios para cada gestor de base de datos

Son los usuarios creados **dentro del propio motor de BD**:

* **PostgreSQL:** `CREATE ROLE/USER` con permisos por esquema/tablas.
* **MySQL/MariaDB:** usuarios con host (`user@host`) y grants.
* **SQL Server:** logins/usuarios con roles de base de datos.
* **Oracle:** usuarios (esquemas) con privilegios y roles.

Cada gestor tiene su propia autenticación y permisos. DBeaver usa estas credenciales para conectar a ese servidor.

> [!tip] **Recomendación**
> Usar siempre el **principio de mínimos privilegios**: crear un usuario específico por aplicación, en lugar de usar el usuario administrador del gestor (`postgres`, `root`, `SYS`/`SYSTEM` en Oracle).