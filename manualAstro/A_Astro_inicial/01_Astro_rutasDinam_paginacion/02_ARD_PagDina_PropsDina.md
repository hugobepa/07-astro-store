(paginaDinamicas)[https://docs.astro.build/en/reference/errors/get-static-paths-required/]
(getStaticPaths())[https://docs.astro.build/en/reference/routing-reference/#getstaticpaths]

# PAGINAS DINAMICAS

## paginas dinamicas arg x URL

0. adaptamos componente para ir paginas `src\components\pokemons\PokemondCard.astro `:

   - pasamos de `div ` a ` a`
   - añadimos href a paginas dimanicas `` href={`/pokemons/${name}`} ``, mediate la prop `name`:

     ```
     <a
     href={`/pokemons/${name}`}
     class="rounded border flex flex-col justify-center items-center p-2">
         <img src={imageSrc} alt={name} />
         <span class="capitalize">{name}</span>
     </a>
     ```

1. creamos pagina dinamica, se `src\pages\pokemons\[name].astro`:

   - se crea con [valorRef]. astro para decir que es dinamica.
   - creamos `getStaticPaths` para pasar los `valorRef`
   - declaramos como propiedad el `valorRef` para llamarlo: `const { name } = Astro.params;`

```
---
import type { GetStaticPaths } from "astro";
import MainLayout from "../../layout/MainLayout.astro"

export const getStaticPaths = (() => {

    return [
        { params: { name: 'bulbasaur' } },
        { params: { name: 'charmander' } },
        { params: { name: 'squirtle' } },
    ];
}) satisfies GetStaticPaths;

const { name } = Astro.params;
---

<MainLayout title="algun pokemon">
    <h1 class="text-3xl capitalize">{name}</h1>

</MainLayout>
```

## props dinamicos

1. añadimos props dinamicas y sonido , se `src\pages\pokemons\[name].astro`:

   ### props dianmicas

   - importamos `<PokemonCard name={name} url={url} /> `
   - creamos la prop `url`: `const {  url } = Astro.props; `
   - añadimos la prop dinamicas url en `getStaticPaths`:
     ` props: { name: 'bulbasaur', url: 'https://pokeapi.co/api/v2/pokemon/1/' },`
   - pasamos las propiedades `url` y `name` a `PokemonCard`: `<PokemonCard name={name} url={url} /> `

### audio

- extraemos la `id` del `url `: ` const id = url.split('/').at(-2);`
- llamanos al archivo de audio con el `id` extraido:
  ```
   const audioSrc = `https://raw.githubusercontent.com/PokeAPI/cries/main/cries/pokemon/latest/${id}.ogg`;
  ```
- usamos el archivo extraido en etiquetas de audio `audio`:

```
    <audio controls class="mt-5">
           <source src={audioSrc} type="audio/ogg" />
           Your browser does not support the audio element.
    </audio>
```

### props en MainLayot:

- añadimos la etiqueta `id` y `name`: `` <MainLayout title={`Pokémon - #${id} ${name}`}>  ``

```
---
import type { GetStaticPaths } from "astro";
import MainLayout from "../../layout/MainLayout.astro"
import PokemonCard from '../../components/pokemons/PokemondCard.astro';

export const getStaticPaths = (() => {

    return [
       {
            params: { name: 'bulbasaur' },
            props: { name: 'bulbasaur', url: 'https://pokeapi.co/api/v2/pokemon/1/' },
            },
            {
            params: { name: 'charmander' },
            props: { name: 'bulbasaur', url: 'https://pokeapi.co/api/v2/pokemon/4/' },
            },
    ];
}) satisfies GetStaticPaths;

const { name } = Astro.params;
const {  url } = Astro.props;
const id = url.split('/').at(-2);
const audioSrc = `https://raw.githubusercontent.com/PokeAPI/cries/main/cries/pokemon/latest/${id}.ogg`;
---

<MainLayout
 title={`Pokémon - #${id} ${name}`}
>

    <section class="mt-10 mx-10 flex flex-col justify-center items-center">
        <div>
            <a href="/">regresar</a>
            <h1 class="text-5xl capitalize">{name}</h1>
        </div>

        <PokemonCard name={name} url={url} />
        <audio controls class="mt-5">
            <source src={audioSrc} type="audio/ogg" />
            Your browser does not support the audio element.
        </audio>

    </section>


</MainLayout>

```
