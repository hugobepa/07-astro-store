(islas)[https://docs.astro.build/en/guides/integrations-guide/]
(solid)[https://www.solidjs.com/]
(astroSolid)[https://docs.astro.build/en/guides/integrations-guide/solid-js/]
(directivasTemplate)[https://docs.astro.build/en/reference/directives-reference/#client-directives]
(viewTransition)[https://docs.astro.build/en/guides/view-transitions/]

# ASTRO ISLA SOLID

## instalacion y prueba

0. instalar adaptador, T: `npx astro add solid`
   - 3 `Y`
   - `success  Successfully updated TypeScript settings`
1. crear y rellenamos componente Solid `src\components\Counter.tsx`:

```
import { createSignal } from "solid-js";

export const Counter = () => {
  const [counter, setCounter] = createSignal(0);
  return (
    <div>
      <h1 class="text-4xl">Counter</h1>
      <h3 class="text-xl">Value:{counter()}</h3>
      <button
        class="bg-blue-500 p-2 mr-2"
        onClick={() => setCounter((prev) => ++prev)}
      >
        +1
      </button>
      <button
        class="bg-blue-500 p-2 mr-2"
        onClick={() => setCounter(counter() - 1)}
      >
        -1
      </button>
    </div>
  );
};
```

2. añadimos componente Solid `src\pages\favorites\index.astro`:
   - importamos componente `<Counter client:load>`
   - añadimos directriz para que se carge y funcione isla ` client:load`

- archivo:

```
--
import { Counter } from "@components/Counter";
---
<MainLayout title="Favorites">
...
<Counter client:load>
</MainLayout>
```

## props y componentes hacia isla

### props components unitarios

0. añadimos properties al componente `src\components\Counter.tsx`:
   - creamos propiedades: `interface Props { initialValue?: number;}`
   - la llamamos desde la funcion: `export const Counter: Component<Props> = (props) `
   - trabajamos desde la funcion: `createSignal(props.initialValue ?? 0);`

```
import { createSignal, type Component } from "solid-js";

interface Props {
  initialValue?: number;
}

export const Counter: Component<Props> = (props) => {
  const [counter, setCounter] = createSignal(props.initialValue ?? 0);
  ...
};
```

1. add valores iniciales al component `src\pages\islands\index.astro`:
   - añadimos un valor inicial `const counterValue = 10;`
   - lo añadimos al component: `initialValue={counterValue}`

```
---
import { Counter } from "@components/Counter";
import Title from "@components/shared/Title.astro";
import MainLayout from "src/layout/MainLayout.astro";

const counterValue = 10;
---
<MainLayout title="Islas">
 <Title>Islas</Title>
....

<Counter client:visible initialValue={counterValue} />
</MainLayout>
```

### compartir diferentes components mismo valor.

- comparten valor la misma compenentes en diferentes pagina:`transition:persist="counter"`
- persiste el valor de la prop: `#transitionpersist-props`

```
<Counter
transition:persist="counter"
transitionpersist-props
client:visible initialValue={counterValue} />
</MainLayout>
```

### video

```
<video controls="" autoplay="" transition:persist="playing-video">
    <source
      src="https://ia804502.us.archive.org/33/items/GoldenGa1939_3/GoldenGa1939_3_512kb.mp4"
      type="video/mp4"
    />
  </video>
```

### enviar componente desde astro a la isla

0. modificar isla componente `src\components\Counter.tsx`:

- añades prop children: `interface Props { children?: JSX.Element;}`
- la configuro en el componente:` <div> {props.children}`

```
interface Props {
  initialValue?: number;
  children?: JSX.Element;
}

export const Counter: Component<Props> = (props) => {
  const [counter, setCounter] = createSignal(props.initialValue ?? 0);
  return (
    <div>
      {props.children}
```

1. usarla comp isla en pagina con comp astro: `<Counter ...><Title>Counter inside Islands</Title></Counter>`

### añadir-view transition a un compement isla

`` <img style={`view-transition-name: ${pokemon.name}-image`}``

### tipos directrices components isla

(viewTransition)[https://docs.astro.build/en/guides/view-transitions/]

0. directrices de props y valores:

`<video controls="" autoplay="" transition:persist="playing-video">`

`<Counter client:load transitionpersist-props transition:persist="counter"`:

- comparten valor la misma compenentes en diferentes pagina:`transition:persist="counter"`
- persiste el valor de la prop: `#transitionpersist-props`

1. directrices de carga:

`<ClientComponent client:xxxx>`

- carga rapida y prioritaria: `client:load`
- carga normal, se carga cuando la pagina esta cargada: `client:idle` o `client:idle={{timeout: 500}}`
- solo se carga cuando se ve esa parte de la pagina: `client:visible`
- con margen gracia para que entre en pantalla : `client:visible={{rootMargin: "200px"}}`
- cargar segun el dispositivo: `client:media="(max-width: 50em)"`
- como el load pero solo en el lado cliente -puro client- (especificar framework)(evitar error localstore): `client:only="react"`

### añadir view-transition a un compement isla

`` <img style={`view-transition-name: ${pokemon.name}-image`}``
