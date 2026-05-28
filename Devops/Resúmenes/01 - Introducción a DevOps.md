---
tags: [devops, teoria, cultura, cams, introduccion]
unidad: 1
clase: 2
---

# 1️⃣ Introducción a DevOps

> [!abstract] Idea central
> DevOps **NO es un puesto, una herramienta ni un título**: es una **cultura**. Surge de unir el **desarrollo (Dev)** y las **operaciones (Ops)** a lo largo de **todo el ciclo de vida** del servicio, rompiendo los "silos" para entregar valor más rápido y de forma más confiable.

[[00 - Índice DevOps|← Volver al índice]] · Siguiente: [[02 - Herramientas - Git, GitHub y Docker]]

---

## ¿Qué es DevOps?
DevOps surgió de la **colisión de dos tendencias**:
1. **Infraestructura ágil / operaciones ágiles** → aplicar enfoques **Agile y Lean** al trabajo de operaciones.
2. La comprensión del valor de la **colaboración entre Dev y Ops** durante **todo el ciclo de vida** (diseño → desarrollo → implementación → soporte en producción).

Es la práctica en la que los ingenieros de **operaciones y desarrollo participan juntos** en todo el ciclo de vida del servicio. Operaciones empieza a usar **técnicas de desarrollo** (control de versiones, infraestructura como código, pruebas, desarrollo ágil) y desarrollo crea aplicaciones pensadas para **operar bien** (observabilidad, resiliencia).

> [!quote] Lo que más recalcó el profesor en clase
> "**DevOps no es un puesto de trabajo, no debería existir como tal. Sé que lo van a ver en LinkedIn que 'se busca un DevOps', pero créanme que no es así: es una cultura.**"
> A veces se confunde DevOps con **SRE** y se mezcla todo. Lo que pasa en muchas empresas (caso de un compañero, donde el rol "DevOps" sí existe y arma los pipelines) es que la organización **asigna roles** y pone énfasis en la **automatización** y en aplicar técnicas de desarrollo a la infraestructura, pero eso es solo *una parte*: la idea de fondo es que **todos los que manejan infraestructura tengan la práctica incorporada**.

### El problema de los silos
Cuando la empresa está dividida en silos (Dev por un lado, Ops por otro), cada equipo tiene **sus propios objetivos y tiempos**. Si Dev necesita infraestructura, tiene que pedírsela a Ops por ticket → Ops está atendiendo otras cosas → **demoras**. DevOps busca **unificar** para ser más ágiles, **aumentar la frecuencia de despliegue**, reducir plazos de entrega, minimizar fallos y **acelerar la recuperación**.

> [!note] "Dev" y "Ops" son términos paraguas
> **Ops** = sysadmins, ingenieros de sistemas/red, DBAs, release engineers, profesionales de seguridad… **Dev** = front/back y *todas las personas involucradas en el producto* (incluye Producto, QA). DevOps pretende ser **lo más inclusivo posible**. *(El profe aclaró que esto depende de la empresa y de la lógica de negocio: en una app simple es fácil que una persona cubra varios roles; en una de contabilidad, con mucha lógica de negocio, es más difícil.)*

---

## CAMS (o CALMS)
Los cuatro valores fundamentales de DevOps, acuñados por los pioneros **John Willis y Damon Edwards**. La premisa de fondo: el problema casi nunca es solo tecnológico, **es cultural**.

| Letra | Valor | Idea |
|---|---|---|
| **C** | **Culture** | Va más allá de lo físico: **comportamientos** y **comprensión mutua** entre equipos. Romper silos, colaborar. |
| **A** | **Automation** | Acelerador para controlar sistemas y apps. Debe equilibrarse con procesos y personas. |
| **M** | **Measurement** | Medir es esencial. Métricas: **MTTR**, *cycle time*, costos, ingresos, satisfacción del empleado. |
| **S** | **Sharing** | Compartir ideas y problemas → colaboración y mejora continua. |

> [!tip] People over Process over Tools
> Prioridad: **1) Personas → 2) Procesos → 3) Herramientas.** Primero asegurar que haya una **persona responsable y capacitada** que resuelva ambigüedades; luego que el **proceso** esté bien definido; **recién entonces** considerar automatizar con herramientas (si el costo/esfuerzo lo justifican).
> *Ejemplo del profe (pregunta de Joana):* para automatizar algo en Contabilidad, primero asegurarte de que el dueño del proceso (el área de contabilidad) esté **capacitado** y que el **proceso esté bien definido**; después automatizás.

### Kaizen (改善)
"Cambio para mejor" = **mejora continua**. Viene del método **Lean / Sistema de Producción de Toyota** (popularizado ~1986). Se basa en pequeños cambios que generan grandes resultados. Usa herramientas como los **5 porqués** para llegar a la causa raíz, enfocándose en las causas y **evitando culpar al error humano**. Énfasis en **ir al lugar real donde se crea el valor / está el problema**, no confiar solo en informes y métricas.

---

## The Three Ways (Las Tres Vías)
Principios de los *DevOps Streetwise*; guía para aplicar los valores a las organizaciones:

1. **Systems Thinking** — entender el sistema **en su totalidad** (del concepto a la entrega), no optimizar partes aisladas a costa del resultado global.
   - **From Concept to Cash:** el flujo completo desde la idea hasta la entrega y **monetización**. Si la organización desarrolla pero no puede entregar valor al cliente, el esfuerzo se pierde. La separación Dev/Ops suele ser donde este flujo **se obstaculiza**.
2. **Amplify Feedback Loops** — crear, **acortar y amplificar** los bucles de retroalimentación → detección temprana de errores (más barato corregir un bug en testing que en producción).
3. **Continuous Experimentation & Learning** — cultura de aprendizaje y experimentación constante; superar la resistencia al cambio.

> [!example] Feedback loops en la práctica
> Tests automáticos (unitarios, de integración, E2E) que corren en cada PR, alertas de monitoreo, logs… todos son *feedback loops* que nos dan la "salud" de la aplicación. **Este concepto se cierra en la [[05 - Monitoreo y Observabilidad|Unidad 5]].**

---

## 10 prácticas para el éxito
(Listadas como "cuenta regresiva" en el material)

1. **Chaos Monkey** — herramienta de **Netflix** que provoca **fallos aleatorios** en la infraestructura para probar la **resiliencia**. Base de la *ingeniería de caos*: definir un estado estacionario → hipótesis → introducir variables del mundo real (servidores caídos, discos, red cortada) → refutar la hipótesis.
2. **Blue-Green Deployment** — dos entornos idénticos: uno **activo (azul)** y otro **inactivo (verde)**; se redirige el tráfico → cero downtime y rollback fácil. *(El profe lo explicó desde la época de las **máquinas virtuales**: clonabas la VM, actualizabas el clon y redirigías el tráfico.)*
3. **Dependency Injection** — desacoplar capas; la app no debe conocer sus dependencias externas (infra, BD). La **lógica de negocio** queda aislada → arquitecturas limpias / hexagonales.
4. **Andon Cords** — mecanismos visuales que **alertan sobre problemas**. Viene de Toyota: en la línea de producción cualquier operador podía **tirar de la cuerda y parar toda la producción**; todos se acercaban a resolver el problema.
5. **Cloud** — escalabilidad, flexibilidad y agilidad para desarrollar, desplegar y escalar (difícil de lograr *on-premise*).
6. **Embedded / Cross-functional Teams** — equipos multidisciplinarios (Dev, Ops, QA, **seguridad**) trabajando juntos. *Ej. del profe:* si seguridad está integrada al equipo, detecta problemas **antes** de terminar el desarrollo, en vez de bloquear el PR al final.
7. **Blameless Postmortems** — informes posteriores a incidentes que analizan **causas sin culpar** a individuos; proponen cómo evitar que se repita (puede ser un problema de proceso, no solo de código).
8. **Status Pages** — interfaces públicas en tiempo real del estado de los servicios (status de Amazon, Slack, Azure…). Dan **transparencia** al usuario.
9. **Developers on Call** — un desarrollador asignado para responder a incidentes **fuera del horario laboral**.
10. **Incident Command System** — sistema estandarizado para coordinar la respuesta a incidentes.

> [!example] Anécdota de incidentes (discusión en clase)
> Los compañeros contaron sus esquemas reales: **NOC** ("pecera" donde están todos juntos), niveles de soporte (1°/2°/3°), **runbooks ("rambo")**, y el concepto de **Major Incident** (cuando se afecta la planta de producción → se para todo y se suma gente). El profe remarcó que con un buen *runbook* **cualquiera del on-call** debería poder resolver, y que lo ideal es un proceso **estandarizado** (no el "desorden" típico de hot-fixes improvisados).

---

## Agile, Waterfall y Lean
- **Waterfall (cascada):** enfoque rígido y secuencial (requisitos → diseño → implementación → verificación → mantenimiento). *El profe lo contextualizó:* tenía sentido en los 80/90 para software robusto de bancos/grandes empresas.
- **Agile:** desarrollo **iterativo y colaborativo**; surge con la masificación de Internet y apps más interactivas.
- **Manifiesto Ágil (2001)** — valores clave: individuos e interacciones **sobre** procesos y herramientas; **software funcionando** sobre documentación extensa; colaboración con el cliente sobre negociación de contratos; **respuesta al cambio** sobre seguir un plan.
- **Lean (7 principios):** eliminar desperdicio, amplificar el aprendizaje, decidir lo más tarde posible, entrega rápida, **empoderar al equipo**, integridad incorporada, **ver el todo**.
- **Build-Measure-Learn** (Eric Ries, *Lean Startup*): construir un **MVP** → medir → aprender → iterar. El producto chico facilita correcciones rápidas.

---

## ❌ Qué NO es DevOps
- No es "no-operaciones".
- No es simplemente **herramientas**.
- No es simplemente **cultura** (es más que eso).
- No es simplemente "Dev + Ops juntos".
- **No es un título fancy.**
- No es "todo".

---

## 🌱 Prácticas derivadas de DevOps (del material)
El éxito de DevOps generó especializaciones que aplican CAMS a dominios concretos:

| Práctica | Foco | Pilar CAMS |
|---|---|---|
| **GitOps** | Git como única fuente de verdad; cambios por PR; reconciliación automática (ArgoCD, Flux). Acuñado por Alexis Richardson (Weaveworks, 2017). | Automatización |
| **MLOps** | Ciclo de vida de ML: versionado de datos/modelos, *drift detection*, reentrenamiento. | Automatización |
| **DevSecOps** | **Seguridad integrada** en cada etapa — *"Shift Left Security"*. Ver [[07 - Seguridad (OWASP Top 10)]]. | Cultura/Automatización |
| **DataOps** | Pipelines y gobierno de datos. | Automatización |
| **FinOps** | Optimización de costos cloud como problema de ingeniería. | Medición |
| **PlatformOps** | Plataformas internas para *self-service* de devs. | Cultura |
| **SRE** | Confiabilidad con **SLOs y error budgets**. Ver [[05 - Monitoreo y Observabilidad]]. | Medición |
| **AIOps / SecOps** | IA para detección/remediación / operación de seguridad. | Automatización |

> [!summary] Cierre de la unidad
> DevOps = **movimiento cultural** que evoluciona. Los pilares permanecen constantes: **Cultura** (romper silos), **Automatización** (eliminar trabajo manual), **Medición** (decisiones por datos) y **Compartir** (transferir conocimiento). Las prácticas derivadas no reemplazan a DevOps; lo **extienden**.
