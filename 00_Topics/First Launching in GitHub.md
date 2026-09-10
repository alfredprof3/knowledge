#type/GitHub/Project #topic/Github/First-Launching #for/Github 

# Preparar tu Primer Lanzamiento (v1.0.0)

Para que los usuarios puedan descargar el código, GitHub usa **Releases**, los cuales están anclados a "Etiquetas" (Tags) de Git bajo el formato SemVer (Mayor.Menor.Parche).

> [!steps]
> 1. Inicializar el repositorio base
>    
>    Asegúrate de estar en la carpeta del código fuente de Syncline (separada de tu carpeta de notas personales) y sube tu código actual a `main`:
>    
> 	```
> 	git init
> 	git add .
> 	git commit -m "chore: initial commit of syncline v1.0.0"
> 	git branch -M main
> 	git remote add origin https://github.com/alfredprof3/syncline.git
> 	git push -u origin main
> 	```
> 
> 2. Crear la etiqueta de versión (Tag)
>    
>    Marca este punto exacto en el historial como tu versión 1.0.0 oficial.
>    
> 	```
> 	git tag -a v1.0.0 -m "Initial public release"
> 	git push origin v1.0.0
> 	```
> 
> 3. Publicar en GitHub Releases
>    
>    Ve a la página de tu repositorio en GitHub. En la barra lateral derecha, haz clic en **Releases** > **Create a new release**. Selecciona tu tag `v1.0.0`. Escribe un título atractivo (ej. "Syncline v1.0.0 - Initial Release") y añade notas describiendo qué hace tu script. Haz clic en **Publish release**.
