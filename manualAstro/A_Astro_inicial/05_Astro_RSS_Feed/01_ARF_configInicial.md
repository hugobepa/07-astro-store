(endpoints)[https://docs.astro.build/en/guides/endpoints/#request]
(rss)[https://docs.astro.build/en/recipes/rss/]
(addStyles)[https://docs.astro.build/en/recipes/rss/#adding-a-stylesheet]
(estiloXML)[https://github.com/genmon/aboutfeeds/blob/main/tools/pretty-feed-v3.xsl]
(automtificacion)[https://docs.astro.build/en/recipes/rss/#enabling-rss-feed-auto-discovery]

# END POINTS

1. crear `src\pages\rss.xml.ts`:

- llamarlo : `http://localhost:4321/rss.xml`
- pasar los `.mdx` a `.md` en `src\content\blog`
- si da error commanderPath-- reload window

```
import type { APIRoute } from "astro";

export const GET: APIRoute = ({ params, request }) => {
  return new Response(
    JSON.stringify({
      path: new URL(request.url).pathname,
    }),
  );
};
```

# RSS

0. install,T: npm install @astrojs/rss
1. modify `astro.config.mjs` add ` site: "https://example.com",`:

```
export default defineConfig({
 ...
  integrations: [mdx()],
  site: "https://example.com",
});
```

2. modify `src\pages\rss.xml.ts`:
   - add `import rss from "@astrojs/rss";`
   - eliminar lo de dentro y poner: `https://docs.astro.build/en/recipes/rss/-3-src/pages/rss.xml.js`
   - add `site` en `export const GET: APIRoute = ({ params, request, site })`
   - change `site:context.site` x `site: site!,` o `site: site ?? "",`
   - customData: `<language>es-es</language>`,
   - cambiar titulo, descripcion
   - obtenemos los datos de nuestro `md`: `const blogPosts = await getCollection("blog");`
     - y los parametrizamos y destucturamos
   - crear estilo. `stylesheet: "/styles/rss.xsl",`
     ```
       items: blogPosts.map(({ data, slug }) => ({
      title: data.title,
      pubDate: data.date,
      description: data.description,
      link: `/posts/${slug}`,
       })),
     ```

```
import type { APIRoute } from "astro";
import rss from "@astrojs/rss";
import { getCollection } from "astro:content";

export const GET: APIRoute = ({ params, request, site }) => {
  const blogPosts = await getCollection("blog");

  return rss({
    //stylesheet: "/styles/rss.xsl",
    // `<title>` field in output xml
    title: "hugo Blog",
    // `<description>` field in output xml
    description: "un blog sobre desarrollo web",
    // Pull in your project "site" from the endpoint context
    // https://docs.astro.build/en/reference/api-reference/#site
    site: site ?? "",
    // Array of `<item>`s in output xml
    // See "Generating items" section for examples using content collections and glob imports
    items: blogPosts.map(({ data, slug }) => ({
      title: data.title,
      pubDate: data.date,
      description: data.description,
      link: `/posts/${slug}`,
    })),
    // (optional) inject custom xml
    customData: `<language>es-es</language>`,
  });
};
```

2. crear estilos y pegar `public\styles\rss.xsl`:

- copiar estilos de :(estiloXML)[https://github.com/genmon/aboutfeeds/blob/main/tools/pretty-feed-v3.xsl]

## desplegar sitio

# - web page github crear nuevo repositorio

- ... -- crear nuevo repositorio

# VS - gitub instalar,T:

git init (inicializar)
git add . (tomar todos los cambios)
git commit -m "primer commit"
git remote add origin https://github.com/hugobepa/astroblog.git
git branch -M main (? Main)
git push -u origin main (? Main)

# https://www.netlify.com/

dashboard-sites-add new site-import an existing project - github
-autentificar - buscar repositorio

- site name: nombre de la web
- branch to deploy: main
- deploy

# VS , `astro.config.mjs`:

- cambiar `site: "https://example.com",` por `site: "https://hugo-blog.netlify.app",`

- y hacer commmit para el cambio:
  git add .
  git commit m "cambio"
  git push

- cambio auatomtico en netlify
- comprobar cambio rss por alguna notificacion

### automtificacion rss

- poner en `head` de`src\layouts\MainLayout.astro`:

```
<head>

....

  <link
    rel="alternate"
    type="application/rss+xml"
    title="hugo blog"
    href={new URL("rss.xml", Astro.site)}
    />
</head>
```

- hacer commit github para que haya cambio en netfly
- añadir mobil la app twine (rss ride)-New feed - pegar dirrecion `https://hugo-blog.netfly.app/` titulo: hugo-blog

## poner imagnes y mas bonita

()[https://gist.github.com/Klerith/8fe5c191e632ea7e0810d0568daaa1db]

0.  instalar, T: `npm i sanitize-html markdown-it` y `npm i -D @types/markdown-it @types/sanitize-html`

1.  `src/pages/rss.xml.ts`:`

- add:

```
   content: sanitizeHtml(parser.render(body), {
allowedTags: sanitizeHtml.defaults.allowedTags.concat(['img']),
}),

customData: `<media:content
 type="image/${data.image.format === 'jpg' ? 'jpeg' : 'png'}"
 width="${data.image.width}"
 height="${data.image.height}"
 medium="image"
 url="${site + data.image.src}" />`,
```

- añadimos `body`: `items: blogPosts.map(({ data, slug, body }) => ({`
- `sanitizeHtml`: add `import sanitizeHtml from "sanitize-html";`
- `parser` : add `import MarkdownIt from "markdown-it";  const parser = new MarkdownIt();`
- evitar error `prefix media` add:

`````
xmlns: {
  media: 'http://search.yahoo.com/mrss/',
},
````

- archivo:

```
import type { APIRoute } from "astro";
import rss from "@astrojs/rss";
import { getCollection } from "astro:content";
import sanitizeHtml from "sanitize-html";
import MarkdownIt from "markdown-it";
const parser = new MarkdownIt();

export const GET: APIRoute = async ({ params, request, site }) => {
  const blogPosts = await getCollection("blog");

  return rss({
    //stylesheet: "/styles/rss.xsl",
    // `<title>` field in output xml
    title: "hugo Blog",
    // `<description>` field in output xml
    description: "un blog sobre desarrollo web",

    xmlns: {
    media: 'http://search.yahoo.com/mrss/',
    },
    // Pull in your project "site" from the endpoint context
    // https://docs.astro.build/en/reference/api-reference/#site
    site: site ?? "",
    // Array of `<item>`s in output xml
    // See "Generating items" section for examples using content collections and glob imports
    items: blogPosts.map(({ data, slug, body }) => ({
      title: data.title,
      pubDate: data.date,
      description: data.description,
      link: `/posts/${slug}`,

      content: sanitizeHtml(parser.render(body), {
        allowedTags: sanitizeHtml.defaults.allowedTags.concat(["img"]),
      }),

      customData: `<media:content
      type="image/${data.image.format === "jpg" ? "jpeg" : "png"}"
      width="${data.image.width}"
      height="${data.image.height}"
      medium="image"
      url="${site + data.image.src}" />`,
    })),
    // (optional) inject custom xml
    customData: `<language>es-es</language>`,
  });
};

```
`````

1. hacer commit engithug y netfly ya actualiza
