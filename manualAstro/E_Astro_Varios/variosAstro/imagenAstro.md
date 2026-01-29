(astroCollections)[https://docs.astro.build/en/guides/content-collections/]
(zodSchema)[https://zod.dev/json-schema]
(images)[https://docs.astro.build/en/guides/images/]

# CONFIGURAR TRABAJAR IMAGENES

0. se recarga al ver la en pantalla:`img loading="lazy"`

1. creamos y guardarmos las imagenes en esta carpeta para astro pueda pueda trabjar con ellas `public\assets\images`

2. creamos path `"@images/*": ["./src/assets/images/*"]` en `tsconfig.json`

# Imagenes

## Image

3. creamos y rellenamos pagina `src\pages\images\index.astro`:
   - `import { Image } from "astro:assets";`:`<Image  src={post05Image} width="250" height="250" alt="Sample Image"/>`
     - `Image quality='low'` (low,medium,high)
     - `Image format='jpg'`(png, avif,webpp,jpg)
     - carga imagen tipo `.webpp` y peso y tamaño demandado

- archivo:

```
---
import MainLayout from "@layouts/MainLayout.astro";
import { Image } from "astro:assets";
import post05Image from '@images/post-05.png';
---

<MainLayout title="imagenes">
  <Image quality='low' src={post05Image} width="250" height="250" alt="Sample Image"/>
```

## Image responosive

0. `import Picture from "astro/components/Picture.astro";` : `<Picture alt='Descripción imagen'/>>`
   - formats={['avif', 'webp','jpeg']}
   - widths={[240, 540, 720,post05Image.width]}
   - sizes={`(max-width: 360px) 240px, (max-width: 720px) 540px, (max-width: 1600px) 720px, ${post05Image.width}px`}
   - density:
     - width = {post05Image.width/2}
     - densities = {[1.5,2]}
