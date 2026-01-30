(guiaAstroDB)[https://docs.astro.build/en/guides/astro-db/]
(ejemploAstroDB)[https://dotmd.io/blog/explorando-astrodb/]
(enumAstroDB)[https://docs.astro.build/en/guides/integrations-guide/db/#table-configuration-reference]
(enumIssuesAstroDB)[https://github.com/withastro/roadmap/discussions/980]
(enumSuportAstroDB)[https://astro.build/blog/astro-5130/#enum-support-in-astro-db-tables]
(bcryptjs)[https://www.npmjs.com/package/bcryptjs]

# ASTRO DB

0. instalar AstroDB (todo `Y`),T: npx astro add db
1. configurar la DB `db\config.ts`:
   - configuramos la tabla de los roles `const Role = defineTable({`
   - configuramos la tabla de User `const User = defineTable({`
     - relacionamos la tabla de roles con user
       ` role: column.text({ references: () => Role.columns.id }), // admin, user, super-user`
   - exportamos tablas para ejecutar las `export default defineDb({ tables: { User, Role },`

- Archivo:

```
import { column, defineDb, defineTable } from "astro:db";

const User = defineTable({
  columns: {
    id: column.text({ primaryKey: true, unique: true }),
    name: column.text(),
    email: column.text({ unique: true }),
    password: column.text(),
    createdAt: column.date({ default: new Date() }),
    role: column.text({ references: () => Role.columns.id }), // admin, user, super-user
  },
});

const Role = defineTable({
  columns: {
    id: column.text({ primaryKey: true }),
    name: column.text(),
  },
});

// https://astro.build/db/config
export default defineDb({
  tables: { User, Role },
});
```

1. para subir los cambios, T: ctrl+c , npm run dev
   - verificar viendo la tabla `.astro\content.db`

2. install bcryptjs para encriptar password,T: `npm i bcryptjs`

3. creamos semilla `db\seed.ts`:
   - creamos roles `const roles = [..]`
   - creamos usuarios ` const johnDoe = {...}`
     - creamos UUID: `id: UUID(),`
     - encriptamos password: `password: bcrypt.hashSync("123456"),`
     - ponemos rol: `role: "admin",`
   - los insertamos: ` await db.insert(Role).values(roles); y await db.insert(User).values([johnDoe, janeDoe]);`

- Archivo:

```
import { Role, User, db } from "astro:db";
import { v4 as UUID } from "uuid";
import bcrypt from "bcryptjs";

// https://astro.build/db/seed
export default async function seed() {
  const roles = [
    { id: "admin", name: "Administrador" },
    { id: "user", name: "Usuario de sistema" },
  ];

  const johnDoe = {
    id: UUID(),
    name: "John Doe",
    email: "john.doe@google.com",
    password: bcrypt.hashSync("123456"),
    role: "admin",
  };

  const janeDoe = {
    id: UUID(),
    name: "Jane Doe",
    email: "jane.doe@google.com",
    password: bcrypt.hashSync("123456"),
    role: "user",
  };

  await db.insert(Role).values(roles);
  await db.insert(User).values([johnDoe, janeDoe]);
}
```

4. para subir los cambios, T: ctrl+c , npm run dev
   - verificar viendo la tabla `.astro\content.db`
