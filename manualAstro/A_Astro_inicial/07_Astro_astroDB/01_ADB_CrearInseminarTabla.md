(astroDB)[https://docs.astro.build/en/guides/astro-db/]
`http://localhost:4321/api/posts/first-post`

# ASTRO DB

## install

0. install,T: npx astro add db
   - todo `Y`
1. astroDB `.astro\content.db` o
   - tablePlus `"C:\Users\User\Documents\programacion2025\astro\udemy\curso_astro\05-astro-http\.astro\content.db"`

## crear BD sencilla

1. configurar BD sencilla tabla `db\config.ts`:

- creamos tabla , e importamos elementos `import {defineTable, column } from "astro:db";` :

```
const Clients = defineTable({
  columns: {
    id: column.number({ primaryKey: true }),
    name: column.text(),
    age: column.number(),
    isActive: column.boolean(),
  },
});
```

- declaramos tabla `tables: { Clients },`

- fichero:

```
import { defineDb, defineTable, column } from "astro:db";

const Clients = defineTable({
  columns: {
    id: column.number({ primaryKey: true }),
    name: column.text(),
    age: column.number(),
    isActive: column.boolean(),
  },
});

// https://astro.build/db/config
export default defineDb({
  tables: { Clients },
});
```

2. despues de crear las Tablas bajar servidor `CTRL+C` y volver a subir `NPM RUN DEV`

3. crear las semillas `db\seed.ts`:
   - llamar a la DDBB `import { Clients } from "astro:db";`
   - array insercion nombre tabla `Clients`: `await db.insert(Clients).values([...]);`
     - elemento de insercion campos tabla: ` { id: 1, name: "Kasim", age: 28, isActive: true },`
   - mensaje inseminado tabla: `console.log("Seeding database...");`

- archivo:

```
import { db, Clients } from "astro:db";

// https://astro.build/db/seed
export default async function seed() {
  await db.insert(Clients).values([
    { id: 1, name: "Kasim", age: 28, isActive: true },
    { id: 2, name: "Mina", age: 24, isActive: false },
    { id: 3, name: "Liam", age: 31, isActive: true },
    { id: 4, name: "Sophia", age: 22, isActive: true },
  ]);

  console.log("Seeding database...");
}
```

4. inserminar tabla -> bajar servidor `CTRL+C` y volver a subir `NPM RUN DEV`
5. abrir tabla con doble click para comprobar `.astro\content.db`
