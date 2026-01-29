(creditCard)[https://www.creative-tim.com/twcomponents/component/profile-card-13]
(avatarAstro)[https://gist.github.com/Klerith/f86dc9e046afb6ade1c4626ef73bcda4]

0. modificacion `src\pages\protected.astro`:
   - **IMPORTANTE EXTRACION DATOS PROVISIONAL DESPUES SE PASAN X MIDDLEWARE**
   - extraemos usuario firebase: `const firebaseUser = firebase.auth.currentUser;`
   - si viene usuario null se redireciona: `throw Astro.redirect('/login');`
   - extraemos los datos del firebaseuser para trabajarlos:
     `const { displayName, email, emailVerified, photoURL } = firebaseUser;`
   - `displayName` puede venir nulo, le damos algun valor:`const name = displayName ?? 'No display Name';`
   - borramos contenido y pegamos el codido de estas fuentes:
     - (creditCard)[https://www.creative-tim.com/twcomponents/component/profile-card-13]
     - (avatarAstro)[https://gist.github.com/Klerith/f86dc9e046afb6ade1c4626ef73bcda4]
   - trabajamos con datos en la card:
     - src={photoURL}, alt={`Avatar de ${name}`},{name.substring(0, 2)}
     - {emailVerified ? 'Email verificado' : 'Email no verificado'}

     -Archivo:

```
---
import MainLayout from '@layouts/MainLayout.astro';
import { firebase } from 'src/firebase/config';

const firebaseUser = firebase.auth.currentUser;

if (firebaseUser === null) {
  return Astro.redirect('/login');
}

await firebaseUser?.reload();
const { displayName, email, emailVerified, photoURL } = firebaseUser;

const name = displayName ?? 'No display Name';
---

<MainLayout title="protegida app">
   <!-- component -->
<div class="bg-gray-200 font-sans rounded-2xl h-125 w-full flex flex-row justify-center items-center">
  <div class="card w-96 mx-auto bg-white  shadow-xl hover:shadow rounded-2xl">

 {
        photoURL ? (
          <img
            class="w-32 mx-auto rounded-full -mt-20 border-8 border-white"
            src={photoURL}
            alt={`Avatar de ${name}`}
            height={128}
            width={128}
          />
        ) : (
          <div class="w-32 h-32 mx-auto rounded-full -mt-20 border-8 border-white bg-gray-300 flex justify-center items-center">
            <span class="text-white text-3xl font-extrabold">
              {name.substring(0, 2)}
            </span>
          </div>
        )
      }
    }



     <div class="text-center mt-2 text-3xl font-medium">{name}</div>
     <div class="text-center mt-2 font-light text-sm">{email}</div>
     <div class="text-center font-normal text-lg">
         {emailVerified ? 'Email verificado' : 'Email no verificado'}
     </div>
```
