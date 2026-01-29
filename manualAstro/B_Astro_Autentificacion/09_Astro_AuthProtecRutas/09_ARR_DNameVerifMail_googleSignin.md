# DISPLAY NAME Y VERIFICACION CORREO

- Verificar con correo real existente

0. modificamos `src\actions\auth\register.action.ts`:
   - actualizamos profile: `updateProfile(firebase.auth.currentUser!, { displayName: name,});`
   - se envia correo verificacion real(normalmente Spam)
   - respuesta correo despues de verificacion.
   ```
   await sendEmailVerification(firebase.auth.currentUser!, {
        url: "http://localhost:4321/protected?emailVerified=true",
      });
   ```

- FICHERO:

```
import { ...,sendEmailVerification,updateProfile} from "firebase/auth";
import { firebase } from "src/firebase/config";

handler: async ({ name, email, password, remember_me }, { cookies }) => {
...
      // Actualizar el nombre (displayName)
      updateProfile(firebase.auth.currentUser!, {
        displayName: name,
      });

      // Verificar el correo electrónico
      await sendEmailVerification(firebase.auth.currentUser!, {
        url: "http://localhost:4321/protected?emailVerified=true",
      });

      return {
        uid: user.user.uid,
        email: user.user.email,
      };
```

1. modificamos `src\pages\protected.astro`:
   - verificar ultima version usuario: `await firebaseUser?.reload();`

- Archivo:

```
---
import MainLayout from '@layouts/MainLayout.astro';
import { firebase } from 'src/firebase/config';

const firebaseUser = firebase.auth.currentUser;

....

await firebaseUser?.reload();
....
---
```

# GOOGLE SIGNIN

0. vamos a `https://console.firebase.google.com/`.
   - dashboard -- proyecto -- compilacion -- authentication -- metodo de acceso -- agregar nuevo proveedor
     - google -- habilitar-lo -- poner tu correo real -- guardar

1. accion para loginGoogle rellenamos con `a_action`en `src\actions\auth\login-google.action.ts`:
   - sera de tipo `json`: `accept: "json",`
   - el esquema sera ninguno: ` input: z.any(),`
   - trabajaremos las credenciales `handler: async (credentials)`:
     - credenciales de google. `const credential = GoogleAuthProvider.credentialFromResult(credentials);`
     - ingresamos con cred. google: `await signInWithCredential(firebase.auth, credential);`

- Fichero:

```
import { defineAction } from "astro:actions";
import { z } from "astro:schema";

import { GoogleAuthProvider, signInWithCredential } from "firebase/auth";
import { firebase } from "src/firebase/config";

export const loginWithGoogle = defineAction({
  accept: "json",
  input: z.any(),
  handler: async (credentials) => {
    const credential = GoogleAuthProvider.credentialFromResult(credentials);

    if (!credential) {
      throw new Error("Google SignIn falló");
    }

    await signInWithCredential(firebase.auth, credential);

    return { ok: true };
  },
});

```

2. añadimos en barril `src\actions\auth\index.ts`: `export * from "./login-google.action";`
3. añadimos en el archivo de server action `src\actions\index.ts`:

```
import { loginUser, loginWithGoogle, logout, registerUser } from "./auth";

export const server = {
 ...
  loginWithGoogle,
};
```

5. modificar `src\pages\login.astro`:

- clonamos y modificamos boton para google:
  - botton tipo normal `type="button"`
  - identificamos botton `id="btn-google"`
- script :
  - obtenemos botton: `const bntGoogle = document.querySelector('#btn-google') as HTMLButtonElement;`
  - disparamos botton: `bntGoogle.addEventListener('click', async () => {`
    - obtenemos provider google `const provider = new GoogleAuthProvider();`
    - obtener credencial L.cliente ` const credentials = await signInWithPopup(firebase.auth, provider);`
    - verificar credenciales backend y extraemos propiedad `error`:
      `const { error } = await actions.loginWithGoogle(credentials);`
      - trabajamos con error: `if (error) {...}`
    - si todo correcto: `window.location.replace('/protected');`

- Fichero:

```

 <div class="flex flex-1 w-full my-3">
            <div class="w-full border-t-2 border-gray-500"></div>
    </div>

    <button
            type="button"
            id="btn-google"
            class="disabled:bg-gray-300 w-full flex justify-center bg-red-400 text-gray-100 p-3 rounded-full tracking-wide font-semibold shadow-lg cursor-pointer transition ease-in duration-500"
      >
      Ingresar con Google
  </button>

<script>
...
const bntGoogle = document.querySelector('#btn-google') as HTMLButtonElement;
...
 bntGoogle.addEventListener('click', async () => {
    bntGoogle.setAttribute('disabled', 'disabled');
    const provider = new GoogleAuthProvider();

    try {
      //! Paso 1: obtener la credenciales del lado del cliente
      const credentials = await signInWithPopup(firebase.auth, provider);

      // Paso 2: Vericar las credenciales en el backend
      const { error } = await actions.loginWithGoogle(credentials);

      if (error) {
        alert(error.message);
        bntGoogle.removeAttribute('disabled');
        return;
      }

      bntGoogle.innerText = 'Redireccionando...';
      window.location.replace('/protected');
    } catch (error) {
      console.log(error);
      bntGoogle.removeAttribute('disabled');
    }
  });

</script>

```
