# Radiografía Financiera — Mis Finanzas con Luis

Simulador interactivo para usar durante la entrevista con prospectos. Recopila datos financieros básicos, **captura el contacto del prospecto (WhatsApp/correo) antes de mostrar el resultado**, y genera una "radiografía" visual con puntaje, insignia, cobertura sugerida y línea de tiempo hasta el retiro.

## 🆕 Captura de leads (Netlify Forms)

Al terminar las 8 preguntas, el prospecto ve una pantalla de "¡Ya está lista tu Radiografía!" donde deja su WhatsApp (obligatorio), correo (opcional) y acepta el Aviso de Privacidad. Solo entonces se revela el resultado.

Esos datos se envían automáticamente al formulario `prospectos` usando **Netlify Forms** (gratis, sin backend que mantener). Así lo activas:

### 1. Después de tu primer deploy
1. Ve a tu sitio en el dashboard de Netlify → **Site configuration → Forms**.
2. Deberías ver el formulario **`prospectos`** ya detectado automáticamente (gracias al formulario oculto en `index.html`). Si no aparece, revisa que el deploy se haya hecho con `npm run build` y que el archivo `dist/index.html` resultante contenga el `<form name="prospectos" ...>` oculto.

### 2. Activa las notificaciones (para que te avisen al instante)
1. En **Forms → prospectos → Settings and usage → Form notifications**.
2. **Add notification → Email notification** → pon el correo donde quieres recibir cada prospecto nuevo.
3. (Opcional, recomendado) **Add notification → Outgoing webhook** conectado a Zapier/Make para que te llegue un WhatsApp automático cada vez que alguien complete el formulario, o para que se agregue automáticamente a una Google Sheet.

### 3. Revisa tus prospectos
Todas las respuestas (nombre, teléfono, correo, edad, ingreso, ahorro, dependientes, tipo de empleo, deudas, si tiene seguro, nivel de confianza, prioridades, presupuesto mensual para protegerse, meta de ahorro anual, puntaje, banda, insignia, cobertura sugerida) quedan guardadas en **Forms → prospectos → Verified/Unverified submissions**, exportables a CSV desde ahí mismo.

> 💡 Netlify Forms es gratis hasta 100 envíos/mes en el plan gratuito. Si esperas más volumen, considera subir de plan o migrar el mismo `submitLead()` en `src/App.jsx` a un webhook propio (Google Apps Script, Airtable, etc.) — es un solo `fetch()` que puedes redirigir.

## ✨ Otras mejoras de esta versión

- **3 preguntas de calificación nuevas:** tipo de empleo (con/sin prestaciones), deudas activas, y presupuesto mensual que destinaría a protegerse. Esto alimenta el puntaje (alguien sin prestaciones y con deudas pesadas puntúa más urgente) y le da a Luis contexto de calificación antes de la llamada.
- **Mensaje de WhatsApp personalizado:** el botón "Tengo dudas" arma automáticamente un mensaje con el nombre, puntaje e insignia del prospecto.
- **Copy de llamada a la acción por banda de resultado:** el texto arriba de los botones cambia según qué tan urgente/encaminado está el prospecto.
- **Frase motivadora** en la pantalla de bienvenida (en vez de un bloque de credenciales).
- **Botón "Compartir mi Radiografía":** genera una imagen de la tarjeta de resultado (usando la librería `html-to-image`) y la descarga o la comparte por el share nativo del celular — pensado para que la gente la suba a su historia de Instagram.
- **Botón de Instagram con tu foto** ("Síguenos") que redirige a `instagram.com/misfinanzasconluis`.
- **Botón "Compartir"** que comparte el enlace del simulador (WhatsApp, correo, etc. vía el share nativo del celular; en escritorio copia el link al portapapeles).
- **Dos casillas de consentimiento separadas** en la pantalla de contacto: una obligatoria (aviso de privacidad) y otra opcional (comunicación comercial) — cumple con la separación de consentimientos que exige buena práctica de protección de datos.

## 🧪 HTML de prueba 100% autónomo (sin instalar nada)

En la carpeta **`standalone-test/`** (fuera de este README, se entrega por separado) hay una versión del simulador que corre con solo abrir un `index.html` — sin `npm install`, sin conexión a internet, con React y los íconos ya incluidos dentro de un único `app.bundle.js`. Sirve para que cualquiera (tú, un colega) pruebe la funcionalidad completa en segundos.

Cómo abrirlo (los navegadores no permiten módulos ES vía doble clic / `file://`):
```bash
cd standalone-test
python3 -m http.server 8000
# abre http://localhost:8000 en tu navegador
```
También puedes arrastrar esa carpeta a [Netlify Drop](https://app.netlify.com/drop) para probarlo en una URL pública real.

Nota: en esa versión de prueba, el botón "Compartir mi Radiografía" usa una reimplementación simplificada local (no la librería oficial) y el envío a Netlify Forms falla silenciosamente (normal, solo funciona una vez desplegado) — el resto de la funcionalidad es idéntica al proyecto real.

## ✅ Datos ya configurados (05-sep-2026)

- **Dominio:** `misfinanzasconluis.online` (falta conectarlo en Netlify → Domain management, ver sección de deploy).
- **Calendly:** `CALENDLY_URL` en `src/App.jsx` apunta a `https://calendly.com/misfinanzasconluis/online`.
- **WhatsApp principal:** `WHATSAPP_URL` en `src/App.jsx` apunta a `https://wa.me/529931961201`.
- **Aviso de Privacidad:** revisado y validado — el texto en `PrivacyModal` (dentro de `src/App.jsx`) ya es la versión definitiva.
- **Favicon + imagen para compartir (Open Graph):** agregados en `public/` y enlazados desde `index.html`. Al compartir el link en WhatsApp/Instagram se verá el logo + tagline con la marca.
- **Analítica:** se agregaron los snippets de Google Analytics (GA4) y Meta Pixel en `index.html`, pero **con IDs de ejemplo (placeholders)**. Antes de publicar, reemplaza:
  - `G-XXXXXXXXXX` por tu Measurement ID real de GA4.
  - `TU_PIXEL_ID` por tu Pixel ID real de Meta.
  Si no vas a usar uno de los dos, borra ese bloque `<script>` completo.
- **Número de WhatsApp del botón "Tengo dudas":** apunta a `+52 993 196 1201` con un mensaje dinámico y personalizado (función `buildDoubtsWhatsAppUrl` en `src/App.jsx`, editable). Es el mismo número que ahora usa el CTA principal.

## Desplegar en Netlify

**Opción A — Netlify Drop (más rápida, sin cuenta de Git):**
1. Entra a https://app.netlify.com/drop
2. En tu computadora, dentro de esta carpeta, ejecuta:
   ```
   npm install
   npm run build
   ```
3. Arrastra la carpeta `dist/` generada a la ventana de Netlify Drop.
4. Listo — tu sitio queda publicado con una URL `.netlify.app`.

**Opción B — Conectar un repositorio Git:**
1. Sube esta carpeta completa a un repositorio de GitHub/GitLab.
2. En Netlify: "Add new site" → "Import an existing project" → selecciona el repo.
3. Netlify detecta automáticamente la configuración gracias a `netlify.toml`:
   - Build command: `npm run build`
   - Publish directory: `dist`
4. Deploy.

## Conectar el dominio misfinanzasconluis.online

1. En el sitio ya desplegado en Netlify: **Domain management → Add a domain** → escribe `misfinanzasconluis.online`.
2. Netlify te va a pedir apuntar los DNS. Hay dos caminos:
   - **Más simple:** usa **Netlify DNS** (Netlify te da 4 nameservers; los cambias en el proveedor donde compraste el dominio — GoDaddy, Namecheap, etc.).
   - **Alterno:** deja el dominio donde está y solo agrega los registros que Netlify te indique (normalmente un registro `A` apuntando a la IP de Netlify y un `CNAME` para `www`).
3. Netlify emite el certificado SSL (HTTPS) automáticamente una vez que el DNS propague (puede tardar de minutos a un par de horas).
4. Una vez conectado, actualiza `og:url` y `og:image` en `index.html` si cambiaste el dominio por otro distinto a `misfinanzasconluis.online`.

## Desarrollo local

```bash
npm install
npm run dev
```

Abre la URL que indique la terminal (usualmente http://localhost:5173). Nota: en local, el envío del formulario a Netlify Forms fallará silenciosamente (es normal, Netlify Forms solo funciona en sitios ya desplegados en Netlify) — el simulador seguirá funcionando y mostrando el resultado igual, solo no quedará el registro guardado hasta que esté en producción.

## Estructura

- `src/App.jsx` — el componente completo del simulador (lógica, estilos, logo embebido, captura de leads).
- `src/main.jsx` — punto de entrada que monta la app.
- `index.html` — plantilla HTML, incluye el formulario oculto que Netlify detecta para habilitar Netlify Forms.
- `netlify.toml` — configuración de build y redirects para Netlify.

## Personalización rápida

- **Número de WhatsApp (CTA principal):** constante `WHATSAPP_URL`.
- **Número de WhatsApp ("Tengo dudas") y mensaje:** constantes `DOUBTS_WHATSAPP_MESSAGE` / `DOUBTS_WHATSAPP_URL`.
- **Enlace de Calendly:** constante `CALENDLY_URL`.
- **Colores de marca:** objeto `C` (paleta) al inicio de `src/App.jsx`.
- **Preguntas de la entrevista:** función `renderQuestion()` dentro de `src/App.jsx`.
- **Campos que se guardan del prospecto:** función `submitLead()` — si agregas preguntas nuevas, agrega también el campo correspondiente ahí Y en el `<form name="prospectos">` de `index.html` (deben coincidir los `name`).
