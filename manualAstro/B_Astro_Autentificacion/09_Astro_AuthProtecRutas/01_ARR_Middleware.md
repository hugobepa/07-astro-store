(middlewareAstro)[https://docs.astro.build/en/guides/middleware/]

# Middleware

## middleware Inicial

0. add `output: 'server', ` en `astro.config.mjs`

middleware.local.ts:

```
vite: {
    plugins: [tailwindcss()],
  },
  output: "server",
  adapter: netlify(),
```

1. crear middleware ``:
   (middlewareDefinite)[https://docs.astro.build/en/guides/middleware/#middleware-types]

- solo se crean middleware asi `src/middleware.ts` o `src/middleware/index.ts`:
  - mensaje de respuesta:
    - web status (400,401,403)
    - bloqueo de poder entrar y pantallita:`headers: {"WWW-Authenticate": 'Basic real="Secure Area"',},`
    - se obtiene la autorizacion `const authHeader = context.request.headers.get("authorization");`
    - se verifica si es ruta protegida: `if (privateRoutes.includes(context.url.pathname))`

  ```
    return new Response("Auth Necesaria", {
    status: 401,
    headers: {
      "WWW-Authenticate": 'Basic real="Secure Area"',
    },
  ```

- FICHERO:

```
import { defineMiddleware } from "astro:middleware";

// `context` and `next` are automatically typed
const privateRoutes = ["/protected"];

export const onRequest = defineMiddleware((context, next) => {
  const authHeader = context.request.headers.get("authorization");

  if (privateRoutes.includes(context.url.pathname)) {
    if (authHeader) {
      return next();
    }
  }

  return new Response("Auth Necesaria", {
    status: 401,
    headers: {
      "WWW-Authenticate": 'Basic real="Secure Area"',
    },
  });
});
```

## middleware autorizacion

### middleware autho demo

0. crear ejemplo autenthificacion local `src/middleware.local.ts`:

```
// No funciona por el nombre del archivo
// Demostración

import type { MiddlewareNext } from 'astro';
import { defineMiddleware } from 'astro:middleware';

const privateRoutes = ['/protected'];

export const onRequest = defineMiddleware(async ({ url, request }, next) => {
  // console.log(context.url);
  const authHeaders = request.headers.get('authorization') ?? '';

  if (privateRoutes.includes(url.pathname)) {
    return checkLocalAuth(authHeaders, next);
  }

  return next();
});

const checkLocalAuth = (authHeaders: string, next: MiddlewareNext) => {
  if (authHeaders) {
    const authValue = authHeaders.split(' ').at(-1) ?? 'user:pass';
    const decodedValue = atob(authValue).split(':');
    const [user, password] = decodedValue;

    if (user === 'admin' && password === 'admin') {
      return next();
    }
  }

  return new Response('Auth Necesaria', {
    status: 401,
    headers: {
      'WWW-Authenticate': 'Basic real="Secure Area"',
    },
  });
};
```

### plantilla middelware auth

(middlewareDefinite)[https://docs.astro.build/en/guides/middleware/#middleware-types]

0. add `output: 'server', ` en `astro.config.mjs`

middleware.local.ts:

```
vite: {
    plugins: [tailwindcss()],
  },
  output: "server",
  adapter: netlify(),
```

1. `/src/middleware.ts`:

```
import type { MiddlewareNext } from 'astro';
import { defineMiddleware } from 'astro:middleware';


const privateRoutes = ['/protected'];

//context.url
export const onRequest = defineMiddleware(
  async ({ url, request, locals, redirect }, next) => {

    return next();
  }
);
```
