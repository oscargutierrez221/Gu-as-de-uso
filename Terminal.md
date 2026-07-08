# Guía de herramientas de terminal

Guía práctica de instalación y uso de las herramientas de terminal configuradas en este sistema: `fzf` + `fzf.fish`, `zoxide`, `glow` y `lazygit`.

Entorno de referencia: Arch Linux, shell **fish**, terminal **Kitty**, WM **Hyprland**.

---

## 1. fzf + fzf.fish

### ¿Qué es?
`fzf` es un buscador difuso (fuzzy finder) de propósito general para la terminal. `fzf.fish` es un plugin para fish que lo integra directamente en la línea de comandos con atajos de teclado.

### Instalación

```fish
sudo pacman -S fzf
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source
fisher install jorgebucaran/fisher
fisher install PatrickF1/fzf.fish
exec fish
```

### Atajos de fzf.fish (dentro de fish)

| Atajo | Función |
|---|---|
| <span style="color:#2563eb"><strong>`Ctrl+R`</strong></span> | <span style="color:#2563eb"><strong>Buscar en el historial de comandos</strong></span> |
| <span style="color:#2563eb"><strong>`Ctrl+Alt+F`</strong></span> | <span style="color:#2563eb"><strong>Buscar archivos/directorios</strong></span> |
| `Ctrl+V` | Buscar variables de shell |
| `Ctrl+Alt+L` | Buscar en el log de git |
| `Ctrl+Alt+S` | Buscar en el status de git |

### Controles dentro de la ventana de fzf (cualquier búsqueda)

| Tecla | Función |
|---|---|
| Escribir texto | Filtra resultados en vivo (fuzzy match) |
| ↑ / ↓ o `Ctrl+P` / `Ctrl+N` | Mover selección |
| <span style="color:#2563eb"><strong>`Enter`</strong></span> | <span style="color:#2563eb"><strong>Confirmar/insertar selección</strong></span> |
| `Tab` | Seleccionar múltiples elementos (multi-select) |
| `Shift+Tab` | Deseleccionar elemento |
| <span style="color:#2563eb"><strong>`Esc` / `Ctrl+C`</strong></span> | <span style="color:#2563eb"><strong>Cancelar búsqueda</strong></span> |
| `Ctrl+/` | Mostrar/ocultar panel de preview |
| `Ctrl+U` | Limpiar la línea de búsqueda |
| `Alt+Bksp` | Borrar la palabra anterior en la búsqueda |

### Personalizar atajos de fzf.fish (opcional)

```fish
fzf_configure_bindings --help
fzf_configure_bindings --variables=\e\cv
```

Agrega la línea elegida a `~/.config/fish/config.fish` para que persista.

### Herramientas complementarias
- `fd` — acelera la búsqueda de archivos: `sudo pacman -S fd`
- `bat` — mejora el preview con resaltado de sintaxis: `sudo pacman -S bat`

---

## 2. zoxide

### ¿Qué es?
Reemplazo inteligente de `cd` que aprende qué directorios visitas más y te deja saltar a ellos escribiendo solo un fragmento del nombre.

### Instalación

```fish
sudo pacman -S zoxide
```

Agrega a `~/.config/fish/config.fish`:

```fish
zoxide init fish | source
```

```fish
exec fish
```

### Comandos

| Comando | Función |
|---|---|
| <span style="color:#2563eb"><strong>`z nombre`</strong></span> | <span style="color:#2563eb"><strong>Saltar al directorio más frecuente/reciente que coincida con "nombre"</strong></span> |
| `z nombre1 nombre2` | Saltar a un directorio que coincida con ambos fragmentos (rutas anidadas) |
| <span style="color:#2563eb"><strong>`zi nombre`</strong></span> | <span style="color:#2563eb"><strong>Igual que `z`, pero abre selector interactivo si hay varias coincidencias</strong></span> |
| `z -` | Volver al directorio anterior |
| `z ..` | Subir un nivel (comportamiento heredado de `cd`) |
| `zoxide query nombre` | Ver qué ruta resolvería `z nombre` sin moverte ahí |
| `zoxide add ruta` | Agregar manualmente una ruta a la base de datos |
| `zoxide remove ruta` | Eliminar una ruta de la base de datos |
| `zoxide import archivo` | Importar historial desde otra herramienta (autojump, z.sh, etc.) |
| `zoxide init fish` | Genera el script de inicialización (ya usado en la config) |

---

## 3. glow

### ¿Qué es?
Renderiza archivos Markdown con formato directamente en la terminal.

### Instalación

```fish
sudo pacman -S glow
```

### Comandos

| Comando | Función |
|---|---|
| <span style="color:#2563eb"><strong>`glow archivo.md`</strong></span> | <span style="color:#2563eb"><strong>Muestra el archivo formateado</strong></span> |
| `glow` | Abre selector interactivo de archivos `.md` en el directorio actual |
| `glow -p archivo.md` | Modo paginado (como `less`), útil para archivos largos |
| `glow -s dark archivo.md` | Fuerza un estilo de color (`dark`, `light`, `auto`, `notty`) |
| `glow -w 80 archivo.md` | Fuerza el ancho de renderizado a 80 columnas |
| `glow https://raw.githubusercontent.com/usuario/repo/main/README.md` | Renderiza un Markdown remoto desde una URL |

### Navegación dentro del selector interactivo (`glow` sin argumentos)

| Tecla | Función |
|---|---|
| ↑ / ↓ | Navegar lista de archivos |
| <span style="color:#2563eb"><strong>`Enter`</strong></span> | <span style="color:#2563eb"><strong>Abrir archivo seleccionado</strong></span> |
| `/` | Buscar archivo por nombre |
| `q` / `Esc` | Salir |

---

## 4. lazygit

### ¿Qué es?
TUI para git. Permite hacer stage, commit, push, pull, manejar branches y resolver conflictos sin memorizar comandos de git.

### Instalación

```fish
sudo pacman -S lazygit
```

### Uso básico

```fish
lazygit
```

### Atajos globales

| Tecla | Función |
|---|---|
| `1` – `5` | Saltar a panel (Status, Files, Branches, Commits, Stash) |
| `[` / `]` | Cambiar de pestaña dentro de un panel |
| <span style="color:#2563eb"><strong>`P`</strong></span> | <span style="color:#2563eb"><strong>Push</strong></span> |
| <span style="color:#2563eb"><strong>`p`</strong></span> | <span style="color:#2563eb"><strong>Pull</strong></span> |
| `x` | Abrir menú de acciones/comandos |
| `?` | Ver todos los atajos disponibles (cheatsheet) |
| <span style="color:#2563eb"><strong>`q`</strong></span> | <span style="color:#2563eb"><strong>Salir</strong></span> |
| `Esc` | Volver / cancelar |

### Panel "Files" (staging y commits)

| Tecla | Función |
|---|---|
| <span style="color:#2563eb"><strong>`Espacio`</strong></span> | <span style="color:#2563eb"><strong>Stage/unstage del archivo seleccionado</strong></span> |
| `a` | Stage/unstage de todos los archivos |
| <span style="color:#2563eb"><strong>`c`</strong></span> | <span style="color:#2563eb"><strong>Abrir cuadro para escribir mensaje de commit</strong></span> |
| `C` | Commit usando tu editor de git configurado (para mensajes largos) |
| `A` | Amend del último commit con lo que está en stage |
| `d` | Descartar cambios del archivo seleccionado |
| `D` | Abrir menú de opciones de descarte/reset |
| `i` | Agregar archivo a `.gitignore` |
| `e` | Editar archivo con tu editor por defecto |
| `o` | Abrir archivo con la app del sistema |
| `R` | Refrescar el estado |

**Pasos para un commit típico:**
1. Selecciona archivo(s) y presiona `Espacio` (o `a` para todos).
2. Presiona `c`.
3. Escribe el mensaje y presiona `Enter`.
4. Presiona `P` para subirlo.

### Panel "Branches"

| Tecla | Función |
|---|---|
| <span style="color:#2563eb"><strong>`Espacio`</strong></span> | <span style="color:#2563eb"><strong>Checkout de la rama seleccionada</strong></span> |
| `n` | Crear nueva rama |
| `M` | Fusionar (merge) la rama seleccionada en la actual |
| `r` | Rebase de la rama actual sobre la seleccionada |
| `d` | Eliminar rama |

### Panel "Commits"

| Tecla | Función |
|---|---|
| `Espacio` / `Enter` | Ver archivos incluidos en el commit |
| `r` | Reword (editar mensaje del commit) |
| `s` | Squash del commit con el anterior |
| `f` | Fixup con el commit anterior |
| `d` | Eliminar commit (en rebase interactivo) |
| `g` | Reset (mover HEAD) hasta este commit |

### Panel "Stash"

| Tecla | Función |
|---|---|
| `Espacio` | Aplicar el stash seleccionado |
| `g` | Aplicar y eliminar (pop) el stash |
| `d` | Eliminar el stash |

---

## Resumen rápido de instalación (todo junto)

```fish
sudo pacman -S fzf zoxide glow lazygit fd bat
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source
fisher install jorgebucaran/fisher
fisher install PatrickF1/fzf.fish
```

Agrega a `~/.config/fish/config.fish`:

```fish
zoxide init fish | source
```

Y recarga:

```fish
exec fish
```