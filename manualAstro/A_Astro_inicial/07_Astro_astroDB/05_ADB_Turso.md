(tutorialTurso)[https://www.youtube.com/watch?v=-uNWDCr9mMk]
(install Curl)[https://inventivehq.com/blog/how-to-install-curl-on-windows-complete-guide-with-5-methods]
(wsl-LinuxOnWindows)[https://learn.microsoft.com/en-us/windows/wsl/install]
(tursoClient)[https://docs.turso.tech/cli/installation]
(turso)[https://docs.astro.build/en/guides/astro-db/#connect-a-libsql-database-for-production]
(tursoAutenhication)[https://docs.turso.tech/cli/authentication]
(tursoAstro)[https://docs.turso.tech/sdk/ts/guides/astro]
(githubExample)[https://github.com/tursodatabase/examples/tree/master/app-tustro-blog]
(tursoDB)[https://app.turso.tech]
(videoInstallAstrodbTursobuena)[https://www.youtube.com/watch?v=QIZ1Mg1q6fc]
(pagianOficialInstalacion)[https://docs.astro.build/en/guides/astro-db/#connect-a-libsql-database-for-production]

# TURSO

## preconfig inicial

0. instalar `wsl` en windows, powershell admin:
   - instalar, P: `wsl --install`
   - ver distros,P: `wsl.exe --list --online`
   - instalar distro especifica `wsl.exe --install [Distro]`

1. verificar que wsl este instalado y que sea ubuntu, powershell admin:
   - P: `wsl -l -v` o `wsl -l`
   - sino tienes la de ubuntu instalar,P: `wsl --install -d ubuntu ` (Ubunt)
     - crear usuario y contrasema unix
   - recomendable reiniciar windows y powershell admin
     - verificar ahora la instalacion,P: `wsl -l -v` o `wsl -l`
     - iniciar, virtual Ubuntu,P: `wsl`
     - actualizar paquetes,WSL: `sudo apt update`
     - install curl,WSL: `sudo apt install curl -y`
     - verificar instalacion curl WSL: `curl --version`
   - OPCIONAL verificar docker desktop:
     - inicializarlo
     - Docker Desktop → Settings → Resources → WSL Integration → Ubuntu activado
   - apagar W e inicilizar para instalacion se configure `WSL-UB` y encender y probar Docker Desktop- containers

## config turso

0.  instalar turso, PowershellAdm: - P: wsl - instalar TURSO, P: `curl -sSfL https://get.tur.so/install.sh | bash` - reiniciar Powershell admin haga efecto - ver instalacion, P: `turso -version`

### WSL

0. iniciar, virtual Ubuntu,P: `wsl`

1. si con `turso auth xxx` sale error `exec: “xdg-open,x-www-browser,www-browser,wslview”: executable file not found in $PATH`:(solucionWeb)[https://medium.com/@kanrangsanwsl-error-exec-xdg-open-x-www-browser-www-browser-wslview-executable-file-not-found-in-path-3fdea92e4ed5]

- WSL: `sudo add-apt-repository ppa:wslutilities/wslu`
- WSL: `sudo apt update`
- WSL: `sudo apt install wslu`

2. autentificacion:
   - login, WSL: `turso auth login` o `turso auth login --headless`
   - sign up,WSL: `turso auth signup` o `turso auth signup --headless`
     - ir a `https://api.turso.tech/signup`
   - logout, WSL: `turso auth logout`

## creacion BBDD TURSO

(quickstart)[https://docs.turso.tech/api-reference/quickstart]
(cliTurso)[https://docs.turso.tech/cli/db/import]

- ir a pryecto,P: `cd "C:\Users\User\Documents\programacion2025\astro\udemy\curso_astro\05-astro-http"`
- abrir WSL,PP: `WSL`
- logearse TURSO, WSL: `turso auth login` (`turso auth logout`)

- crear DDBB, WSL: `turso db create 05-database`

aparece:

```
Start an interactive SQL shell with:

   turso db shell 05-database

To see information about the database, including a connection URL, run:

   turso db show 05-database

To get an authentication token for the database, run:

   turso db tokens create 05-database

   Path to the directory with config file
```

- ver DDBB, WSL: `turso db show 05-database`
  - copiar url:` libsql://05-database-userName.aws-eu-west-1.turso.io`
- VS, proyecto:
  - crear .env y .env.template y añadir segun toque(. template no poner dirrecion solo variable):

  ```
  #https://app.turso.tech/05-database
   ASTRO_DB_REMOTE_URL=libsql://05-database-userName.aws-eu-west-1.turso.io`
  ```

  - `.gitignore` add: `.env`
    min 4:31

- crear token BBDD y copiar lo, WSP: `turso db tokens create 05-database`:
- add en `.env` y `.env.template`,VS:`ASTRO_DB_APP_TOKEN=xxxxxxx`
- ir a pryecto,P: `cd "C:\Users\User\Documents\programacion2025\astro\udemy\curso_astro\05-astro-http"`
- abrir WSL,PP: `WSL`
- logearse TURSO, WSL: `turso auth login` (`turso auth logout`)
- si tenemos tablas hechas se suben al remoto, T proyecto: `npx astro db push --remote`
- ir `https://app.turso.tech/` i verificar la subida
  - aparecera la tabla pero vacio, rellenamos una row para verificar comunicacion
- crear comando conexion DB VS,`package.json` add ` "dev:remote": "astro dev --remote",`:

```
"scripts": {
    "dev": "astro dev",
    "dev:remote": "astro dev --remote",
```

- connectar con tursoDB: `npm run dev:remote`
- POSTMAN: get `http://localhost:4321/api/clients/`
- crear comando construcion remote DB VS,`package.json` add ` "build:remote": "astro build --remote",`:

```
"scripts": {
    "dev": "astro dev",
    "build:remote": "astro build --remote",
```

- construir con tursoDB: `npm run build:remote`

- crear comando de enviar DB VS,`package.json` add ` "db:push": "astro db push --remote"` y
  `"build:push": "npm run db:push && astro build --remote"`:

```
 "scripts": {
    ...
    "db:push": "astro db push --remote",
    "build:push": "npm run db:push && astro build --remote"
```

```
turso db shell mi-db
# luego ejecutar SELECT ... para comprobar tablas/datos
```

- descansar, WSP: `turso relax`
