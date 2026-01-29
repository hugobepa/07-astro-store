# COMPONENT ARCHIVO

0. crear carpeta/component "src\components\Navbar.astro" y crear componente:

- escribir `component+ tab` para añadir scriptComponent en archivo.

```
---
const navTitle = "Navigation";
---

<span>{navTitle}</span>
<nav>
			<a href="/">Home</a>
			<a href="/about">About</a>
			<a href="/contact">Contact</a>
</nav>
```

1. insertar componente "src\pages\about.astro":

- nombre del Component `<Navbar` y apretar tab o ctrl + . para añadir `import`

```
---

import Navbar from '../components/Navbar.astro';


---
<body>
		<Navbar />
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

```
---

import Navbar from '../components/Navbar.astro';

interface Props {
  title: string;
  age?: number,
  isActive?: boolean;
}

const { title,age,isActive } = Astro.props;
---

<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>{title}</title>
  </head>
  <body>
    <Navbar />

    <main>
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

// Server Only
const name = 'Fernando Alberto Herrera Jiménez';
const title = 'Home Page';

const currentDate = new Date();
---

<MainLayout title="Home Page">
  <h1>{name}</h1>

  <p>Momento actual: {currentDate.toLocaleTimeString()}</p>
  <p id="current-time">Momento actual real:</p>

  <script>...js</script>
</MainLayout>
```
