# CREAR FILTRO CONENIDO

0. `src\content\config.ts`:
   - booleano para visionar las entradas :`isDraft: z.boolean().default(false),`

- ARCHIVO:

```
import { defineCollection, reference, z } from "astro:content";

const blogCollection = defineCollection({
  type: "content",
  schema: ({ image }) =>
    z.object({
        ....

      //BOOLEAN
      isDraft: z.boolean().default(false),
    }),
});
```

1. `src\pages\index.astro`:
   - importamos propiedad y le damos una orden:`(post) => post.data.isDraft === false`

```
const blogPost = await getCollection('blog', (post) => post.data.isDraft === false);

```

2. `src\content\blog\post-05.md`:
   -añadimos propiedad para que no sea en `.md,.mdx`:`isDraft: true`

```
---
....
isDraft: true //false
---
```

# LISTADO DE AUTORES

(profileCardTemplate)[https://www.creative-tim.com/twcomponents/component/creating-a-simple-profile-card-with-tailwind-css]

0. creamos y rellenamos archivo `src\pages\authors\index.astro`:
   - rellenamos con loa hay que hay dentro `div` :`[https://www.creative-tim.com/twcomponents/component/creating-a-simple-profile-card-with-tailwind-css`

- archivo:

```
---
import MainLayout from "@layouts/MainLayout.astro";
import { siteConfig } from "src/config/site-config";


---

<MainLayout title="Listado de Autores">

  <section class="container px-6 py-10 mx-auto">
    <h1 class="text-3xl font-semibold capitalize lg:text-4xl text-white">{siteConfig.title}</h1>
 </section>

 <secttion class="grid grid-cols-1 sm:grid-cols-2 gap-2">
    <div class="max-w-lg mx-auto my-10 bg-white rounded-lg shadow-md p-5">
    <img class="w-32 h-32 rounded-full mx-auto" src="https://picsum.photos/200" alt="Profile picture">
    <h2 class="text-center text-2xl font-semibold mt-3">John Doe</h2>
    <p class="text-center text-gray-600 mt-1">Software Engineer</p>
    <div class="flex justify-center mt-5">
      <a href="#" class="text-blue-500 hover:text-blue-700 mx-3">Twitter</a>
      <a href="#" class="text-blue-500 hover:text-blue-700 mx-3">LinkedIn</a>
      <a href="#" class="text-blue-500 hover:text-blue-700 mx-3">GitHub</a>
    </div>
    <div class="mt-5">
      <h3 class="text-xl font-semibold">Bio</h3>
      <p class="text-gray-600 mt-2">John is a software engineer with over 10 years of experience in developing web and mobile applications. He is skilled in JavaScript, React, and Node.js.</p>

        <div class="flex justify-end">
            <a class="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-700">leer mas</a>
        </div>

    </div>
  </div>

 </secttion>

</MainLayout>
```

1. remplazar archivos `.yml` en `src\content\author`.
   - estos `.yml`: tienen estas propiedades nuevas: `twitter,linkedIn,github,bio,subtitle:` (hay que implementarlas)

## resolucion tarea

0. modificamos `src\content\config.ts`:
   - añadimos esta propiedades nuevas: `twitter,linkedIn,github,bio,subtitle:`

- ARCHIVO:

```
const authorCollection = defineCollection({
  type: "data",
  schema: ({ image }) =>
    z.object({
      name: z.string(),
      avatar: image(),
      twitter: z.string(),
      linkedIn: z.string(),
      github: z.string(),
      bio: z.string(),
      subtitle: z.string(),
    }),
});
```

1. creamos y rellenamos el component `src\components\AuthorCard.astro`:
   - creamos interfaces con la proopiedades de `auhor`: `interface Props { author: CollectionEntry<'author'>;}`
   - importamos la propiedad de `author`: `const { author } = Astro.props;`
   - descomponemos `author` para trabajar con sus propiedades: `const { avatar, bio, github, name, linkedIn, subtitle, twitter } = author.data;`
   - trabajamos con el componete `<Image` y `import { Image } from 'astro:assets';` y le pasamos el datos de la imagen: `src={avatar}`
   - ponemos la propiedades en los campos `<a href={github}`
   - para X o twitter:``href={`https://x.com/${twitter}`}``
   - dirigir a la pagina del autor, mediante `${author.id}` : `<a href={`/blog/${author.id}`}`

- ARCHIVO:

```
---
import { Image } from 'astro:assets';
import type { CollectionEntry } from 'astro:content';

interface Props {
  author: CollectionEntry<'author'>;
}

const { author } = Astro.props;

const { avatar, bio, github, name, linkedIn, subtitle, twitter } = author.data;
---

<div class="max-w-lg mx-auto bg-white rounded-lg shadow-md p-5">
  <Image
    src={avatar}
    width="200"
    height="200"
    class="w-32 h-32 rounded-full mx-auto"
    alt="Profile picture"
  />
  <h2 class="text-center text-2xl font-semibold mt-3">{name}</h2>
  <p class="text-center text-gray-600 mt-1">{subtitle}</p>
  <div class="flex justify-center mt-5">
    <a
      href={`https://x.com/${twitter}`}
      class="text-blue-500 hover:text-blue-700 mx-3">Twitter</a
    >
    <a href={linkedIn} class="text-blue-500 hover:text-blue-700 mx-3"
      >LinkedIn</a
    >
    <a href={github} class="text-blue-500 hover:text-blue-700 mx-3">GitHub</a>
  </div>
  <div class="mt-5">
    <h3 class="text-xl font-semibold">Bio</h3>
    <p class="text-gray-600 mt-2">
      {bio}
    </p>
    <div class="flex justify-end">
      <a href={`/blog/${author.id}`} class="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-700">leer mas</a>
    </div>
  </div>
</div>
```

2. importamos datos y los pasamos al component `src\pages\authors\index.astro`:
   - importamos datos de `md, mdx` con `import { getCollection } from "astro:content";` : `const authors = await getCollection('author');`
   - pasamos los datos al componente `import AuthorCard from "@components/AuthorCard.astro";` : `{authors.map((author) => <AuthorCard author={author} />)}`

- ARCHIVO:

```
---
import AuthorCard from "@components/AuthorCard.astro";
import MainLayout from "@layouts/MainLayout.astro";
import { getCollection } from "astro:content";

const authors = await getCollection('author');
---

<MainLayout title="Listado de Autores">



 <secttion class="grid grid-cols-1 sm:grid-cols-2 gap-2">
     <!-- AuthorCard -->
    {authors.map((author) => <AuthorCard author={author} />)}
 </secttion>

</MainLayout>
```
