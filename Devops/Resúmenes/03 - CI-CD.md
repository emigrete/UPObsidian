---
tags: [devops, cicd, integracion-continua, despliegue-continuo, pipelines]
unidad: 3
clase: 4
---

# 3️⃣ CI/CD — Integración y Entrega/Despliegue Continuo

> [!abstract] Idea central
> **CI/CD** automatiza el camino del código desde el repositorio hasta producción. **CI** (Continuous Integration) integra y valida cambios frecuentemente; **CD** (Delivery/Deployment) los lleva a producción de forma controlada o automática. Es la **herramienta técnica que hace posible la cultura CAMS** de la [[01 - Introducción a DevOps|Unidad 1]].

[[02 - Herramientas - Git, GitHub y Docker|← Unidad 2]] · [[00 - Índice DevOps|Índice]] · Siguiente: [[04 - Deployment]]

---

## El ciclo CI/CD
`Plan → Code → Build → Test → Release → Deploy → Operate → Monitor → (vuelta a Plan)`. Es un **flujo continuo**: una vez monitoreado, se vuelve a planificar lo siguiente.

### ¿Por qué es fundamental?
- **Automatización** — elimina tareas manuales (lentas y propensas a error).
- **Velocidad** — desplegar en **minutos** en lugar de semanas → ventaja competitiva.
- **Calidad** — pruebas automáticas detectan errores **temprano** (más fácil y barato corregir).
- **Cultura** — responsabilidad compartida: **"you build it, you run it"**.

---

## CI — Continuous Integration
**Definición:** automatizar la integración de cambios de varios contribuyentes en un proyecto único, de forma **frecuente** (mínimo una vez al día).

**Objetivos:**
1. Detectar errores lo antes posible (**Fail Fast**).
2. Reducir conflictos de integración (evitar el **"Integration Hell"**).
3. Mantener una base de código **siempre estable**.

> [!info] Origen
> Práctica **popularizada por Martin Fowler** y tomada por la metodología **Extreme Programming**. La idea es simple: si cada uno trabaja en ramas aisladas por días/semanas, integrar todo al final es un caos; mejor integrar seguido y que el sistema **avise enseguida** si una rama rompe algo.

> [!example] Anécdotas reales de clase (¡muy ilustrativas!)
> - **"Read-only Fridays" / Code freeze:** no desplegar los viernes (ni a última hora). Si rompés un viernes a la tarde, le cae al que está **on-call** el fin de semana. Un compañero contó que pasaron los Sprints a **mitad de semana** justamente por esto.
> - **GitLab + Jira:** otro compañero integró las ramas con los tickets de Jira (el nombre de la rama = N° de historia) → mejoró la detección de errores y redujo conflictos.
> - El "todos desplegando al final del Sprint" genera **cuellos de botella**, peleas por el release y rollbacks dolorosos. La idea es un **release progresivo**, no sacar todo junto.

### Etapas de CI
1. **Code** — control de versiones (**Git** estándar), *branching strategies* ([[02 - Herramientas - Git, GitHub y Docker|GitHub Flow / GitFlow / Trunk-based]]), **Code Review** vía Pull Requests. *El código debe estar siempre en buen estado para poder desplegarse; los reviews además **comparten conocimiento**.* Herramientas: GitHub, GitLab, Bitbucket.
2. **Build** — transformar el código en algo ejecutable: gestión de dependencias (`npm`, `maven`, `pip`, `nuget`), compilación, y **contenedorización** (imagen Docker). El artefacto final hoy es una **imagen de contenedor** (no solo un `.jar`), que incluye **todas las dependencias y el SO** → ambiente **reproducible** ("en mi máquina funciona" deja de ser excusa). Herramientas de orquestación: **GitHub Actions**, CircleCI, Travis CI, **Jenkins**. De *build*: Webpack, Gradle, Bazel.
3. **Test** — la "red de seguridad":
   - **Unit tests** — lógica interna de funciones (Jest, JUnit, PyTest, Testify).
   - **Integration tests** — cómo se comunican los módulos entre sí.
   - **Static analysis / Linting** — calidad y estilo (espacios, comillas, tabulaciones).
   - **Security (SAST)** — análisis estático de vulnerabilidades (SonarQube, Snyk).
   - **Code coverage** — qué líneas ejecutaron los tests. *Ojo:* no es preciso (un test puede pasar por el código sin probar nada), pero sirve de guía.

> [!example] Herramientas que aportaron los alumnos
> - **Checkov / KICS** — escanean **IaC** buscando malas configuraciones (ej. *security group* abierto a `0.0.0.0/0`); pueden **frenar el pipeline** (fail grave) o avisar (warning).
> - **Infracost** — estima el **costo** de lo que vas a desplegar en la nube (pega a las APIs de los cloud providers) antes de aplicarlo.

---

## CD — Continuous Delivery vs Continuous Deployment
> [!tip] La diferencia clave
> - **Continuous Delivery (Entrega continua):** el código está **siempre listo** para desplegar, pero el paso final a producción se decide **manualmente** (un botón "Aprobar"). Ideal en entornos **muy regulados** o cuando el negocio decide *cuándo* lanzar (marketing, tiempos comerciales).
> - **Continuous Deployment (Despliegue continuo):** **todo automático**; si pasa las pruebas, llega a producción solo. Requiere **madurez técnica** y **confianza total** en la suite de pruebas.
> Casi todos los equipos empiezan por *Delivery* y aspiran a *Deployment*.

### Release & Deployment
- **Release:** versionado (**SemVer**), notas de lanzamiento, almacenamiento en **Artifact Registry**. *Un release no es solo código: incluye el paquete, el versionado y la documentación.*
- **Deployment:** el acto de poner el código en el servidor/cloud.

### Estrategias de despliegue
- **Blue/Green** — versión vieja (azul) y nueva (verde) corriendo a la vez; se migra el tráfico gradualmente → **cero downtime** y rollback fácil.
- **Canary** — desplegar a un **% chico** de usuarios (5% → 10% → 20%…), monitoreando errores antes de ir al 100%.
  > 🐤 *El profe contó el origen:* en las **minas** llevaban un **canario en una jaula**; mientras el canario estuviera vivo, el aire era seguro. Igual: probás con pocos usuarios antes de "entrar del todo". Recomendación: **no más de 3–4 etapas**, dejando tiempo para monitorear uso real entre cada una.
- **Rolling deployment** — actualizar **parte** de los servidores/instancias por vez; la app sigue disponible.
- **Despliegue progresivo** — implementación gradual con control fino; se puede segmentar por usuarios/empresas (clientes "modelo").
- **Feature Flags** — *banderas/toggles* que **habilitan funcionalidades sin modificar el código**. El código se "aprueba" solo cuando el flag está activo. Suelen apoyarse en una **caché** para no impactar la latencia consultando el estado del flag.
- **Rollback (automático)** — volver a la versión anterior si se detectan errores; se puede automatizar (si suben los errores durante el deploy → revertir y marcar el deploy como fallido).

### ¿Todas las empresas tienen CD automatizado? **No.** Razones (del profe):
1. **Cultura/organización** — difícil cambiar cómo se viene trabajando.
2. **Falta de conocimiento** en las herramientas.
3. **Seguridad / compliance** — industrias reguladas requieren revisión y aprobación manual.
4. **Complejidad / Legacy** — entornos heredados difíciles o caros de automatizar (ej. un PHP con un framework muy viejo).

### Herramientas de Deployment
- **Docker** — mantiene el **mismo ambiente** en todas las etapas.
- **Argo** — orquestación de clusters de Kubernetes, pipelines de CD complejos, ejecución en paralelo, gestión de dependencias.
- **Lambdas (serverless)** — escalan según eventos; se paga solo por tiempo de ejecución (baratas vs. un cluster siempre encendido).

### Versionado: Betas
Retroalimentación temprana de usuarios, validación de características, iteración rápida, **reducir el riesgo de lanzamiento** y generar interés. *(Ej. del profe: la beta de macOS antes del release estable.)*

---

## Operate & Monitoring
El ciclo **no termina** cuando el código "está vivo":
- **Operate:** Infraestructura como Código ([[06 - Terraform (Infraestructura como Código)|Terraform]]), escalado automático, **gestión de secretos**.
- **Monitoring:** observabilidad — logs, métricas, **trazas** → ver [[05 - Monitoreo y Observabilidad]].
- **Feedback Loop:** los datos de producción informan la **planificación futura**.

Herramientas: Kubernetes, Prometheus, Grafana, ELK, Terraform, Datadog, New Relic.

> [!quote] CAMS en CI/CD
> - **Culture:** colaboración y transparencia en la entrega.
> - **Automation:** el *core* del pipeline.
> - **Measurement:** pruebas automáticas + monitoreo de performance.
> - **Sharing:** repos compartidos y visibilidad del estado de los despliegues (notificaciones en Slack).
>
> *"Si duele, hacelo más seguido"* — si integrar código es difícil, integrá cada hora, no cada mes. Y como dice **Jez Humble**: el objetivo de CI/CD es que los **deployments sean aburridos** → nada de dramas ni fines de semana arreglando servidores: apretar un botón y que sea rutinario y seguro.

> [!summary] Cierre
> CI/CD conecta la teoría (Unidad 1) con la práctica: en la [[04 - Deployment|Unidad 4]] se implementa el pipeline real con **GitHub Actions + Docker Hub + Render**.
