# INDICADOR RUTAS ACTIVAS NAV-BAR

(navBar)[https://www.creative-tim.com/twcomponents/component/simple-navbar-3]
(componentNavBar)[src\components\shared\NavBar.astro]

- indicamos la ruta que esta el pagina en el navegador
- señalamos la pagina en que estamos: ` const currentPath = Astro.url.pathname`

## indicador sin animacion

0.  disparamos los estilos de la linea del navBar cuando esta en la pagina:
    ```
         class:list={["border-b-2 border-transparent  dark:hover:text-gray-200  mx-1.5 sm:mx-6",
               {
                'text-blue-500 underline border-blue-500': currentPath === href,
               }
               ]}
    ```

- fichero:

```
---
const  links = [
    { href: '/', label: 'listado' },
    { href: '/pokemons/1', label: 'paginado' },
    { href: '/favorites', label: 'favoritos' },
];

const currentPath = Astro.url.pathname

---

<!-- component -->
<nav class="bg-slate-800">
    <div class="container flex items-center justify-center p-6 mx-auto text-gray-600 capitalize dark:text-gray-300">

        {links.map(({href,label}) => (
          <a href={href}
           class:list={["border-b-2 border-transparent  dark:hover:text-gray-200  mx-1.5 sm:mx-6",
               {
                'text-blue-500 underline border-blue-500': currentPath === href,
               }
               ]}
          >{label}</a>
        ))}
```

## indicador con animacion

0. disparador de animacion de un linea en nav bar:

- creamos un trigger de la animacion mediante un div:

  ```
  {currentPath === href ? (
            <div
              transition:name="menu-line"
              class="border-b-2 border-blue-500 mx-4"
            />
          ) : (
            <div class="border-b-2 border-transparent" />
          )}
  ```

- fichero:

      ````
       ---

  const links = [
  { href: '/', label: 'listado' },
  { href: '/pokemons/1', label: 'paginado' },
  { href: '/favorites', label: 'favoritos' },
  ];

const currentPath = Astro.url.pathname

---

<!-- component -->
<nav class="bg-slate-800">
    <div class="container flex items-center justify-center p-6 mx-auto text-gray-600 capitalize dark:text-gray-300">

         {links.map(({ href, label }) => (
        <div>
          <a
            href={href}
            class="text-gray-200 dark:text-gray-200 mx-1.5 sm:mx-6"
          >
            {label}
          </a>

          {currentPath === href ? (
            <div
              transition:name="menu-line"
              class="border-b-2 border-blue-500 mx-4"
            />
          ) : (
            <div class="border-b-2 border-transparent" />
          )}
          {/* <div class="border-b-2 border-blue-500 mx-4" /> */}
        </div>
      ))
    }
    </div>

</nav>
    ````

## Component Title

0. crear y add `src\components\shared\Title.astro `:

```
---

---

<h1 class="text-5xl font-bold capitalize">
    <slot/>
</h1>

<div class="border-b-2 border-blue-500 mt-2"></div>
```

1. utilizar componente en paginas `src\pages\pokemons\[page].astro`:

- eliminar etiquetas `h1` y `hr`
- añadir `Title`

```
import Title from '@components/shared/Title.astro';


<MainLayout title={title}>

  <Title>Listado de Pokémons</Title>
```

2. en index hacer lo mismo paso `1.` `src\pages\index.astro`.
3. cambiarlo en favorites, [name].astro
4. ademas en `src\pages\pokemons\[name].astro` add:
   - eliminar `a`
   - add `button`

- volver pagina anterior: `onclick="history.back()"`

`<button onclick="history.back()" class="text-blue-500 hover:underline ml-4">regresar</button> `
