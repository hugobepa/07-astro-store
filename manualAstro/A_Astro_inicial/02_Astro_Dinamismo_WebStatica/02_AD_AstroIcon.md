# ASTRO ICON

(astroIcon)[https://www.astroicon.dev/getting-started/]
(iconify)[https://icon-sets.iconify.design/]

## instalar iconos y bajar svg

0. instalar paquete: `npx astro add astro-icon`
   - decir `Y` a las tres preguntas
     ` success  Added the following integration to your project:- astro-icon`
1. crear directorio `src\icons`
2. `https://icon-sets.iconify.design/?query=heart`
3. copiar contenido de los elegidos y por `svg` selecionados
4. crear archivo `.svg` y pegar dentro `src/icons/heart-full.svg `

## utilizar Icon y svg

### utilizacion simple

0. añadimos el `svg` mediante `icon` a `src\pages\pokemons\[name].astro`:

- poner la etiqueta `Icon`
- poner la `svg` por el nombre del archivo creado `heart-full`

```
import { Icon } from 'astro-icon/components';

<button>
            <Icon name="heart-full"  size={50}/>
</button>
```

### utilizacion compleja

0. añadimos el `svg` mediante `icon` a `src\pages\pokemons\[name].astro`:

- poner la etiqueta `Icon`
- poner la `svg` por el nombre del archivo creado `heart-full`
- damos estilo al grupo de botones por `id`:

  ```
    <style>
    @reference '../../styles/global.css';
    @import '../../styles/global.css';
    ...
    #btn-favorite {
        @apply hover:animate-pulse;
    }
    </style>
  ```

- usar estilo del botton: `<button id="btn-favorite" class="ml-4 mt-4">`
- ocultamos un icono

- archivo:

```
import { Icon } from 'astro-icon/components';
.....
<MainLayout>

<div >

    <div class="flex items-center justify-center">
        <button onclick="history.back()" class="text-blue-500 hover:underline ml-4">regresar</button>
    </div>
    <Title>{name}</Title>
    </div>

    <button id="btn-favorite" class="ml-4 mt-4">
        <Icon name="heart-outline" size={50}/>
        <Icon class="hidden" name="heart-full" size={50}/>
    </button>
</div>
....

</MainLayout>

<style>
    @reference '../../styles/global.css';
    @import '../../styles/global.css';
    ...
    #btn-favorite {
        @apply hover:animate-pulse;
    }
</style>

```
