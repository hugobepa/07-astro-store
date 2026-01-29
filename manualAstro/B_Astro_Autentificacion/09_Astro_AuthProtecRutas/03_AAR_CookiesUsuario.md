(cookiesAstro)[https://docs.astro.build/es/reference/api-reference/#cookies]

0. add propiedad `name="email"` a `input` correspondiente en `src\pages\register.astro`.

1. mod x cookies `src\actions\auth\register.action.ts`:
   - destructuramos las propiedades para poder trabajar:
     - input: `{ name, email, password, remember_me }`
     - context: `{ cookies }`
     - comprobamos si remember_me `true o false`
       - false, eliminamos la cookie espec y su path `cookies.delete("email", { path: "/" });`
       - true, creamos cookie: `cookies.set(`
         - nombre y campo guardar `"email", email,`
         - duracion cookie `expires: new Date(Date.now() + 1000 * 60 * 60 * 24 * 30), // 30 dias`
         - Opcional el path ` path: "/",`
   - creamos menssaje exito: ``return { ok: true, msg: `User ${name} registered successfully` };``

- ARCHIVO:

```
handler: async ({ name, email, password, remember_me }, { cookies }) => {
    if (remember_me) {
      cookies.set("email", email, {
        expires: new Date(Date.now() + 1000 * 60 * 60 * 24 * 30),
        path: "/",
      }); // 30 days
    } else {
      cookies.delete("email", { path: "/" });
    }

    return { ok: true, msg: `User ${name} registered successfully` };
```

2. hacemos persistente datos de cookie en `src\actions\auth\register.action.ts`:
   - importamos datos de cookie `const email = Astro.cookies.get("email")?.value ?? "";`
   - creamos un `true` de remember con la `!!`
   - trabajamos con los datos importados y creados: `<input value={email}` y `<input checked={rememberMe}`

- Archivo:

```
---
import AuthLayout from "@layouts/AuthLayout.astro";

const email = Astro.cookies.get("email")?.value ?? "";
const rememberMe = !!email;
console.log({email, rememberMe});
---

<input value={email} -- input normal
<input checked={rememberMe} --input chequed

```
