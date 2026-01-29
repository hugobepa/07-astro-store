(astroDB)[https://docs.astro.build/en/guides/astro-db/]
`http://localhost:4321/api/posts/first-post`
`http://localhost:4321/api/clients` - body -- raw -- json -- ``{"name":"john","age":38,"isActive":true}`

`http://localhost:4321/api/clients/1`
astroDB `.astro\content.db`

# CRUD creacion basica basico

0. creamos y rellenamos `aref` `src/pages/api/clients/index.ts`:

- trabajar con servidor `export const prerender = false;`
- creamos el `GET`:
  - creamos `const body = { method: "GET" };`
  - los llamamos `JSON.stringify(body)`
  - le dar formato salida ` headers: { "Content-Type": "application/json" },`

```
import type { APIRoute } from "astro";

export const prerender = false;
//GET
export const GET: APIRoute = async ({ params, request }) => {

  return new Response(JSON.stringify(body), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};
```

- creamos el `POST`:
  - copiamos el la funcion `GET` y cambiamos `GET` X `POST`

```
//POST
export const POST: APIRoute = async ({ params, request }) => {
  const body = { method: "POST" };
  return new Response(JSON.stringify(body), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};
```

- ARCHIVO:

```
import type { APIRoute } from "astro";

export const prerender = false;
//GET
export const GET: APIRoute = async ({ params, request }) => {
  const body = { method: "GET" };
  return new Response(JSON.stringify(body), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

//POST
export const POST: APIRoute = async ({ params, request }) => {
  const body = { method: "POST" };
  return new Response(JSON.stringify(body), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};
```

1. PROBAR CON POSTMAN (GET,POST): `http://localhost:4321/api/clients`

2. creamos `src\pages\api\clients\[clientId].ts`:

- copiamos del `index.ts` y cambiamos x `PATCH, DELETE` y creamos un `GET` para getID
- obtenemos `id` cliente y los usamos. Ponemos en el metodo: `GET,PATCH,DELETE`:
  `const clientId = params.clientId;  const body = { method: "GET", clientId: clientId };`
- convertimos el id en numero y permitimos undefined: `clientId: +clientId!`

```
import type { APIRoute } from "astro";

export const prerender = false;

//GET ID
export const GET: APIRoute = async ({ params, request }) => {
  const clientId = params.clientId;

  const body = { method: "GET", clientId: clientId: +clientId! };
  return new Response(JSON.stringify(body), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

//PATCH
export const PATCH: APIRoute = async ({ params, request }) => {
  const clientId = params.clientId;

  const body = { method: "PATCH", clientId: clientId: +clientId! };
  return new Response(JSON.stringify(body), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};

//DELETE
export const DELETE: APIRoute = async ({ params, request }) => {
  const clientId = params.clientId;
  const body = { method: "DELETE", clientId: clientId: +clientId! };
  return new Response(JSON.stringify(body), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
};


```

3. PROBAR CON POSTMAN (PATCH,DELETE,GET): `http://localhost:4321/api/clients/1`
