(astroCollections)[https://docs.astro.build/en/guides/content-collections/]
(zodSchema)[https://zod.dev/json-schema]

# COLECIONES

## definir colecciones

0. ejemplo de `.md`:

````
---
title: Explorando Funciones de ES6!!!!
date: 2023-06-01
description: Explorando algunas de las nuevas funciones de ES6 en JavaScript.
author: Jane Doe
# image: https://placehold.co/1400x900/
image: "/assets/images/post-01.png"
tags: JavaScript, ES6, Programming

# layout: ../../layouts/BlogLayout.astro
---

# {frontmatter.title}

{2+2}

{/* Mostrar imagen */}

<img
  src="/assets/images/post-01.png"
  width="350"
  alt="Explorando Funciones de ES6"
/>

JavaScript ES6 introdujo varias funciones nuevas que han hecho que la codificación en JavaScript sea más eficiente y agradable. Vamos a explorar algunas de estas funciones.

## Arrow Functions - Funciones de Flecha

Las funciones de flecha proporcionan una nueva sintaxis para escribir expresiones de función. Son más concisas y vinculan léxicamente el valor `this`.

### Ejemplo

```javascript
const add = (a, b) => a + b;
console.log(add(2, 3)); // Output: 5
```
````

0. definimos el tipo archivo con que trabajaremos `src\content\config.ts`:

- `content` seria los contenidos del fichero
- `schema` los campos
- `export const collections = {};` la exportaciond ela collecion

- ARCHIVO:

```
import { defineCollection, z } from "astro:content";

const blogCollection = defineCollection({
  type: "content",
  schema: z.object({
    title: z.string(),
    date: z.date(),
    description: z.string(),
    image: z.string(),

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

1. creamos folder llamado como la export y pegamos todos los `md, mdx` a `src\content\blog`

2. modificamos `src\layouts\BlogLayout.astro` para trabajar con colecciones:

- comentamos o eliminamos lo del frommater
- trabajamos con propiedad de titulo de astro:

```
const{ title } = Astro.props;
---

<html lang="en">
  <head>

    <title>{title}</title>
```

- ponemos un `a` con `id` para llamar le desde `src\pages\posts\[slug].astro`:` <a id="btn-back" href="/"  >regresar</a>`
- ARCHIVO:

```
---
import { ClientRouter } from 'astro:transitions';

import '../styles/blog.css'
// interface Props {
//   title?: string;
// }

// const { title,...rest } = Astro.props;
// const frontmatter = rest.frontmatter || {};

const{ title } = Astro.props;
---

<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>{title}</title>
    <ClientRouter/>
  </head>
  <body class="bg-gray-900">
    <div class="text-center my-5">
      <a
      id="btn-back"
      href="/"  >regresar</a>
    </div>

    <main class="mb-10 max-w-5xl m-auto px-2 py-2">
      <slot />
    </main>
  </body>
</html>
```

3. creamos la pagina del url `src\pages\posts\[slug].astro`:

- trabajar con parametros estaticos:`export const getStaticPaths = (async() => {`
  - llamamos los `.md` del `src\content\blog` tipo `src\content\config.ts`: `const blogPosts = await getCollection('blog');`
    - trabajamos con `blogPosts` : `return blogPosts.map((post) => ({ params: { slug: post.slug },props: { post: post} }))`
- lo creamos como propiedad de la pagina: `const { post} = Astro.props;`
  - creamos variable para trabjar con `post`: `const frontmatter = post.data;`
  - trabajamos con variable `frontmatter`: `<BlogLayout title={frontmatter.title}>`
- llamamos desde abajo a la referencia `#btn-back` de `src\layouts\BlogLayout.astro` para subir en pagina: `<a href="#btn-back">Ir arriba</a>`
- trabajamos con los contenidos y cabezeras de los `md y mdx` mediante llamando valor `post`: `const{Content,headings} = await post.render();`
  - trabajar con valor cabeceras cabeceras:

  ```
    <ol>
        {headings.map((heading, index) => (
            <li >
                <a href={`#${heading.slug}`} class="text-blue-500 hover:underline">
                   {index + 1} - {heading.text}
                </a>
            </li>
        ))}
     </ol>
  ```

  - trabajamos con el contenido de los `md y mdx`: `<Content />`

- Archivo:

```
---
import type { GetStaticPaths } from "astro";
import BlogLayout from "@layouts/BlogLayout.astro";
import { getCollection } from "astro:content";

export const getStaticPaths = (async() => {

const blogPosts = await getCollection('blog');

return blogPosts.map((post) => ({
    params: { slug: post.slug },
    props: { post: post}
}))


    // return [
    //     { params: { slug: "post-01" },
    //     props: { title: "Primer post" }
    // }
    // ];
}) satisfies GetStaticPaths;

const { post} = Astro.props;
const frontmatter = post.data;

const{Content,headings} = await post.render();
---
<BlogLayout title={frontmatter.title}>
    <h4 class="text-xs text-gray-500 mb-0">{frontmatter.title}</h4>
    <h4 class="text-md text-gray-400 mb-0">{frontmatter.author}</h4>

     <h3>Tabla de contenidos</h3>
     <ol>
        {headings.map((heading, index) => (
            <li >
                <a href={`#${heading.slug}`} class="text-blue-500 hover:underline">
                   {index + 1} - {heading.text}
                </a>
            </li>
        ))}
     </ol>

    <!--contenido -->
    <Content />
    <div class="my-20">
        <a href="#btn-back">Ir arriba</a>
    </div>
</BlogLayout>

```

4. creamos y rellenamos `src\components\TypedBlogPost.astro`:

- creamos una interface llamada como `export config.ts`: `export interface Props { post: CollectionEntry<'blog'>;}`
  - creamos propiedad segun interficie:
    ```
      const { post } = Astro.props as Props;
      const frontmatter = post.data;
    ```
- insertamos en fichero: `src={frontmatter.image}`
- formatear fecha: `{Formatter.formatDate(frontmatter.date)}`
- trabajamos con url con la pagina creada `` <a href={post.slug ? `/posts/${post.slug}` : '#'} ...>``

- ARCHIVO:

```
---
import '@styles/blog.css'
import { Formatter } from '@utils/formatter';
import type { CollectionEntry } from 'astro:content';
import { InvalidFrontmatterInjectionError } from 'node_modules/astro/dist/core/errors/errors-data';
//{Formatter.formDate(new Date())}

export interface Props {
  post: CollectionEntry<'blog'>;
}

const { post } = Astro.props as Props;
const frontmatter = post.data;
---
    <div class="lg:flex">
      <img
        class="object-cover w-full h-56 rounded-lg lg:w-64"
        src={frontmatter.image}
        alt={frontmatter.title}
      />

      <div class="flex flex-col justify-between py-6 lg:mx-6">
        <a
          href={post.slug ? `/posts/${post.slug}` : '#'}
          class="text-xl font-semibold hover:underline text-white "
        >
          {frontmatter.description}
        </a>

        <span class="text-sm text-gray-300">{Formatter.formatDate(frontmatter.date)}</span>
      </div>
    </div>

```

5.  trabajamos en la pagina con el componente `src\pages\index.astro`:

- obtenemos los `.md` del `src\content\blog` tipo `src\content\config.ts`:

  ```
  import { getCollection } from 'astro:content';
  const blogPost = await getCollection('blog')
  ```

- trabajamos con `TypedBlogPost` pasandole los datos de los `md`: ` {blogPost.map(post => (<TypedBlogPost post={post} />)) }`

- Archivo:

```
--
import BlogPost from '@components/BlogPost.astro';
import TypedBlogPost from '@components/TypedBlogPost.astro';
import MainLayout from '@layouts/MainLayout.astro';
import { getCollection } from 'astro:content';
const blogPost = await getCollection('blog')
---

            {blogPost.map(post => (
                <TypedBlogPost post={post} />
            )) }


```

6. add `scroll-behavior: smooth;` a `src\styles\blog.css` para suavizar el scroll: `html {...scroll-behavior: smooth;}`

## complementos

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

### site.config en index

1. creamos y escribimos `src\config\site-config.ts`:
   ```
    export const siteConfig = {
    title: "Mi Blog con Astro",
    description: "Un blog creado con Astro y Tailwind CSS",
    };
   ```
1. lo importamos y utilizamos en `src\pages\index.astro`
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
