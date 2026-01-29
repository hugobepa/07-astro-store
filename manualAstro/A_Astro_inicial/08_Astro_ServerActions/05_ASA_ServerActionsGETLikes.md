(adapterVue)[https://docs.astro.build/es/guides/integrations-guide/vue/]
(conocimientosVue)[https://es.vuejs.org/v2/guide/components]
(serverActions)[https://docs.astro.build/en/guides/actions/]

# SERVER ACTIONS LIKES

## GET likes

0. creamos la accion `src/actions/posts/get-post-likes.action.ts` y la rellenamos `actionserver`:
   - crear siempre dentro de `src/actions/`.
   - obtenemos datos y desestructuramos:
     `const [post] = await db.select().from(Posts).where(eq(Posts.id, postId));`
   - trabajamos los datos:
     - si falla enviamos un fake. ` return { likes: 0}`
     - sino enviamos dato: ` return { likes: post.likes,`

- FICHERO:

```
import { defineAction } from "astro:actions";
import { db, eq, Posts } from "astro:db";
import { z } from "astro:schema";

export const getPostLikes = defineAction({
  input: z.string(),
  handler: async (postId) => {
    const [post] = await db.select().from(Posts).where(eq(Posts.id, postId));

    if (!post) {
      return { likes: 0 };
      // return { likes: 0, exists: false };
    }

    return {
      likes: post.likes,
      //exists: true,
    };
  },
});
```

1. lo definimos en el index `src/actions/index.ts`:

-FICHERO:

```
import { getGreeting } from "./greetings/get-greeting.action";
import { getPostLikes } from "./posts/get-post-likes.action";

export const server = { getGreeting, getPostLikes };
```

2. trabajar con el fichero `src\components\likes\LikeCounterAction.vue`:
   - extrameos los datos del SA: `const { data, error } = await actions.getPostLikes(props.postId);`
     - trabajamos con propiedad `Postid`:
       `interface Props { postId: string;}`
       `const props = defineProps<Props>();`
   - segun resultado:
     - error enviamos alerta
     - correcto enviamos datos:`likeCount.value = data.likes;`

- FICHERO:

```
<script setup lang="ts">
import { actions } from "astro:actions";

interface Props {
  postId: string;
}

const props = defineProps<Props>();

const getCurrentLikes = async () => {

   const { data, error } = await actions.getPostLikes(props.postId);
  if (error) {
    return alert(
      `There was an error fetching the like count,${error.message} `,
    );
  }

  likeCount.value = data.likes;
  isLoading.value = false;


```

3. importamos eel componente a la pagina`src\pages\blog\[...slug].astro`:
   - importamos componente : `<LikeCounterAction postId={post.id} client:only="vue"/>`
     - importamos propiedad `id`:

     ```
         type Props = CollectionEntry<'blog'>;
         const post = Astro.props;
     ```

     - le damos propiedad para poder trabajar `client:only="vue"`

- FICHERO:

```


type Props = CollectionEntry<'blog'>;

const post = Astro.props;

<BlogPost {...post.data}>
	<div style={{height:'50px',}}>
		<!-- <LikeCounter postId={post.id} client:only="vue"/> -->
		<LikeCounterAction postId={post.id} client:only="vue"/>
```
