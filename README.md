# 💿 Discothèque — Analog Library

**Tu biblioteca personal de CDs físicos. Un solo archivo HTML. Sin servidor. Sin costos.**

Busca álbumes en iTunes, escanea códigos de barras, organiza por estantes, sincroniza entre dispositivos con Supabase. Todo desde el navegador.

---

## ¿Qué hay en este manual?

- [01 — ¿Qué es y por qué existe?](#01--qué-es-y-por-qué-existe)
- [02 — Cómo funciona (sin tecnicismos)](#02--cómo-funciona-sin-tecnicismos)
- [03 — Primeros pasos: agregar discos](#03--primeros-pasos-agregar-discos)
- [04 — Organizar con estantes](#04--organizar-con-estantes-shelves)
- [05 — Publicar en Netlify](#05--publicar-en-netlify)
- [06 — Sincronización con Supabase](#06--sincronización-con-supabase)
- [07 — Seguridad: todo sobre tus claves API](#07--seguridad-todo-sobre-tus-claves-api)
- [08 — Compartir con otros dispositivos](#08--compartir-con-otros-dispositivos)
- [09 — Troubleshooting](#09--troubleshooting-problemas-comunes)
- [10 — Preguntas frecuentes](#10--preguntas-frecuentes)

---

## 01 — ¿Qué es y por qué existe?

Discothèque es una app de catálogo personal para tu colección de CDs físicos. En vez de una lista en papel o una hoja de Excel, tienes una interfaz visual donde puedes:

- Ver todos tus discos en cuadrícula o lista
- Buscar álbumes en el catálogo de iTunes y agregarlos con un clic
- Escanear el código de barras de un CD físico con la cámara
- Organizar por estantes (Jazz, Rock, Prestados, etc.)
- Ver carátula, tracklist y metadatos de cada disco
- Sincronizar tu colección entre celular, tablet y computadora

> **Discothèque es un solo archivo HTML.** No tiene servidor propio, no guarda tus datos sin tu permiso, y no requiere instalar nada. Funciona en cualquier navegador moderno.

---

## 02 — Cómo funciona (sin tecnicismos)

Hay tres piezas. Ninguna es obligatoria, pero juntas dan la mejor experiencia:

| Pieza | Qué hace | ¿Es obligatoria? |
|-------|----------|-----------------|
| **Tu navegador** | Donde vive la app. Tu colección se guarda aquí por defecto en `localStorage` | ✅ Sí |
| **Netlify** | Le da una URL pública a la app (`miscdiscos.netlify.app`) para que puedas acceder desde cualquier dispositivo | Recomendado |
| **Supabase** | La "nube" que guarda tu colección. Permite que si agregas un disco en el celular, aparezca en la computadora | Recomendado |

**Sin Netlify ni Supabase:** la app solo funciona abriendo el archivo desde tu computadora. Si limpias el caché, o cambias de dispositivo, pierdes la colección.

---

## 03 — Primeros pasos: agregar discos

### Buscar un álbum

1. Escribe el nombre de un artista o álbum en la barra lateral izquierda
2. Haz clic en **Search**
3. Aparecen resultados del catálogo de iTunes — haz clic en uno para seleccionarlo
4. Haz clic en **+ Add to collection** — la carátula y el tracklist se obtienen automáticamente

### Escanear un código de barras

1. Haz clic en **📷 Scan barcode**
2. Acepta el permiso de cámara
3. Apunta al código de barras en la parte trasera del CD
4. La app busca la edición en MusicBrainz y muestra el resultado
5. Confirma con **+ Add to collection**

### Agregar manualmente

Si la búsqueda no encuentra el disco (ediciones raras, prensajes locales, bootlegs):

1. Haz clic en **Add manually** en la barra lateral
2. Solo son obligatorios **Artista** y **Título** — el resto es opcional
3. Puedes editar cualquier campo después: haz clic en el álbum → **✎ Edit**

---

## 04 — Organizar con estantes (shelves)

Los estantes son etiquetas para organizar tu colección. Ejemplos:

- Por género: `Jazz`, `Rock`, `Clásica`, `Latin`
- Por ubicación: `Sala`, `Cuarto`, `Oficina`
- Por estado: `Prestados`, `En venta`, `Favoritos`

### Crear un estante

1. Haz clic en **+ New shelf** (sobre la grilla de álbumes)
2. Escribe el nombre → Enter o **Add**
3. El estante aparece como pastilla de filtro — haz clic para filtrar la grilla

### Asignar un estante a un álbum

1. Haz clic en el álbum → **✎ Edit**
2. Escribe el nombre del estante en el campo **Shelf**
3. **Save**

### Eliminar un estante

Haz clic en la **✕** pequeña junto a la pastilla del estante. Si tiene álbumes asignados, te pide confirmación.

---

## 05 — Publicar en Netlify

### ¿Por qué Netlify?

Sin Netlify, la app solo funciona en tu computadora. Con Netlify:

- ✅ Tienes una URL pública permanente
- ✅ Accedes desde cualquier dispositivo
- ✅ Puedes compartirla
- ✅ El link de sincronización de Supabase funciona

El plan gratuito incluye 100 GB/mes. Para un archivo HTML estático, nunca llegarás a ese límite. No necesitas tarjeta de crédito.

---

### Opción A — Netlify Drop (recomendado, sin cuenta)

**1.** Descarga el archivo `index.html` de Discothèque (renómbralo exactamente así — sin ese nombre Netlify no lo sirve como página principal)

**2.** Ve a **[app.netlify.com/drop](https://app.netlify.com/drop)**

**3.** Arrastra y suelta tu archivo `index.html` directamente sobre la página

**4.** En segundos Netlify genera una URL como `quirky-einstein-abc123.netlify.app`

**5.** Abre esa URL en tu celular — funciona de inmediato

---

### Personalizar el nombre (con cuenta gratuita)

1. Crea una cuenta gratis en [netlify.com](https://netlify.com) (puedes usar Google o GitHub)
2. En el dashboard → haz clic en tu sitio
3. **Site configuration → Change site name** → escribe el nombre que quieras, ej. `miscdiscos`
4. Tu URL queda como `miscdiscos.netlify.app`

---

### Opción B — GitHub Pages

1. Crea un repo público en [github.com/new](https://github.com/new)
2. Sube el archivo renombrado como `index.html`
3. **Settings → Pages → Deploy from branch → main / (root) → Save**
4. Tu URL: `usuario.github.io/nombre-repo`

---

## 06 — Sincronización con Supabase

### ¿Por qué Supabase?

Sin Supabase, tu colección vive en un solo navegador. Si agregas un disco desde el celular, no aparece en la computadora. Si limpias el caché, desaparece todo.

Supabase es una base de datos gratuita en la nube. Piénsalo como el Google Drive de tu colección de CDs. Solo guarda: títulos, artistas, años, géneros, notas y URLs de carátulas. Nada más.

---

### Configuración paso a paso

**Paso 1 — Crea una cuenta gratuita**

Ve a [supabase.com](https://supabase.com) → **Start for free**. Puedes usar Google o GitHub. No necesitas tarjeta.

---

**Paso 2 — Crea un nuevo proyecto**

**New project** → ponle un nombre (ej. `mi-discoteca`) → elige una región → pon una contraseña para la BD → **Create new project**.

Espera 1-2 minutos mientras Supabase crea el proyecto.

---

**Paso 3 — Crea la tabla**

Ve a **SQL Editor** → **New query** → pega esto exactamente y haz clic en **Run**:

```sql
create table if not exists cd_collection (
  id text primary key,
  data text,
  updated_at timestamptz default now()
);

-- Permite que la app lea y escriba sin login
alter table cd_collection enable row level security;

create policy "public_all" on cd_collection
  for all to anon using (true) with check (true);
```

Deberías ver: `Success. No rows returned.`

---

**Paso 4 — Obtén tus credenciales**

En el menú izquierdo: **Project Settings** (⚙️) → **API**

Copia estas dos cosas:

- **Project URL** — algo como `https://abcdefghijkl.supabase.co`
- **anon public key** — cadena larga que empieza con `eyJhbGci...`

---

**Paso 5 — Conecta en la app**

Haz clic en el botón **offline** (esquina superior derecha de Discothèque):

- Pega la **Project URL** en el primer campo
- Pega la **anon key** en el segundo campo
- Escribe una **contraseña opcional** para aislar tu colección (ver sección 07)
- Haz clic en **Connect & Sync**

Si todo está bien, el botón cambia de `offline` a `synced` en verde. Tu colección se sube automáticamente.

---

### Storage para subir carátulas (opcional)

Si quieres subir fotos de carátulas desde tu dispositivo:

1. En Supabase → **Storage → New bucket** → nombre: `covers` → marca **Public** → Create
2. En **SQL Editor** ejecuta:

```sql
create policy "public_covers" on storage.objects
  for all to anon
  using (bucket_id = 'covers')
  with check (bucket_id = 'covers');
```

---

## 07 — Seguridad: todo sobre tus claves API

### ¿Qué es la anon key?

La anon key es una credencial pública que le dice a Supabase que la app tiene permiso de leer y escribir datos. Se llama "anon" (anónimo) porque está diseñada para usarse en el navegador — visible para cualquiera que inspeccione el código.

> **Esto es normal e intencional.** Supabase sabe que la anon key es pública. Por eso existe el sistema de Row Level Security (RLS) — las políticas que configuraste en el paso 3 son las que realmente protegen tus datos.

---

### ¿Qué pasa si alguien ve mi anon key?

Con solo la anon key, alguien podría leer o escribir en tu tabla `cd_collection` — pero solo lo que las políticas RLS permiten. Tu colección de CDs no es información sensible (no es tu número de tarjeta ni tu contraseña). Si te preocupa, usa la contraseña de colección.

---

### La contraseña de colección — cómo funciona

Cuando conectas Supabase con una contraseña, la app la convierte en un hash (cadena críptica) y usa ese hash como el ID de tu fila. Dos personas con el mismo Supabase pero diferentes contraseñas tienen colecciones completamente separadas.

**Ejemplo:**
- Tú usas `"rumba2024"` → tu fila tiene ID `a8f3c2b1...`
- Otra persona usa `"jazz1990"` → su fila tiene ID `f7e2a9d4...`

Nunca se mezclan.

---

### ⚠️ La service_role key — NUNCA la uses aquí

En Supabase verás dos claves: la **anon key** y la **service_role key**.

| Clave | ¿Es pública? | Qué hace |
|-------|-------------|----------|
| Project URL | ✅ Sí, está bien | Solo indica qué proyecto |
| anon key | ✅ Sí, está bien | Acceso controlado vía RLS |
| Contraseña de colección | ⚠️ No compartas | Aísla tu colección |
| **service_role key** | 🚨 **NUNCA pública** | Acceso total, ignora RLS |
| Contraseña de BD (Supabase) | 🚨 **NUNCA pública** | Acceso directo a la BD |

**La service_role key tiene acceso total a tu base de datos, ignorando todas las políticas de seguridad. Nunca la pongas en ninguna app que corra en el navegador.**

---

## 08 — Compartir con otros dispositivos

Una vez que tienes Netlify + Supabase configurados, el link de sincronización te permite acceder desde cualquier dispositivo sin volver a configurar nada.

### Cómo generar el link

1. Asegúrate de estar conectado — el botón muestra `synced` en verde
2. Haz clic en el botón `synced` (esquina superior derecha)
3. Haz clic en **🔗 Copy sync link for other devices**
4. El link se copia al portapapeles y aparece en el panel
5. Envíatelo por WhatsApp, email, o ábrelo directamente en otro dispositivo

Al abrir el link, la app se auto-conecta a tu Supabase — sin necesidad de volver a pegar URL ni claves.

> **⚠️ Nota:** El link contiene tu anon key codificada. No lo publiques en redes sociales. Compártelo solo con tus propios dispositivos o personas de confianza. Si crees que fue comprometido, en Supabase puedes rotar la anon key y el link viejo dejará de funcionar.

---

## 09 — Troubleshooting: problemas comunes

### La búsqueda no devuelve resultados

- Verifica tu conexión a internet
- Prueba con términos más simples: `"Miles Davis"` en vez de `"Miles Davis Kind of Blue 1959"`
- Si el álbum no aparece en iTunes (ediciones raras, bootlegs), usa **Add manually**

---

### La carátula no aparece

- Espera unos segundos — las carátulas se cargan en segundo plano
- Haz clic en el álbum → botón **↺ cover** para intentar buscarla de nuevo
- Edita el álbum → campo **Cover image** → pega una URL de imagen (Google Images, Discogs, etc.)

---

### El botón muestra "error" en vez de "synced"

1. **Verifica la URL de Supabase** — debe ser exactamente `https://tuproyecto.supabase.co` sin barra al final ni espacios
2. **Verifica la anon key** — cópiala de nuevo desde Supabase → Project Settings → API. A veces se copia con espacios invisibles
3. **Verifica que la tabla existe** — Supabase → Table Editor → debe aparecer `cd_collection`. Si no, repite el SQL del paso 3
4. **Verifica las políticas RLS** — Supabase → Authentication → Policies → `cd_collection` → debe haber una política `public_all`. Si no, repite el SQL

---

### Se perdió mi colección al limpiar el caché

> **Sin Supabase conectado, no hay recuperación posible.**

Si tu colección estaba solo en `localStorage` y limpiaste el caché, los datos se perdieron. Conecta Supabase ahora para que no vuelva a pasar.

Haz backups manuales regularmente (ver sección 10).

---

### El link de sincronización no funciona en otro dispositivo

- Asegúrate de copiar el link completo — incluye el `#` y todo lo que viene después
- Verifica que la app esté publicada en Netlify o GitHub Pages — si estás abriendo el archivo local, el link no puede funcionar en otro dispositivo
- Genera el link de nuevo: abre el panel de sincronización → **🔗 Copy sync link**

---

### Banner rojo: "Storage upload blocked"

Necesitas configurar el storage de Supabase. Ve a la sección [Storage para subir carátulas](#storage-para-subir-carátulas-opcional) y sigue los pasos.

---

## 10 — Preguntas frecuentes

**¿Cuánto cuesta todo esto?**
Todo es gratuito. Discothèque: gratis. Netlify: gratis (plan Free, sin tarjeta). Supabase: gratis (plan Free, 500MB de BD). Costo total: $0.

---

**¿Puedo usar la app sin internet?**
Para ver tu colección, sí (si los datos ya están en localStorage). La búsqueda de álbumes y la sincronización con Supabase sí requieren internet.

---

**¿Cómo hago un backup de mi colección?**

1. Abre las herramientas de desarrollador en tu navegador (F12 en Windows/Linux, Cmd+Option+I en Mac)
2. Ve a la pestaña **Console**
3. Ejecuta: `copy(localStorage.getItem('discotheque_v1'))`
4. Tu colección completa (JSON) está ahora en el portapapeles — pégala en un archivo de texto y guárdala

---

**¿Puedo personalizar el nombre o los colores?**
Sí. Abre el archivo HTML en un editor de texto y busca `--accent` para cambiar el color principal, o `Discothèque` para cambiar el nombre. No necesitas saber programar para estos cambios básicos.

---

**¿Puedo compartir mi colección con alguien más?**
- **Solo lectura:** comparte el link de Netlify. Cualquiera puede verla
- **Con sincronización:** comparte el link de sincronización. La otra persona puede agregar y editar discos, y los cambios se sincronizan para ambos

---

**¿La app funciona en celular?**
Sí. La interfaz es responsiva y funciona en Chrome y Safari móvil. El escaneo de código de barras también funciona desde el celular. Para la mejor experiencia, agrégala a tu pantalla de inicio: en iOS → botón compartir → "Añadir a pantalla de inicio".

---

**¿Qué pasa si Supabase o Netlify dejan de ser gratuitos?**
Netlify ha tenido plan gratuito desde 2015 y no hay señales de que cambie para sitios estáticos. Supabase igualmente. Pero si alguna vez cambian, puedes migrar a cualquier otra plataforma — la app es un solo archivo HTML que funciona en cualquier host estático, y tu BD de Supabase se puede exportar completa.

---

## Stack técnico

| Componente | Tecnología |
|-----------|-----------|
| App | HTML + CSS + JavaScript vanilla (un solo archivo) |
| Búsqueda de álbumes | iTunes Search API (gratuita, sin clave) |
| Escaneo de barcodes | MusicBrainz API + ZXing |
| Base de datos | Supabase (PostgreSQL) |
| Hosting | Netlify / GitHub Pages / cualquier host estático |
| Fuente | Inter (Google Fonts) |

---

<div align="center">

**Discothèque** · Un archivo. Sin servidor. Sin costos. Tu colección, siempre contigo.

</div>



# 💿 CD Collection Manager

A self-contained, open-source web app for cataloguing your physical CD collection. No backend required to run — just open the HTML file in any browser. Optional Supabase integration for cloud sync across devices.

Built with vanilla HTML, CSS, and JavaScript. Zero dependencies beyond optional Supabase for sync and ZXing for barcode scanning.

---

## Features

- **Search & add** albums via iTunes API (no API key needed)
- **Barcode scanner** — point your camera at a CD barcode to look it up instantly
- **Cover art** — auto-fetched from iTunes, or upload your own / paste a URL
- **Full metadata** — artist, title, year, label, genre, country, catalog number, shelf, copies, tracklist, personal notes
- **Edit everything** — all fields editable, including tracklist line by line
- **Shelf organization** — filter your collection by custom shelf labels
- **Grid and list views** — sort by artist, title, year, or recently added
- **Online search links** — every album links to Amazon, Discogs, Google Images, eBay, MusicBrainz
- **Cloud sync** — optional Supabase backend syncs your collection across all devices
- **Cross-device setup** — one-click setup link to connect new devices without re-entering credentials
- **Copies tracking** — log how many physical copies you own of each title
- **Offline-first** — works fully without internet; syncs when connected

---

## Quick start

### Option A — Local only (no sync)

1. Download `cd-collection.html`
2. Open it in any browser
3. Start adding CDs

Your collection saves to your browser's `localStorage`. It stays on that device and browser only.

### Option B — Cloud sync across devices (recommended)

Follow the Supabase setup below, then deploy to Netlify so you can access it from any device.

---

## Supabase setup (free)

Supabase is a free open-source Firebase alternative. The free tier is more than enough for a personal CD collection.

### 1. Create a project

1. Go to [supabase.com](https://supabase.com) and sign up
2. Click **New project**
3. Give it a name (e.g. "Music Collection")
4. Choose a strong database password — save it somewhere
5. Select the region closest to you
6. Make sure **Enable Data API** and **Enable automatic RLS** are both checked
7. Click **Create new project** and wait ~2 minutes

### 2. Create the collection table

Go to **SQL Editor** in the left sidebar and run:

```sql
create table cd_collection (
  id text primary key,
  data text not null,
  updated_at timestamptz default now()
);

alter table cd_collection enable row level security;

create policy "Allow all"
on cd_collection for all
using (true)
with check (true);
```

### 3. Create the storage bucket for cover images

Go to **Storage** in the left sidebar:

1. Click **New bucket**
2. Name it exactly `covers`
3. Toggle **Public bucket** ON
4. Click **Save**

Then go back to **SQL Editor** and run:

```sql
create policy "Allow public uploads"
on storage.objects for insert
to anon
with check (bucket_id = 'covers');

create policy "Allow public updates"
on storage.objects for update
to anon
using (bucket_id = 'covers');

create policy "Allow public reads"
on storage.objects for select
to anon
using (bucket_id = 'covers');

create policy "Allow public deletes"
on storage.objects for delete
to anon
using (bucket_id = 'covers');
```

### 4. Get your credentials

Go to **Project Settings → API**:

- **Project URL** — looks like `https://abcdefgh.supabase.co`
- **anon / public key** — the long `eyJ...` string under "Project API keys"

> **Security note:** The anon key is designed to be used in client-side apps. It's safe to use here. Never use the `service_role` key in a browser app.

### 5. Connect the app

Open `cd-collection.html`, click **☁ offline** in the top right, paste your URL and anon key, set a password, and click **Connect & Sync**.

---

## Deploying to Netlify (access from any device)

The easiest way to make the app accessible from your phone, tablet, or any other device.

### Netlify Drop (no account needed)

1. Go to [netlify.com/drop](https://app.netlify.com/drop)
2. Drag and drop `cd-collection.html` onto the page
3. You get an instant URL like `https://graceful-torte-abc123.netlify.app`

### With a Netlify account (recommended — keeps your URL permanent)

1. Create a free account at [netlify.com](https://netlify.com)
2. Go to **Sites → Add new site → Deploy manually**
3. Drag and drop the file
4. In **Site configuration → General** you can rename your site to something like `yourname-cds.netlify.app`
5. To update: drag and drop the new file again

> **Note:** The barcode scanner requires HTTPS to access the camera. The Netlify URL provides this automatically. It won't work opening the HTML file directly from your filesystem.

---

## Setting up on a second device

Once you're connected on one device:

1. Click the ☁ sync button
2. Click **🔗 Copy setup link for other devices**
3. Send the link to yourself (email, WhatsApp, etc.)
4. Open it on the other device — it auto-connects

The link encodes your credentials in the `#hash` fragment, which is never sent to any server.

---

## Adding CDs

### By search
Type an artist or album name in the search bar. The app queries iTunes across multiple regional stores (US, UK, Colombia, Mexico, Spain) for the best coverage.

### By barcode
Click **📷 Scan barcode** and point your camera at the CD's barcode. It looks up the exact pressing in MusicBrainz's database — much more accurate than name search.

If camera access isn't available, a manual barcode entry field appears.

### Manually
Click **Add manually** in the sidebar to enter all fields by hand.

---

## Fixing cover art

When the auto-fetched cover is wrong:

1. Open the album
2. Click the search links at the top (**Google Images**, **Discogs**, **Amazon**)
3. Find the correct cover
4. Right-click the image → **Copy image address**
5. Click **✎ Edit** → paste the URL in the cover field → **Apply** → **Save**

Or upload an image from your device directly via the 📷 overlay that appears when hovering over the cover in edit mode.

---

## Data & privacy

- Your collection data is stored in your Supabase project — a database you control
- Cover images you upload go to your Supabase Storage bucket — also under your control
- The app makes requests to iTunes Search API (for search and covers) and MusicBrainz (for barcode lookup) — both are read-only, no data is sent
- No analytics, no tracking, no ads

---

## File structure

```
cd-collection.html    # The entire app — one self-contained file
README.md             # This file
```

That's it. The app is intentionally a single HTML file so it's easy to share, back up, and deploy anywhere.

---

## Tech stack

| Thing | What it does |
|---|---|
| Vanilla JS | Everything — no framework |
| iTunes Search API | Album search and cover art |
| MusicBrainz API | Barcode lookup |
| Supabase JS SDK | Cloud database and storage |
| ZXing Browser | Barcode scanning fallback for Firefox |
| Web BarcodeDetector API | Native barcode scanning (Chrome/Safari/Edge) |
| localStorage | Offline-first data persistence |
| Web Crypto API | Password hashing (SHA-256) |
| Canvas API | Image compression before upload |

---

## Customization

The app uses CSS variables throughout. To change the color scheme, edit these values in the `<style>` block:

```css
:root {
  --bg: #0e0d0c;           /* page background */
  --surface: #1a1916;      /* card backgrounds */
  --gold: #c9a84c;         /* accent color */
  --cream: #f0ead8;        /* primary text */
}
```

The display font is [Syne](https://fonts.google.com/specimen/Syne), body is [DM Sans](https://fonts.google.com/specimen/DM+Sans), and monospace elements use [DM Mono](https://fonts.google.com/specimen/DM+Mono) — all loaded from Google Fonts.

---

## License

MIT. Do whatever you want with it.
