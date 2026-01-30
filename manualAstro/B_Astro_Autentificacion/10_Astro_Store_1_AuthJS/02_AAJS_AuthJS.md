(authJSAstro)[https://docs.astro.build/en/guides/authentication/#authjs]
(authJS)[https://authjs.dev/guides]
(githubAuthJSAstro)[https://github.com/eapepe0/Astro-AuthJS-AstroDB-Store]

# AUTH JS

## authjs config

(authjsCredentials)[https://authjs.dev/getting-started/providers/credentials#custom-error-messages]

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

0. modificamos `auth.config.ts`:

- creamos credenciales. `Credentials({`
  - configuramos credenciales: ` credentials: {...},`
  - trabajamos con credenciales: ` authorize: async ({ email, password }) => {...})`
    - extraemos usuario de DB: ` const [user] = await db...`
      - comprobamos si existe: `if (!user) { throw new Error("User not found");}`
      - comparamos contrasenya: ` if (!bcrypt.compareSync(password as string, user.password)) {...}`
    - si confirmamos usuario:
      - descomponemos el usuario `const { password: _, ...rest } = user;`
      - enviamos datos sin contrasenya `return rest;`

- Fichero:

```
import { defineConfig } from "auth-astro";
import Credentials from "@auth/core/providers/credentials";
import { User, db, eq } from "astro:db";
import bcrypt from "bcryptjs";

export default defineConfig({
  providers: [
    //TODO:
    // GitHub({
    //   clientId: import.meta.env.GITHUB_CLIENT_ID,
    //   clientSecret: import.meta.env.GITHUB_CLIENT_SECRET,
    // }),

    Credentials({
      credentials: {
        email: { label: "Correo", type: "email" },
        password: { label: "Contraseña", type: "password" },
      },
      authorize: async ({ email, password }) => {
        const [user] = await db
          .select()
          .from(User)
          .where(eq(User.email, `${email}`));

        if (!user) {
          throw new Error("User not found");
        }

        if (!bcrypt.compareSync(password as string, user.password)) {
          throw new Error("Invalid password");
        }

        const { password: _, ...rest } = user;

        return rest;
      },
    }),
  ],
});

```

## authJS iniciar session

0. modificamos `src\pages\login.astro`:

- importamos singin ` const { signIn } = await import('auth-astro/client');`
- verificamos datos formulario : `const resp = await signIn('credentials', {...} as any);`
  - si datos da incorrectos: `  if (resp) {...}`
  - datos coorectos. `window.location.replace('/');`

- Fichero:

```
<script>
  ...

  const { signIn } = await import('auth-astro/client');

  form.addEventListener('submit', async (e) => {
  ....

    const formData = new FormData(form);

        const resp = await signIn('credentials', {
          email: formData.get('email'),
          password: formData.get('password'),
          redirect: false,
        } as any);

         if (resp) {
      Swal.fire({
        icon: 'error',
        title: 'Error al ingresar',
        // text: error.message,
        text: 'Usuario / Contraseña no son correctos',
      });
      btnSubmit.removeAttribute('disabled');
      return;
    }

    // TODO:
    window.location.replace('/');
```

1. hacemos una prueba `src\pages\index.astro`:
   - obtenemos session: `onst session = await getSession(Astro.request);`
   - extraemos usuario `const { user } = session ?? {};`
   - mostramos usuario: `{JSON.stringify(user, null, 2)}`

- Fichero:

```
---
import MainLayout from "@layouts/MainLayout.astro";
import { getSession } from "auth-astro/server";
const session = await getSession(Astro.request);
const { user } = session ?? {};
---

<MainLayout>
  <h1 class="text-3xl">Home Page</h1>
  <pre><code>{JSON.stringify(user, null, 2)}</code></pre>
</MainLayout>
```
