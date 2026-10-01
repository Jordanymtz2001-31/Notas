---
tipo: duda-investigacion
estatus: FInalizado
categoria: Pilares de Software
fecha_registro: 2026-09-16
fecha_resolucion: 2026-09-16
fuente_origen: "[[002-Gestion de ALumnos]]"
---

> [!info] ❓ **La Duda / Reflexión**
> Escribe aquí la pregunta o el pensamiento inicial de forma clara.
> 1. ¿Que son los estereotipos?
> 2. ¿Donse se usan y por que?
> 3. 
> 

---

# 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** En una entrevista me preguntaron y no supe que responder
*   **Sospecha inicial:** La verdad es que no sabia que eran

---

# 🔍 Investigación y Respuestas
### Fuentes Consultadas
* [x] Buscar en Google / Documentación Oficial: ✅ 2026-09-16
* [x] Preguntar a IA / Foros: ✅ 2026-09-16

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
>### **¿Para que sirven? (EL escaneo de Componentes)**
> - Por defecto, Java ve a tus clases como simples archivos de texto. Spring Boot necesita saber cuáles de esas clases son importantes para tomarlas bajo su control, meterlas en su contenedor de memoria (el _Application Context_) y permitir la **Inyección de Dependencias** (como cuando inyectas tu servicio en el controlador).
> - Al poner un estereotipo arriba de una clase, le pones una "etiqueta rastreable". Al arrancar el sistema, Spring hace un proceso llamado **Component Scanning**. Busca todas las clases que tengan estas anotaciones, crea una única instancia de ellas (el _Singleton_) y las deja listas para ser usadas.
> ### **Los 4 Estereotipos Principales (La Familia `@Component`)**
>Todos los estereotipos son, en el fondo, "hijos" de una anotación madre llamada `@Component`. Sin embargo, se crearon especializaciones para que el código sea semánticamente correcto:
> - **`@Component`:** Componente genérico del sistema (utilitarios, mappers, validadores helpers). 
>	- **Ejemplo:** Un mapeador de DTOs personalizado, un validador de archivos, o una clase que calcula algoritmos matemáticos sueltos.
>	
> - **`@Controller` / `@RestController`:** Frontera web. Recibe peticiones HTTP y maneja la comunicación con el cliente. 
> - **`@Service`:** El núcleo del sistema. Contiene las reglas de negocio, validaciones y procesamiento de datos. 
> - **`@Repository`:** Frontera de persistencia. Maneja el acceso a la base de datos y traduce los errores nativos de SQL a excepciones legibles de Spring.

---

# 🚀 Acciones a tomar (Next Actions)
- [ ] 
