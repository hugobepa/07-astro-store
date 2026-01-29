(pokemonApi)[https://pokeapi.co/]

# PETICIONES HTTP

0. copiar enlnace de pokemonApi: `https://pokeapi.co/api/v2/pokemon/ditto`
1. crear interface de dato a extraer ` src\interfaces\pokemon-list.response.ts`:

   - PM: hacer peticion en postman: GET: `https://pokeapi.co/api/v2/pokemon/ `
   - PM: copiar los resultados extraidos de la peticion
   - VS: selecionar archivo interface `src\interfaces\pokemon-list.response.ts`
   - VS: Command palette selecionar `paste json` , poner nombre de interface + enter
   - VS: aparecen tipos de valores + guardar

2. crear peticion de extracionde datos en `src\pages\index.astro `:

   - crear peticion de extracion datos: ` const resp= await fetch('https://pokeapi.co/api/v2/pokemon');`
   - extraer los datos: `const data = await resp.json()`
   - darles formato a los datos ` PokemonListResponse`:
     `const data = await resp.json() as PokemonListResponse; `

```
---
import type { PokemonListResponse } from '../interfaces/pokemon-list.response';
import Mainlayout from '../layout/MainLayout.astro'

const resp= await fetch('https://pokeapi.co/api/v2/pokemon');
const data = await resp.json() as PokemonListResponse;

console.log(JSON.stringify(data,null,2))

---
```

3. trabajar con los datos extraidos `src\pages\index.astro `:
   - extracion de data por array: `{data.results.map `

```
<Mainlayout title={title}>
	<h1>listado de pokemons</h1>
	<ul>
		{data.results.map((pokemon) => (
			<li>{pokemon.name}</li>
		))}
	</ul>
</Mainlayout>
```

# COMPONENTES - PROPS

## component

0.  creamos y rellenamos el component `src\components\pokemons\PokemondCard.astro`:

    - creamos los propiedades de la etiqueta:

      ```
      interface Props{
      url: string
      name: string
      }

          const {url, name} = Astro.props;

      ```

- procesamos la imagen:

      ```
         const id = url.split('/').at(-2);
         const imageSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${id}.png`;
      ```

- tratamos los datos obtenidos:

      ```
         <img src={imageSrc} alt={name} />
         <span class="capitalize">{name}</span>
      ```

```
---
interface Props{
            url: string
            name: string
         }

const {url, name} = Astro.props;
const id = url.split('/').at(-2);
const imageSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${id}.png`;
---

<div class="rounded border flex flex-col justify-center items-center p-2">
    <img src={imageSrc} alt={name} />
    <span class="capitalize">{name}</span>
</div>
```

1.  utilizamos el componente `src\pages\index.astro `:

    - utilizamos el componente:

      ```
         import type { PokemonListResponse } from '../interfaces/pokemon-list.response';
         {data.results.map(({ name, url }) => <PokemonCard name={name} url={url} />)}
      ```

- pasamos los datos:

  ```
     const resp= await fetch('https://pokeapi.co/api/v2/pokemon');
     const data = await resp.json() as PokemonListResponse;
  ```

```
---
import type { PokemonListResponse } from '../interfaces/pokemon-list.response';
import Mainlayout from '../layout/MainLayout.astro'
import PokemonCard from '../components/pokemons/PokemondCard.astro';

const resp= await fetch('https://pokeapi.co/api/v2/pokemon');
const data = await resp.json() as PokemonListResponse;



const title = 'Pokemon static | Home';
---

<Mainlayout title={title}>

	{data.results.map(({ name, url }) => <PokemonCard name={name} url={url} />)}



</Mainlayout>




```
