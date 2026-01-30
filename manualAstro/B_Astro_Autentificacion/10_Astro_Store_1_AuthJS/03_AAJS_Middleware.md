(authJSAstro)[https://docs.astro.build/en/guides/authentication/#authjs]
(authJS)[https://authjs.dev/guides]
(githubAuthJSAstro)[https://github.com/eapepe0/Astro-AuthJS-AstroDB-Store]

0. modificar `src\middleware.ts`:
   - extraemos `request` de context y lo pasamos a session `const session = await getSession(request);`
   - pasamos a boleeano `!!`: `const isLoggedIn = !!session;`
   - creamos usuario `const user = session?.user;`
   - trabajamos con user: ` if (user) {...}`

- Fichero:

```
import { getSession } from "auth-astro/server";
async ({ url, locals, redirect, request }, next) => {
    const session = await getSession(request);
    const isLoggedIn = !!session;
    const user = session?.user;

    if (user) {
      // TODO:
      locals.user = {
        email: user.email!,
        name: user.name!,
      };
    }

```

1. modificamos `src\env.d.ts`:
   - añadimos ` isAdmin: boolean;`

```
declare namespace App {
  interface Locals {
    isLoggedIn: boolean;
    isAdmin: boolean;
    user: User | null;
  }
}
```

2. modificar navbar `src\components\shared\Navbar.astro`:
   - exportamos el `isAdmin`
   - trabajamos con el `{ isAdmin && (...)}`
   - importamos `const { signOut } = await import("auth-astro/client");`
   - trabajamos `await signOut();`
   - redirecionamos ` window.location.href = '/';`

```
---
const { isLoggedIn, isAdmin } = Astro.locals;
---
 {
        isAdmin && (
          <li class="font-semibold text-gray-700">
            <a href="/admin/dashboard">Admin</a>
          </li>
        )
      }


<script>
  const { signOut } = await import("auth-astro/client");

  const logoutElem = document.querySelector('#logout') as HTMLLIElement;

  logoutElem?.addEventListener('click', async () => {
    await signOut();
    window.location.href = '/';
  });
</script>
```
