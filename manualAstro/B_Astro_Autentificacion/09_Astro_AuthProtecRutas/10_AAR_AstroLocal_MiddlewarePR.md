(importsTS)[https://docs.astro.build/en/guides/typescript/#using-imports]
(cambiosAstro5)[https://docs.astro.build/en/guides/upgrade-to/v5/#changed-typescript-configuration]
(usarVariblesEntornoTS)[https://docs.astro.build/en/guides/environment-variables/#intellisense-for-typescript]

# ASTRO LOCALS_env.d.ts

0. creamos `src\env.d.ts`:
   - creamos interfaces:
     - creamos elemento User `interface User {...}`
     - crea elemento `interface Locals {`.Y le añadimos ` isLoggedIn` y`user:`

- Fichero:

```
interface User {
  email: string;
  name: string;
  avatar: string;
  emailVerified: boolean;
}

declare namespace App {
  interface Locals {
    isLoggedIn: boolean;
    user: User | null;
  }
}
```

1. modificamos middleware `src\middleware.ts`:
   - conv booleano `!!` para verificar autententificacion:`const isLoggedIn = !!firebase.auth.currentUser;`
   - extraemos el usuario actual `const user = firebase.auth.currentUser;`
   - pasamos el valor logeado al sistema`context.locals.isLoggedIn = isLoggedIn;`
   - si existe `user` para pasarle datos al sistema: ` if (user) {context.locals.user = {...}}`

- Archivo:

```
import { firebase } from "./firebase/config";

 const isLoggedIn = !!firebase.auth.currentUser;
  const user = firebase.auth.currentUser;

  context.locals.isLoggedIn = isLoggedIn;

  if (user) {
    context.locals.user = {
      avatar: user.photoURL ?? "",
      email: user.email!,
      name: user.displayName!,
      emailVerified: user.emailVerified,
    };
  }
```

2. modificamos `src/components/shared/Navbar.astro`:
   - importamos booleano para trabajar: `const { isLoggedIn } = Astro.locals;`
   - trabajamos con booleano en la pagina: ` { isLoggedIn && (...)}`

- Fichero:

```
---
const { isLoggedIn } = Astro.locals;
---

 {
        isLoggedIn && (
          <li class="font-semibold text-gray-700">
            <a href="/protected">Protegido</a>
          </li>
        )
      }

       {
        !isLoggedIn ? (
          <li class="font-semibold text-gray-700">
            <a href="/login">Ingresar</a>
          </li>
          <li class="font-semibold text-gray-700"><a href="/register">register</a></li>
        ) : (
          <li id="logout" class="font-semibold text-gray-700">
            <a href="#">Salir</a>
          </li>
        )
      }


```

3. modificamoa la pagina con el astro.local `src\pages\protected.astro`:
   - nombramos todo lo de la importacion de firebase.
   - importamos las propiedades creadas:`const { user, isLoggedIn } = Astro.locals;`
   - verificamos que no esten vacias `if (!isLoggedIn || !user) {...}`
   - descomponemos user para trabajar: `const { avatar, email, emailVerified, name } = user;`
   - trabajamos con las propiedades:`` avatar ? ( <img src={avatar} alt={`Avatar de ${name}`}.../>``

- Archivo:

```
---
import MainLayout from '@layouts/MainLayout.astro';


const { user, isLoggedIn } = Astro.locals;

if (!isLoggedIn || !user) {
  return Astro.redirect('/login');
}

const { avatar, email, emailVerified, name } = user;

---

<MainLayout title="protegida app">
 ...

 {
         avatar ? (
          <img
            class="w-32 mx-auto rounded-full -mt-20 border-8 border-white"
            src={avatar}
            alt={`Avatar de ${name}`}
            height={128}
            width={128}
          /> ):...
```

# MIDDLEWARE PROTEGER RUTAS

0. modificamos middleware `src\middleware.ts`:
   - creamos array de rutas no autenticadas. `const notAuthenticatedRoutes = ['/login', '/register'];`
   - booleno `isLoggedIn` y las arrays de las rutas definimos permisos de paso:
     `if (!isLoggedIn && privateRoutes.includes(context.url.pathname))`

- Archivo:

```
const privateRoutes = ["/protected"];
const notAuthenticatedRoutes = ["/login", "/register"];

export const onRequest = defineMiddleware((context, next) => {
  const isLoggedIn = !!firebase.auth.currentUser;

...
 if (!isLoggedIn && privateRoutes.includes(context.url.pathname)) {
    return context.redirect("/");
  }

  if (isLoggedIn && notAuthenticatedRoutes.includes(context.url.pathname)) {
    return context.redirect("/");
  }
```
