0. crear nuevo repositorio en GITHUB y subir proyecto repositorio desde terminal proyecto:
   - git, T: git init (inicializar)
     git add . (añadir ficheros)
     git commit -m "comit primero" (prepara ficheros para subir)
     git remote add origin https://github.com/hugobepa/astro-http.git (llamar a repositorio en la nube)
     git branch -M main (pasar a rama Main) ((? Main))
     git push -u origin main ( subir lo a repositorio web rama main) (? Main)

1. actulizar cambios github teminal proyecto:
   -T: git add .
   git commit -m "add cloudfare"
   git push

2. cambios de producion desde otra rama a main:
   -T: git checkout main (ir a rama main)
   git merge fin-section-11 (nombre ultima rama y poner todos los cambios en main)
   git push (subir cambios rama main)
