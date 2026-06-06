# 📊 Documentación de Tracking y Analytics — Manhua Landing Premium

> Guía completa de Google Analytics 4, eventos configurados y métricas clave para la landing de venta del Pack Premium Manhua Narrado.

---

## 🎯 Google Analytics 4 (GA4) Setup

### 1. Configuración del Tracking ID

**IMPORTANTE**: Reemplaza el placeholder en `index.html` con tu Measurement ID real.

```html
<!-- Línea 32 del index.html -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
```

**Pasos para obtener tu Measurement ID:**

1. Ve a [Google Analytics](https://analytics.google.com/)
2. Crea una propiedad GA4 (o usa una existente)
3. Ve a **Admin** → **Data Streams** → **Web**
4. Copia el **Measurement ID** (formato: `G-XXXXXXXXXX`)
5. Reemplaza `G-XXXXXXXXXX` en 2 lugares del `index.html`:
   - Línea 32: `<script async src="https://www.googletagmanager.com/gtag/js?id=TU-ID-AQUI">`
   - Línea 36: `gtag('config', 'TU-ID-AQUI', {...})`

---

## 📈 Eventos Configurados

### Evento 1: **CTA Click** (Clicks en "Comprar Ahora")

**Trigger**: Cuando el usuario hace click en cualquier botón de "Comprar Ahora" (hay 3 en la página)

**Parámetros enviados:**
```javascript
{
  'event_category': 'CTA',
  'event_label': 'Comprar Ahora →',  // Texto del botón
  'button_location': 'Hero Section' / 'Obtén Tu Pack Ahora' / 'Final CTA',
  'link_url': 'https://emmanuel.gumroad.com/l/pack-premium-manhua'
}
```

**Vista en GA4**:
- **Reports** → **Engagement** → **Events** → Busca `cta_click`
- Filtrar por `button_location` para ver qué CTA convierte más

**Cómo usarlo**:
- Identifica qué sección de la landing genera más clicks
- Optimiza los CTAs con menor rendimiento
- Mide tasa de conversión: (CTA clicks / Pageviews) × 100

---

### Evento 2: **Scroll Depth** (Profundidad de Scroll)

**Trigger**: Cuando el usuario alcanza 25%, 50%, 75% o 100% de la página

**Parámetros enviados:**
```javascript
{
  'event_category': 'Engagement',
  'event_label': '25%' / '50%' / '75%' / '100%',
  'value': 25 / 50 / 75 / 100
}
```

**Vista en GA4**:
- **Reports** → **Engagement** → **Events** → Busca `scroll_depth`
- Agrupa por `event_label` para ver distribución

**Cómo usarlo**:
- Si muchos usuarios no pasan del 50%, el contenido superior no engancha
- Si llegan al 100% pero no compran → problema de persuasión, no de atención
- Benchmark ideal: >60% de usuarios llegan al 75%+

---

### Evento 3: **FAQ Open** (Interacción con FAQ)

**Trigger**: Cuando el usuario abre una pregunta del FAQ

**Parámetros enviados:**
```javascript
{
  'event_category': 'FAQ',
  'event_label': '¿Cuándo recibiré mi pack?' / '¿Qué formato son los archivos?' / etc.
}
```

**Vista en GA4**:
- **Reports** → **Engagement** → **Events** → Busca `faq_open`
- Ordena por `event_count` para ver preguntas más frecuentes

**Cómo usarlo**:
- Las preguntas más abiertas revelan las objeciones principales
- Si "¿Habrá más volúmenes?" es la más popular → indica demanda futura
- Si "¿Cuánto pesan los archivos?" domina → considera mencionarlo antes en la landing

---

### Evento 4: **Time on Page** (Tiempo en Página)

**Trigger**: Cada 30 segundos que el usuario permanece en la página

**Parámetros enviados:**
```javascript
{
  'event_category': 'Engagement',
  'event_label': 'Seconds',
  'value': 30 / 60 / 90 / 120 / etc.
}
```

**Vista en GA4**:
- **Reports** → **Engagement** → **Events** → Busca `time_on_page`
- Suma el `value` promedio para calcular tiempo promedio de sesión

**Cómo usarlo**:
- Tiempo promedio ideal para landing de producto: 2-4 minutos
- < 1 minuto → bounce alto, contenido no retiene
- > 5 minutos → excelente engagement o confusión (verifica scroll depth)

---

### Evento 5: **Email Click** (Click en email de contacto)

**Trigger**: Cuando el usuario hace click en `emmanuel@mundo-manhwa.com`

**Parámetros enviados:**
```javascript
{
  'event_category': 'Contact',
  'event_label': 'emmanuel@mundo-manhwa.com'
}
```

**Vista en GA4**:
- **Reports** → **Engagement** → **Events** → Busca `email_click`

**Cómo usarlo**:
- Si este evento dispara mucho → posible confusión o preguntas no respondidas en FAQ
- Bajo volumen es positivo (la landing responde todas las dudas)

---

## 🔥 Funnel de Conversión Sugerido

### Configurar en GA4:
1. Ve a **Explore** → **Funnel exploration**
2. Crea estos pasos:

```
Step 1: page_view (entrada a la landing)
         ↓
Step 2: scroll_depth (event_label = '50%')  [Usuario lee la mitad]
         ↓
Step 3: faq_open (cualquier FAQ)            [Usuario investiga más]
         ↓
Step 4: cta_click                           [Usuario hace click en "Comprar"]
```

### Interpretación:
- **Drop-off Step 1 → 2**: Problema de atención inicial (mejorar Hero)
- **Drop-off Step 2 → 3**: Contenido intermedio no genera preguntas (buen signo o mal signo, depende)
- **Drop-off Step 3 → 4**: FAQ no convence → reescribir respuestas o agregar social proof

---

## 📊 Métricas Clave (KPIs) a Monitorear

| Métrica | Fórmula | Target Ideal |
|---------|---------|--------------|
| **Tasa de Conversión** | (CTA Clicks / Pageviews) × 100 | 5-15% |
| **Engagement Score** | (50%+ scroll / Total Pageviews) × 100 | >60% |
| **Bounce Rate** | Users que no scrollean >25% | <40% |
| **Tiempo Promedio** | Avg Time on Page | 2-4 min |
| **FAQ Engagement** | (FAQ Opens / Pageviews) × 100 | 20-40% |

---

## 🛠️ Verificación del Tracking

### 1. Test Manual (Antes de Producción)

1. Abre la landing en **modo incógnito**
2. Abre **Chrome DevTools** → **Console**
3. Verás logs como:
   ```
   📊 GA4 Tracking initialized - Events: CTA clicks, Scroll depth, FAQ interactions, Time on page, Email clicks
   GA4 Event: Scroll Depth - 25%
   GA4 Event: CTA Click - Comprar Ahora → in Hero Section
   ```

4. Si ves estos logs → tracking funcionando ✅

### 2. Verificación en GA4 (Después de Deploy)

1. Ve a **Reports** → **Realtime**
2. Abre la landing en otra pestaña
3. Haz scroll, click en FAQ, click en CTA
4. En Realtime deberías ver los eventos aparecer inmediatamente

### 3. Debug con GA4 DebugView

1. Instala la extensión [Google Analytics Debugger](https://chrome.google.com/webstore/detail/google-analytics-debugger)
2. Actívala
3. Recarga la landing
4. Ve a GA4 → **Admin** → **DebugView**
5. Verás TODOS los eventos en tiempo real con parámetros completos

---

## 🚀 Optimizaciones Adicionales Sugeridas

### A. Agregar Enhanced Ecommerce (Opcional)

Si quieres tracking más avanzado de producto:

```javascript
// Agregar después del gtag('config', ...)
gtag('event', 'view_item', {
  currency: 'USD',
  value: 4.99,
  items: [{
    item_id: 'pack-premium-manhua-vol1',
    item_name: 'Pack Premium Manhua Narrado - El Mejor Ingeniero del Mundo',
    price: 4.99,
    item_category: 'Digital Product',
    item_category2: 'Manhwa',
    quantity: 1
  }]
});
```

### B. Agregar Facebook Pixel (para Facebook Ads)

Si planeas hacer ads en Meta:

```html
<!-- Facebook Pixel Code -->
<script>
!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};
if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];
s.parentNode.insertBefore(t,s)}(window, document,'script',
'https://connect.facebook.net/en_US/fbevents.js');
fbq('init', 'TU-PIXEL-ID');
fbq('track', 'PageView');
fbq('track', 'ViewContent', {
  content_name: 'Pack Premium Manhua Narrado',
  content_category: 'Digital Product',
  content_ids: ['pack-premium-manhua-vol1'],
  content_type: 'product',
  value: 4.99,
  currency: 'USD'
});
</script>
<!-- Agregar al final de los CTAs -->
<script>
  document.querySelectorAll('a[href*="gumroad"]').forEach(btn => {
    btn.addEventListener('click', () => {
      fbq('track', 'InitiateCheckout', {
        value: 4.99,
        currency: 'USD'
      });
    });
  });
</script>
```

### C. Agregar Google Tag Manager (GTM)

Para gestionar todos los tags sin tocar código:

1. Crea cuenta en [tagmanager.google.com](https://tagmanager.google.com)
2. Reemplaza el código de GA4 con el snippet de GTM
3. Gestiona GA4, Facebook Pixel, etc. desde la interfaz de GTM

---

## 📋 Checklist de Implementación

- [x] Google Analytics 4 instalado (placeholder `G-XXXXXXXXXX`)
- [x] Eventos custom configurados (5 eventos)
- [x] Meta tags Open Graph completos
- [x] Twitter Cards configurados
- [x] Schema.org Product markup implementado
- [x] Sitemap.xml generado
- [x] Robots.txt creado
- [x] Lighthouse audit ejecutado (scores: 84/96/96/100)
- [ ] **PENDIENTE**: Reemplazar `G-XXXXXXXXXX` con tu Measurement ID real
- [ ] **PENDIENTE**: Crear imagen `og-image.jpg` (1200×630px) y subirla al repo
- [ ] **PENDIENTE**: Reemplazar `@MundoManhwa` con tu handle de Twitter real (si aplica)
- [ ] **PENDIENTE**: Test manual de tracking (ver console logs)
- [ ] **PENDIENTE**: Verificar eventos en GA4 Realtime después del deploy

---

## 🎯 Próximos Pasos

1. **Reemplazar placeholders**:
   - Measurement ID de GA4
   - Imagen Open Graph (`og-image.jpg`)
   - Handle de Twitter

2. **Crear imagen OG optimizada**:
   - Tamaño: 1200×630px
   - Formato: JPG (optimizado)
   - Contenido sugerido:
     - Título: "Pack Premium Manhua Narrado"
     - Precio: $4.99 USD
     - Visual: Thumbnail del manhwa + texto narrado
     - Branding: Logo de Mundo Manhwa

3. **Deploy a GitHub Pages**:
   ```bash
   git add .
   git commit -m "Add GA4 tracking, meta tags, schema.org, sitemap"
   git push origin main
   ```

4. **Verificar tracking en producción**:
   - Abrir landing en incógnito
   - Verificar eventos en GA4 Realtime
   - Confirmar que OG image se ve correcta en Facebook Debugger

5. **Configurar conversiones en GA4**:
   - Marca `cta_click` como "Key Event" (conversión)
   - Crea audiencias para remarketing (ej: "Users who scrolled 75%+")

---

## 🔗 Recursos Útiles

- [GA4 Events Reference](https://support.google.com/analytics/answer/9267735)
- [Open Graph Debugger](https://developers.facebook.com/tools/debug/)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)
- [Schema.org Product Docs](https://schema.org/Product)
- [Lighthouse CI](https://github.com/GoogleChrome/lighthouse-ci)

---

**Última actualización**: 2026-06-06  
**Mantenido por**: Claude Code (Grop)
