(astroActions)[https://docs.astro.build/en/guides/actions/]

# ACCION REGISTRO USUARIO

0. accion de registro `src\actions\auth\register.action.ts`:
   - especifico para formularios: `accept: "form",`
   - campos a validar:` input: z.object({...}),`
   - trabajar los campos. `handler: async ({ name, email, password, remember_me }) => {...return true;},`

- FICHERO:

```
import { defineAction } from "astro:actions";
import { z } from "astro/zod";

export const registerUser = defineAction({
  accept: "form",
  input: z.object({
    name: z.string().min(3),
    email: z.string().email(),
    password: z.string().min(6),
    remember_me: z.boolean().optional(),
  }),

  handler: async ({ name, email, password, remember_me }) => {
    console.log({ name, email, password, remember_me });
    return true;
  },
});
```

1. crearemos un server de hospedaje de acciones `src\actions\index.ts`:
   - importamos `registerUser,`

```
import { registerUser } from "./auth";

export const server = {
  registerUser,
};


```

2. usamos la accion para el formulario `src/pages/register.astro`:

- pones la etiqueta form : ` <form class="space-y-5">`

- añadimos al boton la `id="btn-submit"` para poder trabajar y `disabled:bg-gray-300`

` <button type="submit" id="btn-submit" class="disabled:bg-gray-300...">Ingresar</button>`

- SCRIPT:
  - trabajar con las etiquetas `document.querySelector`
  - accion apretar botton `form.addEventListener('submit', async (event)`
  - cancelar evento `event.preventDefault();`
  - obtenemos datos del formulario: `const formData = new FormData(form);`
  - pasamos y procesamos datos a la acccion:`const{data,error} = await actions.registerUser(formData)`
    - descomponemos para trabajar con los datos `const{data,error}`

```
<script >
import { actions } from "astro:actions";


        const form = document.querySelector('form') as HTMLFormElement;
        const btnSubmit = document.querySelector('#btn-submit') as HTMLButtonElement;

        form.addEventListener('submit', async (event) => {
            event.preventDefault();
            btnSubmit.setAttribute('disabled', 'disabled');
            btnSubmit!.textContent = 'Processing...';

            const formData = new FormData(form);
            const{data,error} = await actions.registerUser(formData);

            if(error){
                alert('Error: ' + error.message);
                console.log(error.message);
            }

            btnSubmit!.textContent = 'Ingresar';
            btnSubmit.removeAttribute('disabled');

        })

    </script>
```
