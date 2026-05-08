```markdown
# Git, GitHub & GitHub Actions — Cheat Sheet

---

## GIT

### ¿Qué es Git?
Git es una herramienta **gratuita de control de versiones** que se instala en tu computadora. Te permite guardar el historial de cambios de tu código, volver a versiones anteriores si algo sale mal, y trabajar en varias versiones del mismo proyecto al mismo tiempo.

- **Commit** — una instantánea del estado de tu código en un momento específico
- **Rama** — una línea de desarrollo independiente que no afecta al código principal
- **Historial** — el registro completo de todos los cambios realizados

> Git trabaja completamente en tu computadora. No necesitas internet para usarlo.

---

### Configuración inicial
Antes de empezar a usar Git, debes registrar tu nombre y correo. Esta información quedará asociada a cada commit que hagas, para saber quién realizó cada cambio.

```bash
git config --global user.name "tu-nombre"
git config --global user.email "tu-correo"
```

---

### Cómo guardar cambios

Guardar cambios en Git es un proceso de dos pasos:
1. **Staging** — seleccionas qué archivos quieres incluir en el próximo commit
2. **Commit** — confirmas y guardas esos cambios en el historial

```
[Archivos modificados] → git add → [Staging] → git commit → [Historial]
```

| Comando | Descripción |
|---|---|
| `git init` | Inicializa un repositorio Git en tu proyecto (solo una vez) |
| `git add <archivo>` | Añade un archivo al staging (en caso de querer seleccionar archivos especificos)|
| `git add .` | Añade todos los archivos modificados al staging (en caso de querer archivos en )|
| `git commit -m "mensaje"` | Guarda los cambios en el historial con una descripción |
| `git status` | Muestra qué archivos están en staging y cuáles no |
| `git log` | Muestra el historial de todos los commits |

> ⚠️ Cada vez que modificas un archivo y quieres incluirlo en un nuevo commit, debes volver a ejecutar `git add`, aunque ya lo hayas añadido antes.

---

### HEAD y navegación entre commits

**HEAD** es un indicador que Git maneja internamente para saber qué versión del código tienes cargada en este momento. Se actualiza automáticamente al hacer un nuevo commit.

| Comando | Descripción |
|---|---|
| `git checkout <ID>` | Carga temporalmente un commit anterior (los cambios recientes no se pierden) |
| `git checkout <rama>` | Cambia a una rama y carga su versión más reciente |

---

### Deshacer cambios

| Comando | Cuándo usarlo | ¿Borra historial? |
|---|---|---|
| `git revert <ID>` | Para deshacer un commit de forma segura. Crea un nuevo commit que revierte los cambios. ✅ Recomendado | No |
| `git reset --hard <ID>` | Para eliminar commits definitivamente. Los cambios eliminados no se pueden recuperar. ⚠️ Usar con mucho cuidado | Sí |

> Siempre prefiere `git revert`. Solo usa `git reset --hard` si estás completamente seguro de lo que haces.

---

### .gitignore
Archivo donde le indicas a Git qué archivos o carpetas debe ignorar siempre al ejecutar `git add`. Es útil para excluir archivos de configuración personal o carpetas que se pueden regenerar automáticamente.

```
# Ejemplos comunes en .gitignore
.vscode/        → configuración del editor de código
.DS_Store       → archivo del sistema de macOS
node_modules/   → dependencias de Node.js (se reinstalan con npm install)
```

---

### Ramas

Las ramas te permiten trabajar en una nueva función o corrección de errores de forma aislada, sin afectar el código principal. Cuando el trabajo esté listo, puedes unir esa rama al código principal.

> Por defecto existe la rama **main** (o master), que es la rama principal del proyecto.

| Comando | Descripción |
|---|---|
| `git branch` | Lista todas las ramas (`*` indica en cuál estás) |
| `git branch <nombre>` | Crea una nueva rama |
| `git branch -d <nombre>` | Elimina una rama |
| `git checkout <nombre>` | Cambia a una rama existente |
| `git checkout -b <nombre>` | Crea una rama y cambia a ella en un solo paso |
| `git merge <rama>` | Une los cambios de una rama a la rama actual |

**Flujo típico:**
```
1. git checkout -b feature-x    → crear y cambiar a la nueva rama
2. (hacer cambios y commits)
3. git checkout main            → volver a la rama principal
4. git merge feature-x          → unir los cambios
5. git branch -d feature-x      → eliminar la rama (ya no se necesita)
```

---

## GITHUB

### ¿Qué es GitHub?
GitHub es una plataforma en internet que extiende Git con tres funcionalidades principales:

- **Almacenamiento en la nube** — guarda tu código de forma remota para no perderlo y acceder desde cualquier dispositivo
- **Colaboración** — trabaja en equipo usando herramientas como issues, pull requests y proyectos
- **Automatización** — ejecuta tareas automáticas con GitHub Actions

> Git y GitHub son cosas distintas. Git es la herramienta local, GitHub es la plataforma en la nube construida alrededor de Git.

Los repositorios pueden ser:
- **Públicos** — cualquiera puede ver el código, pero no editarlo sin permiso
- **Privados** — solo tú y las personas que autorices pueden verlo

---

### Conectar tu proyecto local con GitHub

Para subir tu código a GitHub, primero crea un repositorio en github.com y luego conéctalo a tu proyecto local.

```bash
# Conectar el proyecto local con GitHub
git remote add origin https://usuario@github.com/usuario/repo.git

# Subir el código por primera vez
git push origin main
```

| Comando | Descripción |
|---|---|
| `git remote add origin <URL>` | Conecta tu proyecto local con un repositorio de GitHub |
| `git push origin <rama>` | Sube una rama a GitHub (primera vez) |
| `git push` | Sube los últimos commits a GitHub (después de la primera vez) |
| `git pull` | Descarga los cambios de GitHub a tu computadora |
| `git clone <URL> [carpeta]` | Descarga un repositorio de GitHub a tu computadora |

> **Autenticación:** GitHub no usa tu contraseña normal. Debes crear un **Personal Access Token** en:
> *github.com → Settings → Developer settings → Personal access tokens*

> `git clone` configura automáticamente la conexión con GitHub. No necesitas ejecutar `git remote add` después de clonar.

---

### README.md
Archivo en formato Markdown que GitHub muestra automáticamente en la página principal de tu repositorio. Se usa para explicar de qué trata el proyecto, cómo instalarlo y cómo usarlo.

> 💡 Siempre ejecuta `git pull` antes de continuar trabajando localmente, para asegurarte de tener la versión más reciente del código y evitar conflictos al hacer `git push`.

---

### Pull Requests (PR)

Un **Pull Request** es una solicitud para unir los cambios de una rama a otra, normalmente a `main`. Permite que otros revisen y aprueben el código antes de que se integre al proyecto. Es la forma estándar de colaborar en GitHub.

**Flujo:**
```
1. Subir la rama a GitHub con git push
2. Abrir un PR en GitHub indicando qué rama quieres unir y a cuál
3. El propietario revisa los cambios y puede pedir modificaciones
4. Si todo está bien, se aprueba y se fusiona el PR
5. Todos ejecutan git pull para sincronizar su repositorio local
```

---

### Forks

Un **fork** es una copia de un repositorio ajeno en tu propia cuenta de GitHub. Se usa cuando quieres contribuir a un proyecto del que no eres colaborador, lo cual es muy común en proyectos de código abierto.

**Flujo:**
```
1. Hacer fork del repositorio original en GitHub
2. Clonar tu fork localmente con git clone
3. Crear una rama, hacer cambios y subir al fork con git push
4. Abrir un PR desde tu fork hacia el repositorio original
5. El propietario decide si acepta o rechaza los cambios
```

| | Colaborador | Fork |
|---|---|---|
| **Acceso** | Push directo al repositorio original | Solo a su propia copia |
| **Uso típico** | Equipos pequeños o repositorios privados | Proyectos de código abierto |
| **Cómo contribuye** | Push + PR | Fork + PR |

---

## GITHUB ACTIONS

### ¿Qué es GitHub Actions?
GitHub Actions es un servicio de GitHub que permite **automatizar tareas** relacionadas con tu código. En lugar de ejecutar estas tareas manualmente, se configuran una vez y GitHub las ejecuta automáticamente cuando ocurre algún evento, como subir nuevo código.

Casos de uso comunes:
- Ejecutar pruebas automáticamente cuando alguien sube código
- Publicar la aplicación automáticamente al aprobar un Pull Request
- Revisar la calidad del código de forma automática

> **Gratuito** en repositorios públicos. En repositorios privados hay un límite mensual gratuito.
> Más detalles: [docs.github.com/billing/github-actions](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)

---

### Bloques de construcción

GitHub Actions se organiza en capas. Es importante entender cómo se relacionan entre sí:

```
Repositorio → Workflows → Jobs → Steps
```

| Bloque | ¿Qué es? |
|---|---|
| **Workflow** | El proceso automatizado completo. Se define en un archivo YAML dentro del repositorio. Puede haber varios workflows por repositorio. |
| **Job** | Una tarea dentro del workflow. Contiene varios steps y corre en una máquina virtual. Por defecto, los jobs se ejecutan en paralelo. |
| **Step** | Una acción individual dentro de un job. Los steps se ejecutan uno tras otro, en orden. |
| **Action** | Una aplicación predefinida y reutilizable que realiza una tarea específica, como descargar el código del repositorio. |
| **Runner** | La máquina virtual donde se ejecutan los steps. GitHub ofrece Ubuntu, Windows y macOS. |
| **Trigger/Evento** | La condición que activa el workflow, por ejemplo, cada vez que alguien hace push. |

---

### Estructura de un Workflow

Los workflows se guardan en `.github/workflows/<nombre>.yml` dentro del repositorio. Pueden crearse directamente en GitHub o de forma local como cualquier otro archivo del proyecto.

```yaml
name: Nombre del Workflow        # Nombre descriptivo del workflow

on: push                         # Evento que lo activa

jobs:
  nombre-del-job:                # Identificador del job (lo eliges tú)
    runs-on: ubuntu-latest       # Máquina virtual donde se ejecutará

    steps:
      - name: Descargar código
        uses: actions/checkout@v3      # Usar una Action predefinida

      - name: Instalar Node.js
        uses: actions/setup-node@v3   # Action con configuración
        with:
          node-version: 18            # Parámetro de configuración

      - name: Instalar dependencias
        run: npm ci                    # Ejecutar un comando de terminal

      - name: Ejecutar pruebas
        run: npm test
```

---

### Palabras clave del archivo YAML

| Clave | Descripción |
|---|---|
| `name` | Nombre del workflow o del step |
| `on` | Evento o eventos que activan el workflow |
| `jobs` | Contiene todos los jobs del workflow |
| `runs-on` | Define la máquina virtual donde corre el job |
| `steps` | Lista de pasos dentro de un job |
| `run` | Ejecuta un comando de terminal |
| `uses` | Usa una Action predefinida |
| `with` | Parámetros de configuración para una Action |

---

### Ejecutar múltiples comandos en un step
Si necesitas ejecutar más de un comando en un mismo step, usa el símbolo `|` después de `run:`:

```yaml
- name: Varios comandos
  run: |
    echo "Primer comando"
    echo "Segundo comando"
```

---

### Eventos (Triggers) más comunes

| Evento | Cuándo se activa |
|---|---|
| `push` | Cuando se sube un commit al repositorio |
| `pull_request` | Cuando se abre o cierra un Pull Request |
| `workflow_dispatch` | Cuando el usuario lo activa manualmente desde GitHub |
| `schedule` | En un horario programado (como una tarea automática) |
| `repository_dispatch` | Cuando se activa desde una API externa |

> Lista completa de eventos disponibles: buscar *"GitHub Actions events"* en [docs.github.com](https://docs.github.com)

---

### Actions predefinidas más importantes

> ⚠️ Los jobs se ejecutan en máquinas virtuales limpias que **no contienen tu código**. Por eso, el primer step de casi cualquier workflow debe ser descargar el código con `actions/checkout`.

| Action | Para qué sirve |
|---|---|
| `actions/checkout@v3` | Descarga el código del repositorio a la máquina virtual |
| `actions/setup-node@v3` | Instala una versión específica de Node.js |

> Puedes explorar más actions en el **GitHub Marketplace**: [github.com/marketplace](https://github.com/marketplace)

> 💡 Siempre especifica la versión de una action con `@v3` para evitar que futuras actualizaciones rompan tu workflow de forma inesperada.

---

### Ejemplo completo: ejecutar pruebas automáticamente

Este workflow se activa con cada push. Descarga el código, configura Node.js, instala las dependencias y ejecuta las pruebas del proyecto.

```yaml
name: Ejecutar Pruebas
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Descargar código
        uses: actions/checkout@v3

      - name: Instalar Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Instalar dependencias
        run: npm ci

      - name: Ejecutar pruebas
        run: npm test
```

> **¿Por qué `npm ci` y no `npm install`?**
> `npm ci` instala exactamente las mismas versiones de dependencias que se usaron durante el desarrollo, garantizando que el entorno de pruebas sea idéntico al local.

---

### Permisos del Personal Access Token para workflows

Al subir un workflow por primera vez, el Personal Access Token necesita permisos adicionales además del acceso al repositorio.

Configurarlo en: *Settings → Developer settings → Personal access tokens*
- ✅ Scope `repo` — acceso al repositorio
- ✅ Scope `workflow` — permiso para crear y modificar workflows

---

### Inspeccionar resultados de un workflow

Desde la pestaña **Actions** del repositorio en GitHub puedes monitorear todas las ejecuciones:

| Indicador | Significado |
|---|---|
| 🟡 Círculo amarillo | El workflow está en ejecución |
| ✅ Check verde | El workflow se completó correctamente |
| ❌ X roja | El workflow falló |

> Puedes hacer clic en cualquier ejecución para ver el detalle completo de cada job, cada step y el output exacto de cada comando ejecutado. Esto es especialmente útil para identificar qué salió mal cuando un workflow falla.
```

