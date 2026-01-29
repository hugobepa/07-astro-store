(adapterVue)[https://docs.astro.build/es/guides/integrations-guide/vue/]
(conocimientosVue)[https://es.vuejs.org/v2/guide/components]

# COMPONENT VUE

## config Inicial

0. creamos elemento de `.vue` con estructura basica y css `src\components\likes\LikeCounter.vue`:
   - estruct basica `template,script,style`
   - creamos una funcion al clikcar botton:
     `<script setup lang="ts">const likePost = () => {console.log("+1 Post liked!");};`
   - usamos funcion: ` <button @click="likePost">Like Counter</button>`

- ARCHIVO:

```
<template>

</template>

<script setup lang="ts">
const likePost = () => {
  console.log("+1 Post liked!");
};
</script>

<style scoped>
.. estilo css
</style>

```

1. importamos el component `src\pages\blog\[...slug].astro`:

- `import LikeCounter from '@components/likes/LikeCounter.vue';`: `<LikeCounter/>`
- solo lado cliente y especificamos que es de vue: `<LikeCounter client:only="vue"/>`
- incrustamos el componente entre div para evitar efecto visusal

```
---
import BlogPost from '../../layouts/BlogPost.astro';
import LikeCounter from '@components/likes/LikeCounter.vue';

...
---

<BlogPost {...post.data}>
    <div style={{height:'50px',}}>
		<LikeCounter client:only="vue"/>
	</div>
	<Content />
</BlogPost>

```

2. importamos adaptador,T: `npx astro add vue`

## Vue - Props y cant Likes

0. identifacamos el componenet parar le valores valor `src\pages\blog\[...slug].astro`:
   `<LikeCounter postId={post.id} client:only="vue"/>`
1. añadimos props y trabajamos con ellas `src\components\likes\LikeCounter.vue`:
   - creamos y definimos props:

   ```
   interface Props {
   postId: string;
   }

   const props = defineProps<Props>();
   ```

   - creo elementos reactivos para poder trabajar

   ```
    import { ref } from "vue";

    const likeCount = ref(0);
    const likeClicks = ref(0);
    const isLoading = ref(true);
   ```

   - creem squelton per si s'estan el component:`<div v-if="isLoading">Loading...</div>`
   - sino esta carregan mostrem :`<button v-else @click="likePost">Like Counter</button>`

   - obtener el numero de `likes`:

   ```
    const getCurrentLikes = async () => {
   const response = await fetch(`/api/posts/likes/${props.postId}`);
   if (!response.ok) return;
   const data = await response.json();
   likeCount.value = data.likes;
   isLoading.value = false;
    };

    getCurrentLikes();
    </script>
   ```

   - importamos los datos obtenidos para trabajar

   ```
       button v-else-if="likeCount === 0" @click="likePost">Like this post</button>
   <button v-else @click="likePost">
   Like Counter
   <span>{{ likeCount }}</span>
   </button>
   ```

- ARCHIVO:

```
<template>
  <div v-if="isLoading">Loading...</div>

  <button v-else-if="likeCount === 0" @click="likePost">Like this post</button>
  <button v-else @click="likePost">
    Like Counter
    <span>{{ likeCount }}</span>
  </button>
</template>

<script setup lang="ts">
import { ref } from "vue";

interface Props {
  postId: string;
}

const props = defineProps<Props>();

const likeCount = ref(0);
const likeClicks = ref(0);
const isLoading = ref(true);
const likePost = () => {
  console.log("+1 Post liked!");
};

const getCurrentLikes = async () => {
  const response = await fetch(`/api/posts/likes/${props.postId}`);
  if (!response.ok) return;
  const data = await response.json();

  likeCount.value = data.likes;
  isLoading.value = false;
};

getCurrentLikes();
</script>

<style scoped>... css</style>
```
