# Electromecánica Eprom - Website

## Resumen

Sitio web estático profesional para **Electromecánica Eprom**, taller de electromecánica en Palma de Mallorca. Construido con Astro 6 + Tailwind CSS v4. Todo el contenido en español, optimizado para SEO local.

## Páginas (10 total)

| Página | Ruta | Descripción |
|--------|------|-------------|
| Inicio | `/` | Hero, servicios destacados, razones para elegir, CTA |
| Servicios | `/servicios/` | 6 servicios detallados con listas de prestaciones |
| Galería | `/galeria/` | 9 placeholders con alt texts SEO-friendly |
| Blog (índice) | `/blog/` | Lista de 5 artículos |
| Bujías | `/blog/cuando-cambiar-bujias/` | Artículo completo |
| Alternador | `/blog/sintomas-alternador-averiado/` | Artículo completo |
| Diagnosis | `/blog/que-es-diagnosis-electronica/` | Artículo completo |
| ITV | `/blog/cuando-pasar-itv/` | Artículo completo |
| Mantenimiento | `/blog/mantenimiento-preventivo-basico/` | Artículo completo |
| Contacto | `/contacto/` | Formulario de cita, mapa, WhatsApp, info contacto |

## Estructura de archivos

```
src/
├── components/
│   ├── Footer.astro          # Pie de página con contacto y horario
│   ├── Navbar.astro           # Navegación responsive con menú móvil
│   ├── SEO.astro              # Meta tags, Open Graph, JSON-LD
│   └── WhatsAppButton.astro   # Botón flotante WhatsApp
├── layouts/
│   └── Layout.astro           # Layout principal
├── pages/
│   ├── index.astro            # Página de inicio
│   ├── servicios.astro        # Servicios
│   ├── galeria.astro          # Galería
│   ├── contacto.astro         # Contacto
│   └── blog/
│       ├── index.astro        # Índice del blog
│       └── [5 artículos].astro
├── styles/
│   └── global.css             # Tailwind + colores personalizados
public/
└── favicon.svg                # Favicon personalizado
```

## Características técnicas

- **Astro 6** - Generación estática, 0 JS por defecto
- **Tailwind CSS v4** - Vía plugin de Vite, diseño responsive mobile-first
- **SEO completo** - Meta tags, Open Graph, keywords, JSON-LD (schema.org AutoRepair)
- **Sitemap XML** - Generado automáticamente por @astrojs/sitemap
- **Paleta profesional** - Azul oscuro (#1e3a5f) + rojo (#dc2626)
- **WhatsApp flotante** - Enlace a wa.me/34971906236
- **Formulario de contacto** - HTML5, acción Formspree (placeholder)
- **Google Maps** - iframe embed en página de contacto
- **Navegación responsive** - Menú hamburguesa en móvil
- **Sin dependencias JS pesadas** - Carga ultrarrápida

## Despliegue

### Netlify
1. Conecta el repositorio en [netlify.com](https://netlify.com)
2. Build command: `npm run build`
3. Publish directory: `dist`

### Vercel
1. Importa el proyecto en [vercel.com](https://vercel.com)
2. Framework preset: Astro
3. Se despliega automáticamente

### Comandos

```bash
npm install        # Instalar dependencias
npm run dev        # Servidor de desarrollo
npm run build      # Build de producción
npm run preview    # Previsualizar el build
```

## Pendiente para producción

- [ ] Sustituir imágenes placeholder en galería por fotos reales
- [ ] Configurar Formspree con un ID real en `/contacto/`
- [ ] Verificar coordenadas exactas en Google Maps embed
- [ ] Añadir imagen OG real (`/og-image.jpg`)
- [ ] Configurar dominio personalizado
