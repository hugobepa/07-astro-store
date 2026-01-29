(paginas-estaticas)[https://docs.astro.build/en/reference/routing-reference/#paginate]

# PAGINA ID

0. cambiamos `${name}` x `${id}` en `<a ` `src\components\pokemons\PokemondCard.astro`:
   - cambiamos ``href={`/pokemons/${name}`}`` x ``href={`/pokemon/${id}`}``

```
<!-- href={`/pokemons/${name}`} -->
<a
href={`/pokemon/${id}`}
```

1. creamos y hacemos `src\pages\pokemon\[id].astro `:
   - importamos `getStaticPaths `
   - ponemos extracion de datos denttro del `getStaticPaths ` y la volvemos asycrono:

   ```
   export const getStaticPaths = (async() => {

   const res = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151');
   const {results} = await res.json() as PokemonListResponse
   ```

   - hacemos extracion de datos del array, para sacar el `id ` dentro del `getStaticPaths `:

   ```
        return results.map( ({name, url}) => {

        const id = url.split('/').at(-2);
   ```

   - devolvemos `name`,`url` e `id` dentro de extracion del array,dentro del `getStaticPaths `:

   ```
        export const getStaticPaths = (async() => {
                ....
        return {
        params: { id: id},
        props: { name, url },
        }
    });
   ```

- creamos las `name`,`url` e `id` para poder trabajar:

```
const { id } = Astro.params;
const { name, url } = Astro.props;
```

- ejemplo de trabajo con las props `name`,`url` e `id` :

```
const audioSrc = `https://raw.githubusercontent.com/PokeAPI/cries/main/cries/pokemon/latest/${id}.ogg`;
---

<MainLayout  title={`Pokémon - #${id} ${name}`>
```

- ARCHIVO:

```
---
import type { GetStaticPaths } from "astro";
import MainLayout from "../../layout/MainLayout.astro"
import PokemonCard from '../../components/pokemons/PokemondCard.astro';
import type { PokemonListResponse } from "../../interfaces/pokemon-list.response";


export const getStaticPaths = (async() => {

    const res = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151');
    const {results} = await res.json() as PokemonListResponse
    return results.map( ({name, url}) => {

        const id = url.split('/').at(-2);

        return {
        params: { id: id},
        props: { name, url },
        }
    });

}) satisfies GetStaticPaths;

const { id } = Astro.params;
const { name, url } = Astro.props;
const audioSrc = `https://raw.githubusercontent.com/PokeAPI/cries/main/cries/pokemon/latest/${id}.ogg`;
---

<MainLayout
 title={`Pokémon - #${id} ${name}`}
>

.... igual que  [name.astro]
</MainLayout>
```
