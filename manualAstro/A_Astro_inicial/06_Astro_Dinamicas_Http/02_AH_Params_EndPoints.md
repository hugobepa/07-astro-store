0. web:`http://localhost:4321/api/posts?slug=first-post`
1. web:`http://localhost:4321/api/posts/first-post`
2. determinar si es statica o dinamica `src\pages\api\posts\[slug].ts`:

# SEGMENTO DE RUTAS (GET,POST,PUT,PATCH,DELETE)

## GET

1. crear y rellenar `aget` `src\pages\api\posts\[slug].ts`:
   - trabajar lado servidor o dinamicas: `export const prerender = false;`
   - obtenemos el parametro `slug`:`const { slug } = params;`
   - enviamos respuesta segun resultado:
     - si es amlo el resultado se cambia: `Response(JSON.stringify({ error:`Post ${slug} not found` }),` y ` status: 400,`
   ```
   return new Response(JSON.stringify(post), {
    status: 200,
    headers: { "Content-Type": "application/json" },
   });
   ```

- FICHERO:

```
import type { APIRoute, GetStaticPaths } from "astro";
import { getEntry } from "astro:content";

export const prerender = false;

export const GET: APIRoute = async ({ params, request }) => {
  const { slug } = params;

  const post = await getEntry("blog", slug as any);

  if (!post) {
    return new Response(JSON.stringify({ error: `Post ${slug} not found` }), {
      status: 400,
      headers: { "Content-Type": "application/json" },
    });
  }

  return new Response(JSON.stringify(post), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

```

2. web:`http://localhost:4321/api/posts/first-post`

# Post,Put,Delete (endPoints)

## POST

1. crear y rellenar `aget` `src\pages\api\posts\[slug].ts`:
   - cambiamos `GET` X `POST`: `export const POST: APIRoute`
   - obtener datos :`const body = await request.json();`
   - mostrar datos `...body` y tipo `POST`: `return new Response( JSON.stringify({ method: "POST", ...body}),`

```
export const POST: APIRoute = async ({ params, request }) => {
  const body = await request.json();

  return new Response(
    JSON.stringify({
      method: "POST",
      ...body,
    }),
    {
      status: 200,
      headers: { "Content-Type": "application/json" },
    },
  );
};
```

2. postman: POST: `http://localhost:4321/api/posts/first-post`
   - body -- raw -- json -- ``{"id":100,"name":"john","age":38,"isActive":true}`

## PUT

1. crear y rellenar `aget` `src\pages\api\posts\[slug].ts`:
   - cambiamos `GET` X `PUT`: `export const PUT: APIRoute`
   - obtener datos :`const body = await request.json();`
   - mostrar datos `...body` y tipo `PUT`: `return new Response( JSON.stringify({ method: "PUT", ...body}),`

```
export const PUT: APIRoute = async ({ params, request }) => {
  const body = await request.json();

  return new Response(
    JSON.stringify({
      method: "PUT",
      ...body,
    }),
    {
      status: 200,
      headers: { "Content-Type": "application/json" },
    },
  );
};
```

2. postman: PUT: `http://localhost:4321/api/posts/first-post`
   - body -- raw -- json -- ``{"id":100,"name":"john","age":38,"isActive":true}`

## PATCH

1. crear y rellenar `aget` `src\pages\api\posts\[slug].ts`:
   - cambiamos `GET` X `PATCH`: `export const PATCH: APIRoute`
   - obtener datos :`const body = await request.json();`
   - mostrar datos `...body` y tipo `PATCH`: `return new Response( JSON.stringify({ method: "PATCH", ...body}),`

```
export const PATCH: APIRoute = async ({ params, request }) => {
  const body = await request.json();

  return new Response(
    JSON.stringify({
      method: "PATCH",
      ...body,
    }),
    {
      status: 200,
      headers: { "Content-Type": "application/json" },
    },
  );
};
```

2. postman: PATCH: `http://localhost:4321/api/posts/first-post`
   - body -- raw -- json -- ``{"id":100,"name":"john","age":38,"isActive":true}`

## DELETE

1. crear y rellenar `aget` `src\pages\api\posts\[slug].ts`:
   - cambiamos `GET` X `DELETE`: `export const DELETE: APIRoute`
   - obtener el post a eliminar :` const { slug } = params;`
   - mostrar datos `slug` y tipo `DELETE`: `return new Response( JSON.stringify({ method: "DELETE", slug: slug,}),`

```
export const DELETE: APIRoute = async ({ params, request }) => {
  const { slug } = params;

  return new Response(
    JSON.stringify({
      method: "DELETE",
      slug: slug,
    }),
    {
      status: 200,
      headers: { "Content-Type": "application/json" },
    },
  );
};
```

2. postman: DELETE: `http://localhost:4321/api/posts/first-post`
   - body -- raw -- json -- ``{"id":100,"name":"john","age":38,"isActive":true}`
