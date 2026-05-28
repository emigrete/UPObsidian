---
tags: [devops, indice, moc, resumen]
materia: DevOps
profesor: Lucas Bonanni
universidad: Universidad de Palermo
---

# 🗂️ DevOps — Índice de la materia (MOC)

> [!info] Sobre este resumen
> Resumen completo de la materia **DevOps** (Prof. **Lucas Bonanni**, UP), armado a partir de las **transcripciones de las Clases 1 a 7** (priorizando lo que el profesor dijo y enfatizó en clase) y complementado con el **material de la cátedra** (`MaterialCursada/`). Una página por unidad.

## 👤 El profesor
Lucas Bonanni — experiencia en **PedidosYa, Eventbrite, Thomson Reuters y Megatech**. Tecnicatura en Programación (UTN) + Lic. en Tecnología de la Información (Palermo). Da el enfoque desde la práctica real de la industria.

## 🎯 Objetivo de la materia
Tener una base sólida de conocimientos y habilidades para:
- Entender e implementar el proceso de **CI/CD**.
- Gestión de configuraciones, rendimiento y **seguridad**.
- **Monitoreo** y observabilidad.
- **Automatización de la infraestructura** (IaC).
- Desplegar una aplicación **end-to-end** aplicando CI/CD (y, si alcanza el tiempo, deploy a Kubernetes).

## 📚 Las 7 unidades

| # | Unidad | Clase(s) | Material |
|---|--------|----------|----------|
| 1 | [[01 - Introducción a DevOps]] | Clase 2 | `DevOps Introduction(1).pdf`, `intro.pdf` |
| 2 | [[02 - Herramientas - Git, GitHub y Docker]] | Clase 3 | `DevOps - Tools.pdf` |
| 3 | [[03 - CI-CD]] | Clase 4 | `CI_CD.pdf` |
| 4 | [[04 - Deployment]] | Clase 5 | `04-deployment.pdf` |
| 5 | [[05 - Monitoreo y Observabilidad]] | Clase 6 | `Monitoring & Observability.pdf` |
| 6 | [[06 - Terraform (Infraestructura como Código)]] | Clase 7 | `Terraform.pdf` |
| 7 | [[07 - Seguridad (OWASP Top 10)]] | — (módulo de material) | `Security.pdf` |

> [!note] Nota sobre la Unidad 7 (Seguridad)
> En las transcripciones de Clases 1–7 **no hay una clase dedicada a Seguridad**; el profesor la menciona como un módulo de la materia y toca conceptos de seguridad de forma transversal (DevSecOps, escaneos, gestión de secretos). La Unidad 7 se basa principalmente en el material `Security.pdf` (OWASP Top 10 2025).

## 🧰 Stack de herramientas de la cursada
- **Control de versiones / colaboración:** Git, GitHub (también GitLab, Bitbucket).
- **CI/CD:** GitHub Actions (principal); se mencionan CircleCI, Travis, Jenkins.
- **Contenedores:** Docker, Docker Compose, Container Registry (Docker Hub / GHCR).
- **Deploy:** Render (free tier) vía *deploy hook*; Kubernetes con minikube/LocalStack a modo de laboratorio.
- **IaC:** Terraform.
- **Monitoreo:** New Relic, Datadog, Grafana, Prometheus; estándar **OpenTelemetry**.
- **Comunicación:** **Slack** (consultas rápidas, compartir código) y **Blackboard** (entregas, materiales, comunicaciones formales).

## 📝 Evaluación
- **2 parciales** con preguntas a desarrollar (con *Respondus*). Se necesitan **ambos** para aprobar la cursada. El 1° es más conceptual (teoría de las primeras clases).

> [!question] 2do Parcial — repaso
> - [[Resumen 2do Parcial - DevOps]] — resumen consolidado de una página de las 4 unidades que entran.
> - [[Preguntas Integradoras - DevOps (2do Parcial)]] — preguntas estilo parcial con respuestas plegadas (Deployment, Seguridad, Terraform, Monitoreo).
- **Trabajo Práctico Integrador** → es la **evaluación del final**. Se hace una **pre-entrega + presentación** durante la cursada; en la fecha de final se corrigen los puntos marcados y se pone la nota.
- Se entrega: **repositorio (link/zip)** + **informe en PDF** (decisiones, configuraciones, buenas prácticas con fuentes oficiales, imágenes) + **PPT** para la presentación.

### 🚀 Trabajo Práctico Integrador — checklist
Desarrollar una **API REST sencilla** (3–5 endpoints, lenguaje a elección) e ir "desbloqueando" etapas a medida que avanza la materia:

- [ ] **Dockerfile multi-stage** siguiendo buenas prácticas del framework/lenguaje (justificar con fuente oficial).
- [ ] **Docker Compose** (idealmente con base de datos / servicio extra para enriquecer el monitoreo).
- [ ] **Pipeline CI/CD con GitHub Actions:** build + tests unitarios + algún scanner; correr en cada PR.
- [ ] **Publicar la imagen** versionada (SemVer, **evitar `latest`**) a Docker Hub / GHCR.
- [ ] **Deploy a Render** disparado desde el Action vía **deploy hook** (no basta con linkear la cuenta de GitHub a Render).
- [ ] **Dashboard de monitoreo** (New Relic/Datadog) con métricas, **APM y trazas**; conviene tener un script que genere tráfico/errores antes de presentar.

> [!tip] Lo "mínimo" vs lo "opcional"
> Los puntos marcados en rojo en la consigna son los mínimos para probar → **nota 4**. El resto (Render, monitoreo, etc.) es "opcional" en el sentido de que no resta, pero **no hacerlos = no sumar ese punto**. El profe recomienda hacerlos por la experiencia.

## 🧵 Hilo conductor de la materia
Todo se apoya en la **cultura DevOps (CAMS)** y los **feedback loops**: cada unidad agrega un eslabón al ciclo *Plan → Code → Build → Test → Release → Deploy → Operate → Monitor → (vuelta a Plan)*. La Unidad 5 (monitoreo) **cierra el loop** que se planteó en la Unidad 1.
