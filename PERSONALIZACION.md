# Checklist: personalizar una invitación para un cliente real

Para un cliente real **no se edita el demo directamente** — se copia a una carpeta nueva
`clientes/` (paralela a `demos/`, que es solo para las muestras de venta). Así el cliente no
sale en `sitemap.xml`/`robots.txt` de la landing y no se mezcla con las demos.

```
clientes/
  valentina-mateo.html   ← copia de demos/boda.html, personalizada
  camila-xv.html         ← copia de demos/xv-anos.html, personalizada
```

## 1. Elegir el demo base

Según el tipo de evento: `boda.html`, `xv-anos.html`, `bautizo.html`, `cumpleanos.html`,
`babyshower.html`, `graduacion.html` o `empresa.html`. Copiarlo a `clientes/<slug-del-evento>.html`.

## 2. SEO — lo primero, antes de tocar contenido

Un demo es material de venta (debe salir en Google). **Una invitación real es privada** — trae
nombres, direcciones y fecha de una familia real, no debe ser indexable:
- [ ] `<meta name="robots" content="index, follow">` → `content="noindex, nofollow"`
- [ ] Quitar o dejar genéricos los bloques `<meta property="og:...">` / `twitter:...` (no hace
      falta que compartan bonito en redes si no van a compartirse públicamente)
- [ ] `<link rel="canonical">` → apuntar a la URL real del cliente o quitarlo
- [ ] **No** agregar la URL del cliente a `sitemap.xml` — ese archivo es solo para `demos/`

## 3. Textos (buscar y reemplazar en todo el archivo)

- [ ] Nombre(s) / título del evento (aparece en `<title>`, el sobre `#envelope`, el `#hero`, y
      el `footer` — buscar el nombre de ejemplo, ej. "Valentina" y "Mateo", y reemplazar todas
      las apariciones)
- [ ] Fecha y hora: aparecen en 3 lugares que **deben coincidir entre sí**:
  1. Texto visible (`.date-tag` del hero, `<p class="time">` de la ubicación)
  2. `const EVENT_DATE = new Date('AAAA-MM-DDTHH:MM:SS-06:00')` en el `<script>` (el countdown
     no funciona si esta no coincide con el texto)
  3. Fecha límite de RSVP (texto arriba del formulario, sección `#rsvp`)
- [ ] Ubicación(es): dirección, nombre del lugar, hora — sección `#ubicacion`
- [ ] Sección específica del tipo de evento (la que reemplaza itinerario en los tipos nuevos):
  - Boda/XV/bautizo/cumpleaños → `#itinerario` (horario del evento) y `#familia` (padres/padrinos)
  - Babyshower → `#consejos` (mensajes de familiares — puede quedar genérico o pedirle al
    cliente sus propios consejos)
  - Graduación → `#trayectoria` (Kinder → Prepa → Universidad del egresado real)
  - Empresa → `#trayectoria` (hitos reales del negocio) y el campo "Empresa/puesto" del RSVP
- [ ] Código de vestimenta / mesa de regalos / registro — sección de "Detalles"
- [ ] Menú (solo en boda) si el cliente ya lo tiene definido

## 4. Fotos

- [ ] Reemplazar las 2 fotos de fondo (`#envelope` y `#hero`) por fotos reales del cliente
- [ ] Reemplazar las 6 de la galería (`#galeria .gal-grid img`) — si el cliente no tiene 6, repetir
      las mejores o reducir el grid
- [ ] Actualizar los `alt` de cada `<img>` con una descripción real (no dejar los genéricos del
      demo — afecta accesibilidad aunque la página no sea indexable)
- [ ] Actualizar `og:image` si se va a compartir el link por WhatsApp (para que la vista previa
      no muestre la foto genérica del demo)

## 5. Lista de invitados (opcional según el evento)

Si el cliente trae lista:
- [ ] Correr el workflow de n8n **`Invitaciones — Generar Links de Invitados`** con los nombres
      (y teléfono si se va a usar el recordatorio de WhatsApp, hoy bloqueado por Meta) — genera
      el slug del evento, el slug por invitado, y el link
      `clientes/<slug-evento>.html?event=<slug>&g=<slug-invitado>&n=<Nombre>`
- [ ] **Importante**: ese workflow nunca se ha corrido con una lista real, solo con un invitado
      de prueba — la primera vez, revisar en Neon que los datos queden bien antes de mandar los
      links de verdad

Si el cliente NO trae lista (evento abierto): el RSVP funciona igual sin `?event=&g=&n=`, cada
invitado escribe su nombre a mano — no hace falta correr el workflow.

## 6. Antes de mandarle el link al cliente

- [ ] Abrir `clientes/<slug>.html` en el navegador — revisar countdown, fotos, textos
- [ ] Probar el RSVP una vez (con o sin invitado de prueba según el caso) y confirmar que
      aparece en `admin.html` filtrando por ese `event_slug`
- [ ] Generar y mandarle al cliente su link de `mi-panel.html?event=<slug-evento>` para que vea
      las confirmaciones en vivo
- [ ] Avisarle que tiene una ronda de ajustes de texto/imágenes incluida (ya prometido en el FAQ
      de la landing)
