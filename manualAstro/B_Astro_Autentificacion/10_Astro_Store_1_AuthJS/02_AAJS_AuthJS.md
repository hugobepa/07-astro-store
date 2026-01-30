(authJSAstro)[https://docs.astro.build/en/guides/authentication/#authjs]
(authJS)[https://authjs.dev/guides]
(githubAuthJSAstro)[https://github.com/eapepe0/Astro-AuthJS-AstroDB-Store]

# AUTH JS

## authjs config

0. install auth-astro, T: npx astro add auth-astro, npm i auth-astro@^4.2.0 @auth/core@^0.37.4
1. creamos archivo `.env` y los nombramos en `.gitignore` ademas creamos `.env.template`:

- Archivo:

```
AUTH_TRUST_HOST=true
AUTH_SECRET=<32 character string>
```

2. crear en el root `auth.config.ts`:

- fichero:

```
import { defineConfig } from "auth-astro";

export default defineConfig({
  providers: [
    //TODO:
    // GitHub({
    //   clientId: import.meta.env.GITHUB_CLIENT_ID,
    //   clientSecret: import.meta.env.GITHUB_CLIENT_SECRET,
    // }),
  ],
});
```

## authjs credentials providers
