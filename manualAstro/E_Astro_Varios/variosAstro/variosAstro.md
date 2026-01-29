# ESLINT

[// eslint-disable-next-line no-console]: #
[/* eslint-disable no-alert */]: #
[/* eslint-enable no-alert */]: #

# COMANDOS TERMINAL

npm run dev ( ver en dessarrolo)
npm run build ( hacer los cambios en `/dist` o crearla)
npm run preview ( ver como quedaria en produccion (haber creado antes la `/dist`))

# ABREVIATURAS

1. crear ScriptComponent dentro de "src\components\Navbar.astro": component + TAB
2. si ponemos en archivo ` .flex.flex-row` + tab . Se crea div con esas classes de tailwind
3. github repositorio cambiar ` .com  x  .dev` abrir VS nube
4. `aget` apiREST astro

# VOLVER PAGINA ANTERIOR

1. ademas en `src\pages\pokemons\[name].astro` add:

- volver pagina anterior: `onclick="history.back()"`

`<button onclick="history.back()" class="text-blue-500 hover:underline ml-4">regresar</button> `

# VIEW TRANSACTIONS

(view_transitions)[https://docs.astro.build/es/guides/view-transitions/]
(view_transations)[https://eastondev.com/blog/en/posts/dev/20251202-astro-view-transitions-guide/]

0. los llamamos desde el punto mas alto `src\layouts\MainLayout.astro`:

- fade (default): Fade-in/fade-out, most versatile
- slide: Slide effect, content slides in from the right, suitable for article detail pages
- initial: Use browser default styles, basically no animation
- none: Completely disable animation

- en la consola navegador aparecer esto: ` [astro] Prefetching http://localhost:4321/about with <link rel="prefetch">`

```
---
import { ClientRouter,fade,side } from 'astro:transitions';
---
 <head>
    ...
    <ClientRouter />
</head>
<body > //por defecto fade
<body transition:animate={fade({ duration: '5.5s' })}>
<body transition:animate="side">
```

# acnhorage de regresar

```
<a href="/" class="text-blue-500 hover:underline">
        &larr; regresar
</a>
```

# plantilla middelware

(middlewareDefinite)[https://docs.astro.build/en/guides/middleware/#middleware-types]

0. add `output: 'server', ` en `astro.config.mjs`

middleware.local.ts:

```
vite: {
    plugins: [tailwindcss()],
  },
  output: "server",
  adapter: netlify(),
```

1. `/src/middleware.ts`:

```
import type { MiddlewareNext } from 'astro';
import { defineMiddleware } from 'astro:middleware';


const privateRoutes = ['/protected'];

//context.url
export const onRequest = defineMiddleware(
  async ({ url, request, locals, redirect }, next) => {

    return next();
  }
);
```

# EXPLICACION CIcLOS DE CARGA

(cicloTrabaosViewTransitionJS)[https://docs.astro.build/en/guides/view-transitions/#lifecycle-events]

```
// antes de que la pagina empiece a cargar (loading o snipper)
    document.addEventListener('astro:before-preparation',()=>{console.log('astro:before-preparation')})
    //
    document.addEventListener('astro:after-preparation',()=>{console.log('astro:after-preparation')})

    document.addEventListener('astro:before-swap',()=>{console.log('astro:before-swap')})
    //
    document.addEventListener('astro:after-swap',()=>{console.log('astro:after-swap')})
    // despues de que la pagina se cargo
    document.addEventListener('astro:page-load',()=>{console.log('hola')})
```

# tipos directrices components isla

(directivasTemplate)[https://docs.astro.build/en/reference/directives-reference/#client-directives]

(viewTransition)[https://docs.astro.build/en/guides/view-transitions/]

0. directrices de props y valores:

`<video controls="" autoplay="" transition:persist="playing-video">`

`<Counter client:load transitionpersist-props transition:persist="counter"`:

- comparten valor la misma compenentes en diferentes pagina:`transition:persist="counter"`
- persiste el valor de la prop: `#transitionpersist-props`

1. directrices de carga:

## lado cliente

`<ClientComponent client:xxxx>`

- carga rapida y prioritaria: `client:load`
- carga normal, se carga cuando la pagina esta cargada: `client:idle` o `client:idle={{timeout: 500}}`
- solo se carga cuando se ve esa parte de la pagina: `client:visible`
- con margen gracia para que entre en pantalla : `client:visible={{rootMargin: "200px"}}`
- cargar segun el dispositivo: `client:media="(max-width: 50em)"`
- como el load pero solo en el lado cliente -puro client- (especificar framework)(evitar error localstore): `client:only="react"`

### añadir view-transition a un compement isla

`` <img style={`view-transition-name: ${pokemon.name}-image`}``

# PATH ALIAS

(pathAlias)[https://docs.astro.build/en/guides/imports/#aliases]

0. modificar `tsconfig.json`:

- declara la base en el root. `"baseUrl": ".", `
- se crea ruta especifica:

```
"paths": {
      "@components/*": ["./src/components/*"],
```

- archivo:

```
...,
 "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@components/*": ["./src/components/*"],
      "@interfaces/*": ["./src/interfaces/*"],
      "@layouts/*": ["./src/layouts/*"]
    }
  }
```

1. ejemplos:

```
import **** from '../../compoenets/pokemon/PokemonCard.astro'
import **** from 'src/compoenets/pokemon/PokemonCard.astro'
import **** from '@components/pokemon/PokemonCard.astro'
import Button from '@interfaces/controls/Button.astro';
import logoUrl from '@layouts/logo.png?url';
```
