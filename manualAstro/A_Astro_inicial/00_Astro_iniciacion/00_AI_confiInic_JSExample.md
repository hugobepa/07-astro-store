https://docs.astro.build/en/basics/project-structure/
https://docs.astro.build/en/reference/cli-reference/#astro-preferences

0. Router - File based routing:

| .astro, .md, .html |
| ------------------ | --------------------- |
| .mdx               | requiere instegracion |
| .js,.ts            | API endpoints         |

0. crear carpeta, con nombre de proyecto.
1. instalar astro, T: npm create astro@latest .
   - empty
   * typersctipt -- yes
   * typescript strict -- yes
   * install depedencies -- yes
   * git repository -- yes
2. desabilitar barra astro, T: npm run astro preferences disable devToolbar

   - se crea en ".astro\settings.json":

   ```
   "devToolbar": {
   	"enabled": false
   }
   ```

   - para volver a ver eliminar esta orden o ponerla en `true`

3. ejemplo js sencillo marca año en astro " src\pages\index.astro":

```
<body>
<footer></footer>
		<script>
         //año
			const currentYear = new Date().getFullYear();
			const footer = document.querySelector('footer') as HTMLElement;
			footer.innerHTML =  `Current year: ${currentYear}`;
		</script>
	</body>
```

4. ejemplo js sencillo hora actual en astro " src\pages\index.astro":

```
<body>
		<h1>Astro</h1>

		 <p id="current-time"></p>

<script>
			//reloj actual
			const currentTimeParagraph = document.getElementById('#current-time') as HTMLParagraphElement;
			currentTimeParagraph.innerHTML += `Momento actual real: ${new Date().toLocaleTimeString()}`;
		</script>
	</body>
```

5. probarlo, T: ` npm run preview`
