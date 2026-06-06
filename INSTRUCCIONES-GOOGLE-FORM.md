# Instrucciones: Crear Google Form de Pre-Orden

## Paso 1: Crear el formulario

1. Ve a: https://forms.google.com
2. Click en **+ Blank** (formulario en blanco)
3. Título: **Pre-Orden Pack Premium Manhua**
4. Descripción: **Completa tus datos para reservar tu copia del Pack Premium de El Mejor Ingeniero del Mundo (13 capítulos narrados). Te contactaremos cuando el sistema de pago esté listo.**

## Paso 2: Agregar campos

### Campo 1: Nombre
- Tipo: **Short answer**
- Pregunta: **Tu nombre**
- ✓ Required

### Campo 2: Email  
- Tipo: **Short answer**
- Pregunta: **Tu email**
- ✓ Required
- Click en los 3 puntos → **Response validation**
  - Contains: @
  - Error text: "Ingresa un email válido"

### Campo 3: País
- Tipo: **Short answer**
- Pregunta: **¿De qué país eres?**
- ✓ Required

### Campo 4: Método de pago preferido
- Tipo: **Multiple choice**
- Pregunta: **¿Cómo prefieres pagar?**
- Opciones: Tarjeta de crédito/débito, PayPal, Transferencia bancaria (México), Mercado Pago, Otro

### Campo 5: Cantidad (opcional)
- Tipo: **Multiple choice**
- Pregunta: **¿Cuántas copias te interesan?**
- Opciones: 1 copia ($4.99), 2-3 copias (para regalar), 4+ copias (revender)

### Campo 6: ¿Cómo nos conociste?
- Tipo: **Multiple choice**
- Opciones: YouTube, Reddit, Twitter/X, Facebook, Instagram, TikTok, Un amigo, Otro

### Campo 7: Comentarios
- Tipo: **Paragraph**
- Pregunta: **¿Algún comentario o pregunta?**

## Paso 3: Configurar respuestas

1. Click en **Responses** (arriba)
2. ✓ **Collecting responses** (toggle ON)
3. Click en Google Sheets (verde) → nombre: "Pre-Ordenes Manhua Premium"

## Paso 4: Obtener el código de embed

1. Click en **Send** (arriba derecha)
2. Click en **<>** (Embed HTML)
3. Ajusta width: 640, height: 800
4. Copia TODO el iframe

## Paso 5: Integrar en index.html

Busca `<div id="form-container">` y reemplaza el contenido con el iframe del Google Form.

## Paso 6: Deploy

```bash
cd "C:/Users/Emmanuel/Documents/manhua-landing-premium"
npx surge login
npx surge . manhua-premium.surge.sh
```

URL final: https://manhua-premium.surge.sh
