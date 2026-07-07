# Guía de herramientas para la terminal

Guía práctica de instalación y uso de las herramientas de terminal configuradas en mi sistema: `fzf` + `fzf.fish`, `zoxide` y `glow`.

**Entorno de referencia:** Arch Linux, shell **fish**, terminal **Kitty**, HyDE dotfile en  **Hyprland**.

---

## 1. fzf + fzf.fish

### ¿Qué es?
`fzf` es un buscador difuso (fuzzy finder) de propósito general para la terminal. `fzf.fish` es un plugin para fish que lo integra directamente en la línea de comandos con atajos de teclado para buscar historial, archivos, variables y más.

### Instalación

```bash
# 1. Instalar el binario de fzf
sudo pacman -S fzf

# 2. Instalar el gestor de plugins fisher (si no lo tienes)
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source
fisher install jorgebucaran/fisher

# 3. Instalar el plugin fzf.fish
fisher install PatrickF1/fzf.fish

# 4. Recargar fish
exec fish
```

### Verificar instalación

```bash
fisher list
```

Debe aparecer `patrickf1/fzf.fish` en la lista.

### Uso: atajos de teclado

| Atajo | Función |
|---|---|
| `Ctrl+R` | Buscar en el historial de comandos |
| `Ctrl+Alt+F` | Buscar archivos/directorios (inserta la ruta en la línea de comandos) |
| `Ctrl+V` | Buscar variables de shell (`$HOME`, `$PATH`, etc.) |
| `Ctrl+Alt+L` | Buscar en el log de git (`git log`) |
| `Ctrl+Alt+S` | Buscar en el status de git (`git status`) |

> **Nota:** `fzf.fish` inserta el resultado en la línea de comandos, no ejecuta acciones automáticamente (por ejemplo, no hace `cd` solo). Esto es una decisión de diseño del plugin para mantenerlo predecible.

### Personalizar atajos (opcional)

Si algún atajo choca con tu terminal o quieres cambiarlo:

```bash
fzf_configure_bindings --help
```

Esto muestra todas las opciones disponibles (`--history`, `--directory`, `--variables`, `--git_log`, `--git_status`) para reasignarlas. Ejemplo:

```bash
fzf_configure_bindings --variables=\e\cv
```

Agrega la línea elegida a `~/.config/fish/config.fish` para que persista entre sesiones.

### Herramientas complementarias
`fzf.fish` mejora automáticamente si tienes instalado:
- `fd` — acelera la búsqueda de archivos (`sudo pacman -S fd`)
- `bat` — mejora el preview de archivos con resaltado de sintaxis (`sudo pacman -S bat`)

---

## 2. zoxide

### ¿Qué es?
Reemplazo inteligente de `cd`. Aprende qué directorios visitas más seguido y te permite saltar a ellos escribiendo solo una parte del nombre, sin importar en qué carpeta estés.

### Instalación

```bash
sudo pacman -S zoxide
```

### Configuración en fish

Agrega esto a `~/.config/fish/config.fish`:

```bash
zoxide init fish | source
```

Recarga la shell:

```bash
exec fish
```

### Uso

| Comando | Función |
|---|---|
| `z nombre` | Salta al directorio que coincida con "nombre" (el más frecuente/reciente) |
| `z nombre1 nombre2` | Salta a un directorio que coincida con ambos fragmentos (útil para rutas anidadas) |
| `zi nombre` | Igual que `z`, pero si hay varias coincidencias abre un selector interactivo (usa `fzf` si está instalado) |
| `z -` | Vuelve al directorio anterior |
| `zoxide query nombre` | Muestra qué ruta resolvería `z nombre` sin moverte ahí |
| `zoxide remove ruta` | Elimina una ruta de la base de datos de zoxide |

### Cómo funciona
Cada vez que usas `cd` normalmente (o `z`), zoxide registra esa ruta con un puntaje de frecuencia/recencia. Con el tiempo, `z proy` te llevará directo a `~/Documentos/trabajo/proyecto-x` aunque nunca hayas escrito esa ruta completa.

---

## 3. glow

### ¿Qué es?
Renderiza archivos Markdown con formato (títulos, negritas, listas, bloques de código con color) directamente en la terminal, sin necesidad de abrir un editor o navegador.

### Instalación

```bash
sudo pacman -S glow
```

### Uso

| Comando | Función |
|---|---|
| `glow archivo.md` | Muestra el archivo formateado en la terminal |
| `glow` | Abre un selector de archivos `.md` en el directorio actual |
| `glow -p archivo.md` | Fuerza modo paginado (como `less`), útil para archivos largos |
| `glow https://raw.githubusercontent.com/usuario/repo/main/README.md` | Renderiza un Markdown remoto directo desde una URL |
| `glow -s dark archivo.md` | Usa un estilo de color específico (`dark`, `light`, `auto`, etc.) |

### Ejemplo práctico
Para leer el README de cualquier repo clonado sin salir de la terminal:

```bash
glow README.md
```

---

## Resumen rápido de instalación (todo junto)

```bash
sudo pacman -S fzf zoxide glow fd bat
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source
fisher install jorgebucaran/fisher
fisher install PatrickF1/fzf.fish
```

Luego agrega a `~/.config/fish/config.fish`:

```bash
zoxide init fish | source
```

Y recarga:

```bash
exec fish
```