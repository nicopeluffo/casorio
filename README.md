# 💍 Invitación de casamiento — Ara & Nico

Sitio web de invitación para el casamiento de Araceli Barrera y Nicolás Peluffo.

---

## Estructura de repositorios

Este proyecto usa **dos repositorios**:

| Repo | Visibilidad | Contenido |
|---|---|---|
| `TU_USUARIO/TU_REPOSITORIO` | **Público** | La web (este repo) |
| `TU_USUARIO/casorio-datos` | **Privado** | `confirmados.txt` y `mensajes.txt` |

Los formularios de la web disparan GitHub Actions en el repo público, y esos workflows escriben en el repo privado. Nadie puede ver los datos desde afuera.

---

## Estructura del repo público (este)

```
/
├── index.html                    ← La página web
├── assets/
│   └── hero-bg.svg               ← Imagen de fondo del hero (reemplazable)
└── .github/
    └── workflows/
        ├── rsvp.yml              ← Guarda confirmaciones en casorio-datos
        └── message.yml           ← Guarda mensajes en casorio-datos
```

## Estructura del repo privado (casorio-datos)

```
/
├── confirmados.txt               ← Una línea por persona confirmada
└── mensajes.txt                  ← Todos los mensajes acumulados
```

---

## Configuración inicial (hacer una sola vez)

### Paso 1 — Crear el repo privado de datos

1. En GitHub → **New repository**
2. Nombre: `casorio-datos`, visibilidad: **Private**
3. Inicializalo con un README (para que tenga rama `main`)
4. Dentro del repo, creá dos archivos vacíos: `confirmados.txt` y `mensajes.txt`
   (podés crearlos directamente desde la interfaz web de GitHub)

---

### Paso 2 — Crear el Personal Access Token (PAT)

El token le permite a los workflows del repo público escribir en el repo privado.

1. GitHub → tu foto de perfil → **Settings**
2. Bajar hasta **Developer settings** → **Personal access tokens** → **Fine-grained tokens**
3. Clic en **Generate new token**
4. Completá:
   - **Token name:** `casorio-bot`
   - **Expiration:** la que prefieras (podés poner 1 año)
   - **Resource owner:** tu usuario
   - **Repository access:** seleccioná **Only select repositories** → elegí `casorio-datos`
   - **Permissions → Repository permissions → Contents:** `Read and write`
5. Clic en **Generate token** → copiá el token (empieza con `github_pat_...`)

---

### Paso 3 — Agregar el token como Secret en el repo público

Los workflows lo van a leer desde acá sin que quede visible en el código.

1. En el repo público → **Settings** → **Secrets and variables** → **Actions**
2. Clic en **New repository secret**
3. Nombre: `DATOS_PAT` (exactamente así, en mayúsculas)
4. Valor: pegá el token que copiaste
5. Clic en **Add secret**

---

### Paso 4 — Editar los workflows

En los archivos `.github/workflows/rsvp.yml` y `.github/workflows/message.yml`,
reemplazá `TU_USUARIO` en la línea:

```yaml
repository: TU_USUARIO/casorio-datos
```

por tu usuario real de GitHub. Ejemplo:

```yaml
repository: araceli-barrera/casorio-datos
```

---

### Paso 5 — Editar `index.html`

Buscá el bloque `CONFIGURACIÓN` al inicio del `<script>` y completá:

```js
const GITHUB_OWNER = 'tu-usuario-de-github';   // tu usuario
const GITHUB_REPO  = 'nombre-repo-publico';    // el repo donde está la web
const GITHUB_TOKEN = 'github_pat_XXXX...';     // el mismo token del paso 2
```

> ⚠️ El token queda visible en el código fuente de la página (cualquiera puede verlo).
> Por eso el token solo tiene permiso de escritura sobre `casorio-datos` — lo peor
> que alguien podría hacer con él es agregar líneas a esos dos archivos de texto.

---

### Paso 6 — Publicar con GitHub Pages

1. En el repo público → **Settings** → **Pages**
2. Source: **Deploy from a branch** → rama `main`, carpeta `/ (root)`
3. Guardá. En unos minutos el sitio estará en:
   `https://TU_USUARIO.github.io/TU_REPOSITORIO/`

---

## Qué editar en `index.html`

El archivo tiene comentarios `── Editá ──` en cada sección:

| Sección | Qué editar |
|---|---|
| **Hero** | Subtítulo, reemplazar imagen de fondo |
| **Sobre el evento** | Título y párrafo descriptivo |
| **Fecha y lugar** | Fecha, hora, nombre del salón, dirección, link de Google Maps |
| **Confirmación** | Párrafo con instrucciones (fecha límite, etc.) |
| **Regalos** | Párrafo introductorio y datos bancarios en el modal |
| **Historia** | Frase introductoria y href del botón "Leer más" |

---

## Cómo reemplazar la foto de fondo del hero

1. Agregá tu foto al repo en `assets/` (ej. `assets/foto.jpg`)
2. En `index.html` buscá:
   ```css
   background-image: url('assets/hero-bg.svg');
   ```
3. Cambiala por:
   ```css
   background-image: url('assets/foto.jpg');
   ```
Recomendado: foto horizontal, mínimo 1200×800 px.

---

## Cómo leer los datos

- **Confirmaciones:** abrí `casorio-datos/confirmados.txt`. Una línea por persona.
- **Mensajes:** abrí `casorio-datos/mensajes.txt`. Cada bloque separado por `---`.

---

*Hecho con amor ❤️*
