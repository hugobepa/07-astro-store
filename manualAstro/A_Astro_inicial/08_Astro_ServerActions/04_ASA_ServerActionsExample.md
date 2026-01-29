(adapterVue)[https://docs.astro.build/es/guides/integrations-guide/vue/]
(conocimientosVue)[https://es.vuejs.org/v2/guide/components]
(serverActions)[https://docs.astro.build/en/guides/actions/]

# SERVER ACTIONS

## server action basico

0. creamos un archivo `src\actions\index.ts`:
   - creado siempre en la carpeta `src\actions\`
   - estructura basica de servers:

- FICHERO:

```
import { defineAction } from "astro:actions";
import { z } from "astro/zod";

export const server = {
  getGreeting: defineAction({
    input: z.object({
      name: z.string(),
      age: z.number(),
      isActive: z.boolean(),
    }),
    handler: async ({ name, age, isActive }) => {
      console.log({ name, age, isActive });
      return `Hello, ${name}! You are ${age} years old and your active status is ${isActive}.`;
    },
  }),
};

```

1. usar SA `src\components\likes\LikeCounter.vue`:

- convertimos funcion async
- llamamos la funcion creada `actions.getGreeting({...});`
- le ponemos el await para trabajar y descomponemos `const { data, error } = await`
- llamamso propiedades apara hacer la trabajar ` data, error`
- FICHERO:

```
import { actions } from "astro:actions";

const likePost = async () => {
  ....

  const { data, error } = await actions.getGreeting({
    name: "hugo",
    age: 30,
    isActive: true,
  });

  if (error) {
    console.error("Error calling action:", error);
    return alert("There was an error processing your request.");
  } else {
    console.log("Action response data:", data);
  }

  ...
};
```

### multi server actions

1. creamos fichero propio de esa accion `src\actions\greetings\get-greeting.action.ts`
   - siempre en carpeta actions

- FICHERO:

```
import { defineAction } from "astro:actions";
import { z } from "astro/zod";

export const getGreeting = defineAction({
  input: z.object({
    name: z.string(),
    age: z.number(),
    isActive: z.boolean(),
  }),
  handler: async ({ name, age, isActive }) => {
    console.log({ name, age, isActive });
    return `Hello, ${name}! You are ${age} years old and your active status is ${isActive}.`;
  },
});

```

2. lo exportamos a `src\actions\index.ts`:
   - siempre dentro carpeta `actions`
   - importamos erver especifco

- FICHERO:

```
import { getGreeting } from "./greetings/get-greeting.action";
export const server = { getGreeting };
```

3. usar SA `src\components\likes\LikeCounter.vue`:

- convertimos funcion async
- llamamos la funcion creada `actions.getGreeting({...});`
- le ponemos el await para trabajar y descomponemos `const { data, error } = await`
- llamamso propiedades apara hacer la trabajar ` data, error`
- FICHERO:

```
<script setup lang="ts">
import { actions } from "astro:actions";
....

const likePost = async () => {
  ....

  const { data, error } = await actions.getGreeting({
    name: "hugo",
    age: 30,
    isActive: true,
  });

  if (error) {
    console.error("Error calling action:", error);
    return alert("There was an error processing your request.");
  } else {
    console.log("Action response data:", data);
  }

  ...
};
...
</script>
```

3. creamos elemento de `.vue` con estructura basica y css `src\components\likes\LikeCounter.vue`:
4. `src\actions\index.ts`
   src\actions\get-greeting.action.ts
