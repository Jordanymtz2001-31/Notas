# 📅 Planeación: 2026-08-3

## ☀️ Foco Principal del Día
*¿Cuál es la única cosa que si logro hoy hará que el día valga la pena?*
- Terminar el servicio de Alumnos con sus pruebas con cliente [[002-Gestion de ALumnos]]

## 📝 Tareas del Día
- [x] Terminar los módulos del servicio Alumnos
- [x] Probar los endpoints

> [!abstract] 💭 Reflexiones y Notas
>- Refactorice las excepciones del servicio de alumno para que concordara con la misma estrategia que tiene el servicio de Profesor.
>- Me di cuenta que las excepciones sus métodos son constructores y que solo les paso cierto parámetros y de ahí creo el mensaje que sera utilizado por el GlobalException en su respuesta compuesta
>- El global exceptins se define que tipo de errores es cada excepciones y el formato que se le dará al cliente.
>- En el servicie se maneja la lógica y se captura los errores y pasa por el controlador pero este lo pasa por alto así que ahí entra el global exception para estructurar la respuesta al cliente.
>- También hubo un bug donde esta mal la estructura del proyecto, bueno solo la clase principal del proyecto.

## 📌 Recordatorios Flotantes (Backlog)
*Espacio para ideas o pendientes sin fecha fija.*
- [ ] 📌 
