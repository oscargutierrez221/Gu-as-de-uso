# Guía de uso y configuraciones de Arch Linux Hyprland con Caelestia shell
>*Guía creada para Arch Linux + Hyprland + Caelestia Shell. Actualizada regularmente.*

## Arch Linux + Hyprland + Caelestia Shell: Guía de Configuración Definitiva

Esta bóveda documenta la configuración, mantenimiento y personalización del sistema operativo Arch Linux. El entorno gráfico está basado en el compositor **Hyprland** (Wayland) y la interfaz visual **Caelestia Shell**.

---

## Índice

1. [Configuración de Entorno Gráfico](#1-configuración-de-entorno-gráfico-hyprland--caelestia)
2. [Gestores de Paquetes y Repositorios](#2-gestores-de-paquetes-y-repositorios)
3. [Comandos Básicos del Sistema](#3-comandos-básicos-del-sistema)
4. [Gestión de Software](#4-gestión-de-software)
5. [Instalación de Aplicaciones Específicas](#5-instalación-de-aplicaciones-específicas)
6. [Gestión de Proyectos con Git](#6-gestión-de-proyectos-con-git)
7. [Configurar Impresora](#7-configurar-impresora)
8. [Utilidades del Sistema](#8-utilidades-del-sistema)
9. [Configuración de Entorno Gráfico (Hyprland + Caelestia)](#1-configuración-de-entorno-gráfico-hyprland--caelestia)

---

# 1. Configuración de Entorno Gráfico (Hyprland + Caelestia)

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
```

Donde al agregar en el `kb_layout = latam` donde por ejemplo puedes poner `kb_layout = latam, us` para agregar una distribución para teclados ingleses como los teclados 60% por ejemplo.

---

# 2. Gestores de Paquetes y Repositorios

### 2.1 Pacman (Gestor Oficial de Arch Linux)

Pacman es el gestor de paquetes por defecto de Arch Linux. Utiliza repositorios oficiales y binarios precompilados.

#### Comandos Esenciales de Pacman

| Comando | Descripción |
|---------|-------------|
| `sudo pacman -Syu` | Sincroniza repositorios y actualiza el sistema |
| `sudo pacman -S <paquete>` | Instala un paquete |
| `sudo pacman -R <paquete>` | Elimina un paquete |
| `sudo pacman -Rs <paquete>` | Elimina paquete y dependencias no usadas |
| `sudo pacman -Rns <paquete>` | Elimina paquete, dependencias y archivos de configuración |
| `sudo pacman -Sc` | Limpia la caché de paquetes descargados |
| `sudo pacman -Scc` | Limpia toda la caché (más agresivo) |
| `pacman -Q` | Lista todos los paquetes instalados |
| `pacman -Qe` | Lista paquetes explícitamente instalados |
| `pacman -Qdt` | Lista paquetes huérfanos (dependencias no necesarias) |
| `pacman -Ss <término>` | Busca paquetes en repositorios |
| `pacman -Qi <paquete>` | Muestra información de un paquete instalado |
| `pacman -Ql <paquete>` | Lista archivos instalados por un paquete |

#### Configuración de Pacman

Editar el archivo de configuración:
```bash
sudo nano /etc/pacman.conf
```

**Recomendaciones útiles:**
- Descomentar la línea `Color` para output coloreado
- Descomentar `ParallelDownloads = 5` para descargas paralelas
- Agregar el repositorio `multilib` para soporte de 32-bit (necesario para Steam y algunos juegos):
  ```
  [multilib]
  Include = /etc/pacman.d/mirrorlist
  ```

Después de modificar, actualizar:
```bash
sudo pacman -Syu
```

---

### 2.2 AUR (Arch User Repository)

El AUR es un repositorio mantenido por la comunidad que contiene PKGBUILDs para software no disponible en los repositorios oficiales.

#### ¿Qué es un PKGBUILD?
Es un script que describe cómo compilar e instalar un paquete desde el código fuente.

#### Instalación Manual desde AUR

```bash
# 1. Clonar el repositorio del paquete
git clone https://aur.archlinux.org/<nombre-paquete>.git

# 2. Entrar al directorio
cd <nombre-paquete>

# 3. Revisar el PKGBUILD (IMPORTANTE por seguridad)
cat PKGBUILD

# 4. Compilar e instalar
makepkg -si
```

---

### 2.3 Yay (Helper de AUR)

Yay es un helper de AUR que simplifica la instalación de paquetes del AUR y repositorios oficiales.

#### Instalación de Yay

```bash
# Instalar dependencias necesarias
sudo pacman -S --needed git base-devel

# Clonar y compilar yay
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

# Limpiar
cd ..
rm -rf yay
```

#### Comandos de Yay

| Comando | Descripción |
|---------|-------------|
| `yay -Syu` | Actualiza repositorios oficiales y AUR |
| `yay -S <paquete>` | Instala un paquete (oficial o AUR) |
| `yay -Rns <paquete>` | Elimina paquete y dependencias |
| `yay -Ss <término>` | Busca paquetes |
| `yay -Si <paquete>` | Información del paquete |
| `yay -Qs <término>` | Busca en paquetes instalados |
| `yay -Sc` | Limpia caché de yay |
| `yay -Yc` | Limpia dependencias no necesarias |
| `yay --editmenu -S <paquete>` | Edita PKGBUILD antes de instalar |

---

### 2.4 Paru (Helper de AUR en Rust)

Paru es un helper de AUR escrito en Rust, con mejor rendimiento y características adicionales.

#### Instalación de Paru

```bash
# Instalar dependencias
sudo pacman -S --needed base-devel

# Clonar y compilar
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si

# Limpiar
cd ..
rm -rf paru
```

#### Comandos de Paru

| Comando | Descripción |
|---------|-------------|
| `paru` | Actualiza todo el sistema (equivalente a `paru -Syu`) |
| `paru -S <paquete>` | Instala un paquete |
| `paru -Sua` | Actualiza solo paquetes de AUR |
| `paru -Syu` | Actualiza repositorios oficiales y AUR |
| `paru -Rns <paquete>` | Elimina paquete |
| `paru -Ss <término>` | Busca paquetes |
| `paru -Qs <término>` | Busca en instalados |
| `paru -Sc` | Limpia caché |
| `paru --fm nvim -S <paquete>` | Usa neovim como editor de PKGBUILD |

> **Nota:** Tanto `yay` como `paru` funcionan de manera similar. Puedes elegir el que prefieras o tener ambos instalados.

---

### 2.5 Flatpak (Aplicaciones Sandboxeadas)

Flatpak es un sistema de distribución de aplicaciones que funciona en "sandbox", proporcionando mayor seguridad y aislamiento.

#### Instalación de Flatpak

```bash
sudo pacman -S flatpak
```

#### Configuración de Repositorios Flatpak

```bash
# Agregar repositorio Flathub (el principal)
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Ver repositorios configurados
flatpak remotes
```

#### Comandos de Flatpak

| Comando                            | Descripción                      |
| ---------------------------------- | -------------------------------- |
| `flatpak install <aplicación>`     | Instala una aplicación           |
| `flatpak install flathub <app-id>` | Instala desde Flathub            |
| `flatpak uninstall <aplicación>`   | Desinstala una aplicación        |
| `flatpak update`                   | Actualiza todas las aplicaciones |
| `flatpak list`                     | Lista aplicaciones instaladas    |
| `flatpak search <término>`         | Busca aplicaciones               |
| `flatpak run <app-id>`             | Ejecuta una aplicación           |
| `flatpak info <app-id>`            | Información de la aplicación     |
| `flatpak uninstall --unused`       | Elimina runtimes no usados       |

#### Integración con el Sistema

Para que las aplicaciones Flatpak aparezcan en tu lanzador:

```bash
# Reiniciar sesión o ejecutar:
sudo systemctl restart systemd-binfmt
```

---

# 3. Comandos Básicos del Sistema

### 3.1 Navegación y Gestión de Archivos

| Comando                    | Descripción                              |
| -------------------------- | ---------------------------------------- |
| `pwd`                      | Muestra el directorio actual             |
| `ls`                       | Lista archivos y carpetas                |
| `ls -la`                   | Lista detallada incluyendo ocultos       |
| `ls -lh`                   | Lista con tamaños legibles               |
| `cd <directorio>`          | Cambia de directorio                     |
| `cd ~`                     | Va al directorio home                    |
| `cd ..`                    | Sube un nivel                            |
| `cd -`                     | Vuelve al directorio anterior            |
| `mkdir <nombre>`           | Crea un directorio                       |
| `mkdir -p <ruta>`          | Crea directorios recursivamente          |
| `rm <archivo>`             | Elimina un archivo                       |
| `rm -r <directorio>`       | Elimina un directorio y su contenido     |
| `rm -rf <directorio>`      | Elimina forzosamente (¡CUIDADO!)         |
| `cp <origen> <destino>`    | Copia archivos                           |
| `cp -r <origen> <destino>` | Copia directorios recursivamente         |
| `mv <origen> <destino>`    | Mueve o renombra archivos                |
| `touch <archivo>`          | Crea archivo vacío o actualiza timestamp |
| `cat <archivo>`            | Muestra contenido de archivo             |
| `less <archivo>`           | Muestra contenido con scroll             |
| `head <archivo>`           | Muestra primeras 10 líneas               |
| `tail <archivo>`           | Muestra últimas 10 líneas                |
| `tail -f <archivo>`        | Muestra últimas líneas en tiempo real    |

### 3.2 Permisos y Propietarios

| Comando                                  | Descripción                       |
| ---------------------------------------- | --------------------------------- |
| `chmod 755 <archivo>`                    | Cambia permisos (rwxr-xr-x)       |
| `chmod +x <archivo>`                     | Hace ejecutable                   |
| `chmod -x <archivo>`                     | Quita permiso de ejecución        |
| `chown usuario:grupo <archivo>`          | Cambia propietario                |
| `sudo chown -R $USER:$USER <directorio>` | Cambia propietario recursivamente |

**Permisos numéricos:**
- 7 = rwx (lectura, escritura, ejecución)
- 6 = rw- (lectura, escritura)
- 5 = r-x (lectura, ejecución)
- 4 = r-- (solo lectura)
- 0 = --- (sin permisos)

### 3.3 Búsqueda y Filtrado

| Comando                        | Descripción                               |
| ------------------------------ | ----------------------------------------- |
| `find . -name "*.txt"`         | Busca archivos por nombre                 |
| `find . -type f -size +100M`   | Busca archivos mayores a 100MB            |
| `grep "texto" <archivo>`       | Busca texto en archivo                    |
| `grep -r "texto" <directorio>` | Búsqueda recursiva                        |
| `grep -i "texto" <archivo>`    | Búsqueda insensible a mayúsculas          |
| `locate <archivo>`             | Busca rápida (requiere `updatedb`)        |
| `which <comando>`              | Ubicación del ejecutable                  |
| `whereis <comando>`            | Ubicación de binarios, fuentes y manuales |

### 3.4 Gestión de Procesos

| Comando                   | Descripción                               |
| ------------------------- | ----------------------------------------- |
| `ps aux`                  | Lista todos los procesos                  |
| `ps aux \| grep <nombre>` | Busca proceso específico                  |
| `top`                     | Monitor de procesos interactivo           |
| `htop`                    | Monitor mejorado (necesita instalación)   |
| `kill <PID>`              | Termina proceso por ID                    |
| `kill -9 <PID>`           | Fuerza terminación                        |
| `killall <nombre>`        | Termina todos los procesos con ese nombre |
| `pkill <nombre>`          | Similar a killall                         |
| `pgrep <nombre>`          | Obtiene PID de un proceso                 |

### 3.5 Sistema y Hardware

| Comando               | Descripción                         |
| --------------------- | ----------------------------------- |
| `uname -a`            | Información del kernel              |
| `uname -r`            | Versión del kernel                  |
| `hostnamectl`         | Información del sistema             |
| `lscpu`               | Información de CPU                  |
| `free -h`             | Uso de memoria RAM                  |
| `df -h`               | Uso de disco                        |
| `du -sh <directorio>` | Tamaño de directorio                |
| `du -sh ~/*`          | Tamaño de carpetas en home          |
| `lsblk`               | Información de discos y particiones |
| `lspci`               | Dispositivos PCI conectados         |
| `lsusb`               | Dispositivos USB conectados         |
| `lshw`                | Hardware completo (necesita sudo)   |
| `uptime`              | Tiempo desde el arranque            |
| `whoami`              | Usuario actual                      |
| `id`                  | Información de usuario y grupos     |

### 3.6 Red y Conectividad

| Comando                                                  | Descripción                      |
| -------------------------------------------------------- | -------------------------------- |
| `ip addr`                                                | Muestra direcciones IP           |
| `ip link`                                                | Muestra interfaces de red        |
| `ping <host>`                                            | Test de conectividad             |
| `curl <url>`                                             | Descarga o consulta URL          |
| `wget <url>`                                             | Descarga archivos                |
| `ss -tuln`                                               | Muestra puertos en escucha       |
| `netstat -tuln`                                          | Similar a ss (si está instalado) |
| `traceroute <host>`                                      | Ruta hasta el destino            |
| `nslookup <dominio>`                                     | Resolución DNS                   |
| `dig <dominio>`                                          | Información DNS detallada        |
| `nmcli device wifi list`                                 | Lista redes WiFi disponibles     |
| `nmcli device wifi connect "SSID" password "contraseña"` | Conecta a WiFi                   |

### 3.7 Compresión y Archivos

| Comando                                  | Descripción       |
| ---------------------------------------- | ----------------- |
| `tar -cvf archivo.tar <directorio>`      | Crea tar          |
| `tar -xvf archivo.tar`                   | Extrae tar        |
| `tar -czvf archivo.tar.gz <directorio>`  | Crea tar.gz       |
| `tar -xzvf archivo.tar.gz`               | Extrae tar.gz     |
| `tar -cjvf archivo.tar.bz2 <directorio>` | Crea tar.bz2      |
| `tar -xjvf archivo.tar.bz2`              | Extrae tar.bz2    |
| `zip -r archivo.zip <directorio>`        | Crea zip          |
| `unzip archivo.zip`                      | Extrae zip        |
| `gzip <archivo>`                         | Comprime con gzip |
| `gunzip <archivo.gz>`                    | Descomprime gzip  |

### 3.8 Atajos de Terminal Útiles

| Atajo      | Descripción                            |
| ---------- | -------------------------------------- |
| `Ctrl + C` | Cancela proceso actual                 |
| `Ctrl + Z` | Suspende proceso (bg para background)  |
| `Ctrl + D` | Cierra terminal (EOF)                  |
| `Ctrl + L` | Limpia pantalla                        |
| `Ctrl + A` | Inicio de línea                        |
| `Ctrl + E` | Fin de línea                           |
| `Ctrl + U` | Borra desde cursor al inicio           |
| `Ctrl + K` | Borra desde cursor al final            |
| `Ctrl + W` | Borra palabra anterior                 |
| `Ctrl + R` | Búsqueda en historial                  |
| `!!`       | Repite último comando                  |
| `!n`       | Ejecuta comando número n del historial |
| `sudo !!`  | Ejecuta último comando con sudo        |
| `Tab`      | Autocompletar                          |
| `Tab Tab`  | Muestra opciones de autocompletado     |

---

# 4. Gestión de Software

## Actualización y Mantenimiento del Sistema

```bash
# Actualización completa del sistema
sudo pacman -Syu

# O con yay/paru (incluye AUR)
yay -Syu
# o
paru

# Limpieza de paquetes huérfanos
sudo pacman -Rns $(pacman -Qdtq)

# O con yay/paru
yay -Yc
# o
paru -c

# Limpieza de caché
sudo pacman -Sc        # Conserva versiones recientes
sudo pacman -Scc       # Elimina toda la caché

# Verificar integridad de paquetes
sudo pacman -Qk

# Reinstalar todos los paquetes (útil tras problemas)
sudo pacman -S $(pacman -Qnq)
```

---

## 5. Instalación de Aplicaciones Específicas

### 5.1 Spotify con Spicetify

Spotify es el popular servicio de streaming musical. Spicetify permite personalizar la interfaz de Spotify con temas y extensiones.

#### Instalación de Spotify

```bash
# Desde repositorios oficiales
sudo pacman -S spotify-launcher

# O desde AUR (versión más actualizada)
yay -S spotify
```

#### Instalación de Spicetify

```bash
# Instalar spicetify-cli desde AUR
yay -S spicetify-cli

# O la versión de desarrollo
yay -S spicetify-cli-git
```

#### Configuración de Spicetify

```bash
# 1. Crear directorio de configuración
mkdir -p ~/.config/spicetify

# 2. Ejecutar spicetify para generar archivos de configuración
spicetify

# 3. Dar permisos de escritura a Spotify (necesario para aplicar temas)
sudo chmod a+wr /opt/spotify
sudo chmod a+wr /opt/spotify/Apps -R

# 4. Hacer backup de la instalación original
spicetify backup apply enable-devtool

# 5. Instalar temas y extensiones populares
cd ~/.config/spicetify

# Clonar repositorio de temas
git clone --depth=1 https://github.com/spicetify/spicetify-themes.git Themes

# Aplicar un tema (ejemplo: Dribbblish)
spicetify config current_theme Dribbblish
cp -r Themes/Dribbblish/* .
spicetify config color_scheme dark
spicetify apply
```

#### Comandos Útiles de Spicetify

| Comando | Descripción |
|---------|-------------|
| `spicetify apply` | Aplica cambios de configuración |
| `spicetify backup` | Crea backup de Spotify original |
| `spicetify restore` | Restaura Spotify original |
| `spicetify config` | Muestra configuración actual |
| `spicetify config current_theme <tema>` | Cambia tema |
| `spicetify config color_scheme <esquema>` | Cambia esquema de colores |
| `spicetify update` | Actualiza spicetify |

#### Temas Populares de Spicetify

- **Dribbblish**: Diseño moderno y minimalista
- **Bloom**: Interfaz elegante con transparencias
- **Comfy**: Diseño compacto y limpio
- **Ziro**: Tema oscuro con acentos de color

---

### 5.2 OBS Studio

OBS Studio es software de grabación y streaming en vivo, muy popular entre creadores de contenido.

#### Instalación de OBS Studio

```bash
# Instalación básica
sudo pacman -S obs-studio

# Instalación con soporte completo (recomendado)
sudo pacman -S obs-studio obs-vkcapture libva-mesa-driver
```

#### Plugins Útiles de OBS

```bash
# Captura de ventanas en Wayland (para Hyprland)
yay -S obs-xdg-portal

# Filtros de audio avanzados
yay -S obs-filter-audio

# Transiciones personalizadas
yay -S obs-transition-table

# Source record (grabar fuentes individuales)
yay -S obs-source-record

# Websocket para control remoto
yay -S obs-websocket
```

#### Configuración para Wayland/Hyprland

Para que OBS funcione correctamente en Wayland, necesitas:

```bash
# 1. Instalar xdg-desktop-portal y el backend para wlroots
sudo pacman -S xdg-desktop-portal xdg-desktop-portal-wlr

# 2. Para Hyprland específicamente
yay -S xdg-desktop-portal-hyprland

# 3. Configurar variables de entorno en ~/.config/hypr/hyprland.conf:
# Agregar al inicio del archivo:
env = XDG_CURRENT_DESKTOP,Hyprland
env = XDG_SESSION_TYPE,wayland
env = XDG_SESSION_DESKTOP,Hyprland
```

#### Fuentes de Captura en Wayland

| Fuente | Descripción |
|--------|-------------|
| PipeWire (Screen Capture) | Captura de pantalla completa |
| PipeWire Window Capture | Captura de ventana específica |
| Video Capture Device | Webcams y dispositivos de video |
| Display Capture | Captura de monitor (X11) |

#### Optimización de Rendimiento

```bash
# Habilitar codificación por hardware (NVIDIA)
sudo pacman -S nvidia-utils
# En OBS: Configuración → Salida → Codificador: NVENC H.264

# Habilitar codificación por hardware (AMD)
sudo pacman -S libva-mesa-driver
# En OBS: Configuración → Salida → Codificador: VA-API

# Habilitar codificación por hardware (Intel)
sudo pacman -S intel-media-driver
# En OBS: Configuración → Salida → Codificador: QuickSync H.264
```

---

### 5.3 Otras Aplicaciones Populares

#### Notion (Versión de escritorio)

```bash
# Desde AUR
yay -S notion-app-electron
```

#### Obsidian

```bash
# Desde repositorios oficiales
sudo pacman -S obsidian
```

#### QT Creator

```bash
# Instalación completa para desarrollo
sudo pacman -S base-devel cmake qt6-base qt-creator

# Soporte de Copilot (requiere Node.js)
sudo pacman -S nodejs npm
```

#### Discord

```bash
# Versión estándar
sudo pacman -S discord

# O desde Flatpak (más actualizada)
flatpak install flathub com.discordapp.Discord
```

#### Telegram

```bash
# Versión de escritorio
sudo pacman -S telegram-desktop

# O desde Flatpak
flatpak install flathub org.telegram.desktop
```

#### Visual Studio Code

```bash
# Desde repositorios oficiales
sudo pacman -S code

# O la versión OSS (open source)
sudo pacman -S code-oss

# O desde Flatpak
flatpak install flathub com.visualstudio.code
```

#### Navegadores

```bash
# Firefox
sudo pacman -S firefox

# Chromium
sudo pacman -S chromium

# Brave (desde AUR)
yay -S brave-bin

# Google Chrome (desde AUR)
yay -S google-chrome
```

#### Multimedia

```bash
# VLC (reproductor de video)
sudo pacman -S vlc

# MPV (reproductor ligero)
sudo pacman -S mpv

# GIMP (edición de imágenes)
sudo pacman -S gimp

# Kdenlive (edición de video)
sudo pacman -S kdenlive

# Audacity (edición de audio)
sudo pacman -S audacity
```

#### Productividad

```bash
# LibreOffice
sudo pacman -S libreoffice-still  # Versión estable
# o
sudo pacman -S libreoffice-fresh  # Versión más nueva

# Para español
sudo pacman -S libreoffice-still-es

# OnlyOffice (alternativa a MS Office)
flatpak install flathub org.onlyoffice.desktopeditors
```

---

## 6. Gestión de Proyectos con Git

### Instalación de Git

```bash
sudo pacman -S git
```

### Configuración Inicial

```bash
# Configurar nombre de usuario
git config --global user.name "Tu Nombre"

# Configurar correo electrónico
git config --global user.email "tu@email.com"

# Configurar editor por defecto (ejemplo: nano)
git config --global core.editor nano

# Verificar configuración
git config --list
```

### Comandos Básicos de Git

| Comando | Descripción |
|---------|-------------|
| `git init` | Inicializa repositorio |
| `git clone <url>` | Clona un repositorio |
| `git status` | Muestra estado del repositorio |
| `git add <archivo>` | Agrega archivo al staging |
| `git add .` | Agrega todos los cambios |
| `git commit -m "mensaje"` | Crea commit |
| `git push` | Sube cambios al remoto |
| `git pull` | Descarga cambios del remoto |
| `git fetch` | Descarga cambios sin fusionar |
| `git merge <rama>` | Fusiona rama actual con otra |
| `git branch` | Lista ramas |
| `git branch <nombre>` | Crea nueva rama |
| `git checkout <rama>` | Cambia de rama |
| `git checkout -b <rama>` | Crea y cambia a nueva rama |
| `git log` | Muestra historial de commits |
| `git log --oneline` | Historial resumido |
| `git diff` | Muestra diferencias |
| `git stash` | Guarda cambios temporalmente |
| `git stash pop` | Recupera cambios guardados |
| `git reset --hard HEAD` | Descarta todos los cambios |

### Configuración con GitHub (SSH)

```bash
# Generar clave SSH
ssh-keygen -t ed25519 -C "tu@email.com"

# Agregar clave al agente
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copiar clave pública al portapapeles
cat ~/.ssh/id_ed25519.pub
# Copiar el output y pegar en GitHub → Settings → SSH and GPG keys

# Probar conexión
ssh -T git@github.com
```

---

## 7. Configurar Impresora

### Paso a paso para vincular la impresora:

1. Abre tu navegador web e ingresa a: `http://localhost:631`
2. Ve a la pestaña **Administration** (Administración).
3. Haz clic en **Add Printer** (Añadir impresora).
4. En la lista de "Discovered Network Printers" (Impresoras de red descubiertas), selecciona la serie de tu impresora, por ejemplo **HP Smart Tank 580-590**.
5. Al elegir el controlador (driver), asegúrate de seleccionar el que incluya **HPLIP** en el nombre para garantizar la compatibilidad de tinta y escaneo.

> **Nota:** Si solicita credenciales, utiliza tu usuario de Arch Linux y tu contraseña de sudo.

### Comandos Rápidos de Mantenimiento (Terminal)

```bash
# Reiniciar el sistema de impresión (CUPS)
sudo systemctl restart cups

# Reiniciar detección de red (Avahi)
sudo systemctl restart avahi-daemon

# Limpiar la cola de impresión bloqueada
cancel -a

# Ver estado de la cola de impresión
lpstat -o

# Ver impresoras configuradas
lpstat -p

# Imprimir archivo de prueba
lp -d <nombre_impresora> <archivo>
```

### Instalación de Controladores Comunes

```bash
# HP (HPLIP)
sudo pacman -S hplip
sudo hp-setup -i  # Configuración interactiva

# Brother
sudo pacman -S brother-cups-wrapper-common

# Canon
yay -S cnijfilter2  # Para impresoras Canon Inkjet
```

---

## 8. Utilidades del Sistema

### 8.1 Gestión de Servicios con Systemd

| Comando | Descripción |
|---------|-------------|
| `systemctl status <servicio>` | Estado de un servicio |
| `sudo systemctl start <servicio>` | Inicia servicio |
| `sudo systemctl stop <servicio>` | Detiene servicio |
| `sudo systemctl restart <servicio>` | Reinicia servicio |
| `sudo systemctl enable <servicio>` | Habilita al inicio |
| `sudo systemctl disable <servicio>` | Deshabilita del inicio |
| `systemctl list-units --type=service` | Lista servicios activos |
| `systemctl --failed` | Muestra servicios fallidos |
| `sudo systemctl daemon-reload` | Recarga configuración |

### 8.2 Gestión de Usuarios

```bash
# Crear nuevo usuario
sudo useradd -m -G wheel -s /bin/bash <usuario>

# Establecer contraseña
sudo passwd <usuario>

# Agregar usuario a grupo
sudo usermod -aG <grupo> <usuario>

# Grupos comunes útiles
sudo usermod -aG wheel,audio,video,optical,storage,games,input <usuario>

# Eliminar usuario
sudo userdel -r <usuario>

# Cambiar a otro usuario
su - <usuario>
```

### 8.3 Gestión de Discos

```bash
# Ver particiones
lsblk
fdisk -l

# Montar disco/partición
sudo mount /dev/sdX1 /mnt/punto/montaje

# Desmontar
sudo umount /mnt/punto/montaje

# Formatear (¡CUIDADO!)
sudo mkfs.ext4 /dev/sdX1      # Ext4
sudo mkfs.ntfs /dev/sdX1      # NTFS
sudo mkfs.vfat -F 32 /dev/sdX1 # FAT32

# Ver uso de disco
df -h
du -sh <directorio>

# Herramienta interactiva (ncdu)
sudo pacman -S ncdu
ncdu <directorio>
```

### 8.4 Copias de Seguridad

```bash
# rsync para backups
sudo pacman -S rsync

# Backup completo de home
rsync -av --delete ~/ /mnt/backup/home/

# Backup excluyendo carpetas
rsync -av --delete --exclude='.cache' --exclude='Downloads' ~/ /mnt/backup/home/

# Backup con compresión
tar -czvf backup-$(date +%Y%m%d).tar.gz ~/Documentos ~/Imágenes
```

### 8.5 Herramientas Útiles para Instalar

```bash
# Monitor de sistema
sudo pacman -S btop htop

# Gestor de archivos en terminal
sudo pacman -S ranger

# Buscador de archivos
sudo pacman -S fd fzf

# Mejoras de cat y ls
sudo pacman -S bat eza

# Editor de texto mejorado
sudo pacman -S micro

# Descargador de videos
sudo pacman -S yt-dlp

# Captura de pantalla
sudo pacman -S grim slurp  # Para Wayland
sudo pacman -S flameshot   # Interfaz gráfica

# Grabación de pantalla
sudo pacman -S wf-recorder  # Wayland

# Gestor de portapapeles
sudo pacman -S wl-clipboard

# Notificaciones
sudo pacman -S libnotify

# Información del sistema
sudo pacman -S neofetch fastfetch
```

### 8.6 Alias Útiles para ~/.bashrc o ~/.zshrc

```bash
# Navegación
alias ..='cd ..'
alias ...='cd ../..'
alias ~='cd ~'

# Listado mejorado
alias ls='eza --icons'
alias ll='eza -la --icons'
alias la='eza -a --icons'
alias lt='eza --tree --icons'

# Cat mejorado
alias cat='bat --style=plain'

# Actualización del sistema
alias update='sudo pacman -Syu'
alias update-aur='yay -Syu'
alias cleanup='sudo pacman -Rns $(pacman -Qdtq)'

# Git
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log --oneline'

# Utilidades
alias mkdirp='mkdir -p'
alias ports='ss -tuln'
alias myip='curl ifconfig.me'
alias weather='curl wttr.in'
alias dfh='df -h'
alias duh='du -sh'
```

### 8.7 Solución de Problemas Comunes

#### Pantalla negra después de actualizar

```bash
# Entrar en TTY (Ctrl + Alt + F2)
# Iniciar sesión y ejecutar:
sudo pacman -Syu
# Si hay errores de dependencias:
sudo pacman -Syyu
```

#### Error de llaves PGP

```bash
# Actualizar keyring
sudo pacman -Sy archlinux-keyring

# Si persiste:
sudo pacman-key --init
sudo pacman-key --populate archlinux
sudo pacman-key --refresh-keys
```

#### Espacio en disco lleno

```bash
# Limpiar caché de pacman
sudo pacman -Sc

# Limpiar caché de yay/paru
yay -Sc

# Limpiar logs antiguos
sudo journalctl --vacuum-time=2weeks

# Ver qué ocupa espacio
sudo ncdu /
```

#### Recuperar GRUB

```bash
# Desde live USB
sudo mount /dev/sdXn /mnt  # Tu partición root
sudo mount /dev/sdX1 /mnt/boot/efi  # Partición EFI
sudo arch-chroot /mnt
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
exit
umount -R /mnt
```

---

## 9. Personalizaciones
### 9.1 Cambiar los GIFs de Caelestia Shell (sessionGif y mediaGif)

Caelestia Shell permite reemplazar fácilmente los GIFs por defecto:
- **sessionGif**: GIF que aparece en el menú de sesión (`logout` / `shutdown` / `restart`). Por defecto es `kurukuru.gif`.
- **mediaGif**: GIF que aparece en el reproductor de música/media player. Por defecto es `bongocat.gif`.
#### Crear las carpetas recomendadas
```bash
mkdir -p ~/Pictures/CaelestiaGifs/session
mkdir -p ~/Pictures/CaelestiaGifs/media
````
#### Descargar y colocar tus GIFs
- Descarga GIFs preferiblemente con **fondo transparente** (se ven mucho mejor).
- Coloca los GIFs en las carpetas correspondientes:
    - Para el menú de sesión → ~/Pictures/CaelestiaGifs/session/
    - Para el reproductor de música → ~/Pictures/CaelestiaGifs/media/
**Ejemplo:**
Por si prefieres mover los Gifs desde la terminal
```bash
mv ~/Imágenes/"el_nombre.gif" ~/Pictures/CaelestiaGifs/session/
```
#### Editar la configuración (`Shell.json`)
Abre el archivo de configuración:
```bash
nano ~/.config/caelestia/shell.json
```
Busca la sección "`paths`" y modifica las rutas usando **rutas absolutas** (no uses root:/assets/... para GIFs personalizados).

**Ejemplo de configuración recomendada:**
Tu archivo `Json`deberían de quedar de forma similar
``` Json
"paths": {
  "wallpaperDir": "/home/Tu_usuario/Pictures/Wallpapers",
	"sessionGif":"/home/Tu_usuario/Pictures/CaelestiaGifs/session/Nombre.gif",
  "mediaGif": "/home/Tu_usuario/Pictures/CaelestiaGifs/media/tu-gif-musica.gif"
}
```

> **Importante**: Reemplaza /home/Tu_usuario/ por tu usuario real (echo $HOME). Usa rutas absolutas completas.

#### Aplicar los cambios
Guarda el archivo (`Ctrl + O` → Enter → `Ctrl + X` ) y reinicia Caelestia Shell:
```
pkill quickshell && caelestia shell
```
O usa el atajo de teclado: **Ctrl + Super + Alt + R**

### 9.2 Herramienta recomendada: caelestia-gif
Puedes instalar un gestor TUI para facilitar el cambio de GIFs:
``` bash
yay -S caelestia-gif
```
Ejecuta:
``` bash
/usr/bin/caelestia-gif --init
```

> Nota: En la versión actual (1.0.2) la TUI completa aún está en desarrollo, por lo que el método manual mediante `shell.json` suele ser más fiable.
#### Ajustar velocidad de los GIFs (opcional)
Si los GIFs se ven muy rápidos o lentos, puedes agregar estas claves dentro de la sección "`appearance`":
```Json
"appearance": {
  "sessionGifSpeed": 0.8,
  "mediaGifSpeedAdjustment": 250,
  ...
}
```
Valores recomendados iniciales:
- **sessionGifSpeed:** entre 0.6 y 1.0
- **mediaGifSpeedAdjustment:** entre 200 y 400

---
### 9.3 Consejos adicionales
- Usa GIFs ligeros (< 3 MB) para mejor rendimiento.
- Los GIFs con fondo transparente se ven mejor en el media player.
- Si el cambio no se aplica inmediatamente, haz logout/login completo.
## 10. Cambiar a otro shell
### 10.1 Cambio de Shell / Dotfiles y Respaldo de Caelestia

Si deseas probar un nuevo entorno (dotfiles) en Hyprland sin perder tu configuración actual de Caelestia Shell ni desinstalar las aplicaciones que ya tienes, el proceso es completamente seguro. 

Para lograrlo, utilizaremos el comando `cp` (copiar) en lugar de mover o renombrar los archivos originales. Esto garantiza que tu configuración base quede aislada y protegida en un directorio de respaldo.

#### Paso 1: Respaldar la configuración actual (Caelestia Shell)

Antes de descargar o instalar cualquier dotfile nuevo, debes crear copias exactas de tus directorios de configuración gráfica actuales.

Abre la terminal y ejecuta:

```bash
# 1. Crea un directorio específico para guardar tu respaldo
mkdir -p ~/.config_caelestia_backup

# 2. Copia de forma recursiva (-r) las carpetas clave de tu entorno actual
cp -r ~/.config/hypr ~/.config_caelestia_backup/
cp -r ~/.config/waybar ~/.config_caelestia_backup/
cp -r ~/.config/rofi ~/.config_caelestia_backup/

# Nota: Haz lo mismo con otras utilidades que uses (kitty, alacritty, wlogout, swaync, etc.)
# cp -r ~/.config/kitty ~/.config_caelestia_backup/
````

Al hacer esto, tu configuración de Caelestia queda a salvo en la carpeta `~/.config_caelestia_backup`.

#### Paso 2: Aplicar los nuevos dotfiles

Una vez que tienes el respaldo asegurado, puedes aplicar el nuevo entorno:

1. **Limpia el entorno activo:** Para evitar conflictos, elimina las carpetas originales que vas a reemplazar en tu `.config`.
```bash 
rm -rf ~/.config/hypr ~/.config/waybar ~/.config/rofi
```

2. **Instala los nuevos dotfiles:** Clona el repositorio del entorno que deseas probar y copia sus carpetas a tu `~/.config/` (o ejecuta su script de instalación si lo tiene).

> **⚠️ Importante:** Si el nuevo entorno incluye un script tipo `install.sh`, léelo rápido antes de ejecutarlo para confirmar que solo interactúa con tu carpeta `~/.config` y no realiza cambios no deseados en el sistema.

#### Paso 3: Restaurar Caelestia Shell (Volver a la normalidad)
Cuando decidas que es momento de regresar a tu Caelestia Shell, el proceso consiste en limpiar lo nuevo y restaurar tu respaldo original:

``` bash
# 1. (Opcional) Si te gustó el entorno nuevo, respáldalo también antes de borrarlo
# mkdir -p ~/.config_otro_tema && cp -r ~/.config/hypr ~/.config_otro_tema/

# 2. Elimina la configuración del dotfile que estabas probando
rm -rf ~/.config/hypr ~/.config/waybar ~/.config/rofi

# 3. Restaura tus carpetas de Caelestia copiándolas de vuelta a .config
cp -r ~/.config_caelestia_backup/hypr ~/.config/
cp -r ~/.config_caelestia_backup/waybar ~/.config/
cp -r ~/.config_caelestia_backup/rofi ~/.config/
```

Por último, simplemente recarga Hyprland (usando tu atajo de teclado para reiniciar el entorno o cerrando y volviendo a iniciar sesión). Tu sistema volverá a lucir exactamente como lo dejaste, y todas las aplicaciones (instaladas con `pacman` o `yay`) seguirán funcionando sin problemas.

## Recursos Adicionales

- [Wiki de Arch Linux](https://wiki.archlinux.org/) - Documentación oficial
- [Arch Linux Packages](https://archlinux.org/packages/) - Buscador de paquetes oficiales
- [AUR](https://aur.archlinux.org/) - Repositorio de usuarios
- [Flathub](https://flathub.org/) - Aplicaciones Flatpak
- [Hyprland Wiki](https://wiki.hyprland.org/) - Documentación de Hyprland
- [Caelestia-Gifs](https://gitlab.com/gnoooo/caelestia-gif) - Repo de la interfaz grafica Caelestia Shell Gifs
- [Tenor](https://tenor.com/es/) - Buscador de Gifs
- [Giphy](https://giphy.com/) - Buscador de Gifs

> RECOMIENDACIÓN: Para buscar Gifs sin fondo usa Transparent Gifs en la barra de busqueda
---
