(tutorial min 5 )[https://www.youtube.com/watch?v=DNiqBXCnz78]
(astroGithub)[https://docs.astro.build/es/guides/deploy/github/]
(custom domain)[https://www.youtube.com/watch?v=BPnLC77SpNA]
(ssh github)[https://docs.github.com/es/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account]
###dominio custom githubpage
(dominio github page mirar-lo namechaped)[https://www.youtube.com/watch?v=e5AwNU3Y2es]
(dnsgithubpage)[https://docs.github.com/es/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site]
(dnschequer)[https://dnschecker.org/]
(nameChapped)[https://www.namecheap.com] NEWCOM649

## repositorio github

0. crear en github repositorio un nombre sencillo: hugober.github.io
1. dentro del repositorio (edit this file)modificar "astro.config.mjs":

```
import { defineConfig } from 'astro/config'

export default defineConfig({
  site: 'https://hugobepa.github.io',
  base: '01_astro_foundation'
})
```

2.cambiar todo los links, con el nombre de repositorio `01_astro_foundation`:

`<a href="/01_astro_foundation/acerca">Acerca</a>`

0. crear `.github/workflows/deploy.yml`
1. copiar y pegar `deploy.yml `
   - mirar `branches` por si cal canviar la

```
name: Deploy to GitHub Pages

on:
  # Activa el flujo de trabajo cada vez que hagas push a la rama `main`
  # Usando un nombre de rama diferente? Reemplaza `main` con el nombre de tu rama
  push:
    branches: [ main ]
  # Te permite ejecutar este flujo de trabajo manualmente desde la pestaña Acciones en GitHub.
  workflow_dispatch:

# Permite que este trabajo clone el repositorio y cree un despliegue de página
permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout your repository using git
        uses: actions/checkout@v4
      - name: Install, build, and upload your site
        uses: withastro/action@v3
        # with:
          # path: . # La ubicación raíz de tu proyecto de Astro dentro del repositorio. (opcional)
          # node-version: 20 # La versión específica de Node que debería usarse para construir tu sitio. Por defecto es 20. (opcional)
          # package-manager: pnpm@latest # El gestor de paquetes de Node que debería usarse para instalar dependencias y construir tu sitio. Detectado automáticamente basado en tu lockfile. (opcional)

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

0. T: git add -A
   git commit -m "workflow"
   git push

## github respositorio

0. settings
1. pages
   - build and deployment : actions
   - verificarlo y visitarlo
2. ir a pestaña `actions` para verificar

## crear tu dominio github

0. settings--pages:
   - custom domain: hugoberpar.prueba.com ( (youtube.hectorbliss.com))
   - save

## google domains

    (youtube.hectorbliss.com)
    -registrar, gestionar
    youtube--CNAME- hugobepa.github.io  SUBDOMINIO

## github repositorio page

        custom domain en verde dns check succesful

0. cambiar el archivo configuracion el dominio githun por el actual dominio o base: http://youtube.hectorbliss.com
   a
