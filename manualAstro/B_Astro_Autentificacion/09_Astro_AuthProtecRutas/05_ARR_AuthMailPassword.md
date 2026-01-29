(sweetAlert2)[https://www.npmjs.com/package/sweetalert2]

# AUTH MAIL PASSWORD

0. subir usuario a firebase `src\actions\auth\register.action.ts`:
   - creamos usuario  
     ` const user = await createUserWithEmailAndPassword(firebase.auth,email,password,);`
   - devolvemos usuario formato json:
     `return { uid: user.user.uid, email: user.user.email,};`
   - error usuario creado:

   ```
     const firebaseError = error as AuthError;

     if (firebaseError.code === "auth/email-already-in-use") {
       throw new Error("El correo ya está en uso"
   ```

- Archivo:

```
import { createUserWithEmailAndPassword, type AuthError } from "firebase/auth";
import { firebase } from "src/firebase/config";


try {
      const user = await createUserWithEmailAndPassword(
        firebase.auth,
        email,
        password,
      );
      //actualizar el nommbre
      //verificar el correo elecronico
      return {
        uid: user.user.uid,
        email: user.user.email,
      };

      //return user;
    } catch (error) {
       const firebaseError = error as AuthError;

      if (firebaseError.code === "auth/email-already-in-use") {
        throw new Error("El correo ya está en uso");
      }
      throw new Error("Error registering user");
    }

    return { ok: true, msg: `User ${name} registered successfully` };
```

1. modificamos `src\pages\register.astro`:
   - crear alerta error:
     - install `sweetalaert2`,T: npm i sweetalert2
     - `src\pages\register.astro`:
     - creamos alerta: `Swal.fire({icon: 'error',title: 'Oopss...',text: error.message,});`
     - propiedades boton ` btnSubmit!.`
     - pararar accion `return`

- registro exitoso enviamos a web: `window.location.replace('/protected');`
  - Fichero:

  ```
    <script >
    import { actions } from "astro:actions";
    import Swal from 'sweetalert2';

          ......

               if (error) {
                console.log({error})
              Swal.fire({
            icon: 'error',
             title: 'Credenciales no son correctas',
            text: error.message,
              });

         btnSubmit.removeAttribute('disabled');
          btnSubmit!.textContent = 'Registrarse';
         return;
        }

    window.location.replace('/protected');
  ```
