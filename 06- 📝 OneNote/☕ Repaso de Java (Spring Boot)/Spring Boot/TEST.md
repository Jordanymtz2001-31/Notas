# 🧪 Estrategias y Herramientas de Testing en Proyectos

## 1. Prueba Unitaria y Pruebas de Integración

> [!note] **Documentación Interna del Proyecto**
> 📌 Existen varias pruebas en un proyecto, pero por el momento he realizado pruebas de integración y unitarias, las cuales son bastante importantes en esta industria del software y cada una cumple con cierta garantía de funcionalidad en los sistemas.
> * Para más detalles y comparativa de estas dos consulta [[02-Pruebas Unitarias y de Integracion]]

---
## 🔬 2. Investigación y Tecnologías de Pruebas

> [!note] **Documentación Interna del Proyecto**
> Durante la fase de diseño y aseguramiento de calidad del software, se desarrolló un análisis técnico enfocado en robustecer la suite de pruebas automatizadas:
> * Ver la investigación completa sobre **[[02-Mockito and Jqwik]]** creada durante el desarrollo del proyecto para evaluar las diferencias entre pruebas basadas en datos tradicionales frente a pruebas basadas en propiedades (Property-based Testing).


---

## 📐 3. Estructura y Limpieza del Código de Pruebas

> [!note] **El Patrón AAA (Arrange, Act, Assert)**
> Para la ejecución de las pruebas unitarias integrando **Mockito** y **jqwik**, adoptamos de forma estricta el **[[02-Patron AAA]]**. Este estándar divide conceptualmente cada caso de prueba en tres bloques independientes para maximizar la legibilidad, facilitar el mantenimiento y mantener el código completamente limpio:

```text
1. ARRANGE (Organizar/Preparar) ➔ Configura el entorno, instancia los objetos y define los mocks necesarios.
2. ACT     (Actuar/Ejecutar)    ➔ Invoca de forma directa el método o la lógica de negocio que se desea evaluar.
3. ASSERT  (Afirmar/Verificar)  ➔ Contrasta el resultado obtenido frente al valor esperado para validar el éxito del test.
```
