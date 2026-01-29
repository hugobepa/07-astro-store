(firebase)[https://firebase.google.com/?authuser=1&hl=es-419]
(authAstro)[https://docs.astro.build/en/guides/authentication/]

0. firebase--go to console -- crear proyecto nombre: (astro-authentication) -- no habilitar analytic
   - continuar -- agregar app -- icono web
   - nombre: Astro Auth -- no hosting --registrar applicacion
     - install, T: npm install firebase
       archivo:

     ```
         // Import the functions you need from the SDKs you need

         import { initializeApp } from "firebase/app";

         // TODO: Add SDKs for Firebase products that you want to use

         // https://firebase.google.com/docs/web/setup#available-libraries


         // Your web app's Firebase configuration

         const firebaseConfig = {

         apiKey: "xxxxxx",

         authDomain: "astro-authentication-xxx.firebaseapp.com",

         projectId: "astro-authentication-xxx",

         storageBucket: "astro-authentication-xxxx.firebasestorage.app",

         messagingSenderId: "xxxxxx",

         appId: "1:xxxx:web:xxxxx"

         };


         // Initialize Firebase

         const app = initializeApp(firebaseConfig);
     ```

1. creamos arch config firebase `src\firebase\config.ts`:
   - copiar todo el codigo anterior
   - añadimos :
     - para obtener auth. ` const auth = getAuth(app);`
     - exportamos constanates: ` export const firebase = {app,auth,};`

   ```
    const auth = getAuth(app);
        auth.languageCode = "es";

        export const firebase = {
         app,
        auth,
        };
   ```

- Fichero:

```
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
         // TODO: Add SDKs for Firebase products that you want to use

         // https://firebase.google.com/docs/web/setup#available-libraries


         // Your web app's Firebase configuration

         const firebaseConfig = {

        .....

         };


         // Initialize Firebase

         const app = initializeApp(firebaseConfig);

         const auth = getAuth(app);
        auth.languageCode = "es";

        export const firebase = {
         app,
        auth,
        };
```

2. firebase web -- ir a consola --Pag proyecto --- settings -- general
   ---- `>` -- compilacion -authentificacion
   - comenzar: - correo electronico/contraseña habilitar

3. hacer Test: subir usuario a firebase `src\actions\auth\register.action.ts`:
   - creamos usuario  
     ` const user = await createUserWithEmailAndPassword(firebase.auth,email,password,);`
   - devolvemos usuario:
     `return { uid: user.user.uid, email: user.user.email,};`

- Archivo:

```
import { createUserWithEmailAndPassword } from "firebase/auth";
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
      console.log({ error });
      throw new Error("Error registering user");
    }

    return { ok: true, msg: `User ${name} registered successfully` };
```
