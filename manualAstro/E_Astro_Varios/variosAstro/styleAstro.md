# USAR ESTILOS SCOPE TAILWIND

- para poder trabajar estilos tailwind en scope necesitamos ` @reference` o `@import `

## estilo por etiquetas

```
<style>
    @reference '../../styles/global.css';
    @import '../../styles/global.css';
    a{
        @apply hover:underline text-blue-500;
    }

</style>
```

## estilo por id

- damos estilo al grupo de botones por `id`:

  ```
    <style>
    @reference '../../styles/global.css';
    @import '../../styles/global.css';
    ...
    #btn-favorite {
        @apply hover:animate-pulse;
    }
    </style>
  ```

- usar estilo del botton: `<button id="btn-favorite" class="ml-4 mt-4">`

# ESTILOS GLOBALES

0. crear archivo 'src\layouts\BlogLayout.astro':
   - importar tailwinds: ` @import "tailwindcss";`
   - crear estilos por etiquetas,id : `html { @apply bg-gray-900 px-10;}`

   ```
   @import "tailwindcss";

    html {
        @apply bg-gray-900 px-10;
    }

    ....
   ```

1. importar estilos a layout `src\layouts\BlogLayout.astro` : `---  import '../styles/blog.css' ...---`

# ESTILOS CONDICIONALES

0. crear valor booleano controlar condicion `isBig` en component `src\components\pokemons\PokemondCard.astro`:

- se pone opcional para solo perdirlo en true

```
interface Props{
    isBig?: boolean
}
const {url, name, isBig = false} = Astro.props;
```

1. trabajamos con la condicion `isBig` en component `src\components\pokemons\PokemondCard.astro`:

```
opcion 1:

   class={`rounded border flex flex-col justify-center items-center p-2 ${ !isBig && 'border' }`}>

opcion 2:

class:list={[
`rounded flex flex-col justify-center items-center p-2 `,
{
    'w-26 h-26': isBig,
    'w-16 h-16': !isBig,
}
]}

```

5. activamos el estilo opcional del component `isBig `: ` <PokemonCard name={name} url={url}  isBig/>`

# ViewTransition + Name Transation

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
import { ClientRouter,fade,slide } from 'astro:transitions';
---
 <head>
    ...
    <ClientRouter />
</head>
<body > //por defecto fade
<body transition:animate={fade({ duration: '5.5s' })}>
<body transition:animate="slide">
<img transition:name={`${name}-image`}
```

## Zero-JavaScript View Transitions

```
---
 import { ViewTransitions } from "astro:transitions";
---
<head>
  <ViewTransitions fallback="none" />
  <style>
    @view-transition {
      navigation: auto;
    }
  <style>
</head>
```
