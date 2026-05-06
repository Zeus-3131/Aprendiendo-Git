# 📘 Git Master Notebook - Referencia Completa de Comandos

> 📋 *Tu cuaderno digital personal de Git. Diseñado para consulta rápida en GitHub.*  
> 🔖 *Guarda este archivo como `GIT-COMMANDS.md` en tu repositorio de referencia.*

---

## 🗂️ Tabla de Contenidos

<details open>
<summary><strong>📌 Navegación Rápida (Click para expandir/colapsar)</strong></summary>

1. [⚙️ Configuración Inicial](#-configuración-inicial)
2. [🔄 Ciclo de Trabajo Local](#-ciclo-de-trabajo-local)
3. [🔍 Inspección y Historial](#-inspección-y-historial)
4. [🌿 Ramas y Fusión](#-ramas-y-fusión)
5. [🌐 Sincronización Remota](#-sincronización-remota)
6. [↩️ Deshacer Cambios](#️-deshacer-cambios)
7. [🏷️ Etiquetas (Tags)](#️-etiquetas-tags)
8. [🔧 Comandos Avanzados](#-comandos-avanzados)
9. [🧰 Mantenimiento](#-mantenimiento)
10. [🎨 Banderas Universales](#-banderas-universales)
11. [🚨 Recuperación de Emergencia](#-recuperación-de-emergencia)
12. [📋 Cheat Sheet por Escenario](#-cheat-sheet-por-escenario)

</details>

---

## ⚙️ Configuración Inicial

<details>
<summary><code>git config</code> - Identidad y preferencias</summary>

```bash
# ── Identidad del usuario ──────────────────────────────
git config --global user.name "Tu Nombre"           # Nombre para commits
git config --global user.email "tu@email.com"       # Email para commits
git config --global --list                          # Ver configuración activa
git config --get user.name                          # Consultar valor específico

# ── Ámbito de configuración ───────────────────────────
git config --local user.name "Nombre Local"         # Solo este repo (.git/config)
git config --global user.name "Nombre Global"       # Todos tus repos (~/.gitconfig)
git config --system user.name "Nombre Sistema"      # Todos los usuarios (/etc/gitconfig)

# ── Preferencias útiles ───────────────────────────────
git config --global core.editor "code --wait"       # Editor por defecto (VS Code)
git config --global init.defaultBranch main         # Rama principal por defecto
git config --global pull.rebase false               # pull usa merge (no rebase)
git config --global color.ui auto                   # Colores en terminal
git config --global alias.st status                 # Alias: git st = git status
git config --global alias.co checkout               # Alias: git co = git checkout
```

| Bandera | Descripción |
|---------|-------------|
| `--global` | Aplica a todos los repositorios del usuario |
| `--local` | Aplica solo al repositorio actual *(por defecto)* |
| `--system` | Aplica a todos los usuarios del sistema |
| `--list` | Muestra toda la configuración activa |
| `--get <clave>` | Obtiene el valor de una clave específica |
| `--unset <clave>` | Elimina una configuración |

</details>

<details>
<summary><code>git init</code> / <code>git clone</code> - Crear repositorios</summary>

```bash
# ── Crear nuevo repositorio ───────────────────────────
git init                                          # Repo local estándar
git init --bare                                   # Repo bare (solo servidor)
git init --initial-branch=main                    # Establecer rama principal

# ── Clonar repositorio remoto ─────────────────────────
git clone <url>                                   # Clonación completa
git clone --depth 1 <url>                         # Clonación superficial (último commit)
git clone --branch <rama> <url>                   # Clonar rama específica
git clone --recursive <url>                       # Incluir submódulos
git clone --single-branch <url>                   # Solo la rama principal
```

</details>

---

## 🔄 Ciclo de Trabajo Local

<details>
<summary><code>git status</code> / <code>git add</code> - Preparar cambios</summary>

```bash
# ── Ver estado ────────────────────────────────────────
git status                                        # Estado completo
git status -s                                     # Formato corto (una línea)
git status --porcelain                            # Formato máquina-parseable

# ── Añadir al staging ─────────────────────────────────
git add <archivo>                                 # Archivo específico
git add .                                         # Todos los cambios (incluye nuevos)
git add -A                                        # TODO: modificados + eliminados + nuevos
git add -u                                        # Solo archivos ya rastreados
git add -p                                        # Interactivo por fragmentos (patch)
git add -i                                        # Modo interactivo completo
git add --intent-to-add <archivo>                 # Registrar intención sin contenido
git add --force <archivo>                         # Forzar añadir ignorado por .gitignore

# ── Banderas clave de git add ─────────────────────────
# -p, --patch      : Seleccionar cambios por hunk interactivamente
# -i, --interactive: Menú interactivo completo
# -A, --all        : Añadir absolutamente todo
# -u, --update     : Solo actualizar archivos ya rastreados
# -N, --intent-to-add: Marcar para futuro add sin contenido
# --dry-run        : Simular sin ejecutar
# --force, -f      : Ignorar .gitignore
```

</details>

<details>
<summary><code>git commit</code> - Registrar cambios</summary>

```bash
# ── Commits básicos ───────────────────────────────────
git commit -m "Mensaje claro y descriptivo"       # Commit con mensaje inline
git commit -am "Mensaje"                          # Add + commit (solo archivos rastreados)
git commit                                        # Abre editor para mensaje largo

# ── Opciones avanzadas ────────────────────────────────
git commit --amend                                # Corregir último commit
git commit --amend --no-edit                      # Amend sin editar mensaje
git commit -v                                     # Mostrar diff en editor
git commit --dry-run                              # Simular sin crear
git commit --allow-empty                          # Permitir commit sin cambios
git commit -s                                     # Añadir Signed-off-by
git commit -S                                     # Firmar con GPG
git commit --no-verify                            # Saltar hooks pre-commit

# ── Banderas clave de git commit ──────────────────────
# -m, --message    : Mensaje en línea
# -a, --all        : Auto-add para archivos rastreados modificados
# --amend          : Reemplazar último commit
# --no-edit        : Usar mensaje existente sin abrir editor
# -v, --verbose    : Mostrar diff en editor de commit
# -s, --signoff    : Añadir trailer "Signed-off-by"
# -S, --gpg-sign   : Firmar con GPG
# --dry-run        : Mostrar qué se incluiría sin crear commit
# --allow-empty    : Permitir commit sin cambios en el árbol
```

> ⚠️ **Nota importante**: `git commit -am` **NO** añade archivos nuevos no rastreados. Para archivos nuevos, siempre usa `git add <archivo>` primero.

</details>

---

## 🔍 Inspección y Historial

<details>
<summary><code>git log</code> - Navegar el historial</summary>

```bash
# ── Visualización básica ──────────────────────────────
git log                                           # Historial completo
git log --oneline                                 # Una línea por commit
git log --graph                                   # Árbol ASCII de ramas
git log --all                                     # Todas las ramas y tags
git log --oneline --graph --all                   # 🎯 Combinación más útil

# ── Filtrado y búsqueda ───────────────────────────────
git log -n 5                                      # Últimos 5 commits
git log --since="2 weeks ago"                     # Desde fecha específica
git log --until="2024-01-01"                      # Hasta fecha específica
git log --author="Nombre"                         # Por autor
git log --grep="palabra"                          # Buscar en mensajes
git log -- <ruta/archivo>                         # Solo commits que tocan archivo

# ── Detalle de cambios ────────────────────────────────
git log --stat                                    # Estadísticas por archivo
git log --patch                                   # Diff completo por commit
git log --name-only                               # Solo nombres de archivos
git log --name-status                             # Nombres + estado (A/M/D)
git log --follow <archivo>                        # Seguir historial con renombres

# ── Búsqueda avanzada en código ───────────────────────
git log -G "regex"                                # Cambios que coinciden con regex
git log -S "texto"                                # Cambios en cantidad de "texto"
git log -L :funcion:archivo.py                    # Historial de una función específica

# ── Formato personalizado ─────────────────────────────
git log --pretty=format:"%h - %an, %ar : %s"     # Formato personalizado
git log --pretty=oneline --abbrev-commit          # Hash corto + mensaje
```

| Bandera | Descripción |
|---------|-------------|
| `--oneline` | Formato compacto: hash corto + mensaje |
| `--graph` | Dibujar árbol de ramas con ASCII |
| `--all` | Incluir todas las ramas y tags |
| `--stat` | Mostrar estadísticas de cambios por archivo |
| `--patch`, `-p` | Mostrar diff completo del cambio |
| `--name-only` | Solo listar nombres de archivos |
| `--follow` | Seguir historial a través de renombres |
| `-G <regex>` | Buscar commits donde cambie texto que coincida con regex |
| `-S <string>` | Buscar commits donde cambie la cantidad de ocurrencias |
| `--since=<fecha>` / `--until=<fecha>` | Filtrar por rango de fechas |
| `--author=<patrón>` | Filtrar por autor |
| `--grep=<patrón>` | Buscar patrón en mensaje del commit |

</details>

<details>
<summary><code>git show</code> / <code>git diff</code> - Ver detalles</summary>

```bash
# ── git show: Inspeccionar objetos ────────────────────
git show                                          # Último commit (detalles + diff)
git show <hash>                                   # Commit específico
git show <hash>:<archivo>                         # Contenido de archivo en commit
git show --name-only <hash>                       # Solo archivos del commit
git show --stat <hash>                            # Estadísticas del commit
git show HEAD~2                                   # Penúltimo commit
git show <rama>                                   # Último commit de una rama
git show <tag>                                    # Detalles de una etiqueta

# ── git diff: Comparar cambios ────────────────────────
git diff                                          # Working dir vs staging (no staged)
git diff --staged                                 # Staging vs HEAD (staged)
git diff --cached                                 # Sinónimo de --staged
git diff HEAD                                     # Working dir vs HEAD (todos los cambios)
git diff <commit1>..<commit2>                     # Entre dos commits
git diff <rama1>..<rama2> -- <archivo>            # Archivo específico entre ramas
git diff --color-words                            # Resaltar cambios por palabra
git diff --no-index <f1> <f2>                     # Comparar archivos fuera de Git
git diff --name-only                              # Solo nombres de archivos cambiados
git diff --stat                                   # Estadísticas resumidas

# ── Banderas útiles de diff ───────────────────────────
# --staged, --cached : Comparar staging vs HEAD
# --name-only        : Solo listar archivos, sin diff
# --stat             : Estadísticas de inserciones/eliminaciones
# --color-words      : Resaltar cambios a nivel de palabra
# --no-index         : Comparar archivos fuera del repositorio
# --ignore-space-change, -w : Ignorar cambios de espacio en blanco
```

</details>

<details>
<summary><code>git blame</code> / <code>git reflog</code> - Auditoría y recuperación</summary>

```bash
# ── git blame: ¿Quién cambió esto? ────────────────────
git blame <archivo>                               # Autor de cada línea
git blame -L 10,20 <archivo>                      # Solo líneas 10-20
git blame -e <archivo>                            # Mostrar email en lugar de nombre
git blame -C <archivo>                            # Detectar código copiado de otros archivos
git blame -w <archivo>                            # Ignorar cambios de espacio en blanco

# ── git reflog: El "historial del historial" ─────────
git reflog                                        # Todos los movimientos de HEAD
git reflog show <rama>                            # Reflog de rama específica
git reflog --all                                  # Reflog de todas las referencias
git reflog expire --expire=30.days --all          # Limpiar reflog antiguo

# 💡 Uso clave: Recuperar commits "perdidos"
# 1. git reflog → encontrar hash del commit perdido
# 2. git reset --hard <hash> o git checkout -b nueva-rama <hash>
```

</details>

---

## 🌿 Ramas y Fusión

<details>
<summary><code>git branch</code> - Gestión de ramas</summary>

```bash
# ── Listar ramas ──────────────────────────────────────
git branch                                        # Ramas locales
git branch -a                                     # Todas (locales + remotas)
git branch -r                                     # Solo remotas
git branch -v                                     # Con último commit
git branch --sort=-committerdate                  # Ordenar por fecha

# ── Crear y eliminar ──────────────────────────────────
git branch <nombre>                               # Crear desde HEAD actual
git branch -d <nombre>                            # Eliminar (solo si fusionada)
git branch -D <nombre>                            # Eliminar forzosamente
git branch -m <viejo> <nuevo>                     # Renombrar
git branch -M <nuevo>                             # Renombrar forzosamente

# ── Cambiar de rama ───────────────────────────────────
git checkout <rama>                               # Clásico: cambiar de rama
git checkout -b <nueva>                           # Crear y cambiar
git switch <rama>                                 # Moderno: solo cambiar
git switch -c <nueva>                             # Moderno: crear y cambiar

# ── Banderas clave de git branch ──────────────────────
# -a, --all        : Mostrar locales y remotas
# -r, --remotes    : Solo ramas remotas
# -d, --delete     : Eliminar (requiere fusión previa)
# -D               : Eliminar forzosamente
# -m, --move       : Renombrar
# -v, --verbose    : Información adicional por rama
# --merged         : Ramas ya fusionadas en HEAD
# --no-merged      : Ramas NO fusionadas en HEAD
# --contains <c>   : Ramas que contienen el commit <c>
```

</details>

<details>
<summary><code>git merge</code> - Fusionar ramas</summary>

```bash
# ── Merge básico ──────────────────────────────────────
git merge <rama>                                  # Fusionar <rama> en la actual

# ── Opciones de estrategia ────────────────────────────
git merge --no-ff <rama>                          # Forzar commit de merge (no fast-forward)
git merge --squash <rama>                         # Combinar cambios sin commit de merge
git merge --no-commit <rama>                      # Preparar sin commit automático
git merge --strategy=<estrategia> <rama>          # Estrategia específica
git merge -X<option> <rama>                       # Opciones para la estrategia

# ── Manejo de conflictos ──────────────────────────────
git merge --abort                                 # Cancelar merge en conflicto
git merge --continue                              # Continuar tras resolver conflictos

# ── Banderas clave de git merge ───────────────────────
# --no-ff              : Forzar commit de merge
# --squash             : Combinar sin commit de merge
# --abort              : Cancelar y restaurar estado previo
# --continue           : Continuar tras resolver conflictos manualmente
# --no-commit, -n      : Preparar pero no commit automático
# --strategy=<strat>   : recursive, octopus, ours, etc.
# -X<option>           : -Xtheirs, -Xours para resolver conflictos
# --allow-unrelated-histories : Permitir merge sin ancestro común
```

</details>

<details>
<summary><code>git rebase</code> - Reescribir historial</summary>

```bash
# ── Rebase básico ─────────────────────────────────────
git rebase <rama>                                 # Reaplicar commits actuales sobre <rama>
git rebase --continue                             # Continuar tras resolver conflictos
git rebase --abort                                # Cancelar y restaurar estado previo
git rebase --skip                                 # Saltar commit actual

# ── Rebase interactivo (🔥 PODEROSO) ─────────────────
git rebase -i HEAD~3                              # Editar últimos 3 commits
# En el editor puedes:
# pick   : mantener commit
# reword : cambiar mensaje
# edit   : pausar para modificar
# squash : combinar con anterior
# fixup  : como squash pero descarta mensaje
# drop   : eliminar commit

# ── Rebase avanzado ───────────────────────────────────
git rebase --onto <nueva-base> <antigua-base> <rama> # Cambiar base de rama
git rebase --autosquash                           # Aplicar automáticamente fixup!/squash!
git rebase --exec <cmd>                           # Ejecutar comando tras cada commit

# ── Banderas clave de git rebase ──────────────────────
# -i, --interactive    : Modo interactivo para editar commits
# --continue           : Continuar tras resolver conflictos
# --abort              : Cancelar y restaurar estado previo
# --skip               : Omitir commit actual con conflicto
# --onto <nueva>       : Reaplicar sobre base diferente
# --autosquash         : Aplicar automáticamente commits fixup!/squash!
# --exec <cmd>         : Ejecutar comando después de cada commit
```

> ⚠️ **Advertencia**: Nunca uses `rebase` en ramas compartidas/públicas. Puede romper el historial de otros colaboradores.

</details>

---

## 🌐 Sincronización Remota

<details>
<summary><code>git remote</code> - Gestionar conexiones remotas</summary>

```bash
# ── Listar y configurar ───────────────────────────────
git remote -v                                     # Ver remotos configurados
git remote add origin <url>                       # Añadir nuevo remoto
git remote remove <nombre>                        # Eliminar remoto
git remote rename <viejo> <nuevo>                 # Renombrar remoto
git remote set-url origin <nueva-url>             # Cambiar URL

# ── Mantenimiento ─────────────────────────────────────
git remote update                                 # Actualizar todos los remotos
git remote prune origin                           # Limpiar referencias a ramas remotas borradas
```

</details>

<details>
<summary><code>git fetch</code> / <code>git pull</code> / <code>git push</code> - Sincronizar</summary>

```bash
# ── Descargar cambios (sin fusionar) ──────────────────
git fetch                                         # Descargar de upstream configurado
git fetch origin                                  # Descargar de remoto específico
git fetch --all                                   # Todos los remotos
git fetch --prune                                 # + limpiar ramas remotas eliminadas

# ── Descargar y fusionar ──────────────────────────────
git pull                                          # Fetch + merge automático
git pull --rebase                                 # Fetch + rebase (historial más limpio)
git pull origin <rama>                            # Pull de rama específica
git pull --ff-only                                # Solo fast-forward, abortar si requiere merge

# ── Subir cambios ─────────────────────────────────────
git push                                          # Push de rama actual a su upstream
git push origin <rama>                            # Push de rama específica
git push -u origin <rama>                         # + establecer upstream para futuros push/pull
git push --force                                  # ⚠️ Forzar (sobrescribe remoto)
git push --force-with-lease                       # ✅ Forzar más seguro (verifica estado)
git push --delete origin <rama>                   # Eliminar rama en remoto
git push origin --tags                            # Subir todas las etiquetas

# ── Banderas clave de push/pull ───────────────────────
# push:
#   -u, --set-upstream  : Establecer rama remota como upstream
#   -f, --force         : Forzar sobrescritura (¡peligroso!)
#   --force-with-lease  : Forzar solo si remoto no ha cambiado
#   --delete            : Eliminar rama(s) en remoto
#   --tags              : Push de todas las etiquetas
#   --dry-run           : Simular sin ejecutar
#
# pull:
#   --rebase            : Usar rebase en lugar de merge
#   --ff-only           : Solo permitir fast-forward
#   --no-commit         : Preparar merge sin commit automático
```

</details>

---

## ↩️ Deshacer Cambios

<details>
<summary><code>git reset</code> - El comando más poderoso (y peligroso)</summary>

### 🎯 Los 3 modos de `git reset`

```bash
# ─────────────────────────────────────────────────────
# git reset --soft <commit>
# ─────────────────────────────────────────────────────
# ✅ Mueve HEAD al commit especificado
# ✅ Mantiene cambios en staging area (índice)
# ✅ Mantiene cambios en working directory
# 📌 Ideal: "Rehacer" commits, combinar varios en uno

git reset --soft HEAD~1    # Deshace último commit, mantiene cambios listos para commit


# ─────────────────────────────────────────────────────
# git reset --mixed <commit>  ← MODO POR DEFECTO
# ─────────────────────────────────────────────────────
# ✅ Mueve HEAD al commit especificado
# ❌ Vacía el staging area (índice)
# ✅ Mantiene cambios en working directory
# 📌 Ideal: Deshacer add, reorganizar qué incluir en próximo commit

git reset --mixed HEAD~1   # Deshace commit y unstaged cambios
git reset HEAD~1           # Lo mismo (mixed es default)


# ─────────────────────────────────────────────────────
# git reset --hard <commit>  ← ⚠️ PELIGRO
# ─────────────────────────────────────────────────────
# ✅ Mueve HEAD al commit especificado
# ❌ Vacía el staging area
# ❌ Sobrescribe working directory con estado del commit
# ⚠️  Los cambios no commiteados se PERDEN permanentemente
# 📌 Ideal: Descartar TODO y volver a estado limpio conocido

git reset --hard HEAD~1    # Elimina último commit y todos los cambios locales
git reset --hard origin/main # Sincronizar completamente con remoto
```

### 📊 Tabla comparativa visual

| Estado inicial | `--soft` | `--mixed` (default) | `--hard` |
|---------------|----------|-------------------|----------|
| **HEAD** | → commit | → commit | → commit |
| **Staging (índice)** | ✅ mantiene | ❌ reset a commit | ❌ reset a commit |
| **Working directory** | ✅ mantiene | ✅ mantiene | ❌ sobrescribe |
| **Cambios perdidos** | Ninguno | Ninguno | **Todos los no-commiteados** |

### Reset para archivos específicos (sin mover HEAD)
```bash
git reset <archivo>                    # Unstage un archivo (opuesto a git add)
git reset -- <archivo1> <archivo2>    # Unstage múltiples archivos
git reset -- .                         # Unstage TODO
git reset -p <archivo>                 # Unstage interactivamente por hunks
```

</details>

<details>
<summary><code>git revert</code> - La forma segura de deshacer en repositorios compartidos</summary>

```bash
# ── Revert básico ─────────────────────────────────────
git revert <hash>                                 # Crear commit que deshace el especificado
git revert HEAD                                   # Revertir último commit
git revert -n <hash>                              # Aplicar cambios sin commit automático
git revert --no-edit <hash>                       # Usar mensaje automático (sin editor)
git revert -s <hash>                              # Añadir Signed-off-by
git revert -S <hash>                              # Firmar con GPG
git revert <inicio>..<fin>                        # Revertir rango de commits
git revert -m 1 <merge-commit>                    # Revertir merge (especificar mainline)

# ── Banderas clave de git revert ──────────────────────
# -n, --no-commit    : Aplicar cambios inversos sin crear commit
# --no-edit          : Usar mensaje automático sin abrir editor
# -s, --signoff      : Añadir trailer "Signed-off-by"
# -S, --gpg-sign     : Firmar commit con GPG
# -m <n>, --mainline : Para revertir merges: especificar parent principal (1, 2, 3...)
```

### 🆚 Reset vs Revert - ¿Cuándo usar cuál?

| Característica | `git reset` | `git revert` |
|---------------|-------------|--------------|
| **Historial** | Lo modifica/reescribe | Lo preserva (añade commit nuevo) |
| **Uso ideal** | Commits locales **NO publicados** | Commits **YA publicados/compartidos** |
| **Efecto** | Mueve puntero HEAD hacia atrás | Crea commit nuevo con cambios opuestos |
| **Riesgo en equipo** | 🔴 Alto (puede romper historial de otros) | 🟢 Bajo (seguro para colaboración) |
| **Recuperable** | Difícil sin reflog | Sí, revertiendo el revert |

> ✅ **Regla de oro**: Usa `revert` para corregir errores en ramas compartidas. Usa `reset` solo para limpiar tu historial local antes de hacer push.

</details>

<details>
<summary>Otros comandos de limpieza y recuperación</summary>

```bash
# ── Stash: Guardar cambios temporalmente ─────────────
git stash                                         # Guardar cambios actuales
git stash save "mensaje"                          # Guardar con descripción
git stash list                                    # Ver stashes guardados
git stash pop                                     # Aplicar y eliminar último stash
git stash apply                                   # Aplicar sin eliminar del listado
git stash drop stash@{0}                          # Eliminar stash específico
git stash show stash@{0}                          # Ver contenido de stash
git stash branch <rama> stash@{0}                 # Crear rama desde stash

# ── Clean: Eliminar archivos no rastreados ───────────
git clean -n                                      # Dry-run: mostrar qué se eliminaría
git clean -f                                      # Forzar eliminación de untracked files
git clean -fd                                     # Eliminar archivos Y directorios untracked
git clean -fx                                     # + archivos ignorados por .gitignore
git clean -X                                      # Solo archivos ignorados (.gitignore)

# ── Restore: Alternativa moderna a checkout ──────────
git restore <archivo>                             # Descartar cambios locales en archivo
git restore --staged <archivo>                    # Unstage archivo (equivalente a reset)
git restore --source=<commit> <archivo>           # Restaurar desde commit específico
```

</details>

---

## 🏷️ Etiquetas (Tags)

<details>
<summary><code>git tag</code> - Versionar releases</summary>

```bash
# ── Listar y crear ────────────────────────────────────
git tag                                           # Listar todas las etiquetas
git tag -l "v1.*"                                 # Listar tags que coincidan con patrón
git tag -a v1.0 -m "Versión 1.0 estable"          # Tag anotado (✅ recomendado)
git tag v1.0                                      # Tag ligero (solo puntero)

# ── Inspeccionar y compartir ──────────────────────────
git show v1.0                                     # Ver detalles de tag
git push origin v1.0                              # Subir tag específico
git push origin --tags                            # Subir todas las etiquetas

# ── Eliminar ──────────────────────────────────────────
git tag -d v1.0                                   # Eliminar tag local
git push origin --delete tag v1.0                 # Eliminar tag remoto

# ── Usar tags ─────────────────────────────────────────
git checkout v1.0                                 # Cambiar a estado del tag (HEAD detached)
git checkout -b hotfix-v1.0 v1.0                  # Crear rama desde tag

# ── Banderas clave de git tag ─────────────────────────
# -a, --annotate  : Crear tag anotado (autor, fecha, mensaje, firma GPG opcional)
# -s, --sign      : Firmar tag con GPG
# -m <msg>        : Mensaje para tag anotado
# -f, --force     : Forzar creación/reemplazo de tag existente
# -d, --delete    : Eliminar tag local
# -l, --list      : Listar tags (con patrón opcional)
# -n              : Mostrar mensaje del tag al listar
```

> 💡 **Mejor práctica**: Usa siempre tags anotados (`-a`) para releases. Incluyen metadatos importantes y son más seguros para distribución.

</details>

---

## 🔧 Comandos Avanzados

<details>
<summary><code>git cherry-pick</code> - Traer commits específicos</summary>

```bash
git cherry-pick <hash>                            # Aplicar commit específico a rama actual
git cherry-pick -n <hash>                         # Aplicar sin commit automático
git cherry-pick -x <hash>                         # Añadir referencia al commit original en mensaje
git cherry-pick --continue                        # Continuar tras resolver conflictos
git cherry-pick --abort                           # Abortar cherry-pick
git cherry-pick <inicio>..<fin>                   # Rango de commits (excluye inicio)
git cherry-pick <inicio>^..<fin>                  # Rango de commits (incluye inicio)
```

</details>

<details>
<summary><code>git bisect</code> - Depuración binaria de bugs</summary>

```bash
# ── Proceso paso a paso ───────────────────────────────
git bisect start                                  # Iniciar búsqueda binaria
git bisect bad HEAD                               # Marcar HEAD como "malo" (tiene bug)
git bisect good v1.0                              # Marcar tag como "bueno" (sin bug)

# Git te lleva a commits intermedios para probar...
# Ejecuta tu prueba y marca el resultado:
git bisect good                                   # Este commit está bien
git bisect bad                                    # Este commit tiene el bug

# Repetir hasta encontrar el commit culpable
git bisect reset                                  # Terminar búsqueda y volver a HEAD

# ── Automatización ────────────────────────────────────
git bisect run <script>                           # Ejecutar script automáticamente en cada paso
# Ej: git bisect run ./test.sh
```

> 🎯 **Caso de uso ideal**: Encontrar exactamente qué commit introdujo un bug cuando no sabes cuándo se rompió algo.

</details>

<details>
<summary>Otros comandos avanzados útiles</summary>

```bash
# ── shortlog: Resumen por autor ───────────────────────
git shortlog                                      # Commits agrupados por autor
git shortlog -sn                                  # Con número, ordenado descendente
git shortlog -sne                                 # + incluir email

# ── describe: Nombre legible basado en tags ───────────
git describe                                      # Ej: v2.1-3-gabc1234
git describe --tags                               # Incluir tags ligeros también
git describe --always                             # Fallback a hash si no hay tags

# ── archive: Exportar sin .git ────────────────────────
git archive -o proyecto.zip HEAD                  # Exportar HEAD a ZIP
git archive -o proyecto.tar --format=tar v1.0     # Exportar tag a TAR
git archive --prefix=proyecto/ HEAD \| gzip > proyecto.tar.gz

# ── worktree: Múltiples directorios de trabajo ───────
git worktree add ../feature feature/xyz           # Crear worktree para rama
git worktree list                                 # Listar worktrees activos
git worktree remove ../feature                    # Eliminar worktree
```

</details>

---

## 🧰 Mantenimiento

<details>
<summary>Optimización y administración del repositorio</summary>

```bash
# ── Limpieza y optimización ───────────────────────────
git gc                                            # Garbage collection: optimizar repo
git gc --aggressive                               # Limpieza más profunda (más lenta)
git gc --prune=now                                # Eliminar objetos no referenciados inmediatamente
git fsck                                          # Verificar integridad del repositorio
git fsck --full                                   # Verificación completa (más lenta)

# ── Submódulos ────────────────────────────────────────
git submodule add <url> <ruta>                    # Añadir submódulo
git submodule update --init --recursive           # Inicializar y actualizar submódulos
git submodule foreach git pull origin main        # Actualizar todos los submódulos

# ── Configuración de mantenimiento ───────────────────
git config --global gc.auto 256                   # Ejecutar gc automáticamente tras N commits
git config --global fetch.prune true              # Prune automático en fetch
```

</details>

---

## 🎨 Banderas Universales

<details>
<summary>Opciones que funcionan con casi cualquier comando</summary>

| Bandera | Descripción | Ejemplo |
|---------|-------------|---------|
| `--help` | Mostrar ayuda del comando | `git commit --help` |
| `--version` | Mostrar versión de Git | `git --version` |
| `-C <ruta>` | Ejecutar en directorio específico | `git -C /proyecto status` |
| `-c <clave>=<valor>` | Override temporal de config | `git -c core.editor=nano commit` |
| `--git-dir=<ruta>` | Especificar directorio .git | `git --git-dir=/otro/.git status` |
| `--work-tree=<ruta>` | Especificar directorio de trabajo | `git --work-tree=/app status` |
| `--no-pager` | No usar pager para salida larga | `git --no-pager log` |
| `--bare` | Operar en modo bare | `git --bare init` |
| `--quiet`, `-q` | Modo silencioso | `git push -q` |
| `--verbose`, `-v` | Modo detallado | `git status -v` |
| `--dry-run` | Simular sin ejecutar cambios | `git clean --dry-run` |
| `--force`, `-f` | Forzar operación | `git push -f` |

</details>

---

## 🚨 Recuperación de Emergencia

<details>
<summary>Comandos para salvar el día</summary>

```bash
# ── Recuperar commit "perdido" con reflog ───────────
git reflog                                        # Ver historial de HEAD
git reset --hard <hash-del-reflog>                # Volver a commit específico
git checkout -b recovery-branch <hash>            # Crear rama desde commit recuperado

# ── Recuperar archivo eliminado ───────────────────────
git checkout HEAD -- <archivo>                    # Restaurar desde último commit
git restore --source=HEAD <archivo>               # Alternativa moderna
git log -- <archivo>                              # Encontrar commits que tocaban el archivo

# ── Abortar operaciones en progreso ───────────────────
git merge --abort                                 # Cancelar merge con conflictos
git rebase --abort                                # Cancelar rebase
git cherry-pick --abort                           # Cancelar cherry-pick
git revert --abort                                # Cancelar revert secuencial

# ── Limpiar estado de sequencer interrumpido ─────────
git sequencer --abort                             # (Git 2.29+)
# Manual (con precaución):
rm -rf .git/rebase-merge .git/rebase-apply
```

> 💡 **Tip de supervivencia**: Si algo sale mal, `git reflog` es tu mejor amigo. Casi nada se pierde realmente en Git.

</details>

---

## 📋 Cheat Sheet por Escenario

<details open>
<summary>Flujos de trabajo comunes - Copiar y pegar</summary>

### 🔰 Flujo diario básico
```bash
git clone <url> && cd proyecto
git checkout -b mi-feature
# ... trabajar ...
git add . && git commit -m "Descripción clara del cambio"
git push -u origin mi-feature
```

### 🔄 Sincronizar con el equipo
```bash
git fetch origin
git merge origin/main                           # o: git pull
# Si hay conflictos: resolver, add, commit
```

### 🧹 Limpiar antes de push
```bash
git status
git add -p                                      # Revisar cambios por hunk
git commit -m "Mensaje descriptivo"
git push
```

### 🆘 ¿Hice algo mal? - Soluciones rápidas

```bash
# ❌ Commit con mensaje mal escrito
git commit --amend -m "Nuevo mensaje correcto"

# ❌ Olvidé añadir un archivo al último commit
git add olvidado.txt && git commit --amend --no-edit

# ❌ Quiero deshacer último commit (LOCAL, no publicado)
git reset --soft HEAD~1     # Mantener cambios en staging
git reset --mixed HEAD~1    # Mantener cambios en working dir
git reset --hard HEAD~1     # ⚠️ ¡Perder cambios no commiteados!

# ❌ Ya hice push y necesito corregir (COMPARTIDO)
git revert HEAD             # Crear commit que deshace el anterior
git push                    # Subir el revert

# ❌ Cambios locales que no quiero commitear aún
git stash                   # Guardar temporalmente
# ... trabajar en otra cosa ...
git stash pop               # Recuperar cambios

# ❌ Archivo modificado por error
git restore <archivo>       # Descartar cambios locales
git restore --staged <archivo>  # Unstage archivo
```

### 🔍 Buscar algo específico

```bash
# ¿Quién modificó esta línea?
git blame -L 42,42 archivo.py

# ¿En qué commit se introdujo este bug?
git bisect start
git bisect bad HEAD
git bisect good v1.0
# ... probar y marcar good/bad ...

# ¿Qué archivos cambiaron en este commit?
git show --name-only <hash>

# ¿Cuándo se añadió esta función?
git log -S "nombreFuncion" -- <archivo>
```

</details>

---

## 💡 Consejos Profesionales

<details>
<summary>Mejores prácticas y atajos</summary>

### 🎯 Aliases útiles para agregar a tu `~/.gitconfig`
```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = reset HEAD --
    last = log -1 HEAD
    lg = log --oneline --graph --all
    amend = commit --amend
    undo = reset --soft HEAD~1
    cleanup = fetch --prune && gc --prune=now
```

### ✨ Comandos compuestos útiles
```bash
# Ver historial bonito
git lg  # (si configuraste el alias)

# Limpiar repo y sincronizar
git fetch --prune && git gc --prune=now

# Ver qué haría un comando sin ejecutarlo
git clean --dry-run
git push --dry-run

# Comparar rama actual con main
git diff main...HEAD
```

### 📚 Documentación rápida
```bash
git <comando> --help        # Ayuda en terminal
git help <comando>          # Ayuda en navegador/pager
man git-<comando>           # Página de manual completa
```

> 🔑 **Regla de oro**: Cuando dudes, usa `--dry-run` primero. Es mejor simular que lamentar.

</details>

---

<div align="center">

### 🎉 ¡Listo!

> 📌 **Guarda este archivo** como `GIT-COMMANDS.md` en tu repositorio personal  
> 🔖 **Añádelo a favoritos** en GitHub para acceso rápido  
> ✏️ **Personalízalo** con tus propios aliases y flujos de trabajo  

*Última actualización: $(date +%Y-%m-%d)*  
*¿Encontraste un error o falta un comando? ¡Contribuye con un PR!* 🚀

</div>

---

> ℹ️ **Nota sobre este documento**: Esta referencia está diseñada para ser leída en GitHub con renderizado Markdown. Para la documentación oficial completa de cualquier comando, ejecuta `git <comando> --help` en tu terminal.

```markdown
<!-- 
💡 Instrucciones de uso:
1. Copia todo este contenido
2. Crea un nuevo archivo en tu repo: GIT-COMMANDS.md
3. Pega el contenido y haz commit
4. ¡Accede desde GitHub en cualquier momento!

🔧 Personalización:
- Edita los aliases en la sección de Consejos Profesionales
- Añade tus propios comandos frecuentes en Cheat Sheet
- Actualiza la fecha de última actualización
-->
```
