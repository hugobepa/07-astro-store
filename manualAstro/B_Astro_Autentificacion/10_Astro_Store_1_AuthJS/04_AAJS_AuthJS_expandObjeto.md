(authJSAstro)[https://docs.astro.build/en/guides/authentication/#authjs]
(authJS)[https://authjs.dev/guides]
(githubAuthJSAstro)[https://github.com/eapepe0/Astro-AuthJS-AstroDB-Store]
(declareModule)[https://docs.astro.build/ar/reference/integrations-reference/#adddevtoolbarapp-option]
(typescriptdeclareModule)[https://www.typescriptlang.org/docs/handbook/modules/reference.html#ambient-modules]

0. modificar `auth.config.ts`:
   - llamamos a los callbacks: ` callbacks: {`
     - regresar el token :`  jwt: ({ token, user }) => {...},`
       - el token del usuario `   if (user) {  token.user = user;`
     - trabajamos con la session: ` session: ({ session, token }) => {`
       - obtener rol usuario `ession.user = token.user as AdapterUser;`

- Fichero:

```
import type { AdapterUser } from "@auth/core/adapters";

....
  callbacks: {
    jwt: ({ token, user }) => {
      if (user) {
        token.user = user;
      }

      return token;
    },

    session: ({ session, token }) => {
      session.user = token.user as AdapterUser;
      return session;
    },
  },

   if (!locals.isAdmin && url.pathname.startsWith("/dashboard")) {
      return redirect("/");
    }
```

1. definimos nuestros usaurios `auth.d.ts`:

```
import { DefaultSession, DefaultUser } from "@auth/core/types";

declare module "@auth/core/types" {
  interface User extends DefaultUser {
    role?: string;
  }

  interface Session extends DefaultSession {
    user: User;
  }
}
```

2. comporbamos rol `src\middleware.ts`:
   - ` locals.isAdmin = user.role === "admin";`

```

locals.isAdmin = false;

if (user) {
      // TODO:
      locals.user = {
        email: user.email!,
        name: user.name!,
      };
      locals.isAdmin = user.role === "admin";
    }
```
