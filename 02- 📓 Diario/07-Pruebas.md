# 📅 Planeación: 2026-08-05

## ☀️ Foco Principal del Día
*¿Cuál es la única cosa que si logro hoy hará que el día valga la pena?*
- [x] Terminar el repaso de microservicios con spring boot [[002-Gestion de ALumnos]] (@2026-08-10 10:00)

## 📝 Tareas del Día
- [x] 6 Test unitarias con Mokito
- [x] Revisar el inicio de sesion de https://talent.involverh.com/login si lo inicie con la cuenta de Enucom para preguntarles para la recuperacion de contraseña
- [x] Revisar las pruebas de Mockito para el servicio de profesor
- [x] 12 Pruebas de Integracion con jqwik y Mockito ✅ 2026-09-04

> [!abstract] 💭 Reflexiones y Notas
>- La forma de la estructura para las pruebas por lo que investigue es de las mejores practicas.
>- Tanto como [[02-Mockito and Jqwik]] pueden fortalecer el proyecto en conjunto ya que son para distintas cosas.
>- Solo hice una prueba Unitaria con mockito del servicio de Profesores para el Repositorio, Service y Controller.
>- La pruebas se estructura de la siguiente manera [[02-Patron AAA]]
>- Tanto para las pruebas de Mockito como de Propiedad con Jqwik utilizan el [[02-Patron AAA]]
>- Tambien aqui entiendo que para las clase de los test en la Inyeccion de Dependencias se usa @Autowired por cuestiones del motor de Unit ya que exigue un constructor vacio. Dado que estos test no se usaran en produccion basta con @Autowired, es el estandar.
>- En los siempre por estandar es tener un resources/application.properties(Configuracion especial para los test) para no tocar la base de datos real de produccion.



## 📌 Recordatorios Flotantes (Backlog)
*Espacio para ideas o pendientes sin fecha fija.*
- [ ] 📌 