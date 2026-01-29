(adaptersDynamiSserver)[https://docs.astro.build/en/guides/integrations-guide/]
(cloudfare)[https://www.cloudflare.com/es-es/]
(adapterNodeJS)[https://docs.astro.build/en/guides/integrations-guide/node/]
(adapterCloudfare)[https://docs.astro.build/en/guides/integrations-guide/cloudflare/]
(guiasDeployAstroStatic)[https://docs.astro.build/en/guides/deploy/]

# DEPLOY OPTIONS

## deploy general

### adapter nodejs

(adapterNode)[https://docs.astro.build/en/guides/integrations-guide/node/]

0. install, T: npx astro add node
   - decir 3 `Y`
1. `output:'xxx',` en `astro.config.mjs`:
   - server,static
2. determinar si es statica o dinamica `src\pages\api\posts\index.ts`:
   - no que sea estatica,sino dinamica `export const prerender = false;`
   - build app,T:`npm run build`
   - preview,T: `npm run preview`
   - Fichero:
     `export const prerender = false; export const GET: ...`

## deploy especifico

### adapter cloudafare

(adapterCloudfare)[https://docs.astro.build/en/guides/integrations-guide/cloudflare/]

0. crear nuevo repositorio en GITHUB y subir proyecto repositorio desde terminal proyecto:
   - git, T: git init (inicializar)
     git add . (añadir ficheros)
     git commit -m "comit primero" (prepara ficheros para subir)
     git remote add origin https://github.com/hugobepa/astro-http.git (llamar a repositorio en la nube)
     git branch -M main (pasar a rama Main)
     git push -u origin main ( subir lo a repositorio web rama main)

1. comentar lo del adaptador node `astro.config.mjs`:

```
import mdx from "@astrojs/mdx";
import sitemap from "@astrojs/sitemap";
import { defineConfig } from "astro/config";

//import node from "@astrojs/node";

// https://astro.build/config
export default defineConfig({
  site: "https://example.com",
  integrations: [mdx(), sitemap()],

  // adapter: node({
  //  mode: "standalone",
  // }),
});
```

2. desinstalar adapater node: npm uninstall @astrojs/node
   - rompe applicacion
3. install adapter cloudfare, T: npx astro add cloudflare

- 3 `Y`

4. instalado adaptador cloudfare `astro.config.mjs`:
   - add: `import cloudflare from "@astrojs/cloudflare";`,` adapter: cloudflare()`, ` output: "server"??,`
   - output:"server" puede dar problemas o no sino se cambia o se quita o cambiar por `static`

```
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';
import sitemap from '@astrojs/sitemap';

// import node from "@astrojs/node";
import cloudflare from "@astrojs/cloudflare";


// https://astro.build/config
export default defineConfig( {
  site: 'https://example.com',
  integrations: [ mdx(), sitemap() ],
  output: "server",
  adapter: cloudflare()
} );
```

5. actulizar cambios github teminal proyecto:
   -T: git add .
   git commit -m "add cloudfare"
   git push

6. web cloudflare:
   (cloudfare)[https://www.cloudflare.com/es-es/]
   - dashBoard -- workers & Pages -- Create
     - Connect to Git -- github -- usuario github -- repositorio -- begin setup
       - nombre Project(nombrePagina) - escoger rama (main) - framework astro - npm run build - directory dist - save and deploy
       - continue project
7. probar en postman:
   - cambiados por POST,PUT,PATH,DELETE
   - nombre del dominio `localhost:4321` x `astro-http.pages.dev`
     - postman: POST: `https://astro-http.pages.dev/api/posts/first-post`
       - body -- raw -- json -- ``{"id":100,"name":"john","age":38,"isActive":true}`
