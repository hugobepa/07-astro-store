(adapterVue)[https://docs.astro.build/es/guides/integrations-guide/vue/]
(conocimientosVue)[https://es.vuejs.org/v2/guide/components]
(confetti)[https://www.npmjs.com/package/canvas-confetti]
(issuesAstro)[https://github.com/withastro/astro/issues]
(debounce)[https://www.npmjs.com/package/lodash.debounce]

# COMPONENT VUE

## Vue incrementar localmente

0. install canvasconfetti,P: `npm i canvas-confetti`
1. install complemento confetti,P:`npm i --save-dev @types/canvas-confetti`

2. creamos elemento de `.vue` con estructura basica y css `src\components\likes\LikeCounter.vue`:

- se incrementa el valor del boton,likeClicks y confetti a cada click:

```
</button>
  {{ likeClicks > 0 ? `You have liked this post ${likeClicks} times.` : "" }}
</template>

import confetti from "canvas-confetti";

const likePost = () => {
  likeCount.value++;
   likeClicks.value++;
  confetti({
    particleCount: 100,
    spread: 70,
    origin: {
      x: Math.random(),
      y: Math.random() - 0.2,
    },
  });
```

- archivo:

```
<template>
  <div v-if="isLoading">Loading...</div>

  <button v-else-if="likeCount === 0" @click="likePost">Like this post</button>
  <button v-else @click="likePost">
    Like Counter
    <span>{{ likeCount }}</span>
  </button>
  {{ likeClicks > 0 ? `You have liked this post ${likeClicks} times.` : "" }}
</template>

<script setup lang="ts">
import { ref } from "vue";
import confetti from "canvas-confetti";

interface Props {
  postId: string;
}

const props = defineProps<Props>();

const likeCount = ref(0);
const likeClicks = ref(0);
const isLoading = ref(true);
const likePost = () => {
  likeCount.value++;
  likeClicks.value++;
  confetti({
    particleCount: 100,
    spread: 70,
    origin: {
      x: Math.random(),
      y: Math.random() - 0.2,
    },
  });
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

<style scoped>
...css
</style>

```

## actualizar cantidad BBDD

2. creamos elemento de `.vue` con estructura basica y css `src\components\likes\LikeCounter.vue`:

- tipo useffect : `watch watch(likeCount, () => {`
- afecta a la BBDD:``fetch(`/api/posts/likes/${props.postId}`, {method: "PUT",``

```
import { ref, watch } from "vue";

watch(likeCount, () => {
  fetch(`/api/posts/likes/${props.postId}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ likes: likeClicks.value }),
  });
  likeClicks.value = 0;
});
```

## Lodash Debounce

(debounce)[https://www.npmjs.com/package/lodash.debounce]

0. install lodash.debounce,T:npm i lodash.debounce
1. install aux typeScript: npm i --save-dev @types/lodash.debounce

2. creamos elemento de `.vue` con estructura basica y css `src\components\likes\LikeCounter.vue`:

- envolver la funcion que nos interesa, y le damos un timepoe en milesimas.

```
import debounce from 'lodash.debounce';

watch(
  likeCount,
  debounce(() => {
   ....
  }, 500),
);

```

- archivo:

```
<template>
  <div v-if="isLoading">Loading...</div>

  <button v-else-if="likeCount === 0" @click="likePost">Like this post</button>
  <button v-else @click="likePost">
    Like Counter
    <span>{{ likeCount }}</span>
  </button>
  <!-- {{ likeClicks > 0 ? `You have liked this post ${likeClicks} times.` : "" }} -->
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import confetti from "canvas-confetti";
import { like } from "astro:db";
import debounce from "lodash.debounce";

interface Props {
  postId: string;
}

const props = defineProps<Props>();

const likeCount = ref(0);
const likeClicks = ref(0);
const isLoading = ref(true);

watch(
  likeCount,
  debounce(() => {
    fetch(`/api/posts/likes/${props.postId}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ likes: likeClicks.value }),
    });
    likeClicks.value = 0;
  }, 500),
);

const likePost = () => {
  likeCount.value++;
  likeClicks.value++;
  confetti({
    particleCount: 100,
    spread: 70,
    origin: {
      x: Math.random(),
      y: Math.random() - 0.2,
    },
  });
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

<style scoped>
button {
  background-color: #5e51bc;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}

button:hover {
  background-color: #4a3f9a;
}
</style>

```
