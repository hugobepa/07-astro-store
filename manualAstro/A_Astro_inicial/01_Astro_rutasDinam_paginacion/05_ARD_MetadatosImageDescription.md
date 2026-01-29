(metadate)[https://docs.astro.build/en/guides/configuring-astro/]

# METADATOS

0. crear y hacer `src\consts\site-info.ts`:

```
export const siteInfo = {
  title: 'Pokémon Static',
  description: 'A static site with Pokémon data',
};
```

1. modificar ` src\layout\MainLayout.astro`:
   - add opcional propiedades `image` y `description`
   - ponemos opcional el titulo `title?: string;`
     -inicializamos variables con valores: `title=siteInfo.title`

```
interface Props {
  title?: string;
  image?: string;
  description?: string;
}

const { title=siteInfo.title, image, description=siteInfo.description } = Astro.props;

<!-- SEO -->
		<meta name="description" content={description} />
		<meta property="author" content={"hugobepa"} />
		<meta property="og:title" content={title} />
		<meta property="og:description" content={description} />
		<meta property="og:image" content={image} />
```

2 modificar ` src\pages\pokemons\[name].astro`:

```
const imageSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${id}.png`;
---

<MainLayout
 title={`Pokémon - #${id} ${name}`}
 description={`Detalle del pokémon #${id} ${name}`}
 image={imageSrc}
>
```
