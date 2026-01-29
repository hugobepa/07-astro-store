(cicloTrabaosViewTransitionJS)[https://docs.astro.build/en/guides/view-transitions/#lifecycle-events]

# MARCAR FAVORITOS

## trabajar con etiquetas mediante JS

0. añadir propiedades en al etiquetas para trabajar con js en `src\pages\pokemons\[name].astro `:

- añadimos ` data-name={name} data-id={id}>` para poder trabajar ellos.

` <button id="btn-favorite" class="ml-4 mt-4" data-name={name} data-id={id}>`:`

1. crear script de js en `src\pages\pokemons\[name].astro `:
   - creamos propiedades para poder trabajar

   ```
        interface FavoritePokemon {
      name: string;
       id: number;
   }
   ```

   - creamos un `<script>...</script>` para trabjar con JS
   - obtenemos datos de la etiqueta:

   ```
   const btnFavorite = document.querySelector('#btn-favorite') as HTMLButtonElement;

   const name = btnFavorite?.dataset.name;
   const id = btnFavorite?.dataset.id;

   ```

   - trigger para los obtener datos de la etiqueta:

   ```
       btnFavorite?.addEventListener('click', () => {
       console.log({name,id})
   });
   ```

   - para gestionar el script despues de haberlo cargado, ponemos el codigo anterior dentro:

   ```
       document.addEventListener('astro:page-load',()=>{
       ...
   });
   ```

- FICHERO:

```
<button id="btn-favorite" class="ml-4 mt-4" data-name={name} data-id={id}>
</MainLayout>
<script>

    interface FavoritePokemon {
       name: string;
        id: number;
    }

    document.addEventListener('astro:page-load',()=>{
        const btnFavorite = document.querySelector('#btn-favorite') as HTMLButtonElement;

    if(!btnFavorite) return;
    const name = btnFavorite?.dataset.name;
    const id = btnFavorite?.dataset.id;

    btnFavorite?.addEventListener('click', () => {
        console.log({name,id})
    });
    });
</script>
```

## codigo mas limpio

```
const handlePageLoad = () => {
        const btnFavorite = document.querySelector('#btn-favorite') as HTMLButtonElement;

        if(!btnFavorite) return;
       const name = btnFavorite?.dataset.name ?? '';
        const id = btnFavorite?.dataset.id ?? '';

        btnFavorite?.addEventListener('click', () => {
            console.log({name,id})
        });
    };



    document.addEventListener('astro:page-load', handlePageLoad);
```

# LOCALSTORAGE

## add remove local storage

1. añadir identificadores iconos para trabajar con js en `src\pages\pokemons\[name].astro `:
   - `data-outline` y `data-full`

   ```
     <Icon data-outline name="heart-outline" size={50}/>
    <Icon data-full class="hidden" name="heart-full" size={50}/>
   ```

2. procesamos los identicades de los iconos en `src\pages\pokemons\[name].astro `:
   - dentro `<script> const handlePageLad =()=>{...}</script>`
   - obtenemos los valores de las etiquetas:

   ```
    const heartFull = btnFavorite?.querySelector('[data-full]') as HTMLElement;
    const heartOutline = btnFavorite?.querySelector('[data-outline]') as HTMLElement;
   ```

3. obtenermos el valor de localStore:

`let favoritePokemons: FavoritePokemon[] = JSON.parse(localStorage.getItem('favorites') || '[]');`

- interactuamos con los iconos:

```
       btnFavorite?.addEventListener('click', () => {
         heartOutline.classList.toggle('hidden');
         heartFull.classList.toggle('hidden');
     });
```

- creamos funcion para guardar favoritos en localstore `const toggleFavorite = ()..`
  - sabemos favoritesLocalStore esta vacio del elemento o no: `const isFavorite = favoritePokemons.some( fav => fav.name === name );`
    - si esta vacio, lo agregamos:`  favoritePokemons.push({ name, id: +id });`
    - si esta lleno con el mismo, se eliminamos: `favoritePokemons = favoritePokemons.filter( fav => fav.name !== name );`
  - grabamos los cambios en localStore: ` localStorage.setItem('favorites', JSON.stringify(favoritePokemons));`
- disparamos esta funcion mediante el : ` btnFavorite?.addEventListener('click', () => {... toggleFavorite(); });`

```
     const toggleFavorite = () => {

           const isFavorite = favoritePokemons.some( fav => fav.name === name );
            if(isFavorite) {
                //quitar de favoritos
                favoritePokemons = favoritePokemons.filter( fav => fav.name !== name );
            } else {
                //agrègar a favoritos (+convetir a numero)
                favoritePokemons.push({ name, id: +id });
            }

            //grabar en localstorage
        localStorage.setItem('favorites', JSON.stringify(favoritePokemons));
        };



        btnFavorite?.addEventListener('click', () => {
            heartOutline.classList.toggle('hidden');
            heartFull.classList.toggle('hidden');

            toggleFavorite();
        });
```

## mostrar si corazon esta lleno

```
 //funcion mostrar o coultar corazon lleno
     if( favoritePokemons.some( fav => fav.name === name ) ) {
            heartOutline.classList.add('hidden');
            heartFull.classList.remove('hidden');
     }
```

## codigo completo

```
 <button id="btn-favorite" class="ml-4 mt-4" data-name={name} data-id={id}>
        <Icon data-outline name="heart-outline" size={50}/>
        <Icon data-full class="hidden" name="heart-full" size={50}/>
</button>

<script>

    interface FavoritePokemon {
       name: string;
        id: number;
    }


    const handlePageLoad = () => {

        let favoritePokemons: FavoritePokemon[] = JSON.parse(localStorage.getItem('favorites') || '[]');

        const btnFavorite = document.querySelector('#btn-favorite') as HTMLButtonElement;

        if(!btnFavorite) return;
        const name = btnFavorite?.dataset.name ?? '';
        const id = btnFavorite?.dataset.id ?? '';

        const heartFull = btnFavorite?.querySelector('[data-full]') as HTMLElement;
        const heartOutline = btnFavorite?.querySelector('[data-outline]') as HTMLElement;

        //funcion mostrar o coultar corazon lleno
        if( favoritePokemons.some( fav => fav.name === name ) ) {
            heartOutline.classList.add('hidden');
            heartFull.classList.remove('hidden');
        }

        const toggleFavorite = () => {

           const isFavorite = favoritePokemons.some( fav => fav.name === name );
            if(isFavorite) {
                //quitar de favoritos
                favoritePokemons = favoritePokemons.filter( fav => fav.name !== name );
            } else {
                //agrgar a favoritos (+convetir a numero)
                favoritePokemons.push({ name, id: +id });
            }

            //grabar en localstorage
        localStorage.setItem('favorites', JSON.stringify(favoritePokemons));
        };



        btnFavorite?.addEventListener('click', () => {
            heartOutline.classList.toggle('hidden');
            heartFull.classList.toggle('hidden');

            toggleFavorite();
        });
    };



    document.addEventListener('astro:page-load', handlePageLoad);
</script>
```

# EXPLICACION CCILOS DE CARGA

(cicloTrabaosViewTransitionJS)[https://docs.astro.build/en/guides/view-transitions/#lifecycle-events]

```
// antes de que la pagina empiece a cargar (loading o snipper)
    document.addEventListener('astro:before-preparation',()=>{console.log('astro:before-preparation')})
    //
    document.addEventListener('astro:after-preparation',()=>{console.log('astro:after-preparation')})

    document.addEventListener('astro:before-swap',()=>{console.log('astro:before-swap')})
    //
    document.addEventListener('astro:after-swap',()=>{console.log('astro:after-swap')})
    // despues de que la pagina se cargo
    document.addEventListener('astro:page-load',()=>{console.log('hola')})
```
