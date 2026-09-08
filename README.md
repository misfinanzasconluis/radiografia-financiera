Radiografía Financiera — Mis Finanzas con Luis
Simulador interactivo para usar durante la entrevista con prospectos. Recopila datos financieros básicos, captura el contacto del prospecto (WhatsApp/correo) antes de mostrar el resultado, y genera una "radiografía" visual con puntaje, insignia, cobertura sugerida y línea de tiempo hasta el retiro.
🆕 Captura de leads (Netlify Forms)
Al terminar las 8 preguntas, el prospecto ve una pantalla de "¡Ya está lista tu Radiografía!" donde deja su WhatsApp (obligatorio), correo (opcional) y acepta el Aviso de Privacidad. Solo entonces se revela el resultado.
Esos datos se envían automáticamente al formulario `prospectos` usando Netlify Forms (gratis, sin backend que mantener). Así lo activas:
1. Después de tu primer deploy
Ve a tu sitio en el dashboard de Netlify → Site configuration → Forms.
Deberías ver el formulario `prospectos` ya detectado automáticamente (gracias al formulario oculto en `index.html`). Si no aparece, revisa que el deploy se haya hecho con `npm run build` y que el archivo `dist/index.html` resultante contenga el `<form name="prospectos" ...>` oculto.
2. Activa las notificaciones (para que te avisen al instante)
En Forms → prospectos → Settings and usage → Form notifications.
Add notification → Email notification → pon el correo donde quieres recibir cada prospecto nuevo.
(Opcional, recomendado) Add notification → Outgoing webhook conectado a Zapier/Make para que te llegue un WhatsApp automático cada vez que alguien complete el formulario, o para que se agregue automáticamente a una Google Sheet.
3. Revisa tus prospectos
Todas las respuestas (nombre, teléfono, correo, edad, ingreso, ahorro, dependientes, tipo de empleo, deudas, si tiene seguro, nivel de confianza, prioridades, presupuesto mensual para protegerse, meta de ahorro anual, puntaje, banda, insignia, cobertura sugerida) quedan guardadas en Forms → prospectos → Verified/Unverified submissions, exportables a CSV desde ahí mismo.
> 💡 Netlify Forms es gratis hasta 100 envíos/mes en el plan gratuito. Si esperas más volumen, considera subir de plan o migrar el mismo `submitLead()` en `src/App.jsx` a un webhook propio (Google Apps Script, Airtable, etc.) — es un solo `fetch()` que puedes redirigir.
✨ Otras mejoras de esta versión
3 preguntas de calificación nuevas: tipo de empleo (con/sin prestaciones), deudas activas, y presupuesto mensual que destinaría a protegerse. Esto alimenta el puntaje (alguien sin prestaciones y con deudas pesadas puntúa más urgente) y le da a Luis contexto de calificación antes de la llamada.
Mensaje de WhatsApp personalizado: el botón "Tengo dudas" arma automáticamente un mensaje con el nombre, puntaje e insignia del prospecto.
Copy de llamada a la acción por banda de resultado: el texto arriba de los botones cambia según qué tan urgente/encaminado está el prospecto.
Frase motivadora en la pantalla de bienvenida (en vez de un bloque de credenciales).
Botón "Compartir mi Radiografía": genera una imagen de la tarjeta de resultado (usando la librería `html-to-image`) y la descarga o la comparte por el share nativo del celular — pensado para que la gente la suba a su historia de Instagram.
Botón de Instagram con tu foto ("Síguenos") que redirige a `instagram.com/misfinanzasconluis`.
Botón "Compartir" que comparte el enlace del simulador (WhatsApp, correo, etc. vía el share nativo del celular; en escritorio copia el link al portapapeles).
Dos casillas de consentimiento separadas en la pantalla de contacto: una obligatoria (aviso de privacidad) y otra opcional (comunicación comercial) — cumple con la separación de consentimientos que exige buena práctica de protección de datos.
✅ Datos ya configurados (08-sep-2026)
Dominio: `misfinanzasconluis.online` (falta conectarlo en Netlify → Domain management, ver sección de deploy).
Calendly: `CALENDLY_URL` en `src/App.jsx` apunta a `https://calendly.com/misfinanzasconluis/online`.
WhatsApp principal: `WHATSAPP_URL` en `src/App.jsx` apunta a `https://wa.me/529931961201`.
Aviso de Privacidad: ya no es un modal interno — el enlace "Aviso de Privacidad" en la pantalla de contacto abre directamente `https://avisodeprivacidadonline.netlify.app/` en pestaña nueva.
Favicon + imagen para compartir (Open Graph): logo de marca aplicado en todos los tamaños (`favicon.ico`, 16x16, 32x32, apple-touch-icon, 192, 512) dentro de `public/`, ya enlazados desde `index.html`. Al compartir el link en WhatsApp/Instagram se verá el logo + tagline con la marca.
Analítica: ya configurada con IDs reales en `index.html`, no son placeholders:
Google Analytics (GA4): `G-7DJMJZ93DM`
Meta Pixel: `1372478347828949`
Si en algún momento quieres desactivar alguno, basta con borrar ese bloque `<script>` completo.
Número de WhatsApp del botón "Tengo dudas": apunta a `+52 993 196 1201` con un mensaje dinámico y personalizado (función `buildDoubtsWhatsAppUrl` en `src/App.jsx`, editable). Es el mismo número que ahora usa el CTA principal.
Google Sheet de prospectos: la hoja "Prospectos - Mis Finanzas con Luis" ya está creada en Drive con sus 5 pestañas (Pipeline como maestra + Radiografía + Pensión IMSS/Imagine Ser/Vida Mujer reservadas). Pendiente: aún no hay automatización que lleve los envíos de Netlify Forms hasta ahí — hay que configurarlo con Zapier (u otro webhook) una vez que el sitio esté desplegado.
Desplegar en Netlify
Opción A — Netlify Drop (más rápida, sin cuenta de Git):
Entra a https://app.netlify.com/drop
En tu computadora, dentro de esta carpeta, ejecuta:
```
   npm install
   npm run build
   ```
Arrastra la carpeta `dist/` generada a la ventana de Netlify Drop.
Listo — tu sitio queda publicado con una URL `.netlify.app`.
Opción B — Conectar un repositorio Git:
Sube esta carpeta completa a un repositorio de GitHub/GitLab.
En Netlify: "Add new site" → "Import an existing project" → selecciona el repo.
Netlify detecta automáticamente la configuración gracias a `netlify.toml`:
Build command: `npm run build`
Publish directory: `dist`
Deploy.
Conectar el dominio misfinanzasconluis.online
En el sitio ya desplegado en Netlify: Domain management → Add a domain → escribe `misfinanzasconluis.online`.
Netlify te va a pedir apuntar los DNS. Hay dos caminos:
Más simple: usa Netlify DNS (Netlify te da 4 nameservers; los cambias en el proveedor donde compraste el dominio — GoDaddy, Namecheap, etc.).
Alterno: deja el dominio donde está y solo agrega los registros que Netlify te indique (normalmente un registro `A` apuntando a la IP de Netlify y un `CNAME` para `www`).
Netlify emite el certificado SSL (HTTPS) automáticamente una vez que el DNS propague (puede tardar de minutos a un par de horas).
Una vez conectado, actualiza `og:url` y `og:image` en `index.html` si cambiaste el dominio por otro distinto a `misfinanzasconluis.online`.
Desarrollo local
```bash
npm install
npm run dev
```
Abre la URL que indique la terminal (usualmente http://localhost:5173). Nota: en local, el envío del formulario a Netlify Forms fallará silenciosamente (es normal, Netlify Forms solo funciona en sitios ya desplegados en Netlify) — el simulador seguirá funcionando y mostrando el resultado igual, solo no quedará el registro guardado hasta que esté en producción.
Estructura
`src/App.jsx` — el componente completo del simulador (lógica, estilos, logo embebido, captura de leads).
`src/main.jsx` — punto de entrada que monta la app.
`index.html` — plantilla HTML, incluye el formulario oculto que Netlify detecta para habilitar Netlify Forms, más los snippets de GA4 y Meta Pixel.
`public/` — favicon en todos los tamaños, ícono para agregar a pantalla de inicio (apple-touch-icon), imagen de Open Graph (`og-image.jpg`) para las vistas previas al compartir el link.
`netlify.toml` — configuración de build y redirects para Netlify.
Personalización rápida
Número de WhatsApp (CTA principal): constante `WHATSAPP_URL`.
Número de WhatsApp ("Tengo dudas") y mensaje: constantes `DOUBTS_WHATSAPP_MESSAGE` / `DOUBTS_WHATSAPP_URL`.
Enlace de Calendly: constante `CALENDLY_URL`.
Colores de marca: objeto `C` (paleta) al inicio de `src/App.jsx`.
Preguntas de la entrevista: función `renderQuestion()` dentro de `src/App.jsx`.
Campos que se guardan del prospecto: función `submitLead()` — si agregas preguntas nuevas, agrega también el campo correspondiente ahí Y en el `<form name="prospectos">` de `index.html` (deben coincidir los `name`).
