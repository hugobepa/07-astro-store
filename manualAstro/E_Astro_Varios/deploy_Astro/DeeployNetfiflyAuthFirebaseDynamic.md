(env)[https://docs.astro.build/es/guides/environment-variables/]
(netlify)[web/netiflyhttps://www.netlify.com/]
(firebase)[https://console.firebase.google.com]

# DEPLOY NETLIFY

0. add `.env` en `.gitignore`
1. creamos `.env` y `.env.template` y add:

```
WEBSITE_URL=http://localhost:4321
```

2. ponemos variable `src\actions\auth\register.action.ts`

```
   await sendEmailVerification(firebase.auth.currentUser!, {
        //url: "http://localhost:4321/protected?emailVerified=true",
        url: `${import.meta.env.WEBSITE_URL}/protected?emailVerified=true`,
      });
```

3. subir repositorio: git commit -m "xxxx", git push
4. (netlifyWeb)- dashboard -- add new site -- import an existing project -- github -nombre repositorio:
   - name: astro-authentication , Public directory: dist, add
     - enviroment variables: WEBSITE_URL https://astro-auhtentication.netlify.app
     - new variable para asegurar
   - deploy astro-Authentication
   - site overview (lateral) - entrar url
   - ver error: Logs - functions - Astro SSR - cambiar Real time a last hour
     - problemas de configuracion de auth solucion ir firebase
5. webfirebase/dashboard/project/compilation/authentication/configuracion
   - dominios autorizados -- (agregar un dominio): https://astro-auhtentication.netlify.app
6. cambiar en `src/pages/protected.astro`:
   - cambiar `verifiedEmail`:
     - eliminamos `emailVerified` de `user`: `const { avatar, email, name } = user;`
     - llamamos ala usuario de firebase:` const firebaseUser = firebase.auth.currentUser;`
     - descomentamos. `await firebaseUser?.reload();`
     - descomponemos ` emailVerified` de `firebaseUser`: `const { emailVerified } = firebaseUser;`

   ```
   ---
    const { avatar, email, name } = user;

    const firebaseUser = firebase.auth.currentUser;

    await firebaseUser?.reload();
    const { emailVerified } = firebaseUser;
    ---
   ```

7. subir cambios github
