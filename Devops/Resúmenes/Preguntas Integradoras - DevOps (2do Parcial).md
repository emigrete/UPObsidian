---
title: "Preguntas Integradoras — DevOps (2do Parcial)"
tags:
  - devops
  - preguntas
  - 2do-parcial
aliases:
  - "Preguntas DevOps 2do Parcial"
---

# 🧩 Preguntas Integradoras — DevOps (2do Parcial)

> [!tip] Cómo usar esta página
> Las respuestas están **plegadas**. Leé la pregunta, intentá responderla vos, y recién después abrí el callout (click en el ▸ a la izquierda de "Ver respuesta"). Pensada para repaso/active recall. Entra: Deployment, Seguridad, Terraform y Monitoreo.

## 🚀 Deployment — GitHub Actions, Docker Hub y Render

**1.** ¿Qué es GitHub Actions, dónde se definen los workflows y qué estructura básica tienen (`on`, `jobs`, `steps`, `runs-on`)? Explicá el rol de cada parte.
> [!success]- Ver respuesta
> **GitHub Actions** es un sistema de CI/CD **integrado a GitHub**. Los workflows se definen en archivos **YAML** dentro de `.github/workflows/`, y GitHub los ejecuta gratis (hasta cierto límite de minutos/mes; con la GitHub Student Pack se accede a beneficios pro).
> 
> Estructura de un workflow:
> 
> - **`on`** → los **eventos** que disparan el workflow (`push`, `pull_request`). Existe además `workflow_dispatch` para **ejecutarlo manualmente** desde la UI eligiendo la rama.
> - **`jobs`** → los **trabajos**; cada uno corre en una máquina/contenedor en la nube de GitHub, definida por **`runs-on`** (ej. `ubuntu-latest`).
> - **`steps`** → los **pasos** dentro de un job; pueden usar **actions reutilizables** (`uses:`, ej. `actions/checkout@v3`) o comandos directos (`run:`).
> 
> Ver [[04 - Deployment]].

**2.** ¿Para qué sirve la estrategia *matrix* en un workflow? Dá un ejemplo concreto del resumen.
> [!success]- Ver respuesta
> La estrategia *matrix* permite **iterar** un mismo job sobre varias combinaciones. Ejemplos:
> 
> - Buildear para **distintas versiones de Node** (probar compatibilidad hacia atrás de una librería/SDK).
> - Buildear para **distintas arquitecturas** (`amd64`/`x86_64` y **`arm64`** para las Mac nuevas).
> 
> En clase el profe contó que un grupo tuvo que buildear la imagen **multi-arquitectura** porque a un integrante con Mac ARM no le funcionaba la imagen.

**3.** Diferenciá los **runners** de GitHub de los runners *self-hosted*, y compará GitHub Actions con Jenkins. ¿Es lo mismo "dónde corre el job" que "dónde queda desplegada la app"?
> [!success]- Ver respuesta
> Los jobs corren en **runners** de GitHub (en la nube). Se pueden tener **runners self-hosted** (locales) para **ahorrar minutos**.
> 
> Esto es **distinto** de *dónde queda desplegada la app*: eso es Render / la nube, no el runner.
> 
> Comparación: **Jenkins** es *self-hosted* y da más control para entornos *on-premise*; **GitHub Actions** está **integrado** a GitHub y es más simple para este flujo.

**4.** Explicá qué es un Dockerfile **multi-stage**, cómo se copia entre etapas y qué ventajas aporta. ¿Cuál es el contenedor final?
> [!success]- Ver respuesta
> Un Dockerfile **multi-stage** construye la imagen en **varias etapas** para quedarse solo con lo mínimo de producción → **imagen final más chica y segura**.
> 
> - Cada **stage es una imagen separada**; el **último `FROM`** define lo que queda como **contenedor final**, y las imágenes intermedias se **descartan**.
> - Se copia entre stages con **`COPY --from=<stage>`** (ej. traer solo `dist/` y `node_modules`).
> - Conviene usar **imágenes oficiales** y lo más chicas posibles (**Alpine** = Linux muy liviano; Debian/Slim son más grandes), correr con **`USER node`** (no como root), y recordar que al transpilar TypeScript las interfaces desaparecen y a `dist/` va solo el código productivo.
> 
> Ver [[04 - Deployment]].

**5.** Según el profe, ¿conviene tener un Dockerfile distinto por ambiente? Justificá.
> [!success]- Ver respuesta
> **No.** El profe lo desaconsejó porque **rompe la ventaja de Docker** (mismo ambiente reproducible).
> 
> Lo que sí se suele hacer es un `Dockerfile.dev` o `docker-compose.dev` con *hot-reload* y datos de prueba (volumen + `.gitignore`), pero **la imagen productiva es la misma** que se promueve entre ambientes.

**6.** Describí los pasos para publicar la imagen en Docker Hub desde el workflow. ¿Por qué el push debe correr solo en `main` y no en cada PR?
> [!success]- Ver respuesta
> Pasos:
> 
> 1. Crear cuenta en **Docker Hub** → generar un **Access Token** (API Key).
> 2. En el repo de GitHub (`Settings → Secrets and variables → Actions`) crear los **secrets** (`DOCKER_USER`, `DOCKER_TOKEN`, y los de la app/BD).
> 3. En el workflow: hacer **login** en Docker Hub con los secrets, **extraer la metadata** (tags/labels), **buildear** y hacer **push** de la imagen.
> 
> El push debe correr **únicamente al mergear a `main`** (con una condición `if`). En los **PR solo corren build + tests** para verificar que el código esté sano; la imagen se publica recién cuando el PR está aprobado y mergeado. No tiene sentido publicar una imagen por cada commit.

**7.** ¿Por qué hay que **evitar `latest`** al versionar imágenes? ¿Qué alternativas de versionado menciona el resumen?
> [!success]- Ver respuesta
> Hay que evitar `latest` porque baja siempre "la última" y **no sabés qué versión estás desplegando** (perdés control).
> 
> Alternativas:
> 
> - **Versiones explícitas** (`v1.2.3`, SemVer).
> - Forma simple: usar el **SHA del commit** como tag.
> - Forma más profesional: la action **`docker/metadata-action`** genera SemVer automáticamente (toma la última versión y suma 1) y publica los tags `1.2.3`, `1.2`, `latest`.
> - Alternativa a Docker Hub: **GitHub Container Registry (GHCR)**.

**8.** Explicá cómo se hace el deploy a Render y por qué "linkear la cuenta de GitHub a Render NO cuenta como deploy".
> [!success]- Ver respuesta
> **Render** ofrece un free tier (512 MB RAM, CPU compartida). Pasos:
> 
> 1. Crear cuenta (mejor con email/password, no logueando con GitHub, para no "linkear" el repo).
> 2. Crear un **Web Service** tipo **Existing image** (usar la imagen publicada, **no** el código del repo).
> 3. Copiar el **Deploy Hook** (URL secreta) desde `Settings → Deploy`.
> 4. Guardarlo como secret en GitHub (`RENDER_DEPLOY_HOOK_URL`).
> 5. Agregar un **step de deploy** en el Action (solo en `main`) que hace un **`curl` al deploy hook**, pasándole por *query param* **qué imagen** desplegar (el SHA o la versión SemVer).
> 
> Linkear la cuenta de GitHub a Render **solo conecta el repo**; el deploy "válido" es que el **GitHub Action dispare el deploy hook** indicándole a Render qué imagen Docker usar. Si quedás en `latest`, tampoco podés controlar qué versión se despliega.

**9.** ¿Para qué sirve Kubernetes y cuándo NO conviene usarlo? Nombrá los conceptos que aparecieron.
> [!success]- Ver respuesta
> **Kubernetes** da **estabilidad y escalado** a la app: **balanceo de carga** entre múltiples instancias y **autoscaling** según la demanda, asegurando que siempre haya una instancia disponible.
> 
> **No conviene** para **una sola instancia** (sería sobre-ingeniería); ahí alcanza con un servicio simple. Además, **Docker Compose no se usa en producción**; en prod se usan orquestadores como los servicios de Amazon o Kubernetes.
> 
> Conceptos que aparecieron: **Namespace**, **Deployment**, **Service**, **Pods** y escalado. El laboratorio pensado era Terraform + imagen Docker + Kubernetes (minikube), con LocalStack (que archivaron) o MiniStack como alternativa.

## 🔒 Seguridad — OWASP Top 10 (2025) y DevSecOps

**1.** Definí **API**, **HTTP Request** (sus partes), **Endpoint** y **vulnerabilidad**. ¿Qué comportamientos busca un atacante?
> [!success]- Ver respuesta
> - **API (Application Programming Interface):** interfaz que permite a un software usar otro; una **API REST** incrusta la invocación (método + parámetros) en una **solicitud HTTP**.
> - **HTTP Request:** Método (GET, POST, PUT, DELETE) · URI · versión HTTP · **Headers** · **Body**.
> - **Endpoint:** la **URI** que identifica un recurso. Estructura típica: `Protocolo://dominio/versión/endpoint`.
> - **Vulnerabilidad:** comportamiento **inesperado** de un programa que afecta la **confidencialidad, integridad o disponibilidad**.
> 
> El atacante busca comportamientos que el programador no previó: valores inesperados para un parámetro, **combinaciones** inesperadas de parámetros, **encadenamiento** inesperado de invocaciones, y endpoints o parámetros **ocultos**.
> 
> Ver [[07 - Seguridad (OWASP Top 10)]].

**2.** ¿Qué es el OWASP Top 10 y qué giro hubo en su evolución entre 2019 y 2023? Listá el Top 10 de 2025.
> [!success]- Ver respuesta
> OWASP publica listas de los **riesgos más críticos**. Entre 2019 y 2023 hubo un giro de **vulnerabilidades técnicas simples** hacia **fallas de lógica de negocio** (BOLA/BFLA unificadas, abuso de flujos de negocio, consumo inseguro de APIs).
> 
> **OWASP Top 10 (2025):**
> 
> - A01 — Broken Access Control
> - A02 — Security Misconfiguration
> - A03 — Software Supply Chain Failures
> - A04 — Cryptographic Failures
> - A05 — Injection
> - A06 — Insecure Design
> - A07 — Authentication Failures
> - A08 — Software or Data Integrity Failures
> - A09 — Security Logging and Alerting Failures
> - A10 — Mishandling of Exceptional Conditions

**3.** Explicá **A01 Broken Access Control** con el ejemplo de Alice/Bob y sus contramedidas.
> [!success]- Ver respuesta
> Es cuando los usuarios actúan **fuera de sus permisos**: manipular tokens JWT/cookies/IDs en la URL (**IDOR/BOLA**), endpoints de admin que no verifican el rol, o confiar en datos del cliente en vez del estado del servidor.
> 
> **Ejemplo:** Alice cambia `/recibos?id=80010` → `80011` y ve el recibo de Bob; con un script itera y baja miles de recibos. Casos reales: First American Financial (2019) y Uber (2019, BOLA).
> 
> **Contramedidas:** **denegar por defecto**, **validar pertenencia (ownership)** en el backend, loggear fallos, deshabilitar *listing*, usar **UUIDs** en vez de IDs predecibles, y **rate limits**.

**4.** ¿Qué es **A02 Security Misconfiguration** y cómo enlaza con Terraform/IaC? Dá ejemplos y contramedidas.
> [!success]- Ver respuesta
> Son configuraciones **inseguras de fábrica**: buckets S3 públicos, puertos/credenciales por defecto expuestos, falta de cabeceras de seguridad (HSTS, CSP) y **modo Debug en producción**. Casos: Capital One (2019, WAF mal configurado en AWS) y Twitch (2021).
> 
> **Contramedidas:** usar **IaC** ([[06 - Terraform (Infraestructura como Código)|Terraform]]) para **auditar y versionar** la configuración, aplicar **menor privilegio**, hacer *hardening* (CIS Benchmarks) y parches automáticos.
> 
> El enlace con Terraform es directo: si la config vive como **código versionado y revisable**, las malas configuraciones se auditan y corrigen antes de llegar a producción.

**5.** Describí **A03 Software Supply Chain Failures**. ¿Qué tiene que ver con evitar `latest` en imágenes Docker?
> [!success]- Ver respuesta
> Las apps modernas ensamblan cientos de componentes open-source. Riesgos: dependencias comprometidas (`npm`, `PyPI`, `Maven`), CI/CD sin controles fuertes y no verificar firmas/hashes de imágenes Docker. Casos: SolarWinds (2020) y Kaseya (2021). Ejemplo: un "paquete envenenado" donde atacantes toman una librería abandonada y publican una versión que en el CI/CD roba tokens de AWS.
> 
> **Contramedidas:** verificar **firmas/hashes** (¡**evitar `latest`** en imágenes Docker!), **SCA** (analizar dependencias, generar **SBOM**), **Zero Trust en CI/CD** y repositorios internos auditados.
> 
> El nexo con el deployment es claro: el mismo motivo por el que en [[04 - Deployment]] se evita `latest` (saber exactamente qué versión se despliega) es una contramedida de cadena de suministro: fijar la versión/hash impide que te cambien la imagen por debajo.

**6.** Explicá **A04 Cryptographic Failures** y **A05 Injection**, con ejemplos y contramedidas.
> [!success]- Ver respuesta
> **A04 — Cryptographic Failures:** debilidades al proteger datos **en reposo** y **en tránsito**: HTTP en vez de HTTPS, hashing débil (MD5/SHA1 sin *salt*), llaves *hardcodeadas*, modos anticuados (ECB). Casos: Equifax (2017) y Adobe (2013). Contramedidas: **TLS/HTTPS + HSTS**, cifrado en reposo (AES-256), **gestión de secretos** (Vault, Secrets Manager — nunca hardcodear), hashing **lento** (bcrypt, Argon2) con **salt** único.
> 
> **A05 — Injection:** datos del usuario **no sanitizados** que se ejecutan como comandos (SQL, NoSQL, OS, LDAP, XML). Casos: TalkTalk (2015, SQLi) y Panama Papers (2016). Ejemplo: login con usuario `admin' --` → el `--` comenta el chequeo de contraseña → acceso de superusuario. Contramedidas: **consultas parametrizadas** (Prepared Statements), **ORMs seguros**, **validación positiva** (allow-listing) y escapar variables dinámicas.

**7.** ¿Qué diferencia a **A06 Insecure Design** de un bug de implementación? ¿Cómo se relaciona con el principio "Shift Left"?
> [!success]- Ver respuesta
> **A06 — Insecure Design** introduce vulnerabilidades **desde la arquitectura**, no es un bug de implementación: flujos lícitos pero **abusables**, falta de *threat modeling* y de *rate limits*, recuperación de contraseña demasiado "cómoda". Casos: Zoom (2020, "Zoombombing") y Tesla (2020). Ejemplo: bots que reservan todo el inventario en milisegundos.
> 
> **Contramedidas:** **Threat Modeling** en la arquitectura, fricción por seguridad (rate limiting, CAPTCHA), controles de lógica de negocio y **seguridad por diseño (Shift-Left)**.
> 
> Se relaciona con **Shift Left** porque la seguridad se piensa **desde el diseño** (antes de codear), detectando el problema temprano y no como un parche al final.

**8.** Explicá **A07 Authentication Failures** y **A09 Security Logging and Alerting Failures**. ¿Con qué unidad enlaza A09?
> [!success]- Ver respuesta
> **A07 — Authentication Failures:** debilidades al confirmar la identidad: sin *account lockout* (fuerza bruta, **Credential Stuffing**), sin **MFA**, contraseñas débiles, mal manejo de sesiones. Casos: Nintendo (2020) y 23andMe (2023). Contramedidas: **MFA obligatorio**, defensas anti-fuerza-bruta (bloqueos, rate limit), cotejar contraseñas contra bases filtradas y **gestión estricta de sesiones**.
> 
> **A09 — Security Logging and Alerting Failures:** sin logging disciplinado ni alertas, las brechas son **invisibles** (incidentes no registrados, logs locales dispersos sin SIEM, sin metadatos forenses, sin umbrales de alerta). Casos: Marriott/Starwood (2018, 4 años sin detección) y Equifax (2017, 76 días). Contramedidas: **registrar acciones críticas**, **centralizar en SIEM**, metadatos de calidad (UTC, IP, usuario) y **alertas proactivas**.
> 
> A09 **enlaza directamente con [[05 - Monitoreo y Observabilidad|logs y alertas]]**: las mismas buenas prácticas de logging estructurado y alerting accionable son la base de la detección de incidentes de seguridad.

**9.** ¿Qué es **A08 Software or Data Integrity Failures** y **A10 Mishandling of Exceptional Conditions** (nuevo en 2025)? Explicá *Fail-Open* vs *Fail-Secure*.
> [!success]- Ver respuesta
> **A08 — Software or Data Integrity Failures:** confiar ciegamente en componentes/actualizaciones. Incluye **deserialización insegura** (reconstruir objetos del usuario → **RCE**), CDNs sin **SRI** y parches sin verificar firma. Casos: Asus (2019, ShadowHammer) y CCleaner (2017). Contramedidas: usar **JSON** + validación de esquema en vez de objetos nativos, **SRI** (hash de scripts externos) y **firmar digitalmente** los artefactos en CI/CD (PGP), validándolos en producción.
> 
> **A10 — Mishandling of Exceptional Conditions** (nuevo en 2025): mal manejo de fallos: mostrar **stack traces** completos al público, **Fail-Open** y tolerancia débil a payloads desbordantes (crash/DoS). Casos: Cloudflare "Cloudbleed" (2017) y T-Mobile (2021).
> 
> - **Fail-Open:** si una validación falla (ej. por timeout), **deja pasar** asumiendo que "todo está bien" → inseguro.
> - **Fail-Secure:** ante un fallo, **niega** en vez de saltar la validación → es la contramedida correcta, junto a mensajes de error genéricos (registrar el detalle internamente, devolver "error con ID X") y límites de tamaño de payload.

**10.** ¿Qué es **DevSecOps** y cómo se aplica el modelo **CAMS** a la seguridad? Explicá "Shift Left" y "Speed ≠ Insecurity".
> [!success]- Ver respuesta
> **DevSecOps** integra la seguridad en **todo el ciclo** (no como etapa final). CAMS aplicado a seguridad:
> 
> - **Cultura:** la seguridad es **responsabilidad de todos** (dev, sec, ops), no de *gatekeepers* finales.
> - **Automatización:** *scanning* automático en **cada etapa** (SAST, dependencias, contenedores, IaC, runtime).
> - **Medición:** MTTR de vulnerabilidades, cobertura de scanning, *policy compliance*.
> - **Compartir:** *threat hunting*, post-mortems *blameless*, *training* continuo.
> 
> Principios: **Shift Left** (detectar **antes** de producción), *Automated Compliance* (políticas sin fricción), **Speed ≠ Insecurity** (la automatización permite ir rápido **sin** ser inseguro), y seguridad como **habilitador**, no como bloqueante.

## 🏗️ Terraform — Infraestructura como Código (IaC)

**1.** ¿Qué es Terraform, qué significa que sea **declarativo** y para qué sirve la IaC? ¿Es la única herramienta de IaC?
> [!success]- Ver respuesta
> **Terraform** (de HashiCorp) es una herramienta de **IaC** que permite **definir, aprovisionar y gestionar** infraestructuras complejas de forma **declarativa**, usando su propio lenguaje (HCL). Es **multiplataforma** y soporta múltiples proveedores de nube.
> 
> **Declarativo** significa que describís el **estado deseado** (no los pasos): Terraform llama a las **APIs** de los proveedores para crearlo.
> 
> **No es la única IaC:** también está el **CDK de AWS**, y se puede complementar con **Ansible**.
> 
> Ver [[06 - Terraform (Infraestructura como Código)]].

**2.** ¿Por qué importa la IaC para DevOps frente a la configuración manual? ¿Con qué concepto de la Unidad 1 enlaza?
> [!success]- Ver respuesta
> Antes, configurar infraestructura era **manual** (entrar a la consola de AWS/Azure y crear cada cosa) → **sin trazabilidad ni historial**.
> 
> Con Terraform, **toda la infra vive como código en un repositorio** → versionable, auditable, reutilizable y revisable por **PR**.
> 
> Esto enlaza con **GitOps** (Unidad 1): el mismo flujo de PRs y ambientes que el resto del software se aplica también a la infraestructura.

**3.** Explicá el ciclo **Write / Plan / Apply** (y Destroy). ¿Para qué sirve `terraform plan`?
> [!success]- Ver respuesta
> 1. **Write** — definir los **recursos** (pueden ser multi-cloud). Ej.: una app sobre un servidor con VPC + security groups + load balancer.
> 2. **Plan** (`terraform plan`) — genera un **plan de ejecución** que describe qué se va a **crear, actualizar o destruir**, comparando el estado actual con tu configuración. Sirve para **previsualizar** los cambios antes de aplicarlos.
> 3. **Apply** (`terraform apply`) — ejecuta los cambios, respetando el **orden de dependencias**.
> 4. **Destroy** (`terraform destroy`) — elimina toda la infraestructura creada (no es lo usual, pero existe).

**4.** Describí los componentes de la arquitectura de Terraform: **Core**, **Provider**, **State** y **Backend**.
> [!success]- Ver respuesta
> - **Core** — el **motor** de Terraform (la aplicación en sí; corre localmente).
> - **Provider** — **plugin** que interactúa con la API de un cloud/SaaS. Define **qué recursos** podés configurar y cómo; tiene su propio versionado y lo crean HashiCorp, empresas o terceros. Se buscan en el **Terraform Registry** (público), igual que las imágenes en Docker Hub.
> - **State** — archivo **JSON** que guarda el **estado real** de la infraestructura. Sirve para **mapear recursos reales ↔ configuración** e identificar qué cambios hacer para llegar al estado deseado. Puede guardarse **localmente** o en un **backend remoto** (para colaborar).
> - **Backend** — dónde se almacenan los archivos de estado.

**5.** Explicá los cuatro **bloques del lenguaje**: Resources, Variables, Outputs y Modules.
> [!success]- Ver respuesta
> - **Resources** — el elemento **fundamental**: describe uno o más objetos de infraestructura (redes virtuales, servidores, bases de datos, DNS…).
> - **Variables** — **parametrizan** la configuración para hacerla más flexible y reutilizable.
> - **Outputs** — **exportan** información de los recursos creados (ej. la IP pública de una instancia).
> - **Modules** — agrupan y **encapsulan** recursos/configuraciones para **reutilizar**; se pueden versionar y compartir (Terraform Registry tiene módulos comunitarios y oficiales). Ejemplo del profe: un módulo con la VPC, el usuario del equipo y variables comunes que cualquier recurso nuevo reutilizaba → **estandarización** vía registry interno.

**6.** ¿Qué es el **State Management**? ¿Para qué sirven los **Workspaces/Environments**?
> [!success]- Ver respuesta
> **State Management:** gracias al archivo de estado (JSON), Terraform sabe **qué modificar** para alcanzar el estado deseado y lo muestra en el `plan`.
> 
> **Workspaces / Environments:** permiten gestionar **múltiples instancias** de una misma configuración → entornos separados (**dev / testing / prod**), versiones de infra en paralelo, multi-cliente y pruebas de carga sin afectar producción. Evitan conflictos y **aíslan recursos**.

**7.** ¿Cómo se definen seguridad y compliance en Terraform? Mencioná la herramienta de HashiCorp.
> [!success]- Ver respuesta
> Se pueden definir **políticas de seguridad/compliance como código**, integrándose con **Sentinel** (de HashiCorp) para **evaluar** y hacer *enforcement* de políticas.
> 
> Esto habilita una **automatización segura** en entornos regulados: las reglas se aplican automáticamente como parte del flujo, no manualmente.

**8.** Describí el **flujo de trabajo típico** completo. ¿Qué hace `terraform init`?
> [!success]- Ver respuesta
> El flujo típico es:
> 
> `Write` → `terraform init` → `terraform plan` → `terraform apply` → (eventual `terraform destroy`).
> 
> **`terraform init`** inicializa el directorio de trabajo y **descarga los providers** (genera además el `.terraform.lock.hcl`).

**9.** Enumerá los **beneficios** de Terraform.
> [!success]- Ver respuesta
> - **IaC** → versionable, reutilizable, documentada.
> - **Multi-Cloud** → misma configuración en varias nubes (un "espejo" por nube).
> - **Declarativo** → definís el **estado deseado**, no los pasos.
> - **Plan de ejecución** → previsualización de cambios.
> - **Gráfico de recursos** → gestión automática de **dependencias**.
> - **Modularidad** → código reutilizable mediante módulos.
> - Hasta puede **estimar el costo** del plan antes de aplicarlo (ej. con Infracost).

**10.** En la demo en vivo, ¿qué pasó con el provider de Render y qué detalle importante mostró el provider de Docker al cambiar el nombre de un recurso?
> [!success]- Ver respuesta
> - **Provider de Render:** API Key + Owner ID (tomados de variables de entorno con prefijo `TF_VAR_`); intentó crear un Web Service pero **falló** porque Render ya **no permite el plan Free vía Terraform** (pedía tarjeta).
> - **Provider de Docker (local):** `terraform init` descargó el provider; en `main.tf` declaró un contenedor **nginx**, el `plan` mostró lo que iba a crear y `apply` lo levantó en el puerto 8000. **Detalle importante:** al **cambiar el nombre** del recurso, el `plan` mostró que Docker **destruye y recrea** la instancia → no todos los atributos se actualizan "in-place"; algunos **fuerzan recreación**, y eso **depende del provider**.

## 📊 Monitoreo y Observabilidad

**1.** ¿Por qué "monitorear sin objetivos es solo ruido"? Diferenciá **SLA**, **SLO** y **SLI** con un ejemplo de cada uno.
> [!success]- Ver respuesta
> Porque primero hay que definir **qué garantizamos** y recién después **qué medimos**; sin objetivos, los datos son ruido.
> 
> - **SLA (Service Level Agreement):** **contrato legal** con el cliente, con **penalidades** si se incumple. Ej.: crédito del 10% si la disponibilidad baja del 99.5% en el mes.
> - **SLO (Service Level Objective):** **objetivo interno** de confiabilidad, lo que nos comprometemos a cumplir. Ej.: 99.9% de disponibilidad/mes.
> - **SLI (Service Level Indicator):** la **métrica** que mide el cumplimiento. Ej.: `requests_exitosos / total_requests = 99.95%`.
> 
> Ver [[05 - Monitoreo y Observabilidad]].

**2.** ¿Qué es el **Error Budget** y para qué se usa? Calculá el del SLO 99.9% mensual.
> [!success]- Ver respuesta
> El **Error Budget** es la cantidad de tiempo/requests que **pueden fallar sin romper el SLO**.
> 
> Con SLO 99.9% mensual → 0.1% = **~43.8 minutos/mes**.
> 
> Sirve para decidir con **datos** (no subjetivamente): *¿puedo desplegar hoy?*, *¿es seguro este experimento?*, *¿hay que frenar features y pagar deuda técnica?* En empresas con políticas estrictas, si se **agota** el error budget **no se deja desplegar más** ese mes.

**3.** Explicá los **3 pilares de la observabilidad** (qué es cada uno, qué pregunta responde y cuándo usarlo).
> [!success]- Ver respuesta
> - 📊 **Métricas:** datos numéricos **agregados** en el tiempo, baratas de almacenar y rápidas de consultar. Responden *¿Está bien mi sistema?* Para SLOs, alertas, tendencias y capacity planning.
> - 📋 **Logs:** registros de **eventos discretos** con contexto, ricos pero **costosos**. Responden *¿Qué pasó exactamente?* Para debugging puntual, auditoría/compliance y análisis forense.
> - 🔍 **Trazas:** seguimiento del **camino completo** de una request entre servicios. Responden *¿Dónde está el problema?* Para latencia, dependencias y cuellos de botella en microservicios.
> 
> Regla: no uses `grep` en logs para calcular tasas de error en tiempo real (eso es un **Counter**); y para saber **por qué** falló una request usá logs/traces, no métricas.

**4.** Con el ejemplo del `POST /checkout` (52 ms), explicá la diferencia entre observabilidad y monitoreo tradicional. ¿Qué es el `trace_id`?
> [!success]- Ver respuesta
> Un `POST /checkout` tarda **52 ms** en total. Solo con la **métrica** ves los 52 ms; con la **traza** ves que **33 ms los consume un `INSERT` en la DB** → ahí está el cuello de botella. Esa es la diferencia entre **observabilidad** (entender el porqué/dónde) y el **monitoreo tradicional** (solo el número agregado).
> 
> En microservicios (ej. tipo Uber: API Gateway → Auth → Orders → DB), la traza dice **en qué servicio** falló o se demoró, usando un **`trace_id`** que se **propaga entre servicios** para reconstruir el camino punta a punta.

**5.** Describí los tres **tipos de métrica** (Counter, Gauge, Histogram) con ejemplos. ¿Por qué "el promedio engaña"?
> [!success]- Ver respuesta
> - 📈 **Counter:** solo **incrementa** (se resetea al reiniciar). Ej.: requests totales, errores totales, bytes. Consulta típica: `rate(counter[5m])`.
> - 🌡️ **Gauge:** valor **instantáneo**, sube y baja (un termómetro). Ej.: CPU actual, memoria usada, conexiones activas, *queue depth*.
> - 📊 **Histogram:** **distribución** en *buckets*. Ej.: latencia, tamaño de respuestas, duración de jobs. Permite **percentiles** (`histogram_quantile`, p50/p95/p99).
> 
> Aclaración: los tipos de métrica **NO** son tipos de gráfica; importa la **semántica del dato**.
> 
> El **promedio engaña** porque un promedio de latencia de 100 ms puede **esconder** que el **p99 es 2 segundos** (1 de cada 100 usuarios espera 2 s). El promedio oculta el problema; el **p99 lo revela**.

**6.** Explicá los **Four Golden Signals** (Google SRE). En **Errors**, distinguí explícitos, implícitos y por política.
> [!success]- Ver respuesta
> Si solo pudieras medir 4 cosas:
> 
> 1. **Latency** — cuánto tarda en responder (SLO típico p99 < 500 ms; distinguir latencia de requests exitosas vs fallidas).
> 2. **Traffic** — demanda sobre el sistema (RPS, mensajes/s, transacciones/s); define el *baseline* normal.
> 3. **Errors** — fracción de requests que falla (SLO típico < 0.1%):
>    - **Explícitos:** reconocidos por el protocolo (HTTP 5xx, excepciones no controladas, timeouts).
>    - **Implícitos:** parecen exitosos pero no (HTTP 200 con JSON malformado o que no respeta el contrato, respuesta vacía).
>    - **Por política:** éxitos que **violan el SLO** (responde bien pero fuera del error budget).
> 4. **Saturation** — qué tan "lleno" está (CPU, memoria, *connection pool*, *queue depth*, disco); al 70% bajo carga normal ya es alerta, la latencia sube exponencialmente cerca de la saturación.

**7.** ¿Qué es **OpenTelemetry** y por qué evita el *vendor lock-in*? Describí la arquitectura del **OTel Collector**.
> [!success]- Ver respuesta
> **OpenTelemetry (OTel)** es un **framework open source y *vendor-neutral*** para instrumentar, generar, recolectar y exportar telemetría. Es estándar de la industria (parte de la **CNCF**, como Docker y Kubernetes).
> 
> Evita el **vendor lock-in** porque **separa la instrumentación de la exportación**: instrumentás una sola vez y, para cambiar de backend, **solo cambiás el exporter** (en la capa de infra/variables de entorno), **sin tocar el código de negocio**. Con un SDK propietario (ej. Datadog) tendrías que re-instrumentar toda la app.
> 
> **OTel Collector:** app (SDK + auto-instrumentación) → **Receiver** (OTLP, Prometheus, Jaeger…) → **Processor** (filtrado, sampling, enriquecimiento, batching) → **Exporter** → backends. Podés exportar **al mismo tiempo** a varios destinos (ej. Prometheus gratis + Datadog pago) cambiando solo la config.

**8.** ¿Cómo se eligen las métricas correctas (los pasos desde el SLO) y qué hace a una "buena métrica"? Explicá el problema de la **alta cardinalidad** en labels.
> [!success]- Ver respuesta
> Siempre **desde los SLOs**: 1) definí el SLO → 2) identificá el SLI → 3) elegí el tipo (counter/gauge/histogram) → 4) definí **etiquetas (labels)** → 5) ¿qué acción requiere?
> 
> Una **buena métrica** es: **entendible** (¿la entiende un on-call a las 3am?), **accionable** (sabés qué hacer cuando cambia), **mejorable** y **multidimensional** (con labels). Labels recomendados: `env`, `service`, `region`, `version`, `status`.
> 
> **Alta cardinalidad:** **nunca** uses como label valores de alta cardinalidad (`user_id`, `request_id`, `session_id`) → crearías **millones de series de tiempo**. Para ese nivel de detalle están **traces y logs**.

**9.** Enunciá la "regla de oro del alerting" (los 3 criterios) y explicá la **Alerting Fatigue**. Ordená los tipos de alerta de peor a mejor.
> [!success]- Ver respuesta
> Una alerta válida cumple **los tres** criterios a la vez:
> 
> 1. 🔴 **Urgente** — ¿requiere atención en los próximos minutos?
> 2. ⚡ **Accionable** — ¿hay algo concreto que hacer? ¿existe un **runbook**?
> 3. 👤 **Impacta al usuario** — ¿afecta o afectará el **SLO** comprometido?
> 
> Si la respuesta a cualquiera es **NO → no es una alerta, es ruido**.
> 
> **Alerting Fatigue:** 200 alertas/día sin contexto → el equipo **aprende a ignorarlas** → se silencian las "molestas" → un **incidente crítico pasa desapercibido** → pérdida de confianza y **burnout** del on-call.
> 
> Tipos de alerta, de **peor a mejor**: 🔴 valor estático (`CPU > 80%`, muchos falsos positivos) → 🟡 por **anomalía** (desviaciones estándar) → 🟢 basada en **SLO / error budget burn rate** (*gold standard* de Google SRE). **Toda alerta debe tener runbook.**

**10.** Explicá los **log levels** y las **buenas prácticas de logs** (estructurados, correlación, retención). ¿Cuál es el anti-patrón mencionado?
> [!success]- Ver respuesta
> **Log levels:** `DEBUG` (solo desarrollo) · `INFO` (eventos normales) · `WARN` (situación inesperada pero sigue) · `ERROR` (fallo que requiere revisión) · `FATAL` (irrecuperable, aborta). En producción no abusar de DEBUG/INFO (ocupan almacenamiento que se paga).
> 
> **Buenas prácticas:**
> 
> - **Logs estructurados (JSON):** parseables, buscables y correlacionables; incluir `timestamp`, `level`, `service`, `version`, `env`, `region`, **`trace_id`**, `request_id`, `user_id`. El `trace_id` permite **saltar del log (Loki) a la traza (Jaeger/Tempo)**.
> - **Request correlation:** propagar un `request_id` como "pasamanos" entre servicios.
> - **Políticas de retención**, **centralización** (SIEM/agregación), **proteger el acceso** y **nunca loguear datos sensibles** (passwords, tokens, PII).
> 
> **Anti-patrón:** loguear todo como `ERROR` "para no perderse nada" → misma pérdida de señal que el alerting fatigue: *cuando todo es error, nada es error*.

## 🔁 Integradoras — el loop DevOps

**1.** Describí el **loop completo** Plan → Code → Build/Test → Release → Deploy → Operate → Monitor y cómo el monitoreo "cierra el feedback loop".
> [!success]- Ver respuesta
> El ciclo: **Plan** (SLOs, capacity) → **Code** (instrumentar con OTel) → **Build/Test** (confiabilidad, load testing, chaos eng.) → **Release** (feature flags, canary, blue-green) → **Deploy** (observar error budget) → **Operate** (on-call, runbooks) → **Monitor** (SLIs, alertas, post-mortems) → **Feedback loop** *blameless* y *data-driven* → vuelve a **Plan**.
> 
> El **monitoreo cierra el loop** porque convierte lo que pasa en producción en **datos** (SLIs, alertas, post-mortems) que retroalimentan la siguiente planificación: deploys con confianza guiados por el **error budget**, MTTR reducido y cultura *blameless*. Es lo que conecta esta unidad con el feedback loop planteado en la Unidad 1.
> 
> Ver [[05 - Monitoreo y Observabilidad]] y [[04 - Deployment]].

**2.** ¿Cómo se integra la **seguridad** al pipeline de CI/CD (DevSecOps)? Conectá A02, A03 y A09 con las unidades de Deployment, Terraform y Monitoreo.
> [!success]- Ver respuesta
> En **DevSecOps** la seguridad es **transversal** y se automatiza en cada etapa del pipeline (**Shift Left**: detectar antes de producción). Conexiones concretas:
> 
> - **A02 Security Misconfiguration ↔ Terraform/IaC:** usar [[06 - Terraform (Infraestructura como Código)|Terraform]] para **auditar y versionar** la configuración evita configuraciones inseguras de fábrica.
> - **A03 Supply Chain ↔ Deployment:** verificar firmas/hashes y **evitar `latest`** en las imágenes (lo mismo que se hace al versionar en [[04 - Deployment]]) protege la cadena de suministro; sumar **SCA/SBOM** y Zero Trust en CI/CD.
> - **A09 Logging & Alerting ↔ Monitoreo:** el logging estructurado, la centralización en SIEM y las alertas accionables de [[05 - Monitoreo y Observabilidad]] son lo que hace **visibles** los incidentes de seguridad.

**3.** ¿Cómo habilita la **IaC (Terraform)** un deploy reproducible y auditable, y cómo se enlaza con el flujo de PRs del resto del software?
> [!success]- Ver respuesta
> Con Terraform, la infraestructura deja de configurarse a mano y pasa a ser **código versionado, planificable y reutilizable**:
> 
> - **Reproducible:** al ser **declarativo** (estado deseado) y multi-cloud, la misma configuración levanta la misma infra; `terraform plan` permite **previsualizar** los cambios antes de aplicarlos.
> - **Auditable:** toda la infra vive en un **repositorio** → versionable, revisable por **PR**, con historial (a diferencia de crear cosas a mano en la consola sin trazabilidad).
> - **Mismo flujo que el software:** se aplica el mismo **flujo de PRs y ambientes** (dev/testing/prod vía workspaces) que al resto del código, enlazando con **GitOps**. Terraform es la pieza de **"Operate"** del ciclo CI/CD.

**4.** Atá la pieza Docker (multi-stage + versionado) entre el **Deployment**, la **Seguridad** y el **Monitoreo**.
> [!success]- Ver respuesta
> La imagen Docker es un nexo entre las tres unidades:
> 
> - **Deployment:** el Dockerfile **multi-stage** produce una imagen final chica y segura (imágenes Alpine, `USER node`), que se **versiona** (SemVer/SHA, evitando `latest`) y se promueve sin cambios entre ambientes (mismo entorno reproducible).
> - **Seguridad:** evitar `latest` y verificar firmas/hashes mitiga **A03 (Supply Chain)**; correr como no-root y usar imágenes oficiales reduce superficie de ataque, en línea con **A02**.
> - **Monitoreo:** dentro de esa misma imagen se agrega la **librería de OTel o el agente** (ej. `datadog-agent`) que publica métricas, APM y trazas al backend, cerrando el loop de observabilidad sobre la app desplegada.

**5.** En la presentación del TP, ¿qué truco recomienda el profe para el monitoreo y por qué llamar a una API externa/DB mejora las **trazas**? Conectalo con los pilares de observabilidad.
> [!success]- Ver respuesta
> **Truco para la presentación:** tener un **script** (o un *runner* de Postman) que ejecute la API muchas veces (y genere algún error) **antes de presentar**, porque el monitoreo en el free tier **tarda** en mostrar datos; así llegás con un histórico de métricas y trazas.
> 
> Llamar a una **API gratuita externa** (Star Wars, Pokémon, Chuck Norris…) o tener una **base de datos** local mejora las **trazas** porque en el dashboard podés **ver el camino encadenado** y los tiempos de cada llamada (con su `trace_id`), igual que en el ejemplo del `POST /checkout` donde la traza revela en qué paso está el cuello de botella. Así se aprecian los **3 pilares** juntos: métricas (agregados), logs (qué pasó) y trazas (dónde/por qué).

---
🔙 Volver al índice: [[00 - Índice DevOps]]
