# Landing Claudia Coilla

Sitio estático. Sin build, sin dependencias que instalar. Se abre y se edita directo.

---

## Estructura

```
index.html                      ← todo el sitio (HTML + estilos inline + script de animaciones)
assets/
  js/
    react.production.min.js     ← requerido por dc-runtime
    react-dom.production.min.js ← requerido por dc-runtime
    dc-runtime.js               ← runtime de Claude Design (interpreta <x-dc> y <helmet>)
  fonts/                        ← Archivo, Instrument Sans, Martian Mono (autoalojadas)
  img/                          ← imágenes en WebP (las que usa el sitio)
    originales/                 ← PNG/JPG originales, respaldo. No se publican.
  video/reel-vertical.mp4
```

**Orden de carga que no se puede romper:** react → react-dom → dc-runtime.
El runtime lee `window.React`, así que si cambias el orden la página queda en blanco.

---

## 1. Subir a GitHub

```bash
cd <carpeta-del-proyecto>
git init
git add .
git commit -m "Landing Claudia Coilla"
git branch -M main
git remote add origin https://github.com/<tu-usuario>/landing-claudia-coilla.git
git push -u origin main
```

## 2. Publicar en Vercel

1. vercel.com → **Add New → Project** → importar el repo
2. Framework Preset: **Other**
3. Build Command: vacío · Output Directory: vacío · Root Directory: `./`
4. **Deploy**

Queda en `landing-claudia-coilla.vercel.app` en menos de un minuto.

**Dominio propio:** Project → Settings → Domains → agregar el dominio → copiar los registros
DNS que Vercel indica al panel del registrador. Propaga entre 10 min y 2 horas.

## 3. Seguir modificándola

Cada `git push` a `main` redespliega solo. El ciclo es:

```bash
# editar index.html (a mano o con Claude Code)
python3 -m http.server 4000     # revisar en localhost:4000
git add . && git commit -m "ajuste tipografía sección servicios" && git push
```

Cada rama distinta de `main` genera una **preview URL** propia. Sirve para mostrarle
cambios a la clienta sin tocar la versión publicada:

```bash
git checkout -b nueva-seccion
git push -u origin nueva-seccion   # Vercel comenta la URL de preview
```

---

## Cómo está construido el `index.html`

No es HTML corriente. Viene de Claude Design y usa dos elementos propios:

- `<helmet>` — su contenido lo mueve el runtime al `<head>` (ahí viven los `@font-face`
  y los estilos globales).
- `<x-dc>` — envuelve la página. El runtime lo procesa y renderiza.
- `<script type="text/x-dc">` al final — clase con `componentDidMount` / `revealPass`.
  Ahí está **toda** la lógica de scroll: los reveals, el parallax, el timecode del
  header y los marcadores laterales.

Atributos que controlan el movimiento, por si quieres tocarlos:

| Atributo       | Qué hace                                                        |
|----------------|-----------------------------------------------------------------|
| `data-sec`     | marca una sección (alimenta marcadores laterales y timecode)     |
| `data-fade`    | elemento que aparece al entrar en viewport                       |
| `data-d="280"` | retardo en ms del fade (para escalonar)                          |
| `data-rest`    | transform final al que vuelve (ej. `rotate(-2.5deg)`)            |
| `data-hinge`   | capa con parallax vertical                                       |
| `data-spring`  | elemento con easing tipo resorte                                 |
| `data-mark`    | marcador de progreso lateral                                     |
| `data-tc`      | contador de tiempo del header                                    |
| `data-open`    | ítem abierto del acordeón                                        |

**Al editar SVG:** dentro de `<x-dc>` los atributos camelCase van escritos como
`sc-camel-view-box` y `sc-camel-preserve-aspect-ratio`. El runtime los convierte a
`viewBox` y `preserveAspectRatio`. Si los escribes en camelCase directo, se ignoran.

---

## Notas

- **Volver a Claude Design.** Este proyecto ya no es un bundle; Design no lo reimporta.
  Si quieres rediseñar visualmente, se hace en Design y se vuelve a desempaquetar.
- **Imágenes.** El sitio usa los WebP. Los originales están en `img/originales/` como
  respaldo (3.74 MB → 0.41 MB al convertir; el portada bajó de 1.5 MB a 110 KB).
  Si agregas imágenes nuevas, conviértelas antes de subirlas.
- **Overflow horizontal en móvil.** El `scrollWidth` da ~429px en viewport de 390px por
  el marquee y unos stickers rotados. `body { overflow-x:hidden }` lo contiene, así que
  no se nota. Solo tenlo presente si en algún momento quitas ese `overflow-x`.
- **El video no tiene pista de audio.** Correcto para autoplay: ningún navegador lo bloquea.
- **Metadatos.** Título, descripción y Open Graph ya están en el `<head>`. Cuando tengas
  el dominio final, cambia `og:image` a URL absoluta para que la miniatura salga en
  WhatsApp y LinkedIn.

## Checklist antes de entregar

- [ ] Revisar en móvil real, no solo en el simulador
- [ ] Verificar que los links de contacto apunten donde deben
- [ ] `og:image` con URL absoluta
- [ ] Favicon (falta; ver `<link rel="icon">` en el `<head>`)
- [ ] Confirmar con la clienta que los textos de los proyectos son los definitivos
