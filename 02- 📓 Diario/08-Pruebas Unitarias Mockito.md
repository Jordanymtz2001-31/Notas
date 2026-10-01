# 📅 Planeación: 2026-08-07

## ☀️ Foco Principal del Día
*¿Cuál es la única cosa que si logro hoy hará que el día valga la pena?*
-  Haber aprendido a crear pruebas Unitarias con Mockto en las capas de Service y Controller

## 📝 Tareas del Día
- [x] Terminar las pruebas Unitarias pendientes de [[07-Pruebas]] las cuales son con Mockito del servicio de Alumnos
- [x] Correr las pruebas de ambos servicios con mockito

> [!abstract] 💭 Reflexiones y Notas
> Tuve unos problemas al correrlo de forma visual en Kiro asi que me apoye de Opencode para pedirle que revisara las pruebas de ambos Microservicio. Y encontre las formas de ejecutar los tests de diferente son las siguientes:
> 
> *Tomar encuenta que ambos se ejecutan dentro app*
> 
>- ## Todos los tests del servicio-profesor
>cd app sh mvnw -pl servicio-profesor test
>-  ## Un solo archivo de test 
>sh mvnw -pl servicio-profesor test -Dtest=ProfesorServiceImplTest
>- ## Un solo método específico
>sh mvnw -pl servicio-profesor test Dtest=ProfesorServiceImplTest#save_retornaDTOConIdGenerado
>- ## Ver output detallado (sin colapsar)
> sh mvnw -pl servicio-profesor test -Dsurefire.useFile=false
 


## 📌 Recordatorios Flotantes (Backlog)
*Espacio para ideas o pendientes sin fecha fija.*
- [ ] 📌 
