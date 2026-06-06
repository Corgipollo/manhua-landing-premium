# 🚀 Deploy Checklist — Manhua Landing Premium

> Checklist CRÍTICO para completar antes de lanzar la landing a producción.

---

## ⚠️ OBLIGATORIO — Antes de Deploy

### 1. Reemplazar Google Analytics Measurement ID

**Archivo**: `index.html`  
**Líneas**: 32 y 36  
**Reemplazar**: `G-XXXXXXXXXX` → `G-TU-ID-REAL`

```bash
# Encontrar todas las ocurrencias
grep -n "G-XXXXXXXXXX" index.html

# Reemplazar (ejemplo con sed)
sed -i 's/G-XXXXXXXXXX/G-TU-ID-REAL/g' index.html
```

**Cómo obtener tu Measurement ID**:
1. Ve a [analytics.google.com](https://analytics.google.com)
2. Crea propiedad GA4 nueva (o usa existente)
3. Admin → Data Streams → Web → Copia el ID (formato `G-XXXXXXXXXX`)

---

### 2. Crear y Subir Open Graph Image

**Requerido**: `og-image.jpg` en la raíz del repo  
**Especificaciones**:
- Dimensiones: **1200×630 px** (exactas, no aproximadas)
- Formato: JPG (optimizado, <300KB)
- Contenido sugerido:
  - Título grande: "Pack Premium Manhua Narrado"
  - Subtítulo: "El Mejor Ingeniero del Mundo"
  - Precio visible: "$4.99 USD"
  - CTA: "13 Capítulos Narrados"
  - Visual: Thumbnail atractivo del manhwa

**Herramientas sugeridas**:
- [Canva OG Image Template](https://www.canva.com/templates/?query=open%20graph)
- Figma + plugin "OG Image"
- Photoshop (1200×630px, 72 DPI)

**Validar después de subir**:
```bash
# Verifica que la imagen exista
curl -I https://corgipollo.github.io/manhua-landing-premium/og-image.jpg

# Valida en Facebook
# https://developers.facebook.com/tools/debug/
# Pega: https://corgipollo.github.io/manhua-landing-premium/

# Valida en Twitter
# https://cards-dev.twitter.com/validator
```

---

### 3. Actualizar Twitter Handle (Opcional)

**Archivo**: `index.html`  
**Líneas**: 24-25  
**Reemplazar**: `@MundoManhwa` → `@TuHandleReal`

Si no tienes Twitter, elimina las líneas 24-25:
```html
<!-- ELIMINAR ESTAS LÍNEAS SI NO TIENES TWITTER -->
<meta name="twitter:site" content="@MundoManhwa">
<meta name="twitter:creator" content="@MundoManhwa">
```

---

## 📋 Pre-Deploy Testing Local

### 1. Test de Tracking (Console Logs)

```bash
# Abre index.html en navegador local
# Abre DevTools → Console
# Deberías ver:
📊 GA4 Tracking initialized - Events: CTA clicks, Scroll depth, FAQ interactions, Time on page, Email clicks

# Haz scroll → Deberías ver:
GA4 Event: Scroll Depth - 25%
GA4 Event: Scroll Depth - 50%

# Haz click en "Comprar Ahora" → Deberías ver:
GA4 Event: CTA Click - Comprar Ahora → in Hero Section
```

**Si NO ves los logs**:
- Verifica que reemplazaste `G-XXXXXXXXXX` correctamente
- Abre en modo incógnito (evita extensiones que bloqueen analytics)
- Revisa la consola por errores JS

---

### 2. Test de Meta Tags

```bash
# Valida que todas las meta tags estén presentes
grep -E "(og:|twitter:)" index.html

# Deberías ver:
# - og:title
# - og:description
# - og:type
# - og:url
# - og:image
# - og:locale
# - twitter:card
# - twitter:title
# - twitter:description
# - twitter:image
```

---

### 3. Test de Schema.org

Valida el JSON-LD:
1. Ve a [schema.org validator](https://validator.schema.org/)
2. Pega el contenido del `<script type="application/ld+json">` (líneas 47-74)
3. Verifica que no haya errores

O usa Google Rich Results Test:
```bash
# Después del deploy, valida en:
https://search.google.com/test/rich-results
# Pega: https://corgipollo.github.io/manhua-landing-premium/
```

---

## 🚢 Deploy a GitHub Pages

### Opción A: Deploy Manual

```bash
cd /c/Users/Emmanuel/Documents/manhua-landing-premium

# Verifica estado
git status

# Agrega todos los cambios
git add index.html sitemap.xml robots.txt TRACKING-ANALYTICS.md DEPLOY-CHECKLIST.md og-image.jpg

# Commit
git commit -m "Add GA4 tracking, meta tags, schema.org, sitemap - Landing optimizada para conversión"

# Push a main (GitHub Pages auto-deploy)
git push origin main
```

### Opción B: Deploy Automatizado (GitHub Actions)

Si quieres CI/CD:

```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./
```

---

## ✅ Post-Deploy Verification

### 1. URL Accesible

```bash
# Verifica que la landing carga
curl -I https://corgipollo.github.io/manhua-landing-premium/

# Status: 200 OK ✅
```

---

### 2. Sitemap y Robots.txt

```bash
# Sitemap accesible
curl https://corgipollo.github.io/manhua-landing-premium/sitemap.xml

# Robots.txt accesible
curl https://corgipollo.github.io/manhua-landing-premium/robots.txt
```

---

### 3. Open Graph Preview

**Facebook**:
1. Ve a [Facebook Debugger](https://developers.facebook.com/tools/debug/)
2. Pega: `https://corgipollo.github.io/manhua-landing-premium/`
3. Click en "Scrape Again" (forzar re-fetch)
4. Verifica que muestre:
   - Título correcto
   - Descripción correcta
   - Imagen OG (1200×630)

**Twitter**:
1. Ve a [Twitter Card Validator](https://cards-dev.twitter.com/validator)
2. Pega la URL
3. Verifica preview

**LinkedIn**:
1. Ve a [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/)
2. Pega la URL
3. Verifica preview

---

### 4. GA4 Realtime Test

1. Abre [analytics.google.com](https://analytics.google.com)
2. Ve a **Reports** → **Realtime**
3. En otra pestaña, abre la landing en incógnito
4. Haz scroll, click en FAQ, click en CTA
5. Verifica en Realtime que los eventos aparezcan inmediatamente:
   - `page_view` ✅
   - `scroll_depth` ✅
   - `faq_open` ✅
   - `cta_click` ✅

**Si NO aparecen eventos**:
- Espera 1-2 minutos (a veces hay delay)
- Verifica que el Measurement ID sea correcto
- Abre DevTools → Console para ver logs de debug
- Activa [GA Debugger extension](https://chrome.google.com/webstore/detail/google-analytics-debugger)

---

### 5. Lighthouse Re-Test (Post-Deploy)

```bash
npx lighthouse https://corgipollo.github.io/manhua-landing-premium/ --view

# Targets esperados:
# Performance: 85-95 (después de agregar og-image real)
# Accessibility: 95-100 ✅
# Best Practices: 95-100 ✅
# SEO: 100 ✅
```

---

## 🎯 Configuración en GA4 (Post-Deploy)

### 1. Marcar `cta_click` como Conversión

1. GA4 → **Admin** → **Events**
2. Busca `cta_click`
3. Toggle "Mark as conversion" → ON
4. Ahora puedes trackear conversion rate en Reports

---

### 2. Crear Audiencias para Remarketing

**Audiencia 1**: "Usuarios Engaged" (scrollearon 75%+)
- GA4 → **Admin** → **Audiences** → **New Audience**
- Condición: `Event name = scroll_depth` AND `Event parameter (event_label) = 75%`
- Usa para remarketing en Google Ads / Meta Ads

**Audiencia 2**: "Abandonaron Checkout" (click CTA pero no compraron)
- Condición: `Event name = cta_click`
- Exclusión: `Purchase completed` (esto lo configurarás después con Gumroad webhook)

---

### 3. Crear Dashboard Custom

1. GA4 → **Explore** → **Blank**
2. Agrega estas métricas:
   - Total pageviews
   - CTA clicks (conversiones)
   - Tasa de conversión: `(CTA clicks / Pageviews) × 100`
   - Scroll 75%+ rate
   - FAQ engagement rate
   - Tiempo promedio en página
3. Guarda como "Manhua Landing Dashboard"

---

## 🔥 Optimizaciones Futuras (Post-Launch)

### Semana 1:
- [ ] Recopilar primeros 100-200 pageviews
- [ ] Analizar scroll depth promedio
- [ ] Identificar CTA con mejor conversion rate
- [ ] Revisar FAQs más abiertas → ajustar contenido

### Semana 2-4:
- [ ] A/B test de headlines (si tienes tráfico suficiente)
- [ ] Agregar testimonials si consigues reviews
- [ ] Optimizar images (WebP en lugar de JPG)
- [ ] Implementar lazy loading para imágenes (si agregas más)

### Mes 2+:
- [ ] Integrar Gumroad webhook para trackear ventas reales en GA4
- [ ] Configurar Enhanced Ecommerce tracking completo
- [ ] Agregar Facebook Pixel si haces ads
- [ ] Implementar GTM para gestión de tags más fácil

---

## 📊 Métricas Objetivo (Benchmarks)

| Métrica | Target Ideal | Método de Medición |
|---------|--------------|-------------------|
| **Conversion Rate** | 5-15% | GA4: (cta_click / page_view) × 100 |
| **Scroll Depth 75%+** | >60% | GA4: Events → scroll_depth → filter 75% |
| **Bounce Rate** | <40% | GA4: (1 - engaged_sessions / sessions) × 100 |
| **Avg Time on Page** | 2-4 min | GA4: Reports → Engagement → Pages |
| **FAQ Engagement** | 20-40% | GA4: (faq_open / page_view) × 100 |

---

## 🐛 Troubleshooting

### Problema: GA4 no registra eventos

**Soluciones**:
1. Verifica Measurement ID correcto (línea 32 y 36)
2. Abre DevTools → Console → busca logs `GA4 Event: ...`
3. Desactiva ad blockers / privacy extensions
4. Verifica en modo incógnito
5. Usa [GA Debugger extension](https://chrome.google.com/webstore/detail/google-analytics-debugger)

---

### Problema: Open Graph image no se muestra

**Soluciones**:
1. Verifica que `og-image.jpg` exista en URL pública
2. Tamaño correcto: 1200×630px (exacto)
3. Fuerza re-scrape en [Facebook Debugger](https://developers.facebook.com/tools/debug/)
4. Limpia caché de Twitter con [Card Validator](https://cards-dev.twitter.com/validator)
5. Verifica que la URL en meta tag sea absoluta (no relativa)

---

### Problema: Lighthouse score de Performance bajo

**Causas comunes**:
1. Imagen OG muy pesada (>500KB) → optimizar con TinyPNG
2. Conexión lenta durante test → re-correr audit
3. Tailwind CDN slow → considerar build con Tailwind CLI

**Fixes rápidos**:
```html
<!-- Agregar después de preconnect existentes -->
<link rel="dns-prefetch" href="https://cdn.tailwindcss.com">
<link rel="preload" href="og-image.jpg" as="image">
```

---

## 📞 Contacto y Soporte

**Dudas sobre tracking**: Ver `TRACKING-ANALYTICS.md`  
**Dudas sobre deploy**: Este archivo  
**Bugs en código**: Revisar console logs de browser

---

**Última actualización**: 2026-06-06  
**Próxima revisión**: Post-deploy (después de las primeras 100 visitas)
