# ESTILOS CSS

(estilos)[https://docs.astro.build/es/guides/styling/]
(ejemploEstilos)[https://gist.github.com/Klerith/6659142852f95466340f79f6d226870b]

## estilo scope

0. estilo en scope o en el mismo componente: ` <style>h1{color: blue;}</style>`
1. estilo en scope o en el mismo componente: ` <style is:global>h1{color: blue;}</style>`
   - (global,inline,raw)[https://docs.astro.build/en/reference/directives-reference/#script--style-directives]

## estilo global

0. crear y rellenar archivo de estilos `src\styles\global.css`.

- (ejemploEstilos)[https://gist.github.com/Klerith/6659142852f95466340f79f6d226870b]

1. buscar el punto mas alto (mainLayout) para llamara lo `src\layouts\MainLayout.astro `: `import '../styles/global.css'; `

### fuentes globales

0. bajar fuentes y colocar fuentes en ` public\fonts\atkinson-bold.woff`
1. llamar fuentes ` src\styles\global.css`:

```
@font-face {
  font-family: "Atkinson";
  src: url("/fonts/atkinson-regular.woff") format("woff");
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}
@font-face {
  font-family: "Atkinson";
  src: url("/fonts/atkinson-bold.woff") format("woff");
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}
```

# CUSTOM 404

sencilla. 404 cutomizada, crear y rellenar pagina 404 ` src\pages\404.astro`

```
---
import MainLayout from '../layouts/MainLayout.astro';
---

<MainLayout title="404 | Not Found">
     <h1> estas perdido </h1>
    <a href="/" >Regresar a home</a>
   </MainLayout>

```

1. 404 cutomizada, crear y rellenar pagina 404 ` src\pages\404.astro`

- (plantillas 404 buscar solo html,css,js)[https://dev.to/stackfindover/35-html-404-page-templates-5bge]
- poner el cuerpo dentro del MainLayout y los styles despues

```
---
import MainLayout from '../layouts/MainLayout.astro';
---

<MainLayout title="404 | Not Found">
   <div class="wrapper">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1920 1080">
    ....
   </MainLayout>
<style> .... </style>
```

# VIEW TRANSACTIONS

(view_transitions)[https://docs.astro.build/es/guides/view-transitions/]
(view_transations)[https://eastondev.com/blog/en/posts/dev/20251202-astro-view-transitions-guide/]

0. los llamamos desde el punto mas alto `src\layouts\MainLayout.astro`:

- fade (default): Fade-in/fade-out, most versatile
- slide: Slide effect, content slides in from the right, suitable for article detail pages
- initial: Use browser default styles, basically no animation
- none: Completely disable animation

- en la consola navegador aparecer esto: ` [astro] Prefetching http://localhost:4321/about with <link rel="prefetch">`

```
---
import { ClientRouter,fade,side } from 'astro:transitions';
---
 <head>
    ...
    <ClientRouter />
</head>
<body > //por defecto fade
<body transition:animate={fade({ duration: '5.5s' })}>
<body transition:animate="side">
```
