# blog - template de instalacion

0. `[...slug]` cualquier entrada que pase por blog va ser valida :`blog-starter\src\pages\blog\[...slug].astro`
   - `src/content/blog/2024/new-post.mdx` igual de valida que `src/content/blog/new-post.mdx`

# temas terceros

(astroshpere)[https://astro.build/themes/details/astrosphere/]
(astro-build_Theme)[https://astro.build/themes/1/?search=&price%5B%5D=free]

0. ir a `astro-build_Theme` -- selecionar `astroshpere`--get_starter--copiar autor y nombre de proyecto `markhorn-dev/astro-sphere `
1. en nuestro proyecto, T: npm create astro@ -- --template=markhorn-dev/astro-sphere
   - actualizar , T: npx @astrojs/upgrade
2. opcional: (actualizar_paquetes)[https://www.npmjs.com/package/npm-check-updates]
   - install only project,T: npx npm-check-updates
   - actulizar paquetes: npx npm-check-update -u
   - install, T: npm i
   - subir servidor, T: npm run dev

# starlight documentacion

## install

(startLightTemplate) [https://astro.build/themes/1/?search=starlight]
(startLightWeb)[https://starlight.astro.build/es/]

0. ir a `astro-build_Theme` -- selecionar `startlight`--get_starter-- buscar y copiar codigo como install
1. en nuestro proyecto, T: npm create astro@latest -- --template starlight

## contenido

### agregar guias

0. `startlight\astro.config.mjs`:
   - cambiar titulo, github x tuyo
   - crear una nueva entrada en el menu: ` items: [ { label: 'nuevos casos', slug: 'guides/nuevos-casos' },],`
   ```
     title: 'mi documentacion'
     github: cambiar por el mio
     items: [
   					// Each item here is one entry in the navigation menu.
   					{ label: 'Example Guide', slug: 'guides/example' },
   					{ label: 'nuevos casos', slug: 'guides/nuevos-casos' },
   				],
   ```
1. crear el arhivo de entrada,`startlight\src\content\docs\guides\nuevos-casos.md`:

```
---
title: nuevos-casos
description: A reference page in my new Starlight docs site.
---

Reference pages are ideal for outlining how things work in terse and clear terms.

## Further reading

- Read [about reference](https://diataxis.fr/reference/) in the Diátaxis framework
```

### agregar referencias

0. crear el archivo directamente `startlight\src\content\docs\reference\example2.md`:

```
--
title: Example Reference 2
description: A reference page in my new Starlight docs site.
---

Reference pages are ideal for outlining how things work in terse and clear terms.

```

### probar search

0. crear build,T: npm run build
1. preview,T: npm run preview
