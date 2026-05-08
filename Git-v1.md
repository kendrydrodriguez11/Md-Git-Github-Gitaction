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
| `git add .` | Añade todos los archivos modificados al staging |
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
| **Contexto** | Datos de entorno accesibles en los steps con la sintaxis `${{ }}` |

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
| `needs` | Hace que un job espere a que otro termine antes de iniciar |
| `outputs` | Define valores que un job expone para que otros jobs los usen |

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
| `pull_request` | Cuando se abre, sincroniza o reabre un Pull Request |
| `issues` | Cuando se crea, edita, cierra, etc. un issue |
| `workflow_dispatch` | Cuando el usuario lo activa manualmente desde GitHub |
| `schedule` | En un horario programado (como una tarea automática) |
| `repository_dispatch` | Cuando se activa desde una API externa |

> Lista completa de eventos disponibles: buscar *"GitHub Actions events"* en [docs.github.com](https://docs.github.com)

---

### Múltiples triggers

Puedes definir varios eventos en un mismo workflow envolviéndolos entre corchetes:

```yaml
on: [push, workflow_dispatch]
```

Con esta configuración el workflow se activa tanto automáticamente con cada push, como manualmente desde la pestaña Actions en GitHub.

---

### Tipos de actividad (Activity Types)

Algunos eventos tienen distintas acciones internas. Puedes usar `types` para controlar exactamente qué acción específica dispara el workflow.

```yaml
on:
  pull_request:
    types: [opened, edited]   # solo al abrir o editar una PR

  workflow_dispatch:           # también acepta activación manual
```

> ⚠️ Si no defines `types` en `pull_request`, por defecto solo reacciona a `opened`, `synchronize` y `reopened`. Acciones como `closed` deben agregarse explícitamente.

> 💡 Consulta qué tipos de actividad tiene cada evento en la documentación oficial: [docs.github.com](https://docs.github.com)

---

### Filtros de eventos

Los filtros permiten restringir aún más cuándo se ejecuta el workflow. Funcionan principalmente con `push`, `pull_request`, `pull_request_target` y `workflow_call`.

**Filtrar por rama:**
```yaml
on:
  push:
    branches:
      - main          # solo pushes a main
      - dev           # o a dev
      - feat/**       # o a ramas como feat/login, feat/ui/navbar
```

**Ignorar ramas:**
```yaml
on:
  push:
    branches-ignore:
      - dev-*         # ignora dev-new, dev-feature, etc.
```

**Filtrar por archivos modificados:**
```yaml
on:
  push:
    branches:
      - main
    paths-ignore:
      - '.github/workflows/*'   # no se ejecuta si solo cambiaron workflows
```

| Comodín | Qué representa |
|---|---|
| `*` | Cualquier texto excepto `/` (ej. `dev-*` → `dev-new`, `dev-x`) |
| `**` | Cualquier texto incluyendo `/` (ej. `feat/**` → `feat/ui/navbar`) |

---

### Pull Requests desde forks

Cuando alguien externo hace fork de tu repositorio público y abre una pull request, el workflow **no se ejecuta automáticamente** la primera vez.

GitHub requiere **aprobación manual** por razones de seguridad y para evitar abusos de recursos.

- Primera contribución de un usuario externo → requiere aprobación manual del propietario.
- Contribuciones siguientes del mismo usuario → se ejecutan automáticamente.
- Colaboradores oficiales del repositorio → siempre se ejecutan sin aprobación.

---

### Cancelar y omitir workflows

**Cancelación automática:** Si un job falla, el workflow se cancela automáticamente (a menos que se configure lo contrario).

**Cancelación manual:** Desde la pestaña Actions en GitHub, abre la ejecución en curso y usa el botón "Cancel workflow". Útil cuando detectas un error justo después del push.

**Omitir un workflow con anotaciones en el commit:**
```bash
git commit -m "fix typo in comments [skip ci]"
```

Otras variantes válidas: `[skip actions]`

> Útil para cambios irrelevantes como comentarios o documentación que no necesitan ejecutar el pipeline completo.

---

### Jobs en paralelo y secuenciales

**Por defecto los jobs se ejecutan en paralelo.** La duración total del workflow es la del job más largo, no la suma de todos.

```yaml
jobs:
  test:                   # corre en paralelo con deploy
    runs-on: ubuntu-latest
    steps: ...

  deploy:                 # corre en paralelo con test
    runs-on: ubuntu-latest
    steps: ...
```

**Para ejecutar jobs en secuencia, usa `needs`:**

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps: ...

  deploy:
    needs: test           # deploy solo inicia si test terminó con éxito
    runs-on: ubuntu-latest
    steps: ...
```

- Si `test` falla → `deploy` no se ejecuta.
- Si `test` tiene éxito → `deploy` comienza automáticamente.
- Para esperar múltiples jobs: `needs: [test, lint]`

> ⚠️ Cada job tiene su propio runner independiente, por lo que los steps de configuración (`checkout`, `setup-node`, `npm ci`) deben repetirse en cada job que los necesite.

---

### Actions predefinidas más importantes

> ⚠️ Los jobs se ejecutan en máquinas virtuales limpias que **no contienen tu código**. Por eso, el primer step de casi cualquier workflow debe ser descargar el código con `actions/checkout`.

| Action | Para qué sirve |
|---|---|
| `actions/checkout@v3` | Descarga el código del repositorio a la máquina virtual |
| `actions/setup-node@v3` | Instala una versión específica de Node.js |
| `actions/upload-artifact@v3` | Sube archivos generados por un job como artefacto |
| `actions/download-artifact@v3` | Descarga artefactos subidos en otro job |
| `actions/cache@v3` | Almacena y reutiliza carpetas entre ejecuciones del workflow |

> Puedes explorar más actions en el **GitHub Marketplace**: [github.com/marketplace](https://github.com/marketplace)

> 💡 Siempre especifica la versión de una action con `@v3` para evitar que futuras actualizaciones rompan tu workflow de forma inesperada.

---

### Artefactos (Artifacts)

Los **artefactos** son archivos o carpetas que genera un job y que quieres guardar para no perderlos cuando el workflow termina.

Casos de uso:
- Archivos de un sitio web generado (`dist/`)
- Binarios de una aplicación
- Logs de pruebas

**Subir un artefacto:**
```yaml
- name: Subir artefacto
  uses: actions/upload-artifact@v3
  with:
    name: dist-files        # nombre del artefacto
    path: |
      dist
      package.json          # puedes incluir varias rutas
```

**Descargar el artefacto en otro job:**
```yaml
- name: Descargar artefacto
  uses: actions/download-artifact@v3
  with:
    name: dist-files        # debe coincidir con el nombre usado al subir
```

> ⚠️ Los archivos descargados aparecen directamente en el directorio de trabajo del job, no dentro de una carpeta `dist`. GitHub los extrae automáticamente.

Los artefactos también pueden descargarse manualmente desde la interfaz de GitHub Actions como un archivo ZIP.

---

### Outputs de job (Job Outputs)

Los **outputs** permiten pasar valores simples (texto, nombres de archivos, IDs) entre jobs, sin necesidad de subir artefactos completos.

**Definir una salida en un step:**
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      script-file: ${{ steps.publish.outputs.script-file }}   # expone la salida

    steps:
      - name: Obtener nombre del archivo
        id: publish                                            # id del step
        run: |
          FILENAME=$(ls dist/assets/*.js | head -1)
          echo "script-file=$FILENAME" >> $GITHUB_OUTPUT      # registra la salida
```

**Usar esa salida en otro job:**
```yaml
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Mostrar nombre del archivo
        run: echo "${{ needs.build.outputs.script-file }}"
```

| Concepto | Descripción |
|---|---|
| `outputs` (nivel job) | Declara qué valores expone el job |
| `id` (nivel step) | Identificador del step para referenciar sus salidas |
| `$GITHUB_OUTPUT` | Variable especial donde se escriben los pares `clave=valor` |
| `needs.<job>.outputs.<nombre>` | Forma de acceder a la salida en un job dependiente |

> ⚠️ La sintaxis antigua `::set-output` está obsoleta. Usa siempre `$GITHUB_OUTPUT`.

---

### Diferencia entre Artefactos y Outputs

| | Artefactos | Outputs de job |
|---|---|---|
| **¿Qué son?** | Archivos o carpetas completos | Valores simples (texto, IDs, nombres) |
| **Cuándo usarlos** | Para compartir archivos entre jobs o descargarlos manualmente | Para pasar datos pequeños entre jobs |
| **Cómo se acceden** | Con `download-artifact` | Con `needs.<job>.outputs.<nombre>` |
| **Ejemplo** | Carpeta `dist/` generada por el build | Nombre del archivo JS generado |

---

### Caché de dependencias

El **caché** permite almacenar carpetas entre ejecuciones del workflow para no tener que reinstalar dependencias cada vez.

```yaml
- name: Cachear dependencias
  uses: actions/cache@v3
  with:
    path: ~/.npm                          # carpeta a cachear (caché interna de npm)
    key: deps-${{ hashFiles('package-lock.json') }}  # clave única basada en el contenido
```

**¿Cómo funciona la clave (`key`)?**
- `hashFiles('package-lock.json')` genera un valor único basado en el contenido del archivo.
- Si las dependencias no cambian, el hash es el mismo → se reutiliza la caché.
- Si `package-lock.json` cambia, el hash cambia → se crea una nueva caché.

**Comportamiento de GitHub Actions:**
- ✅ Caché existente y válida → la descarga y reutiliza. Instalación muy rápida.
- 🔄 Sin caché o clave diferente → instala normalmente y guarda una nueva caché al terminar.

> ⚠️ Como cada job corre en su propio runner, debes agregar el step de caché en **cada job** que instale dependencias (test, build, etc.).

**Flujo completo con caché:**
```yaml
steps:
  - uses: actions/checkout@v3

  - name: Cachear dependencias
    uses: actions/cache@v3
    with:
      path: ~/.npm
      key: deps-${{ hashFiles('package-lock.json') }}

  - name: Instalar dependencias
    run: npm ci
```

> 💡 La primera ejecución siempre será más lenta (no hay caché). Las siguientes serán notablemente más rápidas mientras `package-lock.json` no cambie.

---

### Contexto y expresiones

GitHub Actions proporciona datos de contexto con información sobre el workflow, el evento que lo disparó, el runner, entre otros.

```yaml
- name: Mostrar contexto completo
  run: echo "${{ toJSON(github) }}"

- name: Mostrar solo datos del evento
  run: echo "${{ toJSON(github.event) }}"
```

| Sintaxis | Descripción |
|---|---|
| `${{ }}` | Sintaxis para acceder a datos dinámicos dentro de un step |
| `github` | Objeto con información general del workflow y el evento (repo, rama, usuario, etc.) |
| `github.event` | Solo los datos del evento que disparó el workflow (título del issue, URL, etc.) |
| `toJSON()` | Convierte un objeto a texto legible |
| `needs` | Contexto con resultados de los jobs de los que depende el job actual |
| `hashFiles()` | Genera un hash basado en el contenido de uno o más archivos |

> Documentación completa: buscar *"GitHub Actions context"* y *"GitHub Actions expressions"* en [docs.github.com](https://docs.github.com)

---

### Inspeccionar resultados de un workflow

Desde la pestaña **Actions** del repositorio en GitHub puedes monitorear todas las ejecuciones:

| Indicador | Significado |
|---|---|
| 🟡 Círculo amarillo | El workflow está en ejecución |
| ✅ Check verde | El workflow se completó correctamente |
| ❌ X roja | El workflow falló |

> Puedes hacer clic en cualquier ejecución para ver el detalle completo de cada job, cada step y el output exacto de cada comando ejecutado. Esto es especialmente útil para identificar qué salió mal cuando un workflow falla.

**Flujo para corregir un fallo:**
```
1. Revisar el log del paso fallido en la pestaña Actions
2. Corregir el código o revertir el commit con: git revert <ID>
3. Hacer push → se activa automáticamente una nueva ejecución
```

> Si el workflow incluye pruebas automatizadas, un fallo es el comportamiento **esperado** cuando el código no cumple las expectativas definidas. El objetivo es detectar estos problemas antes de que lleguen a producción.

---

### Permisos del Personal Access Token para workflows

Al subir un workflow por primera vez, el Personal Access Token necesita permisos adicionales además del acceso al repositorio.

Configurarlo en: *Settings → Developer settings → Personal access tokens*
- ✅ Scope `repo` — acceso al repositorio
- ✅ Scope `workflow` — permiso para crear y modificar workflows

---

### Resumen de bloques de GitHub Actions

| Elemento | Descripción |
|---|---|
| **Workflow** | Proceso automatizado vinculado al repositorio. Definido en `.github/workflows/*.yml` |
| **Trigger/Evento** | Condición que activa el workflow. Puede tener uno o varios (ej. `push`, `workflow_dispatch`) |
| **Job** | Tarea dentro del workflow. Define el runner y los steps. Paralelo por defecto, secuencial con `needs` |
| **Step** | Acción individual dentro de un job. Se ejecutan en orden con `run` (comando) o `uses` (action) |
| **Runner** | Servidor donde se ejecutan los jobs. GitHub ofrece Linux, Windows y macOS |
| **Action** | Aplicación predefinida y reutilizable usada en un step (oficial, de la comunidad o propia) |
| **Contexto** | Datos de entorno accesibles en los steps con la sintaxis `${{ }}` |
| **Artefacto** | Archivo o carpeta generado por un job que se guarda para su descarga o reutilización |
| **Output** | Valor simple que un job expone para que otros jobs dependientes lo reutilicen |
| **Caché** | Carpeta almacenada entre ejecuciones para evitar reinstalar dependencias innecesariamente |

---

### Ejemplo completo: Lint, Test & Deploy con caché y artefactos

```yaml
name: Deploy Website
on: push

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - name: Cachear dependencias
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: deps-${{ hashFiles('package-lock.json') }}
      - run: npm ci
      - run: npm run lint
      - run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest
    outputs:
      script-file: ${{ steps.publish.outputs.script-file }}
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - name: Cachear dependencias
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: deps-${{ hashFiles('package-lock.json') }}
      - run: npm ci
      - run: npm run build
      - name: Capturar nombre del archivo JS
        id: publish
        run: |
          FILENAME=$(ls dist/assets/*.js | head -1)
          echo "script-file=$FILENAME" >> $GITHUB_OUTPUT
      - name: Subir artefacto
        uses: actions/upload-artifact@v3
        with:
          name: dist-files
          path: dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Descargar artefacto
        uses: actions/download-artifact@v3
        with:
          name: dist-files
      - name: Mostrar archivo principal
        run: echo "${{ needs.build.outputs.script-file }}"
      - name: Desplegar
        run: echo "Desplegando sitio web..."
```

---

### Ejemplo: Workflows de ejercicio

**Workflow 1 — Un solo job**
```yaml
name: Ejercicio de Despliegue
on: push
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npm run lint
      - run: npm run test
      - run: npm run build
      - run: echo "Desplegando..."
```

**Workflow 2 — Issues**
```yaml
name: Manejar Issues
on: issues
jobs:
  info:
    runs-on: ubuntu-latest
    steps:
      - name: Mostrar detalles del evento
        run: echo "${{ toJSON(github.event) }}"
```

> Este workflow se activa cuando ocurre cualquier acción relacionada con issues (crear, editar, cerrar, etc.). Para probarlo, crea un issue en el repositorio; no se activa con push.