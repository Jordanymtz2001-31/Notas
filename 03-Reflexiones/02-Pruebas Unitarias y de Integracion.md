---
tipo: duda-investigacion
estatus: FInalizado
categoria: Unit Test
fecha_registro: 2026-09-07
fecha_resolucion: 2026-09-07
fuente_origen: Con la IA de Google
---

# Pruebas Unitarias y de Integracion

> [!info] ❓ **La Duda / Reflexión**
> Escribe aquí la pregunta o el pensamiento inicial de forma clara.
> 1. Las pruebas que realice en un Proyecto de [[07-Pruebas]] pense que tanto pruebas Unitarias como de integracion eran casi lo mismo pero no, queria saber para que se usan cada uno y que es lo que cumplen.
> 2. 
> 3. 
> 

---

## 🧠 Contexto / ¿Por qué me surgió?
*   **Detonante:** Me surguio en identificar cuales son pruebas de Integracion y Unitarias
*   **Sospecha inicial:** (Lo que tú crees o intuyes antes de investigar, ej: "Pense que solo era cuando se integraba con servicios externos(API). Pero no, sino es cuando se ejecuta toda la logica del sistema").

---

## 🔍 Investigación y Respuestas
### Fuentes Consultadas
* [x] Buscar en Google / Documentación Oficial: ✅ 2026-09-07
* [x] Preguntar a IA / Foros: ✅ 2026-09-07

### Conclusión / Lo que aprendí
> [!abstract] Resumen en tus propias palabras
> **Las pruebas unitarias evalúan una sola clase o método de forma aislada, mientras que las pruebas de integración verifican cómo interactúan múltiples componentes y capas de la aplicación juntos (como controladores, servicios y bases de datos). **
> 
> Pruebas unitarias en Spring Boot
> - **Alcance:** Una sola unidad de código, como un método o una clase de servicio.
>- **Aislamiento:** No levantan el contenedor ni el contexto de Spring. Usan objetos simulados o _mocks_ (con herramientas como [[02-Mockito and Jqwik]] para evitar dependencias externas.
>- **Velocidad:** Extremadamente rápidas (se ejecutan en milisegundos).
>- **Anotaciones comunes:** `@Test` de JUnit sin cargar configuraciones pesadas de Spring
>
> Pruebas de integración en Spring Boot
> 
> *Son los que usan `@SpringBootTest`, que levanta el contexto completo de Spring Boot con base de datos H2 en memoria.*
> 
> - **Alcance:** Varios componentes o capas trabajando de manera conjunta.
>- **Contexto:** Suelen cargar parcial o totalmente el contexto de la aplicación de Spring utilizando beans reales.
>- **Velocidad:** Más lentas porque inician el entorno o contenedores necesarios.
>- **Anotaciones comunes:** `@SpringBootTest`, `@WebMvcTest` o `@DataJpaTest`.

---

## 🚀 Acciones a tomar (Next Actions)
- [ ] 
