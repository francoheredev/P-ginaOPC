# Propuesta Técnica - Sitio Web Institucional WordPress

---

## 1. Estructura de Carpetas (Tema Personalizado)

```
wp-content/themes/institucional-theme/
├── assets/
│   ├── css/
│   │   ├── main.css
│   │   └── responsive.css
│   ├── js/
│   │   ├── main.js
│   │   └── carousel.js
│   └── images/
├── inc/
│   ├── custom-post-types.php
│   ├── acf-fields.php
│   ├── theme-setup.php
│   ├── enqueue.php
│   └── helpers.php
├── template-parts/
│   ├── hero.php
│   ├── carousel.php
│   ├── featured-content.php
│   ├── section-nav.php
│   └── activity-card.php
├── page-templates/
│   ├── home.php
│   ├── plantilla-apartado.php
│   └── plantilla-contacto.php
├── functions.php
├── header.php
├── footer.php
├── index.php
├── single.php
├── archive.php
├── 404.php
├── style.css
└── screenshot.png
```

---

## 2. Tema WordPress Recomendado

**Opción A (Recomendada): Tema hijo de Blocksy**
- **Base:** [Blocksy](https://wordpress.org/themes/blocksy/) (gratuito, ligero, compatible con FSE)
- **Ventajas:** Editor de bloques nativo, responsive out-of-the-box, integración con ACF, panel de personalización visual
- **Por qué:** Acelera el desarrollo, evita escribir CSS desde cero, mantiene la flexibilidad de diseño editorial

**Opción B: Tema personalizado desde _underscores**
- **Base:** [_Underscores](https://underscores.me/) + ACF + Bootstrap 5
- **Ventajas:** Control total del markup
- **Desventaja:** Mayor tiempo de desarrollo

**Recomendación: Opción A (Blocksy child theme)** para cumplir con la entrega del 01/07.

---

## 3. Plugins Necesarios

| Plugin | Función | ¿Esencial? |
|--------|---------|-----------|
| **Advanced Custom Fields (ACF)** | Campos personalizados (galerías, enlaces, fechas) | ✅ Sí |
| **ACF Extended** | Funcionalidades extra para ACF (más limpio) | 🔶 Recomendado |
| **Smart Slider 3** | Carrusel administrable desde WP | ✅ Sí |
| **Yoast SEO** | Meta títulos, descripciones, sitemap, ALT | ✅ Sí |
| **Safe SVG** | Permitir subir SVGs | 🔶 Recomendado |
| **Regenerate Thumbnails** | Regenerar tamaños de imagen tras cambios | 🔶 Recomendado |
| **WP Super Cache** | Caching para velocidad | 🔶 Recomendado |
| **Really Simple SSL** | SSL con Hostinger | 🔶 Recomendado |
| **Limit Login Attempts Reloaded** | Seguridad básica | 🔶 Recomendado |

---

## 4. Custom Post Types (CPT) Necesarios

### 4.1. `apartado` (Sección)
- **Propósito:** Cada uno de los 7 apartados del sitio + Home
- **Soporte:** título, editor, imagen destacada, extracto
- **Campos ACF:**
  - `galeria` → Gallery (imágenes múltiples)
  - `fecha` → Date Picker
  - `enlace_externo` → URL
  - `destacado` → True/False (para marcarlo como destacado en Home)
  - `orden` → Number (para ordenar en la navegación)
- **Archivo:** `/seccion/` (slug: seccion)

### 4.2. `actividad` (Actividad con formulario)
- **Propósito:** Seminarios, cursos, eventos con botón de inscripción a Google Forms
- **Soporte:** título, editor, imagen destacada
- **Campos ACF:**
  - `url_formulario` → URL (link a Google Forms)
  - `texto_boton` → Text (ej. "Inscribirse")
  - `fecha_evento` → Date Picker
- **Archivo:** `/actividad/`

### 4.3. `slide` (Slide del Carrusel)
- **Propósito:** Slides del carrusel del Home
- **Soporte:** título, editor (descripción), imagen destacada
- **Campos ACF:**
  - `link_slide` → URL
  - `orden` → Number
- **Archivo:** (no tiene archivo público propio, se renderiza en home)

### 4.4. `publicacion` (opcional, o usar Posts nativos)
- Si se necesitan separar noticias de los apartados
- **Propósito:** Contenido editorial adicional
- **Soporte:** título, editor, imagen destacada, extracto, categorías

---

## 5. Campos Personalizados (ACF) - Resumen

### Grupo: Slide
| Campo | Tipo | 
|-------|------|
| link_slide | URL |
| orden | Number |

### Grupo: Apartado
| Campo | Tipo |
|-------|------|
| galeria | Gallery |
| fecha | Date Picker |
| enlace_externo | URL |
| destacado | True/False |
| orden | Number |

### Grupo: Actividad
| Campo | Tipo |
|-------|------|
| url_formulario | URL |
| texto_boton | Text |
| fecha_evento | Date Picker |

### Grupo: Opciones del Tema (ACF Options Page)
- `redes_sociales` → Repeater (nombre, url, ícono)
- `info_contacto` → Text (dirección, email, teléfono)
- `logo` → Image
- `hero_imagen` → Image (imagen por defecto del hero)

---

## 6. Estrategia de Deploy en Hostinger

```
1. Contratar hosting Hostinger (plan Business o WordPress)
2. Acceder a hPanel → WordPress → Instalador
3. Instalar WordPress en dominio definitivo
4. Configurar SSL (gratuito en Hostinger)
5. Subir tema por FTP a /wp-content/themes/ (o por gestor de archivos)
6. Subir carpeta /uploads/ con banco de imágenes (si aplica)
7. Instalar plugins manualmente (subir .zip por FTP si no hay acceso admin)
8. Importar ACF: Herramientas → ACF → Exportar/Importar grupo de campos JSON
9. Configurar:
   - Permalinks → Entradas → Nombre de la entrada (/%postname%/)
   - Crear páginas: Inicio (home), Contacto
   - Crear menús
10. Configurar Yoast → Sitemap → /sitemap_index.xml
11. Verificar responsive y testear
```

**Flujo alternativo:** Usar WP-CLI si Hostinger lo soporta (acceso SSH).

---

## 7. Flujo de Edición para el Cliente

### Para editar textos e imágenes de un Apartado:
1. Ir a `Escritorio` → `Secciones` (CPT)
2. Click en la sección a editar
3. Modificar título, contenido (editor visual), imagen destacada (columna derecha)
4. Ir a la pestaña "Campos personalizados" debajo del editor
5. Modificar galería, fecha, enlace externo
6. Click en **Actualizar**

### Para cambiar slides del carrusel:
1. `Escritorio` → `Slides` → Añadir nuevo / Editar
2. Subir imagen, escribir título y descripción
3. Poner URL en "Link del Slide"
4. Publicar

### Para cambiar enlace de formulario de Google:
1. `Escritorio` → `Actividades` → Editar actividad
2. Cambiar el campo "URL del Formulario"
3. Actualizar

### Para crear nueva actividad:
1. `Escritorio` → `Actividades` → Añadir nueva
2. Llenar: título, descripción, imagen destacada
3. Llenar campos: URL del formulario, texto del botón, fecha
4. Publicar → aparece automáticamente en su sección

---

## 8. Estructura del Sitio (Sitemap)

```
/                            → Home (Hero + Carrusel + Destacados + Navegación)
/seccion/apartado-1/         → Apartado 1
/seccion/apartado-2/         → Apartado 2
/seccion/apartado-3/         → Apartado 3
/seccion/apartado-4/         → Apartado 4
/seccion/apartado-5/         → Apartado 5
/seccion/apartado-6/         → Apartado 6
/seccion/apartado-7/         → Apartado 7
/contacto/                   → Página de contacto
/actividad/seminario-ejemplo → Actividad individual
```

---

## 9. Próximos Pasos (Plan de Trabajo)

| Día | Tarea | 
|-----|-------|
| 1 | Instalar WordPress local + tema + plugins + CPT + ACF |
| 2 | Maquetar Header, Footer, Home (Hero + Carrusel) |
| 3 | Maquetar Home (Destacados + Navegación de secciones) |
| 4 | Maquetar página de Apartado + página de Contacto |
| 5 | Responsive testing + ajustes |
| 6 | SEO (Yoast), sitemap, meta tags, ALT |
| 7 | Deploy en Hostinger + pruebas finales |
| **01/07** | ✅ **Entrega Intermedia** |
