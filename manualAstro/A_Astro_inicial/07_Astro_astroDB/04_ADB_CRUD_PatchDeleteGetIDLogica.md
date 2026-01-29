(drizzleUtils)[https://docs.astro.build/es/guides/astro-db/#drizzle-utilities]

`http://localhost:4321/api/clients/1` 3. PROBAR CON POSTMAN (PATCH,DELETE,GET): `http://localhost:4321/api/clients/1`
creamos `src\pages\api\clients\[clientId].ts`:

## patch

- patch:
  - eliminamos todo lo que hay dentro funcion menos ``const clientId = params.clientId;`
  - copiamos el interior de la funcion post de `index.ts`
  - creamos funcion para actualizar registro especifico por `id`:
    - convertimos `clientID` en numero: `+clientId`
    - y modificamos `const clientId` venir undenfined: `const clientId = params.clientId ?? "";`
      `const result = await db.update(Clients).set(body).where(eq(Clients.id, +clientId));`
  - traemos el registro actualizado:
    `const updatedClient = await db.select().from(Clients).where(eq(Clients.id, +clientId));`
  - pasamos el registro actualizado al message: `return new Response(JSON.stringify(updatedClient.at(0)),`
    - eliminar cochetes en salida `.at(0)`

```
//PATCH
export const PATCH: APIRoute = async ({ params, request }) => {
  const clientId = params.clientId ?? "";

  try {
    const { id, ...body } = await request.json();

    const result = await db
      .update(Clients)
      .set(body)
      .where(eq(Clients.id, +clientId));

    const updatedClient = await db
      .select()
      .from(Clients)
      .where(eq(Clients.id, +clientId));

    return new Response(
      JSON.stringify(
        updatedClient.at(0),
      ),
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

- POSTMAN: PATCH `http://localhost:4321/api/clients/1` body-raw-json: `{"name":"hugo"}`

## delete

- funcion cliente por id y extraemos `rowsAffected`:
  - el `id` lo pasamos a numero `+clientId`
  - y si viene algun indefinido `const clientId = params.clientId ?? "";`
    `const { rowsAffected } = await db.delete(Clients).where(eq(Clients.id, +clientId));`
- mensajes:
  - Si `rowsAffected`sup `0` exito eliminacion `JSON.stringify({ msg: "Client deleted successfully" })`
  - Sino mensaje de error ``  JSON.stringify({ msg: `No client found with ID ${clientId}` }),``

- delete:

```
//DELETE
export const DELETE: APIRoute = async ({ params, request }) => {
  const clientId = params.clientId ?? "";

  const { rowsAffected } = await db
    .delete(Clients)
    .where(eq(Clients.id, +clientId));

  if (rowsAffected > 0) {
    return new Response(
      JSON.stringify({ msg: "Client deleted successfully" }),
      {
        status: 200,
        headers: { "Content-Type": "application/json" },
      },
    );
  }

  return new Response(
    JSON.stringify({ msg: `No client found with ID ${clientId}` }),
    {
      status: 404,
      headers: { "Content-Type": "application/json" },
    },
  );
};

```

- POSTMAN: DELETE `http://localhost:4321/api/clients/1`

## get Id

- obtenemos el cliente por la `id`:
  - convertimos id es numero: `+clientId`
  - eliminamos valor indefinido: `const clientId = params.clientId ?? "";`
    `const rows = await db.select().from(Clients).where(eq(Clients.id, +clientId));`
  - convertimos el `rows` en un elemento quitandole corchetes: `const client = rows.at(0);`
  - convertimos `client` en un boleano `!client`:
    - si da `false`: ``JSON.stringify({ error: `No client found with ID ${clientId}` })``
    - si da `true`: `JSON.stringify(client)`

- GET ID:

```
export const GET: APIRoute = async ({ params }) => {
  const clientId = params.clientId ?? "";

  const rows = await db.select().from(Clients).where(eq(Clients.id, +clientId));

  const client = rows.at(0);

  if (!client) {
    return new Response(
      JSON.stringify({ error: `No client found with ID ${clientId}` }),
      { status: 404, headers: { "Content-Type": "application/json" } },
    );
  }

  return new Response(JSON.stringify(client), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};
```

- POSTMAN: GET `http://localhost:4321/api/clients/1`

- FICHERO:

```
import type { APIRoute } from "astro";
import { Clients, db, eq } from "astro:db";

export const prerender = false;

//GET ID
export const GET: APIRoute = async ({ params }) => {
  const clientId = params.clientId ?? "";

  const rows = await db.select().from(Clients).where(eq(Clients.id, +clientId));

  const client = rows.at(0);

  if (!client) {
    return new Response(
      JSON.stringify({ error: `No client found with ID ${clientId}` }),
      { status: 404, headers: { "Content-Type": "application/json" } },
    );
  }

  return new Response(JSON.stringify(client), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

//PATCH
export const PATCH: APIRoute = async ({ params, request }) => {
  const clientId = params.clientId ?? "";

  try {
    const { id, ...body } = await request.json();

    const result = await db
      .update(Clients)
      .set(body)
      .where(eq(Clients.id, +clientId));

    const updatedClient = await db
      .select()
      .from(Clients)
      .where(eq(Clients.id, +clientId));

    return new Response(JSON.stringify(updatedClient.at(0)), {
      status: 201,
      headers: { "Content-Type": "application/json" },
    });
  } catch (error) {
    return new Response(JSON.stringify({ error: "No body found" }), {
      status: 401,
      headers: { "Content-Type": "application/json" },
    });
  }
};
//DELETE
export const DELETE: APIRoute = async ({ params, request }) => {
  const clientId = params.clientId ?? "";

  const { rowsAffected } = await db
    .delete(Clients)
    .where(eq(Clients.id, +clientId));

  if (rowsAffected > 0) {
    return new Response(
      JSON.stringify({ msg: "Client deleted successfully" }),
      {
        status: 200,
        headers: { "Content-Type": "application/json" },
      },
    );
  }

  return new Response(
    JSON.stringify({ msg: `No client found with ID ${clientId}` }),
    {
      status: 404,
      headers: { "Content-Type": "application/json" },
    },
  );
};

```
