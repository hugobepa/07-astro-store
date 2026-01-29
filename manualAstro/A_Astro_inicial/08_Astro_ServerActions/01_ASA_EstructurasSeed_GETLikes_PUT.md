# ESTRUCTURAS

0. creamos un nuevo esquema tabla para los likes `db\config.ts`:
   - creamos el esquema BBDD: `const Posts = defineTable({...});`
   - exportamos el esquema para crearlo: `export default defineDb({tables: { Clients, Posts },});`
     `

- FICHERO:

```
const Posts = defineTable({
  columns: {
    id: column.text({ primaryKey: true }),
    title: column.text(),
    likes: column.number(),
  },
});

export default defineDb({
  tables: { Clients, Posts },
});

```

1. bajamos el servidor y lo volvemos a subir: `npm run dev`

2. crear semilla para la nueva tabla `db\seed.ts`:
   - obtenemos los posts que hay:`const posts = await getCollection('blog')`
   - insertamos valores en la nueva tabla:`await db.insert(Posts).values( posts.map(post => ({`
     - valores obtenidos: ` id: post.id, title: post.data.title,`
     - nuevo valor: `likes: Math.floor(Math.random() * 100),`

- FICHERO:

```
    const posts = await getCollection('blog')

  await db.insert(Posts).values(
    posts.map(post => ({
      id: post.id,
      title: post.data.title,
      likes: Math.floor(Math.random() * 100),
    }))
```

3. bajamos el servidor y lo volvemos a subir: `npm run dev`

# GETS Likes

0. creamos un nuevo API para los likes con `aget` en `src\pages\api\likes\[id].ts`:

- archivo dinamico del servidor. `export const prerender = false;`
- obtenemos id de params y configuramos por si viene undefined: ` const postId = params.id ?? "";`
- obtenemos el Post segun el Id: ` const posts = await db.select().from(Posts).where(eq(Posts.id, postId));`
- creamos un post fake sino viene ninguno y enviamos x mensaje: `if (posts.length === 0) { ...}`
- si recibimos Post lo envimos x mensaje: `return new Response(JSON.stringify(posts.at(0)),`

- FICHERO:

```
import type { APIRoute } from "astro";
import { db, Posts, eq } from "astro:db";

export const prerender = false;

export const GET: APIRoute = async ({ params, request }) => {
  const postId = params.id ?? "";

  const posts = await db.select().from(Posts).where(eq(Posts.id, postId));
  if (posts.length === 0) {
    const post = {
      id: postId,
      title: `Post not found ${postId}`,
      likes: 0,
    };

    return new Response(JSON.stringify(post), {
      status: 200,
      headers: {
        "Content-Type": "application/json",
      },
    });
  }

  return new Response(JSON.stringify(posts.at(0)), {
    status: 200,
    headers: {
      "Content-Type": "application/json",
    },
  });
};

```

1. POSTMAN: GET `http://localhost:4321/api/posts/likes/first-post`

# PUT

0. creamos api `aget` en `src\pages\api\likes\[id].ts`:

- obtenemos `id` de params: `const postId = params.id ?? "";`
- obtenemos los posts de DB: ` const posts = await db.select().from(Posts).where(eq(Posts.id, postId));`:
  - cogemos el primer post si viene y no indefinido: `const post = posts.at(0)!;`
  - si no existe los creamos: `if (posts.length === 0) {const newPost = {...}`:
    - preparamos insertamos en la DB:` await db.insert(Posts).values(newPost);`
    - subimos a la DB: `posts.push(newPost);`
- cogemos todos los likes que tenga sino 0 :`const { likes = 0 } = await request.json();`
  - les sumamos los nuevos likes: `post.likes = post.likes + likes;`
- likes asumados actualizamos el post en DB: `await db.update(Posts).set(post).where(eq(Posts.id, postId));`
- ARCHIVO:

```
export const PUT: APIRoute = async ({ params, request }) => {
  const postId = params.id ?? "";

  const posts = await db.select().from(Posts).where(eq(Posts.id, postId));
  const { likes = 0 } = await request.json();

  if (posts.length === 0) {
    const newPost = {
      id: postId,
      title: `Post not found ${postId}`,
      likes: 0,
    };

    await db.insert(Posts).values(newPost);

    posts.push(newPost);
  }

  const post = posts.at(0)!;
  post.likes = post.likes + likes;

  await db.update(Posts).set(post).where(eq(Posts.id, postId));

  return new Response("OK!", { status: 200 });
};
```
