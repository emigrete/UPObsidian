---
tags: [devops, git, github, docker, herramientas, contenedores]
unidad: 2
clase: 3
---

# 2️⃣ Herramientas: Git, GitHub y Docker

> [!abstract] Idea central
> Las tres herramientas base del flujo DevOps: **Git** (control de versiones), **GitHub** (hosting y colaboración) y **Docker** (contenedores). Juntas permiten versionar, colaborar y **empaquetar la app con su entorno** para que corra igual en cualquier lado.

[[01 - Introducción a DevOps|← Unidad 1]] · [[00 - Índice DevOps|Índice]] · Siguiente: [[03 - CI-CD]]

---

## 🔧 Git
Sistema de **control de versiones distribuido**, creado por **Linus Torvalds en 2005** para el kernel de Linux. Rastrea los cambios del código en el tiempo.

**Objetivos de un VCS:** trazabilidad (quién cambió qué y cuándo), colaboración segura, recuperar versiones estables, soportar *branching/merging/releases*.
**Beneficios:** menor riesgo de pérdida de trabajo, auditoría técnica, más velocidad para experimentar y corregir.

### Tipos de VCS
- **Local** — historial en una sola máquina.
- **Centralizado (CVCS)** — un servidor central único; **depende** de él, hay que estar conectado.
- **Distribuido (DVCS)** — cada dev tiene **copia completa** del repo → trabajo *offline*, mayor resiliencia y flexibilidad. *(El profe: con un DVCS hasta podrías compartir cambios por email; al hacer `git clone` te bajás todo el repo.)*

### Comandos esenciales
```bash
git init                # inicializa repositorio local
git clone <url>         # clona un repositorio remoto
git add . / git add <f> # prepara cambios (→ staging)
git commit -m "msg"     # registra cambios en el repo local
git status              # estado del working tree
git log --oneline       # historial resumido
git pull                # trae y fusiona cambios del remoto
git push                # publica commits en el remoto
```

### Modelo de datos interno (lo que el profe quiso que se entienda)
Git modela el historial como un **grafo de snapshots inmutables**:
- **Blob** → contenido de un archivo.
- **Tree** → estructura de directorios/archivos.
- **Commit** → *snapshot* con autor, fecha, mensaje y referencia al **commit padre**.
- **Branch** → **puntero móvil** al commit más reciente de una línea de trabajo.
- **HEAD** → referencia a la rama/commit donde estás parado actualmente.
- **Tag** → referencia **fija** a un commit (marcar versiones `v1.0.0`, facilitar *rollback*, asociar *releases* y *release notes*). Todo vive dentro de la carpeta `.git`.

> [!example] Demo en vivo del profe
> Mostró `git log --graph` / *one-liners* y **alias** para ver el historial bonito; `git init` → `touch` → `git add -p` (elegir qué cambios mandar) → `git commit`; y la diferencia entre **`git checkout -b`** (crea rama) y **`git switch`** (cambiar de rama, más intuitivo). Al cambiar de rama, los archivos de la otra rama "desaparecen" del working dir.

### Conventional Commits
Convención de mensajes **claros y automatizables**: `<tipo>(scope opcional): descripción`.
- `feat` (nueva funcionalidad), `fix` (bug), `docs`, `chore` (mantenimiento), `refactor`.
- **Buenas prácticas:** verbo **imperativo** (add, fix, remove, update), **una idea por commit** (atómicos), título breve (≤ 72), explicar el *qué* y, si hace falta, el *por qué*.
- **Beneficio clave:** estandariza el historial, facilita revisiones y permite **automatizar el versionado** en CI/CD (calcular *bump* major/minor/fix según los commits y generar *changelogs*).

> [!tip] No te apoyes solo en Jira
> El profe advirtió: poner descripciones pobres en el commit y "que se vea en el ticket de Jira" es riesgoso → si la empresa migra de proveedor de tickets, **perdés la referencia**. El contexto debe quedar **dentro del control de versiones**.

### Conventional Comments (para *code reviews*)
Prefijos estandarizados en los comentarios de PR para dar **contexto y prioridad** a quien recibe el feedback:
- `praise:` reconocimiento · `nitpick:`/`picky:` detalle menor, subjetivo · `suggestion:` mejora **no bloqueante** · `issue:` problema (suele ser bloqueante) · `question:` duda · `todo:` pendiente · `note:`.
- Más info: *conventionalcomments.org*. *(El profe lo mostró sobre un repo real de Meta.)*

### Flujos de trabajo (branching strategies)
- **GitHub Flow** — una rama corta por cambio + PR hacia `main`; integración frecuente y deploy continuo. Ideal para **equipos pequeños/medianos** con releases frecuentes.
- **Git Flow** — ramas dedicadas `feature/*`, `release/*`, `hotfix/*`; más estructurado, para **ciclos de release formales** con ventanas definidas (ej. kernel de Linux).
- **Trunk-Based Development** — cambios chicos y frecuentes sobre `main`, ramas de vida **muy corta** + *feature flags*. Requiere **fuerte automatización de CI/CD**.

> [!note] Conflictos al mergear (consejo del profe)
> Si dos personas tocan el mismo archivo (típico: los *imports* al inicio), el **último que mergea resuelve el conflicto**. Regla: primero **dejarlo funcionando** (incluso conservando imports de más) y **después limpiar en otro commit**. Lo ideal es organizarse para tocar partes distintas.

### Workflow de Git
`Working Directory` → `Staging Area` (`git add`) → `Local Repository` (`git commit`) → `Remote Repository` (`git push`).

---

## 🐙 GitHub
Plataforma de hosting y colaboración sobre Git. Otras: **GitLab** y **Bitbucket** (estas dos permiten repos **privados** en cuenta gratuita; ahí está su principal diferencia con GitHub gratis).

**Funcionalidades clave:** repositorios, colaboración, **Issues** (reportar bugs/tareas), **Pull Requests** (revisión y aprobación de cambios), integraciones, wikis/documentación.

**Flujo inicial:** crear repo → `git clone` → cambios en una rama → `git push` → abrir **Pull Request** → review → **merge** a `main`.

**GitHub Flow:** todo lo que está en `main` está **listo para producción**; cada cosa nueva = rama nueva; al terminar → PR → review → merge → a producción (idealmente `main` siempre sincronizado con producción).

> [!info] Impacto de GitHub
> Transformó cómo se desarrolla y colabora; impulsó la cultura **open source** y la transparencia. *(El profe matizó: a veces "open source" es relativo, no siempre se publica todo el código.)*

---

## 🐳 Docker
Plataforma de **virtualización de contenedores**, lanzada en **2013**. Empaqueta y despliega aplicaciones de forma eficiente.

### Contenedores vs Máquinas Virtuales
| Aspecto | Contenedor Docker | Máquina Virtual |
|---|---|---|
| Qué emula | Espacio de usuario (abstrae el **SO**) | Máquina física completa (abstrae el **hardware**) |
| Kernel | **Comparte** el del host (namespaces + cgroups) | Kernel **propio** por VM |
| Gestión | Docker Engine | Hipervisor |
| Recursos | Bajo demanda, **livianos** | Cantidad fija, **pesados** |

**Ventajas de Docker sobre VMs:** arranque en **milisegundos**, *overhead* en MB (no GB), **densidad** (muchos por máquina), **portabilidad** (misma imagen = mismo comportamiento), CI/CD ágil y **escalado horizontal**.
**Desventaja:** todos comparten el **mismo kernel** → si hay una vulnerabilidad en el kernel, todos quedan expuestos.

### Conceptos base
- **Imagen** → plantilla **versionada e inmutable** (la app + runtime + dependencias).
- **Contenedor** → **instancia en ejecución** de una imagen (proceso aislado).
- **Dockerfile** → archivo con instrucciones para construir la imagen.
- **Registry** → almacén de imágenes (Docker Hub, registries privados).

### Arquitectura
**Client** (CLI) → **Daemon** (servicio de fondo que gestiona contenedores e imágenes; puede ser otro, ej. *colima*) → **Images** → **Containers** → **Registry**.
Flujo: el cliente manda el comando al daemon → el daemon verifica/descarga la imagen del registry → crea y ejecuta el contenedor. (`docker run` hace todo junto.)

### Capas (layers) y caché — lo que más explicó el profe
Cada instrucción del Dockerfile crea una **capa** sobre la anterior:
```dockerfile
FROM nginx:alpine          # capa base
RUN apt-get install curl   # nueva capa (binarios de curl)
COPY . /app                # nueva capa (tu código)
ENTRYPOINT ["nginx"]       # configuración
```
**Ventaja:** Docker **cachea** las capas que no cambian; al reconstruir solo regenera **de la capa modificada hacia adelante**.

> [!tip] Buena práctica de orden de capas
> Poner lo que **casi nunca cambia** (instalar dependencias del sistema, `curl`, etc.) **arriba**, y el **`COPY` del código** (que cambia siempre) lo **más abajo posible**. Así las capas pesadas se reutilizan de caché y el *build* es mucho más rápido. *(Larga discusión con un alumno: el orden importa para el caché aunque dos instrucciones independientes den el mismo resultado final.)*

### Volúmenes (persistencia / compartir datos)
- **Bind mount** (`-v /host/path:/container/path`) → mapea un directorio del host al contenedor (ideal en desarrollo: *live update*, persistir datos de una BD local).
- **Named volume** → gestionado por Docker.
- **Anonymous volume** → temporal, se borra con el contenedor.
- Regla: separar **datos (stateful)** de la **app (stateless)**; persistir BD en un volumen, no en `/tmp`.

### Golden Image
Imagen base **confiable, versionada e inmutable**, compartida en la organización con todo lo que se necesita habitualmente (mismo stack/framework + cosas de seguridad). El profe la describió como un **"template seguro"**. Reduce diferencias entre dev/test/prod, mejora seguridad y consistencia, y facilita *rollback* por tags.
> Buenas prácticas: **versionar imágenes** (`app:1.2.0`) y **evitar `latest`**; **promover la misma imagen** entre ambientes.

### Casos de uso
Despliegue de apps · entornos de **desarrollo** reproducibles (no instalar todo localmente) · **pruebas/QA** aisladas y versionadas · **microservicios** (cada servicio en su contenedor, escala/reinicia independiente).

> [!summary] Flujo end-to-end (cómo se conectan las 3)
> 1) Cambios en **local** → 2) versionar con **Git** (`add`, `commit`) → 3) publicar rama + **Pull Request** en **GitHub** → 4) construir **imagen Docker** → 5) desplegar la **misma imagen** en distintos entornos. Esto se profundiza en [[03 - CI-CD]] y [[04 - Deployment]].
