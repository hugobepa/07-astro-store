# STATIC SIDE GENERATED

0. modificaremos el archivo `src\pages\pokemons\[name].astro`:

   ### obtener datos
   - convertimos en asyncrono ` getStaticPaths`: ``export const getStaticPaths = (async() => { ```
   - obtenemos losa datos de la paginas: `const res = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151'); `
   - visualizar datos y definirlos: `const {results} = await res.json() as PokemonListResponse `
   - obtenemos los resultados de los elementos del array:

   ```
     return results.map( ({name, url}) => ({
        params: { name },
        props: { name, url },
    }));
   ```

```

```

# PAGINAS ESTATICAS (SERVER STATIC)

(paginas-estaticas)[https://docs.astro.build/en/reference/routing-reference/#paginate]

- paginate() assumes a file name of [page].astro or [...page].astro

## crear paginacion estatica

0. creamos y modificamos `src\pages\pokemons\[page].astro`:
   - importamos ` getStaticPaths`
   - añadimos la extracion de datos dentro de ` getStaticPaths` y conv en async:

   ```
      import type { GetStaticPaths } from 'astro';

     export const getStaticPaths = (async () => {
      const resp= await fetch('https://pokeapi.co/api/v2/pokemon');
      const {results} = await resp.json() as PokemonListResponse;
         return paginate();
      }) satisfies GetStaticPaths;
   ```

   - añadimos limite a la extracion `?limit=151 `: `'https://pokeapi.co/api/v2/pokemon?limit=151' `
   - añadimos paginacion al ` getStaticPaths`:

   ```
      export const getStaticPaths = (async ({paginate}) => {
            ...
      return paginate(results,{pageSize:20});
   }) satisfies GetStaticPaths;
   ```

   - convertimos la pagina en propiedad `const{page}	= Astro.props; `

   - usamos la paginacion: ` {page.data.map(({ name, url }) => <PokemonCard name={name} url={url} />)}`

```
---

import PokemonCard from '../../components/pokemons/PokemondCard.astro';
import type { PokemonListResponse } from "../../interfaces/pokemon-list.response";
import MainLayout from '../../layout/MainLayout.astro';
import type { GetStaticPaths } from 'astro';

export const getStaticPaths = (async ({paginate}) => {
const resp= await fetch('https://pokeapi.co/api/v2/pokemon?limit=151');
const {results} = await resp.json() as PokemonListResponse;
	return paginate(results,{pageSize:20});
}) satisfies GetStaticPaths;

const{page}	= Astro.props;

const title = 'Pokemon static | Home';
---


<MainLayout title={title}>

	{page.data.map(({ name, url }) => <PokemonCard name={name} url={url} />)}



</MainLayout>
```

## paginador

### paginador basico

1. crear paginador basico `src\pages\pokemons\[page].astro`:

```
<section
	class="flex  mt-10 gap-2 items-center justify-center"
	>
		<a class="btn" href={page.url.prev}>Anteriores</a>
		<a class="btn" href={page.url.next}>Siguientes</a>
		<div class="flex flex-1"></div>
		<span>{page.currentPage}</span>

	</section>
   </MainLayout>

<style>
	  @reference '../../styles/global.css';
	  .btn {
	@apply bg-blue-500 hover:bg-blue-700 px-2 py-1 text-white font-bold rounded-md cursor-pointer;
	  }
	  .disabled {
		@apply bg-gray-600 text-gray-400  px-2 py-1 font-bold rounded-md cursor-not-allowed;
	  }
</style>
```

### paginador completo

1. crear paginador basico `src\pages\pokemons\[page].astro`:

```
<section class="flex mt-10 gap-2 items-center">
    <!-- {
      page.url.prev && (
        <a class="btn" href={page.url.prev}>
          Anteriores
        </a>
      )
    } -->
    <a
      class:list={[
        'btn',
        {
          disabled: !page.url.prev,
        },
      ]}
      href={page.url.prev}
    >
      Anteriores
    </a>

    <a
      class:list={[
        'btn',
        {
          disabled: !page.url.next,
        },
      ]}
      href={page.url.next}>Siguientes</a
    >

    <div class="flex flex-1"></div>

    <span class="text-xl font-bold">Página {page.currentPage}</span>
  </section>
```
