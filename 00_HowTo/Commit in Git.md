#type/HowTo #topic/Git/Commits/Standard-Practices #for/Git 

# El Estándar de la Industria: Conventional Commits

En proyectos corporativos, no escribimos mensajes de commit al azar. Se utiliza una convención estricta que permite generar historiales de cambios (changelogs) automáticamente y facilita la revisión de código.

|Prefijo|Propósito|Ejemplo|
|---|---|---|
|**feat:**|Una nueva característica para el usuario|`feat: añadir opción de sincronización SSH`|
|**fix:**|Solución de un bug o error|`fix: reparar fallo silencioso del keychain en macOS`|
|**docs:**|Cambios en la documentación|`docs: agregar guía de instalación en Linux`|
|**refactor:**|Cambio de código que no añade funciones ni corrige bugs|`refactor: consolidar comandos clear y cancel`|
|**chore:**|Tareas de mantenimiento, dependencias o configuración|`chore: actualizar script instalador`|