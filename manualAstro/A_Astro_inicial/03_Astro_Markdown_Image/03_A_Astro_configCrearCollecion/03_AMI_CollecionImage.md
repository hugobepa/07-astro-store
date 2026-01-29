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

# IMAGENES COLLECION

0. copiar imagenes en `src\content\blog\images`
1. cambiar en todas los `md, mdx`: `image: "/assets/images/post-01.png"` x `image: "images/post-01.png"`
2. ir `src\content\config.ts`:
   - añadimos para tratar la imagen como imagen y como string: `({ image }) =>`
   - cambiamos el tipos de la imagen: `image: z,string()` a `image: image()`

```
   import { defineCollection, z } from "astro:content";


      const blogCollection = defineCollection({
      type: "content",
      schema: ({ image }) =>
      z.object({
      title: z.string(),
      date: z.date(),
      description: z.string(),
      image: image(),

            // Relación
            author: z.string(),

            // Relación
            tags: z.string(),
          }),

      });

      export const collections = {
      blog: blogCollection,
      };

```

- esto ya no funciona, da error:

```

image().refine((img) => img.width < 1300, {
message: "Image should be lower than 1300px",
}),

```

- todavia no funciona falta siguiente cambio

3. cambiar en `src\components\TypedBlogPost.astro`:
   cambiar `img` x `Image`: `import { Image } from "astro:assets";` y `<Image ... />`
   - src={frontmatter.image}
   - alt={frontmatter.title}
   - width={500}
   - height={500}
   - optional:
     - quality='low'
     - loading="lazy"
   - propiedades externa:
     - class="object-cover w-full h-62.5 my-5 rounded-md"
     - transition:name={`${post.slug}-image`}

```

<Image
class="object-cover w-full h-56 rounded-lg lg:w-64"
src={frontmatter.image}
alt={frontmatter.title}
width={500}
height={500}
quality='low'
loading="lazy"
class="object-cover w-full h-62.5 my-5 rounded-md"
transition:name={`${post.slug}-image`}
/>

```
