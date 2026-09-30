# Gran Sorteo · Ann Crochet

Página **autocontenida** (un solo `index.html`, sin backend) para:

1. **Verificar inscripción** — cada persona escanea el QR, busca su nombre y ve cuántas boletas tiene.
2. **La ruleta** — sorteo ponderado por boletas (5 boletas = 5 veces más probabilidad), con conteo regresivo, redoble, fanfarria estilo Bridgerton, carta de Lady Whistledown y confeti de crochet.
3. **Organizador** — agregar nombres y boletas (uno a uno o pegando una lista), generar el QR, copiar el enlace, descargar/importar `participantes.json`.

## Subir a Cloudflare (2 minutos)

1. Entra a <https://dash.cloudflare.com> → **Workers & Pages** → **Create** → pestaña **Pages** → **Upload assets**.
2. Nombre del proyecto: `sorteo-anncrochet` (la URL queda `https://sorteo-anncrochet.pages.dev`).
3. Arrastra **la carpeta completa** de este proyecto (o selecciona `index.html`, `participantes.json` y `_headers`).
4. **Deploy site**. Listo.

Para actualizar (por ejemplo con la lista definitiva): en el proyecto → **Create new deployment** → arrastra la carpeta otra vez.

Alternativa por consola:

```bash
npx wrangler pages deploy . --project-name sorteo-anncrochet
```

## Cómo cargar los participantes

**Opción A — rápida (solo un dispositivo):** abre la página → abajo **Organizador** → agrega los nombres. Quedan guardados en ese navegador y viajan dentro del enlace/QR que generas (cabe hasta ~80-100 nombres en un QR).

**Opción B — para todos los dispositivos:** en Organizador → **Descargar participantes.json** → reemplaza el archivo `participantes.json` de la carpeta y vuelve a desplegar en Cloudflare. Así el QR puede ser simplemente la URL de la página y cualquiera verá la lista completa.

Formato de `participantes.json`:

```json
[
  { "n": "Ana María Ramírez", "t": 15 },
  { "n": "Carolina Gómez", "t": 10 }
]
```

## Incrustar en Shopify (anncrochet.com)

El tema es **Dawn**. Dos formas:

**1. Página con iframe** (Online Store → Pages → nueva página → botón `<>` para ver HTML):

```html
<iframe src="https://sorteo-anncrochet.pages.dev" title="Gran Sorteo Ann Crochet"
  style="width:100%;height:100vh;min-height:720px;border:0;border-radius:16px"
  allow="fullscreen; autoplay; clipboard-write" allowfullscreen loading="lazy"></iframe>
```

**2. Sección Custom Liquid** (Personalizar tema → Agregar sección → *Liquid personalizado*) con el mismo `<iframe>`.

El archivo `_headers` ya permite que `anncrochet.com` y `*.myshopify.com` incrusten la página. Para un subdominio propio (`sorteo.anncrochet.com`): en el proyecto de Pages → **Custom domains** → agregar, y crear el CNAME que indica Cloudflare en el DNS del dominio.

## Atajos

- `Espacio` o `Enter` en la ruleta: girar.
- `Esc`: cerrar la carta del ganador.
- Botón **Pantalla completa** para proyectar en TV.
- URL con `#admin` abre directo el panel del organizador.

## Nota legal

En Colombia las rifas con premio en dinero o especie están reguladas por la Ley 643 de 2001 (Coljuegos / entidades territoriales). Si el sorteo es promocional de la marca, verifica los requisitos que apliquen.
