# 🌐 HaLian Prime — Landing v1.0

Landing page mínima viable para `halianprime.com`. Objetivo principal: destrabar **Stripe domain verification** (deadline 4 mayo 2026 transferencias · 18 mayo pagos) y capturar primeros leads inbound.

---

## 📁 Estructura de archivos

```
halianprime-landing/
├── index.html        ← Landing principal (ES default + toggle EN)
├── privacy.html      ← Política de Privacidad (Stripe la requiere)
├── terms.html        ← Términos de Servicio (Stripe los revisa)
├── robots.txt        ← Instrucciones para crawlers de Google
├── og-image.png      ← Imagen 1200x630 para preview en WhatsApp/FB/LinkedIn
└── README.md         ← Este archivo
```

**Tamaño total:** ~130KB. Sirve incluso en conexiones lentas.

---

## 🚀 Deploy paso a paso (45 min total)

### Paso 1 — Configurar Formspree para recibir leads (5 min)

El formulario actualmente apunta a un placeholder. Hay que reemplazarlo con un Form ID real de Formspree para que los leads lleguen a `harold@halianprime.com`.

1. Ve a [formspree.io](https://formspree.io) y crea cuenta gratis con `harold@halianprime.com`
2. Click en **"+ New form"** → nombre: `HaLian Prime Landing Leads`
3. Copia el **Form ID** que te asigna (algo como `xqkrwjzg`)
4. Abre `index.html` y busca esta línea:
   ```html
   <form id="lead-form" action="https://formspree.io/f/YOUR_FORMSPREE_ID" method="POST" novalidate>
   ```
5. Reemplaza `YOUR_FORMSPREE_ID` con tu Form ID real. Queda así:
   ```html
   <form id="lead-form" action="https://formspree.io/f/xqkrwjzg" method="POST" novalidate>
   ```
6. Guarda el archivo

Plan gratis de Formspree = 50 leads/mes. Si excedes, cuesta $10/mes por 1000 submissions.

### Paso 2 — Deploy en Vercel (10 min)

**Opción A — Drag & Drop (la más fácil):**

1. Ve a [vercel.com](https://vercel.com) y crea cuenta gratis (puede ser con GitHub)
2. Click en **"Add New..."** → **"Project"**
3. En la siguiente pantalla, click en **"Import Third-Party Git Repository"** y al final verás un link **"deploy a static site without Git"** o similar — usa esa opción
4. **Drag & drop** la carpeta `halianprime-landing/` completa
5. Vercel detecta los archivos y los publica
6. Te asigna una URL temporal tipo `halianprime-abc123.vercel.app`
7. **Verifica que funcione** abriendo la URL en el navegador

**Opción B — Con CLI (si Harold prefiere):**

```bash
npm install -g vercel
cd halianprime-landing
vercel
# Sigue los prompts: cuenta, project name, etc.
vercel --prod
```

### Paso 3 — Apuntar `halianprime.com` al deploy (15 min)

En Vercel:

1. En el dashboard del proyecto, ve a **Settings → Domains**
2. Click **"Add"** y escribe `halianprime.com`
3. Vercel te pedirá que agregues 2 records DNS en tu registrar (donde compraste el dominio: GoDaddy/Namecheap/Cloudflare/etc.)

Los registros típicos son:

| Tipo | Nombre | Valor | TTL |
|---|---|---|---|
| `A` | `@` | `76.76.21.21` | 3600 |
| `CNAME` | `www` | `cname.vercel-dns.com` | 3600 |

4. Ve al panel de tu registrar, agrega esos records, guarda
5. Espera **5-30 minutos** para que se propague el DNS (a veces hasta 1h)
6. Verifica con `https://halianprime.com` — debe abrir tu landing

**Si después de 1h no funciona:** revisa que los DNS records estén exactamente como Vercel los pidió. Errores típicos: TTL muy alto, registro en lugar incorrecto, o Cloudflare con proxy activado (debe estar **en gris**, no naranja).

### Paso 4 — Verificar SSL/HTTPS (automático, 5 min)

Vercel emite certificado SSL gratis automáticamente. Cuando termine la propagación DNS, debe servirse en HTTPS sin acción manual de tu parte. Verifica que aparezca el candadito 🔒 en el navegador.

### Paso 5 — Verificar dominio en Stripe (5 min)

1. Ve al dashboard de Stripe → **Settings → Branding** o el lugar donde te pidió verificar dominio
2. Indica que `halianprime.com` ahora es tu dominio activo
3. Stripe puede pedirte agregar un meta tag o un archivo de verificación. Si te pide:
   - **Meta tag:** abre `index.html`, agrega el tag dentro del `<head>` (después de los OG tags), redeploya
   - **Archivo:** crea el archivo en la raíz de la carpeta, redeploya
4. Click "Verify" en Stripe
5. ✅ Stripe domain verification destrabada

### Paso 6 — Test final (5 min)

Antes de declarar victoria, validar:

- [ ] El sitio abre en `https://halianprime.com`
- [ ] El toggle ES/EN cambia el contenido correctamente
- [ ] El formulario envía un lead de prueba a `harold@halianprime.com`
- [ ] El email recibido en Formspree tiene todos los campos completos
- [ ] El link de WhatsApp en el footer abre WhatsApp con el número correcto
- [ ] Los links de Privacy y Terms abren las páginas correctas
- [ ] La página se ve bien en móvil (abrir desde el iPhone)
- [ ] Compartir el link en WhatsApp muestra preview con la imagen OG

---

## 🛠️ Cómo iterar después

### Cambiar copy

El copy en español está como contenido directo en `index.html`. El copy en inglés está en el bloque `I18N.en` cerca del final del archivo (busca `// Capture the original Spanish from the DOM on first load` para encontrarlo).

Para cambiar un texto:

1. Abre `index.html`
2. Busca el atributo `data-i18n` correspondiente (ej. `data-i18n="hero.h1"`)
3. Cambia el texto del elemento HTML (esa es la versión ES)
4. Busca la misma key en `I18N.en` (ej. `'hero.h1': '...'`) y cambia la versión EN
5. Guarda y redeploya en Vercel (drag & drop nueva carpeta o `vercel --prod` desde CLI)

### Cambiar colores

Los colores están en CSS variables al inicio del `<style>` del archivo:

```css
--orange: #FF6B35;        /* Acento principal */
--cream: #FFF8F3;         /* Background dominante */
--text: #1A1A1A;          /* Texto primario */
```

Cambia el valor hex y se aplica en toda la página automáticamente.

### Reemplazar la imagen OG

Cuando Liana tenga una imagen mejor (foto del hero, logo finalizado, etc.):

1. Genera una imagen 1200x630 PNG llamada `og-image.png`
2. Reemplaza el archivo en la carpeta
3. Redeploya
4. **Importante:** Los caches de Facebook/WhatsApp tardan 24-48h en refrescar. Si quieres forzar update, usa el [Sharing Debugger de Facebook](https://developers.facebook.com/tools/debug/) y haz "Scrape Again"

---

## ⚠️ Limitaciones conocidas v1

| Limitación | Impacto | Solución v2 |
|---|---|---|
| Form submission via Formspree (50/mes gratis) | Si pasamos 50 leads/mes hay que upgrade ($10/mes) o migrar a backend propio | Webhook al backend `halian-bots` cuando esté en Railway (S5-S6) |
| OG image es texto auto-generado | Preview funciona pero no es bonito | Liana genera diseño profesional en S5 cuando tenga branding |
| Privacy y Terms son plantillas básicas | Cubre Stripe + protección legal mínima | Revisión por abogado cuando llegue primer cliente serio |
| Sin analítica configurada | No sabemos quién visita ni cómo | Agregar Plausible o GA4 en S5 |
| Idiomas via JS toggle (no rutas separadas) | SEO suboptimal para EN | Migrar a `/en/` separado en S6 si EN tracción |
| Audio demo del hero es solo animación visual | Visitor no puede escuchar el agente real | Embed audio real cuando agente esté en producción Railway (Mié 29) |
| WhatsApp number en footer hardcoded | Si cambia, hay que editar HTML | Mover a config JSON cuando haya múltiples páginas |

---

## 📞 Información de contacto del sitio

Verificada al deploy:

| Campo | Valor |
|---|---|
| Empresa | HaLian Prime LLC |
| EIN | 99-4183191 |
| Texas File # | 805638859 |
| Dirección | 2908 Wiscasset Dr Apt 209, Arlington, TX 76010 |
| Email | harold@halianprime.com |
| WhatsApp | 214-309-2721 |
| Dominio | halianprime.com |

⚠️ Si alguno de estos cambia, actualizar:
- `index.html` (footer)
- `privacy.html` (sección 1 y 11)
- `terms.html` (sección 2 y 11)

---

## 🧠 Decisiones de diseño documentadas

Para que cuando vuelvan a esto en 2 meses sepan **por qué** algunas cosas son así:

1. **Light Mode + naranja vibrante:** decisión meeting domingo 26 abr — buyer hispano SMB DFW está más cómodo con UI light que dark mode tipo developer tool. Naranja calienta el cream sin ser cliché.

2. **"Tu esposa no puede contestar todo":** copy verificado con buyer real (discoveries S3). En EN se mantiene literal porque el buyer English-dominant también identifica el patrón.

3. **Sin pricing detallado en página:** decisión opción "B" — "Desde $200/mes" + CTA agendar llamada. Los 3 paquetes Starter/Pro/Elite se discuten en la conversación, no en landing.

4. **No LinkedIn en plataformas:** decisión meeting — el plomero hispano DFW no está en LinkedIn. Si quieres actualizar perfil personal Harold sí, pero la landing no le habla a LinkedIn.

5. **WhatsApp como canal warm:** click-to-WhatsApp en footer abre directo el chat. Esto es deliberado — en hispanic SMB DFW el WhatsApp convierte 10x mejor que email.

6. **Stripe como blocker primario:** todo este landing existe primero para destrabar Stripe domain verification. El lead capture es bonus. Si algo se debe priorizar, es que Stripe acepte el dominio.

---

## 🗺️ Próximos pasos (post-deploy)

**Esta semana (S4):**
- ✅ Deploy en Vercel + DNS apuntado
- ✅ Stripe domain verification destrabada
- 🔄 Liana refina branding visual: logo definitivo, paleta final, plantillas
- 🔄 Harold continúa backlog técnico (4 tools CRUD martes 28, GitHub+Railway miércoles 29)

**Próximas 2 semanas (S5-S6):**
- Reemplazar OG image con diseño profesional de Liana
- Agregar analítica (Plausible o GA4)
- Embed audio demo real cuando agente esté en producción
- Sección de testimonios cuando haya primer cliente
- Link a video demo profesional grabado en S6

**Mes 2-3 (S7-S10):**
- Migrar form submission de Formspree a backend `halian-bots`
- Crear `/en/` con SEO en inglés (rutas separadas)
- Agregar landing por vertical (`/plomeros`, `/hvac`, etc.) con copy específico
- Caso de estudio del primer cliente en página `/clientes`

---

**Última actualización:** 26 abril 2026
**Versión:** 1.0
**Mantenido por:** Harold Gutierrez
