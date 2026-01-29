(Adapters)[https://docs.astro.build/en/guides/integrations-guide/]
(adapterNode)[https://docs.astro.build/en/guides/integrations-guide/node/]

# REST API estatica

## ejemplo de GET

0. crear un get `src\pages\api\get-person.json.ts`:

- se le pone `.json.ts` para que cree un archivo `.json`
- `aget ` abrev para llamar a funcion
- las propiedades que tenemos que enviar de `APIRoute`: `export const GET: APIRoute = async ({ params, request }) =>`
- los valores que enviamos: `const person = { name: "John Doe",  age: 30};`
- como enviar la respuesta para que aparezca como un `.json`: ` return new Response(`
  - los pasamos a a string: `JSON.stringify(person)`
  - un codigo aceptacion web: `status: 200,`
  - enviarlo formato `.json `: `headers: { "Content-Type": "application/json" },`

- Archivo:

```
import type { APIRoute } from "astro";

export const GET: APIRoute = async ({ params, request }) => {
  const person = {
    name: "John Doe",
    age: 30,
  };

  return new Response(JSON.stringify(person), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};
```

## ejemplo de POST conjunto entradas

0. `src\pages\api\posts\index.ts`:
   - `aget` para llamar plantilla
   - igual que el anterios, menos:
     - importamos datos de blog `import { getCollection } from "astro:content";`:`const post = await getCollection("blog");`
     - lo pasamos como string: `JSON.stringify(post)`

- archivo:

```
import type { APIRoute } from "astro";
import { getCollection } from "astro:content";

export const GET: APIRoute = async ({ params, request }) => {
  const post = await getCollection("blog");

  return new Response(JSON.stringify(post), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

```

## ejemplo POST 1 entrada

### adapter nodejs

(adapterNode)[https://docs.astro.build/en/guides/integrations-guide/node/]

0. install, T: npx astro add node
   - decir 3 `Y`
1. `output:'xxx',` en `astro.config.mjs`:
   - server,static
2. determinar si es statica o dinamica `src\pages\api\posts\index.ts`:
   - no que sea estatica,sino dinamica `export const prerender = false;`
   - build app,T:`npm run build`
   - preview,T: `npm run preview`
   - Fichero:
     `export const prerender = false; export const GET: ...`

### query parameters

0. web:`http://localhost:4321/api/posts?slug=first-post`
1. determinar si es statica o dinamica `src\pages\api\posts\index.ts`:

- para crear paginas dinamicas, sino no funciona: `export const prerender = false;`
- lo bajamos antes del return final para que no se ejecute: ` const post = await getCollection("blog");`
- obtener parametro `slug` de la dirrecion:

```
const url = new URL(request.url);
const slug = url.searchParams.get("slug");`
```

- obtenemos los datos del post: `const post = await getEntry("blog", slug);`
- segun contenido obtenemos respuesta:
  - respuesta valida:

  ```
    return new Response(JSON.stringify(post), {
       status: 200,
       headers: { "Content-Type": "application/json" },
     });
  ```

  -respuesta nula:

  ```
      return new Response(JSON.stringify({ error: `Post ${slug} not found` }), {
     status: 400,
     headers: { "Content-Type": "application/json" },
   });
  ```

- FICHERO:

```
import type { APIRoute } from "astro";
import { getCollection, getEntry } from "astro:content";

//no creada de manera statica
export const prerender = false;

export const GET: APIRoute = async ({ params, request }) => {
  const url = new URL(request.url);
  const slug = url.searchParams.get("slug");
  console.log({ slug: slug });

  if (slug) {
    const post = await getEntry("blog", slug);

    if (post) {
      return new Response(JSON.stringify(post), {
        status: 200,
        headers: { "Content-Type": "application/json" },
      });
    }
  }

  return new Response(JSON.stringify({ error: `Post ${slug} not found` }), {
    status: 400,
    headers: { "Content-Type": "application/json" },
  });
  //esta en standby
  const post = await getCollection("blog");
  return new Response(JSON.stringify(post), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

```
