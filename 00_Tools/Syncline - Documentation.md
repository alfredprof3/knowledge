

# ⚡ Syncline

[![Bash](https://img.shields.io/badge/Bash-4.0%2B-blue.svg)](https://gnu.org/software/bash/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS-lightgrey.svg)]()

> Un gestor de tareas y notas atómicas para la terminal, respaldado por Git para una sincronización en segundo plano fluida.

Syncline combina la filosofía Zettelkasten con la velocidad de la línea de comandos. Crea notas, administra tareas y deja que el motor de sincronización maneje las actualizaciones con tu repositorio remoto de forma totalmente silenciosa.

## ✨ Características Principales (Features)

* **Background Auto-Sync:** Sincronización automática con GitHub al crear o editar (soporta SSH y PAT).
* **Smart Credential Management:** Abstracción nativa para integrarse con macOS Keychain, libsecret y kwallet.
* **Fricción Cero:** Instalador interactivo (Global o Local) con autoconfiguración del `$PATH`.
* **Multi-Entorno:** Historial de ramas dinámico, compatible con Linux, macOS y Termux.

## 🚀 Instalación (Installation)

Clona este repositorio y ejecuta el instalador interactivo. Syncline te guiará para elegir entre una instalación a nivel sistema (`sudo`) o a nivel usuario local.

```bash
git clone [https://github.com/tu-usuario/syncline.git](https://github.com/tu-usuario/syncline.git)
cd syncline
./build_installer.sh
./install.sh
````

## ⚙️ Configuración (Setup)

Para activar la sincronización en segundo plano, vincula Syncline con tu repositorio remoto. Puedes usar HTTPS o SSH.

  

Bash

```
syncline remote add [https://github.com/usuario/mi-repo-notas.git](https://github.com/usuario/mi-repo-notas.git)
```

## 💻 Uso (Usage)

La sintaxis de Syncline está diseñada para ser rápida e intuitiva:

  

|**Comando**|**Descripción**|
|---|---|
|`syncline note "Texto"`|Crea una nueva nota atómica y la sube al repositorio.|
|`syncline list`|Muestra tus tareas y notas pendientes.|
|`syncline list -a`|Muestra todo el historial, incluyendo elementos completados.|
|`syncline check <ID>`|Marca un elemento como completado (✓).|
|`syncline cancel <ID>`|Marca un elemento como cancelado (✗).|
|`syncline clear checked`|Purga únicamente los elementos completados.|
|`syncline sync`|Fuerza una sincronización manual (Pull/Push).|

## 🧠 Filosofía Zettelkasten

Syncline no es solo un gestor de tareas, es un ecosistema de conocimiento. [Aquí puedes explicar brevemente en 2-3 líneas cómo usar las notas atómicas con tu herramienta].

  

## 🤝 Contribuciones (Contributing)

Las contribuciones, issues y feature requests son bienvenidos.

  

1. Haz un Fork del proyecto
    
      
    
2. Crea tu rama de característica (`git checkout -b feat/AmazingFeature`)
    
      
    
3. Haz commit de tus cambios (`git commit -m 'feat: Add some AmazingFeature'`)
    
      
    
4. Haz Push a la rama (`git push origin feat/AmazingFeature`)
    
      
    
5. Abre un Pull Request
    
      
    

## 📄 Licencia

Distribuido bajo la Licencia MIT. Consulta el archivo `LICENSE` para más información.

```

---

### Por qué funciona esta estructura:

1. **Insignias (Badges):** Las pequeñas etiquetas visuales debajo del título le dicen inmediatamente a otros programadores los requisitos técnicos (Bash, Licencia, Sistemas Operativos) sin tener que leer texto.
2. **El Bloque de "Cita" (`>`):** Es tu "Elevator Pitch". En una sola oración defines qué es el software.
3. **Bloques de Código Copiables:** Nadie quiere leer un manual extenso para instalar algo. El bloque de instalación permite hacer "copiar y pegar" directo a la terminal.
4. **Tabla de Comandos:** Las tablas son el mejor formato para la sección de Uso (*Usage*). Son increíblemente fáciles de escanear visualmente.

<FollowUp label="¿Quieres saber cómo agregar una Licencia MIT a tu repositorio?" query="¿Cómo genero y agrego un archivo de Licencia MIT para proteger mi código antes de hacerlo público en GitHub?"/>
```