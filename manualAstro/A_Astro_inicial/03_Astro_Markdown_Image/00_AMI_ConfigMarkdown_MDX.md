(astroMarkdown)[https://docs.astro.build/en/guides/markdown-content/]
(astroMDX)[https://docs.astro.build/en/guides/integrations-guide/mdx/]
(importmetaglob)[https://docs.astro.build/en/guides/imports/#importmetaglob/]

# MARKDOWN-MDX

## instalacion y prueba

0. install mdx,T: npx astro add mdx
   - 2 `Y`
   - ` success  Added the following integration to your project: @astrojs/mdx`
   - bajar y subir servidor
1. cambiar archivos interes de `.md` a `.mdx` (los comentarios dentro archivos son diferentes )
2. utilizar datos del `frontmatter` dentro de `.mdx ` de`src\pages\posts\post-01.mdx`:

```
---
title: Explorando Funciones de ES6
date: 2023-06-01
description: Explorando algunas de las nuevas funciones de ES6 en JavaScript.
author: Jane Doe
# image: https://placehold.co/1400x900/
image: "/assets/images/post-01.png"
tags: [JavaScript, ES6, Programming]
---

#{frontmatter.title}
//expesiones JS
{2+2}
```
