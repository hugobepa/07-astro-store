(paginaAuthor)[https://gist.github.com/Klerith/e754477e54c7393820c066f95f91f9ef]

# CREAR PAGINA AUTHOR

0. creamos pagina author `src/pages/blog/[author].astro`:
   - pegamos el contenido `https://gist.github.com/Klerith/e754477e54c7393820c066f95f91f9ef`
     - importamos los `missing imports`
     - añadimos el de Imagen:`import { Image } from "astro:assets";`
     - añadimos en `<style>  a {}`: ` a { @import "tailwindcss" ...};`
   - creamos parametros estaticos e importamos datos:
     - importamos parametros estaticos `import type { GetStaticPaths } from 'astro';`:
       ` export const getStaticPaths = (async () => {... }) satisfies GetStaticPaths;`
       - en parametros importamos los datos `import { getCollection } from 'astro:content';`:

       ```
       const [authors, posts] = await Promise.all([
        getCollection('author'),
        getCollection('blog'),
        ]);

       ```

       - enviamos los valores a las propiedades:

       ```
            return authors.map((author) => ({
                params: { author: author.id },
                props: {
                blogPosts: posts.filter((post) => post.data.author.id === author.id),
                author: author.data,
            },
       ```

   - creamos las prpiedades para trabajar: `const { blogPosts, author } = Astro.props;`

- importamos losa datos mediante las propiedades al componente:`{blogPosts.map((post) => <TypedBlogPost post={post} />)},  <a>{author.name}</a>`

-FICHERO:

```
---
import TypedBlogPost from '@components/TypedBlogPost.astro';
import { siteConfig } from 'src/config/site-config';
import MainLayout from '../../layouts/MainLayout.astro';
import { Image } from "astro:assets";
import { getCollection } from 'astro:content';
import type { GetStaticPaths } from 'astro';



export const getStaticPaths = (async () => {
//    const authors = await getCollection('author')
//    const posts =  await getCollection('blog')
  const [authors, posts] = await Promise.all([
    getCollection('author'),
    getCollection('blog'),
  ]);

    return authors.map((author) => ({
        params: { author: author.id },
        props: {
       blogPosts: posts.filter((post) => post.data.author.id === author.id),
        author: author.data,
     },
    }));

}) satisfies GetStaticPaths;


const { blogPosts, author } = Astro.props;
---

<MainLayout>
  <section class="bg-gray-900">
    <div class="container px-6 py-10 mx-auto">
      <h1 class="text-3xl font-semibold capitalize lg:text-4xl text-white">
        {siteConfig.title}
      </h1>

      <div class="text-md text-gray-400 mb-0 flex flex-row mt-2">
        <Image
          class="rounded-full"
          src={author.avatar}
          alt={author.name}
          width={50}
          height={50}
        />

        <div class="flex flex-col ml-5">
          <a>{author.name}</a>
          <span>Listado de todos mis posts</span>
        </div>
      </div>

      <div class="grid grid-cols-1 gap-8 mt-8 md:mt-16 md:grid-cols-2">
        {blogPosts.map((post) => <TypedBlogPost post={post} />)}
      </div>
    </div>
  </section>
</MainLayout>

<style is:global>
  a {
    @import "tailwindcss";
    @apply text-blue-500 hover:underline;
  }
</style>

```

1. cambios el link `src\pages\posts\[slug].astro`: ` <a href={`/blog/${frontmatter.author.id}`}>{author.data.name}</a>`
