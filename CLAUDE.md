# invitaciones-boda

Sitio estático en Astro v4.15 para invitaciones de boda, 100% personalizable vía `src/data/config.json`. Sin backend (a propósito, ver "Decisiones de arquitectura" abajo).

Levantar en local: `npm install` + `npm run dev` → http://localhost:4321

## Estructura

```
src/
├── components/    Hero, OurStory, Itinerary, Gallery, LocationSection, RSVP, GiftRegistry
├── layouts/       WeddingLayout.astro (aplica tema/colores)
├── pages/
│   ├── index.astro       ensambla el sitio real, lee config.json
│   └── onboarding.astro  formulario interno para que la pareja genere su config.json
└── data/
    └── config.json       toda la info editable de la boda (primer test real: Zaira & Ruben)
```

## `/onboarding`

Herramienta para que la pareja cuente los detalles de su boda sin ver nada técnico. Genera un JSON y lo manda por correo (ver Web3Forms abajo). Es un solo archivo (`onboarding.astro`, ~950 líneas, toda la lógica en un `<script>` inline sin módulos ni tests) con:

- Hashtag autogenerado desde los nombres + año de la boda (editable u omitible)
- Itinerario con 9 momentos sugeridos por default, cada uno omitible, que se reordenan solos por hora al editarlos
- 4 temas (elegante / bohemio / minimalista / **moderno audaz**, este último de alto contraste) con paletas curadas o color personalizado
- Preview de color en vivo: toda la página cambia de fondo/bordes/botones según la paleta elegida (contraste de texto de los botones calculado con la fórmula de luminancia relativa, no hardcodeado)
- Fotos por correo (ver abajo, no se adjuntan)
- Mesas de regalo múltiples (agregar/quitar cuantas necesiten)
- Autoguardado de borrador en `localStorage`, con opción de "Empezar de nuevo"
- Responsive: los renglones de dos columnas se apilan en pantallas <480px

## Decisiones de arquitectura

**Sin backend, a propósito.** Se pospuso Supabase deliberadamente para mantener el proyecto simple mientras se valida el flujo completo con datos reales. No proponer agregar backend/DB a menos que se pida explícitamente.

**Web3Forms — solo para el JSON, no para fotos.** El onboarding manda el JSON de la boda por Web3Forms (access key hardcodeada en `onboarding.astro`, cuenta gratuita de `rubenreichelbauman@gmail.com`). Los adjuntos de archivo son una **feature exclusiva del plan Pro de Web3Forms** (~$12-18 USD/mes) — con cuenta gratuita el archivo se descarta silenciosamente aunque el resto del formulario sí llegue. Se evaluaron alternativas (Cloudinary/ImgBB, EmailJS, mailto) y se descartaron por costo o por las mismas limitaciones de adjuntos. En vez de eso, la sección de Fotos abre Gmail/Outlook/correo predeterminado prellenados para que la pareja mande las fotos directo por correo. Si en el futuro se automatiza esto, la ruta más alineada con el proyecto es un hosting de imágenes gratuito con "unsigned upload" desde el navegador (Cloudinary/ImgBB), no un servicio de email con adjuntos.

**RSVP en `localStorage`.** El RSVP es funcional solo en frontend — no manda la respuesta a ningún lado, cada invitado la guarda en su propio navegador. Es una decisión consciente (para no montar backend), pero significa que **hoy la pareja no tiene forma de ver quién confirmó**. Ver "Pendientes" abajo.

**`giftOptions` es un arreglo, no una URL.** `config.json` usa `giftOptions.registries: [{label, url}]` para soportar múltiples mesas de regalo (tienda departamental + en línea, etc.), no un solo `registryUrl`. Si se toca `GiftRegistry.astro` o `index.astro`, respetar ese shape.

**Links de mapa simples, no embeds.** Los eventos usan `mapUrl` (no `mapEmbedUrl`) — es el link normal que da el botón "Compartir" de Google Maps al buscar un lugar, usado como href de un botón "Cómo llegar" (`LocationSection.astro`), no como `src` de un iframe. Ya no existe la distinción "link para compartir vs. link para insertar mapa" que había antes; se abandonó el iframe embebido por completo.

**Horas siempre en 24h.** Los campos de hora en Eventos e Itinerario del onboarding son `<input type="time">` (no texto libre), así el valor guardado siempre es `HH:MM` inequívoco. Antes eran texto libre y causaron un bug real (una pareja escribió "4:00"/"8:00" queriendo decir 4pm/8pm).

## Pendientes (en orden de impacto)

1. **Visibilidad del RSVP** — bajo evaluación un giro más grande: convertir el proyecto en una pequeña plataforma con login por cliente y un dashboard (onboarding, lista de invitados con link personalizado por invitado + botón de copiar para compartir manualmente, organización de mesas, posible agenda/checklist a futuro). Esto requeriría traer de vuelta Supabase (auth + DB) y resolvería de paso el pendiente de multi-tenant de abajo. El usuario está evaluando el modelo de negocio antes de meterle ingeniería — no asumir alcance ni empezar a programarlo sin confirmar primero.
2. **Sin hosting/deploy configurado** — solo GitHub + local. Bloqueante para compartir una invitación real.
3. **Sin multi-tenant** — cada boda nueva requiere reemplazar `config.json` y fotos a mano en `public/images/`.
4. `onboarding.astro` podría dividirse en módulos si se le sigue agregando funcionalidad (no urgente).

> Nota: el `README.md` describe el estado inicial del proyecto y ya quedó desactualizado (dice como "siguientes pasos" cosas que ya están hechas, como RSVP y onboarding). Este archivo es la referencia vigente.
