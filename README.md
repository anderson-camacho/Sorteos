# Gran Sorteo · Ann Crochet

Página **autocontenida** (un solo `index.html` + `participantes.json`, sin backend) para el sorteo del **Sweater Alma**:

1. **Verifica tu inscripción** — cada persona escanea el QR, busca su nombre y ve sus boletas y su **porcentaje real de probabilidad** (boletas ÷ total). Debajo, la tabla de probabilidades ordenada de mayor a menor.
2. **La ruleta** — sorteo ponderado por boletas con `crypto.getRandomValues` (sin sesgo), conteo 3-2-1, "La Primavera" de Vivaldi en clavecín sintetizado, redoble, fanfarria, carta de *Lady Ovillo* con sello de lacre, borde de puntos que se teje solo y confeti de crochet. Queda un **acta** con boleta ganadora, hora y huella SHA-256 de la lista.
3. **Organizador** — pegar la lista (cada nombre repetido = una boleta más), editar, generar QR, copiar enlace, descargar/importar `participantes.json`.

Diseño: paleta real de la marca (lila `#CEA1DD`, ciruela `#54274F`, orquídea `#BC5FB4`, salvia `#B1D8B7`), fuentes Cormorant Garamond + Assistant + Pinyon Script, detalles de crochet en SVG procedural (ovillo, hebra, flor, picots, cadeneta).

## Subir a Cloudflare Pages (2 minutos)

1. <https://dash.cloudflare.com> → **Workers & Pages** → **Create** → pestaña **Pages** → **Upload assets**.
2. Nombre del proyecto: `sorteo-anncrochet` → URL `https://sorteo-anncrochet.pages.dev`.
3. Arrastra la carpeta con `index.html`, `participantes.json` y `_headers` → **Deploy site**.

Para actualizar la lista: descarga `participantes.json` desde Organizador, reemplázalo en la carpeta y en el proyecto → **Create new deployment** → arrastra la carpeta otra vez (hay que subir la carpeta completa; no se reemplazan archivos sueltos).

Por consola:

```bash
npx wrangler pages deploy . --project-name sorteo-anncrochet
```

Subdominio propio (`sorteo.anncrochet.com`): el dominio ya usa nameservers de Cloudflare, así que en el proyecto → **Custom domains** → **Set up a domain** → `sorteo.anncrochet.com` y Cloudflare crea el CNAME solo.

## Datos

`participantes.json` (los nombres se guardan **tal como los envió cada persona**):

```json
{
  "premio": "Sweater Alma",
  "generado": "2026-09-30T23:30:00.000Z",
  "participantes": [
    { "n": "ana osorio", "t": 8 },
    { "n": "Yusmila novo", "t": 7 }
  ]
}
```

Prioridad al cargar: enlace con `#d=` (QR con lista incluida) → lista guardada en el dispositivo si es más reciente que `generado` → `participantes.json`.

Importante: dentro de un iframe (Shopify) y en los celulares del público solo se ve `participantes.json`; por eso la lista definitiva debe publicarse en Cloudflare.

## Incrustar en Shopify (anncrochet.com, tema Dawn)

Online Store → Themes → **Customize** → Add section → **Custom Liquid**, y pega:

```html
<iframe src="https://sorteo-anncrochet.pages.dev/?embed=1" title="Gran Sorteo Ann Crochet"
  style="display:block;width:100%;border:0;min-height:720px;border-radius:16px"
  allow="fullscreen; autoplay; screen-wake-lock; clipboard-write" allowfullscreen loading="lazy"></iframe>
<script>
addEventListener('message', function (e) {
  if (e.origin !== 'https://sorteo-anncrochet.pages.dev' || !e.data || e.data.type !== 'ann-sorteo:height') return;
  var f = document.querySelector('iframe[src^="https://sorteo-anncrochet.pages.dev"]'); if (f) f.style.height = e.data.height + 'px';
});
</script>
```

`_headers` ya permite que `anncrochet.com`, `*.myshopify.com` y el editor de temas incrusten la página. En modo embed aparece el botón "Abrir en pantalla completa" (sonido y pantalla completa pueden estar limitados dentro del iframe).

## Presentar en TV

- Botón **Modo TV** (o `?modo=tv`, o tecla `T`): tipografía a escala de la pantalla, zona segura del 5 %, foco visible para control remoto (flechas + OK).
- **Pantalla completa** (`F`), silencio (`M`), girar (`Espacio`), cerrar carta (`Esc` / Back).
- Mantiene la pantalla encendida durante el giro (Wake Lock).

## Nota legal

En Colombia las rifas con premio en dinero o especie están reguladas por la Ley 643 de 2001 (Coljuegos / entidades territoriales). Si el sorteo es promocional de la marca, verifica los requisitos que apliquen.
