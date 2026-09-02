# Landing Claudia Coilla

Sitio estático (una sola página). Sin build, sin dependencias que instalar, sin frameworks:
HTML + CSS inline + un `<script>` con JavaScript plano al final del archivo.

---

## Estructura

```
index.html                ← todo el sitio (HTML + estilos + script de interacciones)
assets/
  img/
    claudia.jpg            ← retrato usado en "Sobre mí"
    posts/                 ← ejemplos de la tarjeta 01 (Posts)
    tarjetas/               ← ejemplos de la tarjeta 02 (Tarjetas y chapitas)
    chapitas/               ← ejemplos de la tarjeta 02 (Tarjetas y chapitas)
    flyers/                 ← ejemplos de la tarjeta 03 (Flyers)
    pendones/               ← ejemplos de la tarjeta 04 (Pendones y banners)
    libretas/               ← ejemplos de la tarjeta 05 (Booklets)
    brochures/              ← ejemplos de la tarjeta 06 (Brochures)
    presentaciones/         ← ejemplos de la tarjeta 07 (Slide Deck)
vercel.json
```

Todas las rutas de imagen en `index.html` son relativas (`assets/img/<categoría>/archivo.jpg`),
así que el sitio funciona igual en local, en preview de Vercel o en el dominio final.

---

## 1. Subir a GitHub

```bash
cd <carpeta-del-proyecto>
git add .
git commit -m "..."
git push
```

Este repo ya está conectado a `github.com/iamjairotoro/landing-claudia-coilla` (rama `main`).

## 2. Publicar en Vercel

El proyecto ya está importado en Vercel y desplegado en **claudiacoilla.vercel.app**.
Cada `git push` a `main` redespliega solo — no hay que tocar nada en el panel de Vercel.

Cada rama distinta de `main` genera su propia **preview URL**, útil para mostrarle
cambios a la clienta sin tocar la versión publicada:

```bash
git checkout -b nueva-seccion
git push -u origin nueva-seccion   # Vercel comenta la URL de preview
```

## 3. Seguir modificándola

```bash
# editar index.html (a mano o con Claude Code)
python3 -m http.server 4000     # revisar en localhost:4000
git add . && git commit -m "ajuste sección servicios" && git push
```

---

## Cómo está construido el `index.html`

HTML plano de arriba a abajo. Al final del `<body>` hay un único `<script>` (sin
dependencias externas) con funciones independientes:

- **Modal de obra** — cada tarjeta de "La colección" tiene `data-open-modal="N"`; el
  botón de cerrar y el fondo tienen `data-close-modal`. El script alterna el atributo
  `hidden` del backdrop (`#modal-backdrop`) y del panel correspondiente
  (`.modal-panel[data-panel="N"]`). Tecla `Escape` también cierra.
- **Nav spy** — resalta en azul Klein el enlace del menú flotante que corresponde a la
  sección visible (`IntersectionObserver` sobre `#inicio, #sobre-mi, #coleccion,
  #proyectos, #contacto`).
- **Cursor personalizado** — solo en dispositivos con mouse fino (`hover:hover` +
  `pointer:fine`); se apaga solo con `prefers-reduced-motion`.
- **Scroll reveal** — clase `.reveal` + `IntersectionObserver`, agrega `.is-visible`.
- **Tilt 3D** — la polaroid de "Sobre mí" (`[data-tilt]`) sigue el mouse en desktop.
- **Typewriter** — el `<h1>` del hero escribe "Claudia" / "Coilla" letra por letra al
  cargar.

---

## Notas importantes

- **Ancho fijo de 1440px, todavía no es responsive.** El diseño (neobrutalista tipo
  sketchbook) usa un lienzo de `width:1440px` con elementos posicionados de forma
  absoluta. Para que no se vea roto en celular, el `<meta name="viewport">` está fijado
  a `width=1440`: en el teléfono el sitio se ve completo pero en miniatura (hay que hacer
  zoom para leer), en vez de reventar el layout. **Antes de dar el sitio por
  definitivamente terminado conviene hacer una pasada de diseño responsive** (breakpoints
  para el hero, la grilla de tarjetas, el nav flotante, etc.), especialmente porque la
  mayoría de quienes abran el link vía WhatsApp lo harán desde el celular.
- **Metadatos.** Título, descripción y Open Graph/Twitter Card ya están en el `<head>`.
  `og:image` apunta a `assets/img/claudia.jpg` (el retrato) como miniatura para
  WhatsApp/redes — si se quiere una imagen de portada distinta, diseñarla en formato
  horizontal (1200×630 aprox.) y reemplazar esa ruta.
- **Favicon.** Todavía no tiene (falta agregar `<link rel="icon">` en el `<head>`).

## Checklist antes de entregar

- [ ] Pasada de diseño responsive (ver nota de ancho fijo arriba)
- [ ] Revisar en móvil real, no solo en el simulador
- [ ] Favicon
- [ ] Confirmar con la clienta que los textos y precios de "La colección" son los definitivos
