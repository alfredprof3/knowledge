#type/Workflow #topic/Github/Workflow #for/Github 

# GitHub Flow: Tu Día a Día

Este es el ciclo de vida que los equipos de ingeniería utilizan para añadir código nuevo sin romper el entorno de producción (tu herramienta estable).

> [!steps]
> 1. Regla de oro: main es intocable
>    
>    La rama principal debe ser siempre desplegable
>    Nunca programes directamente en `main`. Esta rama representa la versión oficial que tus usuarios están descargando.
>    
> 1. Crear una rama de trabajo (Feature branch)
>    
>    Cuando vayas a crear algo nuevo o arreglar un bug, aíslalo creando una rama nueva desde `main`: `git checkout -b feat/auto-sync` o `git checkout -b fix/macos-keyring`.
>    
> 1. Commits atómicos
>    
>    Guarda los cambios lógicos en fragmentos pequeños usando los prefijos convencionales: `git commit -m "feat: integrar libsecret para linux"`.
>    
> 1. Merge (Pull Request)
>    
>    Una vez probada tu rama localmente, la envías a GitHub (`git push origin feat/auto-sync`), abres un _Pull Request_ (PR) en la interfaz web, revisas los cambios y la fusionas hacia `main`.
