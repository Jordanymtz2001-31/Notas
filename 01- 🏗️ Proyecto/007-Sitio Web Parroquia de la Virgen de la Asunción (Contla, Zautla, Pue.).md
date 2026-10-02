# 🚀 Proyecto: Sitio Web Parroquia de la Virgen de la Asunción (Contla, Zautla, Pue.)

**Estatus:** 🔴 No iniciado / 🟡 / 🟢 
**Fecha de inicio:** 
**Fecha límite:** 

---

## 🎯 Objetivo General
*Crear una plataforma web informativa y cultural dedicada al templo de la Virgen de la Asunción en la comunidad de Contla. El sitio preservará la historia local, difundirá las festividades del 15 de agosto (mayordomías, danzas, alfombras de aserrín) y servirá como canal oficial para los horarios de servicios y sacramentos.*

## 📌 Tareas Principales
- [ ] Crear el backend
- [ ] Crear el frontend
- [ ] Despliegue

## 🗂️ Recursos y Enlaces Rápidos
*Enlaces a páginas web, carpetas locales o notas clave.*
- **Diseño Arquitectura Tecnológica (Stack):** 
	- **Frontend:** **Angular** (Para una interfaz pública moderna, fluida y con la estética limpia inspirada en el portal del _Santuario de la Quinta Aparición Guadalupana_).
	- **Backend:** **Django REST Framework (DRF)** (Para exponer la API de datos en formato JSON).
	- **Panel de Control:** **Django Admin** nativo (Usa la interfaz `/admin` para gestionar contenidos sin programar un panel desde cero).
	- **Base de Datos:** **MySQL** (Relacional, ideal para la estructura de crónicas, eventos y galerías).
	- **Almacenamiento de Medios:** **Cloudflare R2** (Para hospedar fotos y videos de la comunidad de forma segura, económica y sin costos por descarga de datos).
	
- **Estructura del Sitio Web (Vistas en Angular)**
	- 1. **Inicio (Home):** Imagen principal del altar/Virgen, mensaje de bienvenida del párroco, ubicación en Google Maps y accesos rápidos.
	- 2. **Historia y Patrimonio:** Crónica de la fundación del templo, llegada de la imagen, arquitectura de la iglesia y arte sacro de Contla.
	- 2. **La Fiesta Patronal (15 de Agosto):** Espacio cultural para explicar el sistema de mayordomías, las danzas tradicionales (Negritos/Tocotines), las alfombras y el programa religioso anual.
	- 2. **Vida Comunitaria y Servicios:** Horarios de misas, requisitos para sacramentos (bautizos, bodas, etc.) y avisos parroquiales.
	- 2. **Galería Multimedia:** Álbumes fotográficos organizados por año/evento y paisajes de Zautla.
	
- Diseño Preliminar de la Base de Datos (Modelos en Django)
	-  **`Articulo` (Historia/Crónicas):** `id`, `titulo`, `contenido` (soporte para Markdown/HTML), `fecha_publicacion`, `url_imagen_r2`.
	- **`Evento` (Programa de Fiestas/Avisos):** `id`, `titulo_actividad`, `descripcion`, `fecha_hora`, `lugar`.
	- **`Galeria` (Fotografías):** `id`, `titulo_foto`, `url_imagen_r2`, `categoria` (Fiesta Patronal, Arquitectura, Paisajes), `fecha_registro`.
	- **`HorarioServicio` (Misas y Sacramentos):** `id`, `tipo_servicio` (Misa, Confesión, Bautizo), `dia_semana`, `hora`.

- **Documentación:** 

## 📝 Notas Relacionadas
*Usa `[[NombreDeLaNota]]` para enlazar notas de reuniones, ideas o especificaciones de este proyecto.*
- 

---
## ⏳ Bitácora de Avance
*Notas breves sobre qué se hizo en cada fecha.*
- **2026-08-25**: Creación del proyecto y definición de objetivos.
