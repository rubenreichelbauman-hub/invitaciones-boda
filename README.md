# Invitaciones de boda — proyecto base

Este es el esqueleto de tu producto: un sitio Astro con componentes reutilizables
(Hero con contador, Galería, Ubicación, Mesa de regalos) que se personalizan
100% desde `src/data/config.json`.

## Requisitos previos

1. Instala **Node.js** (versión 18 o superior): https://nodejs.org/ (elige la versión LTS)
2. Instala **Visual Studio Code**: https://code.visualstudio.com/
3. Dentro de VS Code, instala la extensión **Astro** (buscar "astro-build" en Extensiones)

## Cómo correr el proyecto en tu computadora

1. Descomprime este `.zip` en una carpeta, por ejemplo `Documentos/invitaciones-boda`
2. Abre esa carpeta en VS Code (`Archivo > Abrir carpeta...`)
3. Abre una terminal dentro de VS Code (`Terminal > Nueva terminal`)
4. Corre este comando una sola vez para instalar las dependencias:

   ```
   npm install
   ```

5. Corre el servidor de desarrollo:

   ```
   npm run dev
   ```

6. Abre tu navegador en la dirección que aparezca en la terminal (normalmente `http://localhost:4321`)

Cada vez que guardes un cambio en cualquier archivo, el navegador se actualiza solo.

## Cómo personalizar una boda nueva

Todo lo que ves en la página sale de un solo archivo: `src/data/config.json`.
Para "crear" una boda nueva, en este punto del proyecto (aún sin Supabase)
solo tienes que:

1. Editar `src/data/config.json` con los datos de la nueva pareja
2. Reemplazar las fotos en `public/images/` con las fotos reales
3. Correr `npm run build` para generar el sitio final (queda en la carpeta `dist/`)

## Estructura del proyecto

```
src/
├── components/       ← Hero, Gallery, LocationSection, GiftRegistry
├── layouts/           ← WeddingLayout.astro (aplica el tema/colores)
├── pages/
│   └── index.astro    ← ensambla todos los componentes
└── data/
    └── config.json    ← toda la información editable de la boda
```

## Siguientes pasos (para cuando quieras seguir avanzando)

- Agregar el componente de RSVP conectado a Supabase
- Agregar la sección "Nuestra historia"
- Conectar el formulario de onboarding para generar este `config.json` automáticamente
- Configurar multi-tenant para manejar varios clientes desde un solo proyecto desplegado
