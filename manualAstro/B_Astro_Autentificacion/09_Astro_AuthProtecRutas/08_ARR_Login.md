# LOGIN

0. creamos accion para login y rellenamos `a_action` `src\actions\auth\login.action.ts`:
   - configuramos los campos como formulario `accept: "form",input: z.object({...}),`
   - trabajamos propiedades config. y cookies ` handler: async ({ email, password, remember_me }, { cookies })`
     - creamos cookies o la eliminamos: `if (remember_me) {...}else{...}`
     - hacemos login con las propiedades:
       ` const user = await signInWithEmailAndPassword(firebase.auth,email,password,);`
       - devolvemos user con formato json: ` return { uid: user.user.uid,email: user.user.email,};`
     - customizamos error `const firebaseError = error as AuthError;`

- FICHERO:

```
import { defineAction } from "astro:actions";
import { z } from "astro/zod";
import { firebase } from "src/firebase/config";
import {
  signInWithEmailAndPassword,
  type Auth,
  type AuthError,
} from "firebase/auth";

export const loginUser = defineAction({
  accept: "form",
  input: z.object({
    email: z.string().email(),
    password: z.string().min(6),
    remember_me: z.boolean().optional(),
  }),
  handler: async ({ email, password, remember_me }, { cookies }) => {
    // Cookies
    if (remember_me) {
      cookies.set("email", email, {
        expires: new Date(Date.now() + 1000 * 60 * 60 * 24 * 365), // 1 año,
        path: "/",
      });
    } else {
      cookies.delete("email", {
        path: "/",
      });
    }

    try {
      const user = await signInWithEmailAndPassword(
        firebase.auth,
        email,
        password,
      );

       return {
        uid: user.user.uid,
        email: user.user.email,
      };
    } catch (error) {
      const firebaseError = error as AuthError;

      if (firebaseError.code === "auth/email-already-in-use") {
        throw new Error("El correo electrónico ya está en uso.");
      }
      console.error({ error });
      throw new Error("No se pudo ingresar usuario");
      return;
    }
  },
});

```

1. añadimos al archivo de barril `src\actions\auth\index.ts`:

```
export * from "./login.action";
export * from "./logout.action";
export * from "./register.action";
```

2. importamos al index de acciones `src\actions\index.ts`:

```
import { loginUser, logout, registerUser } from "./auth";

export const server = {
  registerUser,
  logout,
  loginUser,
};
```

3. importamos y modificamos datos accion a pagina `src\pages\login.astro`:
   - importamos cookie: `const email = Astro.cookies.get('email')?.value ?? '';`
   - pasamos el valor a input check `const rememberMe = !!email;`
   - crear formulario: ` <div class="space-y-5">` L-26 a ` <form class="space-y-5">`
     - definimos, identificamos y pasamos valor en email: `<input type="email" name="email" value={email}`
     - definimos e identificamos password: `<input type="password"name="password"`
     - definimos,identificmos y damos valor check:
       `<input id="remember_me" name="remember_me" type="checkbox" checked={rememberMe}`
     - definimos,identificamos y desabilitamos boton: `<button id="btn-submit" type="submit"  class="disabled:`
   - script:
     - obtenemos valores de los campos:
       ```
       const form = document.querySelector('form') as HTMLFormElement;
       const btnSubmit = document.querySelector('#btn-submit') as HTMLButtonElement;
       ```
     - disparamos el evento `form.addEventListener('submit', async (e) => {`:
       - obtenemos datos `const formData = new FormData(form);`
       - extraemos error: `const { error } = await actions.loginUser(formData);`
         - trabajamos con error `if (error) { Swal.fire({...})...}`
       - si es exito, trabajamos con el: ` window.location.replace('/protected');`

- Fichero:

```
---
import AuthLayout from "@layouts/AuthLayout.astro";

const email = Astro.cookies.get('email')?.value ?? '';
const rememberMe = !!email;
---

 <form class="space-y-5">
  <input type="email" name="email" value={email}
  <input type="password"name="password"
  <input id="remember_me" name="remember_me" type="checkbox" checked={rememberMe}

  <button id="btn-submit" type="submit"  class="disabled:bg-grey-300....>Ingresar</button>
 </form>

<script>
import { actions } from "astro:actions";
import Swal from 'sweetalert2';


 const form = document.querySelector('form') as HTMLFormElement;
 const btnSubmit = document.querySelector('#btn-submit') as HTMLButtonElement;

 form.addEventListener('submit', async (e) => {
    e.preventDefault();
    btnSubmit.setAttribute('disabled', 'disabled');
    btnSubmit!.textContent = 'Processing...';

    const formData = new FormData(form);

    const { error } = await actions.loginUser(formData);

     if (error) {
      Swal.fire({
        icon: 'error',
        title: 'Error al ingresar',
        text: error.message,
      });
       btnSubmit.removeAttribute('disabled');
        btnSubmit!.textContent = 'Ingresar';
      return;
    }

    window.location.replace('/protected');
 } );

</script>
```
