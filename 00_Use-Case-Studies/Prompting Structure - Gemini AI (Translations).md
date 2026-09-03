#type/Use-Case-Study #topic/Gemini-AI/Prompting-Troubleshooting #for/AI 

# Manual de Interacción con IA: Prompting y Troubleshooting

## 1. Arquitecturas de Prompts (Plantillas)

### Fase de Creación (El Inicio)
* **Rol:** Actúa como Rol específico, [ej. Administrador de Sistemas Linux Senior.]
* **Objetivo:** Escribe/Crea (Entregable).
* **Contexto:** Por qué lo necesitas y cómo se usará.
* **Formato de Salida:** Output, dile a la IA como debe estructurar su respuesta.
* **Restricciones:** Qué NO debe hacer, formato de salida, compatibilidad.

```mermaid
graph LR
    A([Rol]) --> B([Objetivo])
    B --> C([Contexto])
    C --> D([Output])
    D --> E([Restricciones])
```

### Fase de Refinamiento (Iteración)
* **Ancla:** Sobre el [elemento específico] que acabas de generar...
* **Acción/Problema:** Necesito que [agregues/modifiques/elimines] esto.
* **Contexto/El Porqué:** El objetivo de este cambio es [razón].
* **Control de Salida:** Imprime únicamente los snippets modificados y dime en qué línea insertarlos. No devuelvas todo el código.
* **Restricciones:** Qué NO debe hacer, formato de salida, compatibilidad.

```mermaid
graph LR
    A([Ancla]) --> B([Action/Bug])
    B --> C([Contexto/Why])
    C --> D([Output])
    D --> E([Restricciones])
```

### Fase de Depuración (Debugging/Troubleshooting)
* **Síntoma/Contexto:** Al ejecutar [acción], sucede [comportamiento actual] en lugar de [comportamiento esperado].
* **El Error/Bug:** Describe qué falló. Si la terminal muestra un eror, pega textualmente: [Pegar log].
* **Descubrimiento:** Revisando el código, noté que [tu lógica o descubrimiento].
* **Acción Correctiva:** Corrige este flujo. Imprime solo el snippet corregido.
* **Nevas Instrucciones (Opcional):** En caso de contar con nuevas instrucciones, añadirlas. No es obligatorio ni necesario.
* **Output:** Para que no vuelva a escribir todo desde cero y sobre todo en programación, se le indica que imprima los cambios.

```mermaid
graph LR
    A([Síntoma]) --> B([Bug/Error])
    B --> C([Descubrimiento])
    C --> D([Corrección])
    D --> E([+ Instrucciones])
    E --> F([Output])
```

### Reporte de Falla (Failed Fix)
* **El Problema Persiste (The Issue Persist):** Apliqué los cambios, pero el problema persiste. [Describir síntoma exacto, ej. archivo de 0 bytes].
* **Contexto de Ejecución:** Estoy usando [SO, terminal, comandos exactos].
* **Petición de Rastreo (Debugging Mode / Verbose Mode):** Indícame cómo habilitar el modo de depuración (`set -x`) para ver dónde falla, o corrige el error lógico.
* **Control de Salida (Output Control):** La regla de oro siempre será limitar la respuesta, pedirle a la IA que solamente entregue los cambios con base al problema, contexto y la petición.

```mermaid
graph LR
    A([Issue]) --> B([Context])
    B --> C([Debugging Mode])
    C --> D([Output])
```

### Conversaciones Pausadas
Si regresas a una conversación después de unas horas o días, la IA puede perder «frescura» en el contexto. Ábrele el mensaje con un recordatorio rápido para realinearla, ej. «_Retomemos el script de tareas. En la versión actual ya configuramos la creación y el borrado. Ahora quiero que nos enfoquemos..._»

---

## 2. Glosario Técnico (Inglés a Español)
* **Context-switching:** Pérdida de foco y tiempo al cambiar entre diferentes aplicaciones o ventanas (ej. salir de la terminal para abrir una app de notas).
* **Verbose logging / Debug mode:** Modo de ejecución que imprime cada paso que da un programa antes de ejecutarlo. En Bash se activa con `set -x`.
* **Silent failure:** Cuando un programa o script falla pero no arroja ningún mensaje de error en la pantalla.
* **Payload extraction:** El proceso de descomprimir o sacar el código fuente original que está escondido dentro de un instalador.
* **Refactoring:** Reestructurar el código fuente para mejorarlo (hacerlo más limpio o eficiente) sin cambiar su comportamiento final.
* **Code snippets:** Fragmentos pequeños de código.

---

## 3. Casos de Estudio (Historial de Errores)
* **Problema:** Instalador de un solo archivo en Bash generaba un archivo vacío (0 bytes) en macOS. Era una falla silenciosa.
* **Diagnóstico:** Al activar `set -ex`, el rastreo mostró `base64: invalid argument`.
* **Causa Raíz:** Diferencias entre entornos UNIX. macOS usa utilidades BSD (que requieren la bandera `-i` en `base64`), mientras que Linux usa GNU. Además, el script constructor fallaba si el archivo fuente (payload) no estaba en el mismo directorio.
