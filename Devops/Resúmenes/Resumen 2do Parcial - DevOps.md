---
title: "Resumen 2do Parcial — DevOps"
tags:
  - devops
  - resumen
  - 2do-parcial
aliases:
  - "Resumen DevOps 2do Parcial"
materia: DevOps
profesor: Lucas Bonanni
universidad: Universidad de Palermo
---

# 📘 Resumen 2do Parcial — DevOps

> [!info] Qué entra en el 2do parcial
> Según [[Preguntas Integradoras - DevOps (2do Parcial)]], en el parcial entran **4 unidades**:
> - 4️⃣ [[04 - Deployment|Deployment]] — GitHub Actions, Docker Hub, Render, Kubernetes
> - 5️⃣ [[05 - Monitoreo y Observabilidad|Monitoreo y Observabilidad]] — SLI/SLO/SLA, 3 pilares, OTel, alertas
> - 6️⃣ [[06 - Terraform (Infraestructura como Código)|Terraform (IaC)]]
> - 7️⃣ [[07 - Seguridad (OWASP Top 10)|Seguridad — OWASP Top 10 (2025)]]
>
> Este es un resumen **consolidado de una página** para repaso. Para el detalle de cada tema, seguí los wikilinks a la nota de la unidad.

> [!tip] Cómo usar este resumen
> Es para leer "de corrido" antes del parcial. Si querés practicar *active recall*, abrí [[Preguntas Integradoras - DevOps (2do Parcial)|las preguntas integradoras]] (respuestas plegadas). El parcial es a **desarrollar** (con Respondus), así que importa **explicar el porqué** y **conectar unidades**, no solo definir.

---

## 4️⃣ Deployment — GitHub Actions, Docker Hub y Render

> [!abstract] Idea central
> Implementar el pipeline de **CI/CD de punta a punta**: GitHub Actions corre build + tests, construye la imagen Docker **multi-stage**, la publica **versionada** en Docker Hub y dispara el deploy a **Render** vía *deploy hook*. Es la columna vertebral del **TP Integrador**.

### GitHub Actions
Sistema de CI/CD **integrado a GitHub**. Workflows en **YAML** dentro de `.github/workflows/`. GitHub los ejecuta gratis (límite de minutos/mes; la **GitHub Student Pack** da beneficios pro).

Estructura de un workflow:
- **`on`** → eventos que lo disparan (`push`, `pull_request`). `workflow_dispatch` permite **ejecutarlo manualmente** desde la UI eligiendo la rama.
- **`jobs`** → trabajos; cada uno corre en una máquina/contenedor en la nube de GitHub, definida por **`runs-on`** (ej. `ubuntu-latest`).
- **`steps`** → pasos; usan **actions reutilizables** (`uses:`, ej. `actions/checkout@v3`) o comandos directos (`run:`).

- **Estrategia *matrix*:** itera un mismo job sobre varias combinaciones → distintas versiones de Node (compatibilidad) o distintas arquitecturas (`amd64` y **`arm64`** para Mac ARM).
- **Runners:** los jobs corren en runners de GitHub (nube); se pueden tener **self-hosted** (locales) para ahorrar minutos. ⚠️ *Dónde corre el job* ≠ *dónde queda desplegada la app* (eso es Render). **Jenkins** es self-hosted (más control on-premise); **GitHub Actions** está integrado y es más simple.

### Dockerfile multi-stage
Construir la imagen en **varias etapas** para quedarse solo con lo mínimo de producción → **imagen final más chica y segura**.
- Cada **stage es una imagen separada**; el **último `FROM`** define el contenedor final, las intermedias se **descartan**.
- Se copia entre etapas con **`COPY --from=<stage>`** (ej. traer solo `dist/` y `node_modules`).
- Buenas prácticas: **imágenes oficiales y chicas** (**Alpine** = Linux liviano), correr con **`USER node`** (no root). Al transpilar TS las interfaces desaparecen; a `dist/` va solo el código productivo.

> [!question] ¿Un Dockerfile por ambiente?
> **No** — rompe la ventaja de Docker (mismo ambiente reproducible). Se puede tener un `Dockerfile.dev`/`docker-compose.dev` con *hot-reload* y datos de prueba, pero **la imagen productiva es la misma** que se promueve entre ambientes.

### Publicar en Docker Hub
1. Cuenta en Docker Hub → **Access Token**.
2. En GitHub (`Settings → Secrets and variables → Actions`) crear los **secrets** (`DOCKER_USER`, `DOCKER_TOKEN`, app/BD).
3. En el workflow: **login** → extraer **metadata** → **build** → **push**.

> [!important] Push solo en `main` + evitar `latest`
> El push de la imagen corre **únicamente al mergear a `main`** (con un `if`); en los **PR solo build + tests**. **Evitar `latest`** porque baja siempre "la última" y no sabés qué versión desplegás. Alternativas: versiones explícitas (SemVer `v1.2.3`), el **SHA** del commit, o la action **`docker/metadata-action`** (genera SemVer automático). Alternativa a Docker Hub: **GHCR**.

### Deploy a Render
Free tier (512 MB RAM, CPU compartida).
1. Crear cuenta con **email/password** (no logueando con GitHub, para no "linkear" el repo).
2. Crear un **Web Service** tipo **Existing image** (usa la imagen publicada, **no** el código).
3. Copiar el **Deploy Hook** (URL secreta) de `Settings → Deploy`.
4. Guardarlo como secret (`RENDER_DEPLOY_HOOK_URL`).
5. Step de deploy en el Action (solo en `main`) que hace **`curl` al deploy hook** pasando por *query param* **qué imagen** (SHA/SemVer).

> [!warning] Error MUY común
> **Linkear la cuenta de GitHub a Render NO cuenta como deploy** — eso solo conecta el repo. El deploy válido es que el **Action dispare el deploy hook** indicándole a Render qué imagen Docker usar.

### Kubernetes (intro/laboratorio)
- **Para qué:** **estabilidad y escalado** → balanceo de carga entre instancias + autoscaling; siempre hay una instancia disponible.
- **Cuándo NO:** para **una sola instancia** es sobre-ingeniería. *(Docker Compose **no se usa en producción**; en prod van orquestadores como los servicios de Amazon o Kubernetes.)*
- Conceptos: **Namespace, Deployment, Service, Pods**, escalado.
- Laboratorio pensado: Terraform + imagen Docker + Kubernetes (minikube), con LocalStack (lo archivaron) o MiniStack como alternativa.

---

## 5️⃣ Monitoreo y Observabilidad

> [!abstract] Idea central
> Observar la salud y el rendimiento para ser **proactivos**. Pero **monitorear sin objetivos es solo ruido**: primero definís **SLOs** (qué garantizás), después **qué medís**. Tres pilares: **métricas, logs y trazas**. Esta unidad **cierra el feedback loop** de la Unidad 1.

### SLI / SLO / SLA + Error Budget
> [!tip] La cadena (muy preguntada)
> - **SLA (Agreement):** **contrato legal** con el cliente, con **penalidades**. Ej.: crédito del 10% si la disponibilidad baja del 99.5%/mes. *(Amazon: SLA 99.99%.)*
> - **SLO (Objective):** **objetivo interno** de confiabilidad. Ej.: 99.9%/mes.
> - **SLI (Indicator):** la **métrica** que mide el cumplimiento. Ej.: `requests_exitosos / total = 99.95%`.

**Error Budget:** cuánto puede fallar **sin romper el SLO**. SLO 99.9% mensual → 0.1% = **~43.8 min/mes**. Sirve para decidir con datos: *¿despliego hoy?*, *¿freno features?*. Si se agota, en empresas estrictas **no se despliega más** ese mes.

### Los 3 pilares de la observabilidad

| Pilar | Qué es | Responde | Cuándo |
|---|---|---|---|
| 📊 **Métricas** | Datos numéricos **agregados** en el tiempo (baratas, rápidas). | *¿Está bien mi sistema?* | SLOs, alertas, tendencias. |
| 📋 **Logs** | **Eventos discretos** con contexto (ricos pero **costosos**). | *¿Qué pasó exactamente?* | Debugging, auditoría, forense. |
| 🔍 **Trazas** | **Camino completo** de una request entre servicios. | *¿Dónde está el problema?* | Latencia, cuellos de botella en microservicios. |

> [!example] El ejemplo del checkout
> Un `POST /checkout` tarda **52 ms**. Solo con la métrica ves los 52 ms; con la **traza** ves que **33 ms los come un `INSERT` en la DB** → ahí está el cuello de botella. Esa es la diferencia entre **observabilidad** (porqué/dónde) y **monitoreo tradicional** (solo el número). El **`trace_id`** se propaga entre servicios para reconstruir el camino punta a punta.

### Tipos de métrica
⚠️ Los tipos NO son tipos de gráfica; importa la **semántica del dato**.

| Tipo | Qué es | Ejemplos |
|---|---|---|
| 📈 **Counter** | Solo **incrementa** (resetea al reiniciar). | requests totales, errores, bytes. `rate(counter[5m])`. |
| 🌡️ **Gauge** | Valor **instantáneo**, sube y baja. | CPU, memoria, conexiones activas, *queue depth*. |
| 📊 **Histogram** | **Distribución** en *buckets*. | latencia, tamaños. Permite **percentiles** p50/p95/p99. |

> [!warning] El promedio engaña
> Un promedio de latencia de 100 ms puede esconder que el **p99 es 2 segundos** (1 de cada 100 usuarios). El promedio oculta el problema; el **p99 lo revela**.

### Four Golden Signals (Google SRE)
1. **Latency** — cuánto tarda (SLO típico p99 < 500 ms; distinguir éxitos de fallos).
2. **Traffic** — demanda (RPS, transacciones/s); define el *baseline*.
3. **Errors** — fracción que falla (SLO típico < 0.1%):
   - **Explícitos:** los reconoce el protocolo (HTTP 5xx, excepciones, timeouts).
   - **Implícitos:** parecen exitosos pero no (HTTP 200 con JSON roto / que no respeta el contrato).
   - **Por política:** éxitos que **violan el SLO** (fuera del error budget).
4. **Saturation** — qué tan "lleno" está (CPU, memoria, pool, disco); al 70% bajo carga normal ya alerta (la latencia sube exponencialmente cerca de la saturación).

### OpenTelemetry (OTel)
**Framework open source y *vendor-neutral*** para instrumentar/recolectar/exportar telemetría (estándar **CNCF**, como Docker y Kubernetes).
- **Evita vendor lock-in:** separa la **instrumentación** de la **exportación** → instrumentás una vez y para cambiar de backend **solo cambiás el exporter** (no tocás el código de negocio). Con un SDK propietario (Datadog) habría que re-instrumentar todo.
- **OTel Collector:** app (SDK) → **Receiver** → **Processor** (filtrado, sampling, batching) → **Exporter** → backends. Podés exportar a **varios destinos a la vez** (ej. Prometheus gratis + Datadog pago).

### Elegir métricas + Labels
Siempre **desde los SLOs**: SLO → SLI → tipo (counter/gauge/histogram) → **labels** → ¿qué acción requiere? Buena métrica = **entendible** (¿la entiende un on-call a las 3am?) + **accionable** + **multidimensional**. Labels: `env`, `service`, `region`, `version`, `status`.
> [!warning] Alta cardinalidad
> **Nunca** uses como label valores de alta cardinalidad (`user_id`, `request_id`, `session_id`) → millones de series de tiempo. Para ese detalle están **traces y logs**.

### Alertas y *Alerting Fatigue*
> [!danger] Regla de oro del alerting (los 3 a la vez)
> 1. 🔴 **Urgente** — ¿atención en los próximos minutos?
> 2. ⚡ **Accionable** — ¿hay algo concreto que hacer? ¿hay **runbook**?
> 3. 👤 **Impacta al usuario** — ¿afecta el **SLO**?
>
> Si alguna es **NO → no es alerta, es ruido**.

- **Alerting Fatigue:** 200 alertas/día sin contexto → el equipo las ignora → silencia las molestas → un incidente crítico pasa desapercibido → burnout.
- **Tipos, de peor a mejor:** 🔴 valor estático (`CPU > 80%`, falsos positivos) → 🟡 por **anomalía** → 🟢 basada en **SLO / error budget burn rate** (*gold standard*). **Toda alerta debe tener runbook.**

### Logs — buenas prácticas
- **Levels:** `DEBUG` (dev) · `INFO` · `WARN` · `ERROR` · `FATAL`. En prod no abusar de DEBUG/INFO (ocupan almacenamiento pago).
- **Estructurados (JSON):** parseables y correlacionables; incluir `timestamp`, `level`, `service`, `version`, `env`, **`trace_id`**, `request_id`. El `trace_id` permite **saltar del log a la traza**.
- **Request correlation**, **políticas de retención**, **centralización (SIEM)**, **nunca loguear datos sensibles** (passwords, tokens, PII).
> [!note] Anti-patrón
> Loguear todo como `ERROR` "para no perderse nada" → *cuando todo es error, nada es error*.

> [!tip] Truco para la presentación del TP
> Tené un **script / runner de Postman** que ejecute la API muchas veces (y genere algún error) **antes de presentar**: el monitoreo en free tier **tarda** en mostrar datos. Llamar a una API externa o tener una DB local enriquece las **trazas**.

---

## 6️⃣ Terraform — Infraestructura como Código (IaC)

> [!abstract] Idea central
> **Terraform** (HashiCorp) define la infraestructura **como código** de forma **declarativa**: describís el **estado deseado** y Terraform llama a las **APIs** de los proveedores para crearlo. Multi-cloud, versionable, con **plan previo** antes de aplicar.

- **¿Qué es?** Herramienta de IaC para **definir, aprovisionar y gestionar** infra de forma **declarativa** (lenguaje **HCL**), multiplataforma y multi-proveedor. No es la única IaC (también **CDK de AWS**; se complementa con **Ansible**).
- **Por qué importa:** antes la infra se configuraba **a mano** (sin trazabilidad); con Terraform vive **como código en un repo** → versionable, auditable, revisable por **PR**. Enlaza con **GitOps** (Unidad 1).

### Ciclo Write / Plan / Apply (+ Destroy)
1. **Write** — definir los **recursos** (pueden ser multi-cloud).
2. **`terraform plan`** — genera un **plan de ejecución** (qué se crea/actualiza/destruye) comparando el estado actual con tu config → **previsualiza** antes de aplicar.
3. **`terraform apply`** — ejecuta respetando el **orden de dependencias**.
4. **`terraform destroy`** — elimina la infra creada (no es lo usual).

Flujo típico: `Write` → **`terraform init`** (inicializa el directorio y **descarga los providers**; genera `.terraform.lock.hcl`) → `plan` → `apply` → (eventual `destroy`).

### Arquitectura / componentes
- **Core** — el **motor** de Terraform (corre localmente).
- **Provider** — **plugin** que habla con la API de un cloud/SaaS; define qué recursos podés crear. Versionado propio; se buscan en el **Terraform Registry** (como las imágenes en Docker Hub).
- **State** — archivo **JSON** con el **estado real** de la infra; mapea recursos reales ↔ config para saber **qué cambiar**. Local o en **backend remoto** (colaborar).
- **Backend** — dónde se almacena el state.

### Bloques del lenguaje
- **Resources** — el elemento **fundamental** (servidores, redes, BD, DNS…).
- **Variables** — **parametrizan** la config (flexible y reutilizable).
- **Outputs** — **exportan** info de los recursos (ej. la IP pública).
- **Modules** — **encapsulan** recursos para **reutilizar** (versionables, compartibles). *Ej. del profe:* un módulo con VPC + usuario + variables comunes que todos reutilizaban → **estandarización** vía registry interno.

### State Management / Workspaces / Security
- **State management:** con el JSON Terraform sabe qué modificar y lo muestra en el `plan`.
- **Workspaces / Environments:** múltiples instancias de una misma config → **dev / testing / prod** separados, infra en paralelo, multi-cliente, pruebas de carga sin afectar prod.
- **Security & Compliance:** políticas **como código** con **Sentinel** (HashiCorp) para *enforcement* → automatización segura en entornos regulados.

### Beneficios
IaC (versionable/documentada) · **Multi-cloud** · **Declarativo** (estado deseado) · **Plan de ejecución** · **Gráfico de recursos** (dependencias automáticas) · **Modularidad** · puede **estimar el costo** del plan (Infracost).

> [!example] Demo en vivo del profe
> - **Render:** API Key + Owner ID (`TF_VAR_`); **falló** porque Render ya no permite el plan Free vía Terraform (pedía tarjeta).
> - **Docker (local):** `init` descargó el provider, declaró un contenedor **nginx**, `plan` mostró lo que iba a crear, `apply` lo levantó. **Detalle clave:** al **cambiar el nombre** del recurso, Docker **destruye y recrea** → no todo se actualiza "in-place"; algunos atributos **fuerzan recreación** (depende del provider).

---

## 7️⃣ Seguridad — OWASP Top 10 (2025) y DevSecOps

> [!abstract] Idea central
> La seguridad se integra en **todo el ciclo** (**DevSecOps**, *Shift-Left*: detectar vulnerabilidades **temprano**, no al final). Cubre conceptos de API/HTTP/vulnerabilidad y el **OWASP Top 10 (2025)** con ejemplos, casos reales y contramedidas.

### Conceptos base
- **API:** interfaz para que un software use otro; una **API REST** incrusta la invocación en una **solicitud HTTP**.
- **HTTP Request:** Método (GET/POST/PUT/DELETE) · URI · versión · **Headers** · **Body**.
- **Endpoint:** la **URI** que identifica un recurso (`Protocolo://dominio/versión/endpoint`).
- **Vulnerabilidad:** comportamiento **inesperado** que afecta **confidencialidad, integridad o disponibilidad**. El atacante busca lo no previsto: valores raros, **combinaciones** y **encadenamientos** inesperados, endpoints/parámetros **ocultos**.
- **Evolución 2019→2023:** giro de vulnerabilidades **técnicas simples** hacia **fallas de lógica de negocio** (BOLA/BFLA, abuso de flujos).

### OWASP Top 10 (2025)

| # | Riesgo | En una línea | Contramedida clave |
|---|---|---|---|
| **A01** | Broken Access Control | Usuarios fuera de sus permisos (**IDOR/BOLA**). Alice cambia `?id=80010→80011` y ve el recibo de Bob. *(First American, Uber)* | Denegar por defecto, **validar ownership** en backend, **UUIDs**, rate limits. |
| **A02** | Security Misconfiguration | Config insegura de fábrica: S3 públicos, **Debug en prod**, sin HSTS/CSP. *(Capital One, Twitch)* | **IaC ([[06 - Terraform (Infraestructura como Código)|Terraform]])** para auditar/versionar, menor privilegio, hardening (CIS). |
| **A03** | Software Supply Chain Failures | Dependencias/imágenes comprometidas; "paquete envenenado" roba tokens en CI/CD. *(SolarWinds, Kaseya)* | Verificar **firmas/hashes** (¡**evitar `latest`**!), **SCA/SBOM**, Zero Trust en CI/CD. |
| **A04** | Cryptographic Failures | Datos mal protegidos en reposo/tránsito: HTTP, MD5/SHA1 sin salt, llaves hardcodeadas. *(Equifax, Adobe)* | **TLS/HTTPS+HSTS**, AES-256, **gestión de secretos** (Vault), hashing lento (bcrypt/Argon2) con salt. |
| **A05** | Injection | Datos no sanitizados ejecutados como comandos (SQL/NoSQL/OS). Login `admin' --`. *(TalkTalk, Panama Papers)* | **Consultas parametrizadas**, ORMs seguros, **validación positiva** (allow-list). |
| **A06** | Insecure Design | Falla **desde la arquitectura** (no un bug): flujos lícitos pero **abusables**, sin threat modeling. *(Zoom, Tesla)* | **Threat Modeling**, fricción (rate limit, CAPTCHA), **Shift-Left**. |
| **A07** | Authentication Failures | Sin lockout/MFA, contraseñas débiles, **Credential Stuffing**, sesiones mal manejadas. *(Nintendo, 23andMe)* | **MFA obligatorio**, anti-fuerza-bruta, cotejar contra bases filtradas, gestión de sesiones. |
| **A08** | Software/Data Integrity Failures | Confiar ciegamente en componentes/updates; **deserialización insegura → RCE**, CDNs sin SRI. *(Asus, CCleaner)* | **JSON** + validación de esquema, **SRI**, **firmar artefactos** en CI/CD (PGP). |
| **A09** | Security Logging & Alerting Failures | Sin logging/alertas las brechas son **invisibles**. *(Marriott: 4 años; Equifax: 76 días)* | Registrar acciones críticas, **centralizar en SIEM**, metadatos (UTC/IP/user), alertas proactivas. → [[05 - Monitoreo y Observabilidad\|Monitoreo]] |
| **A10** | Mishandling of Exceptional Conditions *(nuevo 2025)* | Mal manejo de fallos: **stack traces** públicos, **Fail-Open**, payloads que tiran el sistema. *(Cloudbleed, T-Mobile)* | Errores genéricos (detalle interno), **Fail-Secure** (ante fallo, **negar**), límites de payload. |

> [!note] Fail-Open vs Fail-Secure
> - **Fail-Open:** si una validación falla (ej. timeout), **deja pasar** asumiendo que "todo está bien" → **inseguro**.
> - **Fail-Secure:** ante un fallo, **niega** en vez de saltar la validación → contramedida correcta.

### DevSecOps — CAMS aplicado a seguridad
> [!quote] CAMS + seguridad
> - **Cultura:** seguridad **responsabilidad de todos** (no *gatekeepers* finales).
> - **Automatización:** *scanning* en **cada etapa** (SAST, dependencias, contenedores, IaC, runtime).
> - **Medición:** MTTR de vulnerabilidades, cobertura de scanning, *policy compliance*.
> - **Compartir:** threat hunting, post-mortems *blameless*, training.
>
> Principios: **Shift Left** (detectar antes de prod), *Automated Compliance*, **Speed ≠ Insecurity** (la automatización deja ir rápido sin ser inseguro), seguridad como **habilitador**.

---

## 🔁 Integradoras — cómo se conectan las 4 unidades

> [!tip] Lo más probable a desarrollar
> El parcial premia **atar unidades**. Tené listos estos hilos:

- **El loop DevOps:** `Plan (SLOs) → Code (instrumentar OTel) → Build/Test → Release (canary, blue-green) → Deploy (mirar error budget) → Operate (runbooks) → Monitor (SLIs, alertas, post-mortems) → Feedback`. El **monitoreo cierra el loop** convirtiendo prod en datos que retroalimentan el Plan.
- **Docker como nexo:** la imagen **multi-stage + versionada** (Deployment) es la misma que mitiga **A03/A02** (Seguridad) y la que lleva la **librería de OTel/agente** que publica métricas y trazas (Monitoreo).
- **Terraform ↔ Seguridad:** **A02 Misconfiguration** se combate con **IaC** → config versionada y revisable por PR antes de prod.
- **Evitar `latest` ↔ Supply Chain:** fijar versión/hash (Deployment) es a la vez control de **A03** (que no te cambien la imagen por debajo).
- **A09 Logging ↔ Monitoreo:** el logging estructurado + SIEM + alertas accionables son lo que hace **visibles** los incidentes de seguridad.
- **Terraform ↔ GitOps:** infra como código con el **mismo flujo de PRs y ambientes** (dev/test/prod vía workspaces) que el resto del software; Terraform es la pieza de **"Operate"**.

---

🔙 [[00 - Índice DevOps|Índice de la materia]] · 🧩 [[Preguntas Integradoras - DevOps (2do Parcial)|Preguntas integradoras (active recall)]]
