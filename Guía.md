# Guía de uso y configuraciones de Arch Linux Hyprland con Caelestia shell

## Arch Linux + Hyprland + Caelestia Shell: Guía de Configuración Definitiva

Esta bóveda documenta la configuración, mantenimiento y personalización del sistema operativo Arch Linux. El entorno gráfico está basado en el compositor **Hyprland** (Wayland) y la interfaz visual **Caelestia Shell**. 

---
## 1. Configuración de Entorno Gráfico (Hyprland + Caelestia)

### Rutas de Archivos de Configuración (`.conf`)
El sistema utiliza una estructura modular para facilitar la edición:

* **Archivo Maestro:** `~/.config/hypr/hyprland.conf` (Punto de entrada que permite configurar cosas del entorno y en especial atajos).
* **Módulo de Entrada:** `~/.config/hypr/hyprland/input.conf` (Configuración de idioma y mouse).
### Configuración del Teclado (Latinoamericano)
Para asegurar que los símbolos y la disposición coincidan con el teclado físico o sea la distribución de teclado que deseas, el archivo `input.conf` debe contener:
```text
input {
    kb_layout = latam
}
````

Donde al agregar en el `kb_layout = latam` donde por ejemplo puedes poner `kb_layout = latam, us` para agregar una distribución para teclados ingleses como los teclados 60% por ejemplo.

---

## 2. Gestión de Software y Comandos de Arch Linux

## Comandos de Terminal (Pacman y AUR)

Procedimientos estándar para la instalación y mantenimiento:

- **Actualización Completa:** `yay -Syu` (Sincroniza repositorios oficiales y AUR).
    
- **Búsqueda e Instalación:** `yay -S <nombre_del_paquete>`.
    
- **Limpieza de Huérfanos:** `sudo pacman -Rns $(pacman -Qdtq)` (Elimina paquetes que ya no son necesarios).

> Siendo que de la misma funciona de la misma forma para los paquetes `paru` y por defecto paquetes del sistema `pacman`

## Instalación de Aplicaciones Específicas
Para instalar algunas aplicaciones en tu sistema puedes usar el repositorio `AUR` o independientemente el por defecto de Arch Linux `pacman`, aunque si lo prefieres también existe la posibilidad de instalarlos por medio de `paru`

- **Notion (Versión de escritorio):** Se utiliza el repositorio AUR para obtener una versión integrada con Electron:
``` bash
yay -S notion-app-electron
```
- **Obsidian:** Instalación directa desde el repositorio de la comunidad:
``` bash
sudo pacman -S obsidian
```

- **QT - Creator:** Para el desarrollo de proyectos con interfaces gráficas y herencia compleja. Para su instalación podemos usar el repositorio por defecto de Arch Linux de la comunidad:
``` bash
sudo pacman -S base-devel cmake qt6-base qt-creator
```
**Soporte de Inteligencia Artificial (Copilot):** Qt contiene un asistente de autocompletado de código como lo es Copilot, que si bien muchas personas no lo usan. Para habilitar Copilot como LSP (Language Server Protocol) dentro de `Qt Creator`, se requiere el motor de ejecución de `JavaScript`:
``` bash
   sudo pacman -S nodejs npm
```
## Gestión de Proyectos con Git

Para `Git` y `Github` es lo mismo que en cualquier otro sistema, pasando en primer indicio por la instalación de `Git`
```bash
sudo pacman -S git
```
y logicamente continuar con la configuración tanto de tu correo como de tu usuario como si fuera un sistema `Windows`

**Usuario:**
```bash
git config --global user.name "Tu Nombre"`
```

**Correo:**
```bash
git config --global user.email "Tu correo"`
```

Y puedes verificar la configuración con:
```bash
git config --list
```

para después poder continuar con los comandos regulares de Git y GitHub

---
### Configurar impresora
#### Paso a paso para vincular la impresora:

1. Abre tu navegador web e ingresa a: `http://localhost:631`
2. Ve a la pestaña **Administration** (Administración).
3. Haz clic en **Add Printer** (Añadir impresora).
4. En la lista de "Discovered Network Printers" (Impresoras de red descubiertas), selecciona la serie de tu impresora, por ejemplo **HP Smart Tank 580-590**.
5. Al elegir el controlador (driver), asegúrate de seleccionar el que incluya **HPLIP** en el nombre para garantizar la compatibilidad de tinta y escaneo.

>Nota: Si solicita credenciales, utiliza tu usuario de Arch Linux y tu contraseña de sudo
#### Comandos rápidos de mantenimiento (Terminal):
- **Reiniciar el sistema de impresión (CUPS):**
```bash
sudo systemctl restart cups
```

- **Reiniciar detección de red (Avahi):**
```bash
sudo systemctl restart avahi-daemon
```
 - **Limpiar la cola de impresión bloqueada:**
```bash
cancel -a
```
