(relleno de pagina)[https://gist.github.com/Klerith/01bd995e401d5bb6d8d449d0599b1c3d]

0. creamos pagina y la llenamos `src\pages\blog\[page].astro`:
   - funcion trabajar param estaticos`import type { GetStaticPaths } from "astro";` : `export const getStaticPaths`
     - lo pasamos a async y obtenemos la paginacion : `async ({paginate})`
     - obtenemos los datos de los blogs: `const blogPosts = await getCollection('blog');`
     - enviamos los datos obtenidos y 2 elementos por pagina : `blogPosts,{pageSize: 2`
   - Declarariamos la propiedad `page` para poder trabajar: `const { page } = Astro.props;`
   - obtenenemos los datos de los `posts` mediante `page` y los pasamsoa al componente:
     `  {page.data.map((post) => <TypedBlogPost post={post} />)}`
   - pasamos los datos `page` para pasar las paginas `page.url.prev`.

- fichero:

```
---
import type { GetStaticPaths } from "astro";
  // Type GetStaticPaths de Astro
import TypedBlogPost from '@components/TypedBlogPost.astro';
import MainLayout from '../../layouts/MainLayout.astro';
import { getCollection } from "astro:content";

export const getStaticPaths = ( async ({paginate})=> {

    const blogPosts = await getCollection('blog');

    return paginate(blogPosts,{pageSize: 2});
}) satisfies GetStaticPaths;

const { page } = Astro.props;
---


  {page.data.map((post) => <TypedBlogPost post={post} />)}


   <a href={page.url.prev}

    <a href={page.url.next}
```

# crear y configurar Authors

0. bajarse recursos y ponerlos en la carpeta `src\content`
1. pasar los nombres de los autores en `.md,.mdx` de `Hugo Ber` a `hugo-ber`
2. añadir autores al `src\content\config.ts`:

- creamos una nueva collecion: `const authorCollection = defineCollection({`
  - al trabajar con archivos `yml,json,` hay poner `type: "data",`
  - para trabajar con la Imagen(astro): `({ image }) =>z.object({` y ` avatar: image(),`
- exportamos schema para poder trabajar: `export const collections = { ... author: authorCollection,};`
- pasamos como refencia `const blogCollection = defineCollection({` para poder trabajar:`author: reference("author"),`
- EXTRA-OPCIONAL, convertimos los tags de string a array(string):
  - `const blogCollection = defineCollection({` : `  tags: z.array(z.string()),`
  - cambios en dentro de `.md,.mdx`: `tags: [Flutter, Mobile Development, Dart]`

- ARCHIVO:

```
import { defineCollection, reference, z } from "astro:content";

const blogCollection = defineCollection({
  type: "content",
  schema: ({ image }) =>
 ...
      author: reference("author"),

      // Relación
      tags: z.array(z.string()),
      //tags: z.string(),
    }),
});

const authorCollection = defineCollection({
  type: "data",
  schema: ({ image }) =>
    z.object({
      name: z.string(),
      avatar: image(),
    }),
});

export const collections = {
  blog: blogCollection,
  author: authorCollection,
};

```

0. sale [object Object] en la pagina (hacer csonsole.log para acomprobar si llega en component o [slug] )

1. mostrar autor `src\pages\posts\[slug].astro`:
   - `import { getEntry } from "astro:content";`
   - importamos los datos de los autores `const author =await getEntry('author', post.data.author.id); `
     - importamos los datos de los autores: `await getEntry('author', post.data.author.id)`
   - añadimos este codigo para evitar aviso `Object is possibly 'undefined'.` en los campos: `if (!author) {throw new Error('Author not found');`
   - trabajamos con los datos importados `  <Image src={author.avatar} alt={author.data.name}`

```
---

const blogPosts = await getCollection('blog');
const author =await getEntry('author', post.data.author.id)  ;
if (!author) {
  throw new Error('Author not found');
---
 <BlogLayout title={frontmatter.title}>
    <h4 class="text-xs text-gray-500 mb-0">{frontmatter.title}</h4>
    <!-- <h4 class="text-md text-gray-400 mb-0">{frontmatter.author}</h4>  -->
     <div class="text-md text-gray-400 mb-0 flex flex-row mt-2">
        <Image
            src={author.data.avatar}
            alt={author.data.name}
            width={50}
            height={50}
            class="rounded-full"
            loading="eager"/>
            <div class="flex flex-col ml-5">
                <a href="#">{author.data.name}</a>
                <span class="text-xs text-gray-200">{Formatter.formatDate(frontmatter.date)}</span>
            </div>
     </div>


```
