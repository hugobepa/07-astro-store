# LOGOUT

0. crear `src\actions\auth\logout.action.ts` y relenar `a_accion`:
   - eliminamos ` input: z.string(),` pero tenemos que mantener `accept: "json",`
   - al eliminar el `input` tenemos que poner `_` para llamar otras propiedades
     `handler: async (_, { cookies }) => {`
   - cerramos la sesion pasandole nuestra identificador ` return await signOut(firebase.auth);`

- Fichero:

```
import { defineAction } from "astro:actions";
import { z } from "astro/zod";
import { signOut } from "firebase/auth";
import { firebase } from "src/firebase/config";

export const logout = defineAction({
  accept: "json",
  handler: async (_, { cookies }) => {
    return await signOut(firebase.auth);
  },
});
```

1. archivo de barril `src\actions\auth\index.ts`:

```
export { registerUser } from "./register.action";
export * from "./logout.action";
```

2. importar accion para poder trabajar `src\actions\index.ts`:

```
import { logout, registerUser } from "./auth";

export const server = {
  registerUser,
  logout,
};

```

3. implementamos la accion `src\components\shared\Navbar.astro`:
   - damos un `id` a la etiqueta para poder trabajar:`<li id="logout"`
   - creamos script para trabajar con el:
     - llamamos etiqueta ` const logoutElem = document.querySelector('#logout') as HTMLLIElement;`
     - disparamos evento de asyncrono `logoutElem?.addEventListener('click', async () `.
       - llamamos accion para recibir promesas `await actions.logout();`
       - enviamos a home `window.location.href = '/';`

- FICHERO

```
 <li id="logout" class="font-semibold text-gray-700"><a href="#">logout</a></li>

 <script>
  import { actions } from 'astro:actions';

  const logoutElem = document.querySelector('#logout') as HTMLLIElement;

  logoutElem?.addEventListener('click', async () => {
     await actions.logout();
    window.location.href = '/';
  });
</script>
```
