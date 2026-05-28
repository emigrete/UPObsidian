---
tags: [devops, deployment, github-actions, docker-hub, render, kubernetes, practica]
unidad: 4
clase: 5
---

# 4️⃣ Deployment — GitHub Actions, Docker Hub y Render

> [!abstract] Idea central
> La **clase práctica**: implementar el pipeline de [[03 - CI-CD|CI/CD]] de punta a punta. **GitHub Actions** corre build + tests, construye la imagen Docker (**multi-stage**), la publica en **Docker Hub** y dispara el deploy a **Render** mediante un *deploy hook*. Es la columna vertebral del **Trabajo Práctico Integrador**.

[[03 - CI-CD|← Unidad 3]] · [[00 - Índice DevOps|Índice]] · Siguiente: [[05 - Monitoreo y Observabilidad]]

---

## 🔄 GitHub Actions
Sistema de CI/CD **integrado a GitHub**. Los workflows se definen en **YAML** dentro de `.github/workflows/`. GitHub los **ejecuta gratis** (hasta cierto límite de minutos/mes; con la **GitHub Student Pack** se accede a beneficios pro).

### Cómo crear uno
`Repo → Actions → Marketplace` → elegir un template (Node.js, Python, etc.) → `Configure`. El template básico ya: descarga dependencias → buildea → corre tests. Se commitea y queda en `.github/workflows/node.js.yml`.

### Estructura de un workflow
```yaml
name: Node.js CI
on: [push, pull_request]        # eventos que lo disparan
jobs:
  build-and-test:
    runs-on: ubuntu-latest       # imagen/VM donde corre (en la nube de GitHub)
    steps:
      - uses: actions/checkout@v3        # checkout del código
      - name: Setup Node
        uses: actions/setup-node@v3
        with: { node-version: '18' }
      - run: npm install
      - run: npm test
      - run: npm run build
```
- **`on`** → eventos (`push`, `pull_request`).
- **`jobs`** → trabajos; cada uno corre en una **máquina/contenedor en la nube de GitHub** (`runs-on`).
- **`steps`** → pasos; pueden usar **actions reutilizables** (`uses:`) o comandos (`run:`).
- **`workflow_dispatch`** → permite **ejecutarlo manualmente** desde la UI (elegir la rama).

> [!tip] Estrategia *matrix*
> Permite **iterar** un job sobre varias combinaciones. Ej.: buildear para **distintas versiones de Node** (compatibilidad hacia atrás de una librería/SDK) o para **distintas arquitecturas** (`amd64`/`x86_64` y **`arm64`** para las Mac nuevas). *El profe contó que en cuatrimestres pasados un grupo tuvo que buildear la imagen multi-arquitectura porque a un integrante con Mac ARM no le funcionaba la imagen.*

> [!note] Runners y self-hosting
> Los jobs corren en **runners** de GitHub (en la nube). Se pueden tener **runners self-hosted** (locales) para ahorrar minutos. Esto es **distinto** de *dónde queda desplegada la app* (eso es Render/la nube). **Jenkins vs GitHub Actions:** Jenkins es *self-hosted* y da más control para *on-premise*; GitHub Actions está **integrado** a GitHub y es más simple para este flujo.

---

## 🐳 Dockerfile multi-stage
Construir la imagen en **varias etapas** y quedarse solo con lo mínimo para producción → **imagen final más chica y segura**.

```dockerfile
# ── Stage 1: build ──
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci                 # solo dependencias necesarias
COPY . .
RUN npm run build          # transpila TS → JS (carpeta dist/)

# ── Stage 2: producción ──
FROM node:20-alpine
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
USER node                  # no correr como root
CMD ["node", "dist/index.js"]
```
- Cada **stage es una imagen separada**; el último `FROM` define **lo que queda como contenedor final** (las imágenes intermedias se **descartan**).
- Se **copia entre stages** con `COPY --from=<stage>` (ej.: traer solo `dist/` y `node_modules`).
- Usar **imágenes oficiales** y lo más **chicas** posibles (**Alpine** = Linux muy liviano; Debian/Slim son más grandes). Si Alpine pide instalar libs del sistema, conviene revisarlo.
- Al transpilar TypeScript, las **interfaces** desaparecen (JS puro no las usa); a `dist/` va solo el **código productivo**.

> [!question] ¿Un Dockerfile por ambiente? (pregunta de un alumno)
> El profe desaconsejó tener imágenes distintas por ambiente: **rompe la ventaja de Docker** (mismo ambiente reproducible). Lo que sí se suele hacer es un `Dockerfile.dev` o `docker-compose.dev` con *hot-reload* y datos de prueba (volumen + `.gitignore`), pero **la imagen productiva es la misma** que se promueve entre ambientes.

---

## 📦 Publicar en Docker Hub
1. Crear cuenta en **Docker Hub** → generar **Access Token** (API Key).
2. En el repo de GitHub: `Settings → Secrets and variables → Actions` → crear **secrets** (`DOCKER_USER`, `DOCKER_TOKEN`, y los de la app/BD).
3. En el workflow: hacer **login** en Docker Hub con los secrets, **extraer metadata** (tags/labels que pusiste en el Dockerfile), **buildear** y hacer **push** de la imagen.

> [!important] Publicar solo en `main`
> El push de la imagen debe correr **únicamente al mergear a `main`** (con una condición `if`), no en cada PR. En los **PR solo corren build + tests** para verificar que el código está sano; la imagen se publica recién cuando el PR está aprobado y mergeado. *No tiene sentido publicar una imagen por cada commit.*

### Versionado: SemVer vs `latest`
- Usar **versiones** (`v1.2.3`) y **evitar `latest`**: `latest` baja siempre "la última" y **no sabés qué versión estás desplegando**.
- Forma simple: usar el **SHA** del commit como tag.
- Forma más profesional (aporte de un alumno): usar la **action `docker/metadata-action`** para generar **SemVer** automáticamente (toma la última versión y le suma 1) y subir los tags `1.2.3`, `1.2`, `latest`.
- Alternativa a Docker Hub: **GitHub Container Registry (GHCR)**.

---

## 🚀 Deploy a Render
**Render** (`render.com`) ofrece un **free tier** (512 MB RAM, CPU compartida). Pasos:
1. Crear cuenta (mejor con **email/password**, no logueando con GitHub, para no "linkear" el repo).
2. Crear un **Web Service** → tipo **Existing image** (usar la imagen que publicamos, **no** el código del repo) → free tier.
3. En `Settings → Deploy` copiar el **Deploy Hook** (URL secreta).
4. Guardar el hook como **secret** en GitHub (`RENDER_DEPLOY_HOOK_URL`).
5. Agregar un **step de deploy** en el Action (también solo en `main`) que hace un **`curl` al deploy hook**, pasándole por *query param* **qué imagen** desplegar (el SHA o la versión SemVer).

```yaml
- name: Deploy a Render
  if: github.ref == 'refs/heads/main'
  run: |
    curl "${{ secrets.RENDER_DEPLOY_HOOK_URL }}&imgURL=usuario/mi-app:${{ github.sha }}"
```

> [!warning] Error MUY común (lo repitió varias veces)
> **Linkear la cuenta de GitHub a Render NO cuenta como deploy.** Eso solo conecta el repo. El deploy "válido" es que el **GitHub Action dispare el deploy hook** y le diga a Render **qué imagen de Docker** usar. Si no versionás (queda en `latest`), tampoco podés controlar qué versión se despliega.

---

## ☸️ Kubernetes (introducción / laboratorio)
- **Para qué sirve:** dar **estabilidad y escalado** a la app → **balanceo de carga** entre múltiples instancias y **autoscaling** según la demanda. Siempre hay una instancia disponible.
- **Cuándo NO usarlo:** para **una sola instancia** no tiene sentido (sería sobre-ingeniería); ahí alcanza con un servicio simple. *(Docker Compose **no se usa en producción**; en prod se usan motores que orquestan las imágenes, como los servicios de Amazon o Kubernetes.)*
- Conceptos que aparecieron: **Namespace**, **Deployment**, **Service**, **Pods**, escalado.
- **Laboratorio que el profe quería armar:** un flujo completo con **Terraform** (crear infra) + imagen Docker + **Kubernetes** (minikube) para hacer un mini-deploy local. Pensaba usar **LocalStack** (simular AWS/EKS localmente, sin tarjeta) pero **archivaron el proyecto open source** justo antes; se evaluó **MiniStack** como alternativa.

> [!summary] Resultado para el TP
> Lo imprescindible de esta unidad: saber armar el **GitHub Action** (build + tests en cada PR; publicación de imagen versionada solo en `main`), crear la cuenta y el servicio en **Render**, y disparar el deploy con el **hook**. Con eso el TP queda desplegado end-to-end y listo para conectar el [[05 - Monitoreo y Observabilidad|monitoreo]].
