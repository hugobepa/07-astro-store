# CREAR Y CONFIGURAR PROYECTO

0. crear carpeta, con nombre de proyecto.
1. instalar astro, T: npm create astro@latest .
   - empty
   * typersctipt -- yes
   * typescript strict -- yes
   * install depedencies -- yes
   * git repository -- yes
2. desabilitar barra astro, T: npm run astro preferences disable devToolbar
   - se crea en ".astro\settings.json":

   ```
   "devToolbar": {
   	"enabled": false
   }
   ```

   - para volver a ver eliminar esta orden o ponerla en `true`

# TAILWIND

(tailwindAstro)[https://docs.astro.build/en/guides/styling/#tailwind]

1. instalar tailwind: npx astro add tailwind
   - instalar tailwindxxxx: yes
   - Astro will scaffold ./src/styles/global.css. : yes
   - Astro will make the following changes to your config file: yes
   - add `import ../styles/global.css` to `src/layouts/Layout.astro`
2. hacer la prueba en `src\pages\index.astro`

```
---
import '../styles/global.css'
---

<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
		<link rel="icon" href="/favicon.ico" />
		<meta name="viewport" content="width=device-width" />
		<meta name="generator" content={Astro.generator} />
		<title>Astro</title>
	</head>
	<body>
		<h1 class="text-3xl font-bold text-red-600 underline">Astro</h1>
	</body>
</html>
```

# LAYOUTS - PROPS

(layoutsAstro)[https://docs.astro.build/es/basics/layouts/]

1. crear y llenar layout `src\layouts\MainLayout.astro`:

- copiar de la estructura de index tanto la obertura y el cerrado: `<html> <head></head><body>...</body></html>`
- añadimos dentro de body elementos base ` <Navbar /> <main>...</main>` . Importar Navbar con `TAB` o `ctrl + .`
- dentro del main, añadimos `<slot /> ` para incluir las demas paginas en plantilla: ` <main><slot /></main>``

## props

- llamar Props titulo MainLayout ` const { title } = Astro.props;`
- definir elementos de las props: ` interface Props {title: string;}`
- utilizar Props titulo titulo MainLayout ` <head>... <title>{title}</title></head>`
- poner transiciones `import { ClientRouter } from 'astro:transitions';` y `<ClientRouter/>`

```
---
import { ClientRouter } from 'astro:transitions';
import '../styles/global.css'
interface Props {
  title?: string;
}

const { title } = Astro.props;
---

<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>{title}</title>
     <ClientRouter/>
  </head>
  <body>
    <main class="mb-10 max-w-5xl m-auto px-2 py-2">
      <slot />
    </main>
  </body>
</html>
```

2. llamar layout desde pagina e introducir elementos en `MainLayout ` a ` src\pages\index.astro`:

- poner layout en pagina: ` <MainLayout>... </MainLayout>`
- importar layout `import MainLayout from '../layouts/MainLayout.astro' ` . Importar Navbar con `TAB` o `ctrl + .`

## props

- utilizar Props titulo de MainLayout: ` <MainLayout title="Home Page">... </MainLayout>`

```
---
import MainLayout from '../layouts/MainLayout.astro';

---

<MainLayout title="Home Page">
    <h1 class="text-3xl font-bold text-red-600 underline">Astro</h1>
</MainLayout>
```

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
      "@layouts/*": ["./src/layouts/*"],
      "@styles/*": ["./src/styles/*"],
      "@utils/*": ["./src/utils/*"],
      "@images/*": ["./src/assets/images/*"]
    }
  }
```

1. ejemplos:

```
import **** from '@components/pokemon/PokemonCard.astro'
import Button from '@interfaces/controls/Button.astro';
import logoUrl from '@layouts/logo.png?url';
```

### deploy

### netlify

(netlifyAstro)[https://docs.netlify.com/build/frameworks/framework-setup-guides/astro/]

0. instalat, T: npx astro add netlify

### cloudfare

(cloudfareAstro)[https://docs.astro.build/es/guides/integrations-guide/cloudflare/]
