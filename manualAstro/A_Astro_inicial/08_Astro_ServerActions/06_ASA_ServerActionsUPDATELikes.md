(adapterVue)[https://docs.astro.build/es/guides/integrations-guide/vue/]
(conocimientosVue)[https://es.vuejs.org/v2/guide/components]
(serverActions)[https://docs.astro.build/en/guides/actions/]

# SERVER ACTIONS LIKES

## UPDATE likes

0. modificamos accion anterior para tener el `exists` en `src\actions\posts\get-post-likes.action.ts`.
   - añadimos `exists` y le damos valor:
     `return { likes: 0, exists: false };`
     `return {likes: post.likes,exists: true,};`

- FICHERO:

  ```
    if (!post) {
      return { likes: 0, exists: false };
    }

    return {
      likes: post.likes,
      exists: true,
    };
  ```

1. creamos la accion update y rellenamos `aactionserver` en `src\actions\posts\update-likes.action.ts`
   - declaramos las variables:`input: z.object({postId: z.string(),increment: z.number(),}),`
   - trabajamos con las variables `handler: async ({ postId, increment }) => {`:
     - devolvemos true : `return true;`
     - obtenemos los likes llamando directamente a `Post` en `.astro\content.db`:
       ` const rows = await db.select().from(Posts).where(eq(Posts.id, postId));`
       - trabajamos con los datos extraidos:
         - extraemos informacion valida: `const row = rows[0];`
         - lo pasamos a booelan para saber si existe: `const exists = Boolean(row);`
         - obtenemos los likes: `const likes = typeof row?.likes === "number" ? row.likes : 0;`
       - trabajamos con el `exists` si esta vacio:
         - creamos fake post: ` const newPost = {id: postId,title: "Post not found",likes: 0,};`
         - insertamos DB `Post` en `.astro\content.db`: ` await db.insert(Posts).values(newPost);`

       ```
         if (!exists) {
             const newPost = {
               id: postId,
               title: "Post not found",
               likes: 0,
             };

             await db.insert(Posts).values(newPost);
           }
       ```

       - Si viene algo actualizamos con `likes` en db `Post` en `.astro\content.db`:
         `await db.update(Posts).set({likes: likes + increment,}.where(eq(Posts.id, postId));`

- FICHERO:

```
export const updatePostLikes = defineAction({
  accept: "json",
  input: z.object({
    postId: z.string(),
    increment: z.number(),
  }),
  handler: async ({ postId, increment }) => {
    try {
      // Read current likes directly from the database instead of calling another action
      const rows = await db
        .select()
        .from(Posts)
        .where(eq(Posts.id, postId));

      const row = rows[0];
      const exists = Boolean(row);
      const likes = typeof row?.likes === "number" ? row.likes : 0;

      if (!exists) {
        await db.insert(Posts).values({
          id: postId,
          title: "Post not found",
          likes: 0,
        });
      }

      await db
        .update(Posts)
        .set({
          likes: likes + increment,
        })
        .where(eq(Posts.id, postId));

      return true;
    } catch (err) {
      console.error("updatePostLikes error:", err);
      return false;
    }
  },
});


```

2. importamos la accion `src/actions/index.ts`:

- FICHERO:

```
import { getGreeting } from './greetings/get-greeting.action';
import { getPostLikes } from './posts/get-post-likes.action';
import { updatePostLikes } from './posts/update-likes.action';

export const server = {
  getGreeting,

  // posts
  getPostLikes,
  updatePostLikes,
};
```

3. trabajamos con action `src\components\likes\LikeCounterAction.vue`:

- poner async `debounce(async() => {`
- importamos la accion:
  ` await actions.updatePostLikes({postId: props.postId,increment: likeClicks.value,});`
  - trabajamos con `postId`: `interface Props {postId: string;}` y `const props = defineProps<Props>();`
  - y `const likeClicks = ref(0);`
- ARCHIVO:

```
import { actions } from "astro:actions";
import { ref, watch } from "vue";
interface Props {
  postId: string;
}

const props = defineProps<Props>();
const likeClicks = ref(0);
debounce(async() => {
    await actions.updatePostLikes({
      postId: props.postId,
      increment: likeClicks.value,
    });
```
