(astroDB)[https://docs.astro.build/en/guides/astro-db/]
`http://localhost:4321/api/posts/first-post`
`http://localhost:4321/api/clients` - body -- raw -- json -- ``{"name":"john","age":38,"isActive":true}`

`http://localhost:4321/api/clients/1`
astroDB `.astro\content.db`

# CRUD creacion basica basico

0. modificamos fichero `src/pages/api/clients/index.ts`:

- POST:
  - creamos un `try-catch`
  - eliminamos `const body` y creamos nueva para obtener datos del body. Separamos id y las otras propiedades:
    ` const { id, ...body } = await request.json();`
  - creamos funcion de ingresar en DB y extraemos el id que se creo:
    `const { lastInsertRowid } = await db.insert(Clients).values(body);`
  - mostramos los datos ingresados por mensaje:
    -tratamos `lastInsertRowid` conv numero decimal, sin undefined y en string al final: `+lastInsertRowid!.toString()`
    `return new Response( JSON.stringify({  id: +lastInsertRowid!.toString(), ...body,}),`
  - creamos mensaje de error: `JSON.stringify({ error: "No body found" }`
  - definimos tipo de mensaje `status: 201/401, headers: { "Content-Type": "application/json" },`

```
//POST
export const POST: APIRoute = async ({ params, request }) => {
  try {
    const { id, ...body } = await request.json();

    const { lastInsertRowid } = await db.insert(Clients).values(body);

    return new Response(
      JSON.stringify({
        id: +lastInsertRowid!.toString(),
        ...body,
      }),
      {
        status: 201,
        headers: { "Content-Type": "application/json" },
      },
    );
  } catch (error) {
    return new Response(JSON.stringify({ error: "No body found" }), {
      status: 401,
      headers: { "Content-Type": "application/json" },
    });
  }
};
```

- GET:
  - eliminamos const body anterior
  - creamos funcion para obtener usuarios de la tabla:
    `const users = await db.select().from(Clients);`
  - mostramos los usuarios por mensaje: `JSON.stringify(users)`

```
export const GET: APIRoute = async ({ params, request }) => {
  const users = await db.select().from(Clients);
  return new Response(JSON.stringify(users), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};
```

1. PROBAR CON POSTMAN (GET,POST): `http://localhost:4321/api/clients`
   - body -- raw -- json -- ``{"name":"john","age":38,"isActive":true}`

- FICHERO GENERAL:

```
import type { APIRoute } from "astro";
import { Clients, db } from "astro:db";

export const prerender = false;
//GET
export const GET: APIRoute = async ({ params, request }) => {
  const users = await db.select().from(Clients);
  return new Response(JSON.stringify(users), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

//POST
export const POST: APIRoute = async ({ params, request }) => {
  try {
    const { id, ...body } = await request.json();

    const { lastInsertRowid } = await db.insert(Clients).values(body);

    return new Response(
      JSON.stringify({
        id: +lastInsertRowid!.toString(),
        ...body,
      }),
      {
        status: 201,
        headers: { "Content-Type": "application/json" },
      },
    );
  } catch (error) {
    return new Response(JSON.stringify({ error: "No body found" }), {
      status: 401,
      headers: { "Content-Type": "application/json" },
    });
  }
};

```
