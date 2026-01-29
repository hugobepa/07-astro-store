(astroMarkdown)[https://docs.astro.build/en/guides/markdown-content/]
(astroMDX)[https://docs.astro.build/en/guides/integrations-guide/mdx/]
(importmetaglob)[https://docs.astro.build/en/guides/imports/#importmetaglob/]

# MARKDOWN-MDX

## instalacion y prueba

0. install mdx,T: npx astro add mdx
   - 2 `Y`
   - ` success  Added the following integration to your project: @astrojs/mdx`
   - bajar y subir servidor
1. cambiar archivos interes de `.md` a `.mdx` (los comentarios dentro archivos son diferentes )
2. utilizar datos del `frontmatter` dentro de `.mdx ` de`src\pages\posts\post-01.mdx`:

```
---
title: Explorando Funciones de ES6
date: 2023-06-01
description: Explorando algunas de las nuevas funciones de ES6 en JavaScript.
author: Jane Doe
# image: https://placehold.co/1400x900/
image: "/assets/images/post-01.png"
tags: [JavaScript, ES6, Programming]
---

#{frontmatter.title}
//expesiones JS
{2+2}
```

## markdown layout

0. crear copia y renombrar de `MainLayout ` en `src\layouts\BlogLayout.astro`.
1. llamar layout desde `.mdx` con etiqueta `layout:`: `--- ... layout: ../../layouts/BlogLayout.astro---`
2. extraer datos de `.mdx` desde `layout` en `src/layouts/BlogLayout.astro`;
   - comentamos interface para que no moleste
   - sacamos todas la propiedades de `Astro.props` con `...rest`: `const { title,...rest } = Astro.props;`
   - cogemos solamente la de `frontmatter`: `const frontmatter = rest.frontmatter || {};`
   - la utilizamos en archivo: ` <title>{frontmatter.title}</title>`

- FICHERO:

```
---

// interface Props {
//   title?: string;
// }
const { title,...rest } = Astro.props;
const frontmatter = rest.frontmatter || {};
console.log(frontmatter)

---

<html lang="en">
  <head>
   ...
    <title>{frontmatter.title}</title>
```

## añadir css propio a markdown layout

0. crear archivo 'src\layouts\BlogLayout.astro':
   - importar tailwinds: ` @import "tailwindcss";`
   - crear estilos por etiquetas,id : `html { @apply bg-gray-900 px-10;}`

   ```
   @import "tailwindcss";

    html {
        @apply bg-gray-900 px-10;
    }

    ....
   ```

1. importar estilos a layout `src\layouts\BlogLayout.astro` : `---  import '../styles/blog.css' ...---`

## creacion de pagina index

1. modificamos nuestra pagina principal `src\pages\index.astro`:
   - add dentro de la pgina: `https://www.creative-tim.com/twcomponents/component/grid-blog-page`
2. creamos y escribimos `src\config\site-config.ts`:
   ```
    export const siteConfig = {
    title: "Mi Blog con Astro",
    description: "Un blog creado con Astro y Tailwind CSS",
    };
   ```
3. lo importamos y utilizamos en `src\pages\index.astro`
   - `import { siteConfig } from 'src/config/site-config';` : `{siteConfig.title}`

```
---
import { siteConfig } from 'src/config/site-config';
...
---
<MainLayout title="Home Page">
   <!-- component -->
<section class="bg-white dark:bg-gray-900">
    <div class="container px-6 py-10 mx-auto">
        <h1 class="text-3xl font-semibold text-gray-800 capitalize lg:text-4xl dark:text-white">{siteConfig.title}</h1>
```

4. creamos component de cardPost `src\components\BlogPost.astro`

- copiamos y pegamos codigo de una tarjeta. ` <div class="lg:flex">...</div>`

```
---

---


    <div class="lg:flex">
      <img
        class="object-cover w-full h-56 rounded-lg lg:w-64"
        src="https://images.unsplash.com/photo-1492724441997-5dc865305da7?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"
        alt=""
      />

      <div class="flex flex-col justify-between py-6 lg:mx-6">
        <a
          href="#"
          class="text-xl font-semibold text-gray-800 hover:underline dark:text-white "
        >
          {title}
        </a>

        <span class="text-sm text-gray-500 dark:text-gray-300">On: 15 August 2021</span>
      </div>
    </div>
```

5. add BlogPost nuestra pagina principal `src\pages\index.astro`:

- importamos `import BlogPost from '@components/BlogPost.astro';` :`<BlogPost title='prueba'/>`
- eliminamos las otras entradas y dejamos estructura basica

```
---
import BlogPost from '@components/BlogPost.astro';
import MainLayout from '@layouts/MainLayout.astro';
import { siteConfig } from 'src/config/site-config';

---

<MainLayout title="Home Page">
   <!-- component -->
<section class="bg-white dark:bg-gray-900">
    <div class="container px-6 py-10 mx-auto">
        <h1 class="text-3xl font-semibold text-gray-800 capitalize lg:text-4xl dark:text-white">{siteConfig.title}</h1>

        <div class="grid grid-cols-1 gap-8 mt-8 md:mt-16 md:grid-cols-2">

            <BlogPost/>


        </div>
    </div>
</section>
```

## importar datos de markdown y mostrarla en /BlogPost.astro:

1. preparamos `BlogPost.astro` para leer los datos:
   - creamos interface
   - llamamos a las props de `Astro.props`
   - procesamos fecha `const parsedDate: Date`
   - usamos las propiedades `{title}`
   - pasamos a fecha local : `${parsedDate?.toLocaleDateString`

- FICHEROs:

  -Blogspot:

```
---
import '@styles/blog.css'

export interface Props {
  title: string;
  description?: string;
  date?: Date;
  tags?: string[];
  image?: string;
 url?: string;
}

const { title, description, date, tags, image, url } = Astro.props as any;
console.log(url)
const parsedDate: Date | undefined = date
  ? date instanceof Date
    ? date
    : new Date(date)
  : undefined;
---


    <div class="lg:flex">
      <img
        class="object-cover w-full h-56 rounded-lg lg:w-64"
        src={image ?? "https://images.unsplash.com/photo-1492724441997-5dc865305da7?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"}
        alt={title}
      />

      <div class="flex flex-col justify-between py-6 lg:mx-6">
        <a
          href={url ?? '#'}
          class="text-xl font-semibold hover:underline text-white "
        >
          {description}
        </a>

        <span class="text-sm text-gray-300">{date ? `On: ${parsedDate?.toLocaleDateString() || new Date().toLocaleDateString()}` : ''}</span>
      </div>
    </div>

```

- index:
  - recogemos imformacion de los post:

  ```
     const matches = import.meta.glob("./posts/*.md", { eager: true });
  const posts = Object.values(matches) as any[];
  ```

  - para que funcione url: `  const posts = Object.entries(matches).map(([path, mod])...`

  - pasamos datos al component:

  ```{posts.map(post => (
               <BlogPost {...post.frontmatter} url={post.url} />
           )) }
  ```

```
---
import BlogPost from '@components/BlogPost.astro';
import MainLayout from '@layouts/MainLayout.astro';
import type { ACTION_ERROR_CODES, ACTION_QUERY_PARAMS } from 'astro:actions';
import { siteConfig } from 'src/config/site-config';


// imports all files that end with `.md` in `./src/pages/post/`
        const matches = import.meta.glob("./posts/*.{md,mdx}", { eager: true });
        // keep path so we can build a URL for each post
        const posts = Object.entries(matches).map(([path, mod]) => {
            const m: any = mod as any;
            // try common locations for frontmatter
            const frontmatter = m.frontmatter ?? m.metadata ?? (m.default && m.default.frontmatter) ?? {};
            const url = path.replace(/^\./, '').replace(/\.(md|mdx)$/, ''); // ./posts/post-01.mdx -> /posts/post-01
            return { frontmatter, url };
        }) as any[];
    //{posts.map(post => <li>{post.frontmatter.title}</li>)}
---

<MainLayout title="Home Page">
   <!-- component -->
<section class="bg-white dark:bg-gray-900">
    <div class="container px-6 py-10 mx-auto">
        <h1 class="text-3xl font-semibold text-gray-800 capitalize lg:text-4xl dark:text-white">{siteConfig.title}</h1>

        <div class="grid grid-cols-1 gap-8 mt-8 md:mt-16 md:grid-cols-2">



            {posts.map(post => (
                <BlogPost {...post.frontmatter} url={post.url} />
            )) }
```

#### formaterar fecha

0. `src\utils\formatter.ts`:

```
export class Formatter {
  static formatDate(value: Date): string {
    const date = new Date(value);

    return Intl.DateTimeFormat("es-ES", {
      year: "numeric",
      month: "long",
      day: "2-digit",
    }).format(date);
  }
}
```

0. lo aplicamos `src/components/BlogPost.astro`:

```
---
import { Formatter } from '@utils/formatter';


---

    <span class="text-sm text-gray-300"
      >{Formatter.formatDate(frontmatter.date)}</span
    >
```
