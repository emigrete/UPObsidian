---
tags: [devops, monitoreo, observabilidad, sli-slo-sla, opentelemetry, metricas, logs, traces]
unidad: 5
clase: 6
---

# 5️⃣ Monitoreo y Observabilidad

> [!abstract] Idea central
> Observar la **salud y el rendimiento** de los sistemas para ser **proactivos**. Pero **monitorear sin objetivos es solo ruido**: primero se definen **SLOs** (qué garantizamos), luego se elige **qué medir**. Tres pilares: **métricas, logs y trazas**. Esta unidad **cierra el feedback loop** planteado en la [[01 - Introducción a DevOps|Unidad 1]].

[[04 - Deployment|← Unidad 4]] · [[00 - Índice DevOps|Índice]] · Siguiente: [[06 - Terraform (Infraestructura como Código)]]

---

## Monitoreo vs Observabilidad
- **Monitoreo:** observar, rastrear y analizar el rendimiento y la salud; recolectar datos, métricas y logs para identificar y resolver problemas **proactivamente**.
> [!quote] "You can't manage what you can't measure" — Peter Drucker

## SLI / SLO / SLA y Error Budget
> [!tip] La cadena (muy preguntada en clase)
> Primero definís **qué garantizás**, después **qué medís**.
> - **SLA (Service Level Agreement):** **contrato legal** con el cliente, con **penalidades** si se incumple. Ej.: crédito del 10% si la disponibilidad baja del 99.5% en el mes.
> - **SLO (Service Level Objective):** **objetivo interno** de confiabilidad. Ej.: 99.9% de disponibilidad/mes. Es lo que nos comprometemos a cumplir.
> - **SLI (Service Level Indicator):** la **métrica** que mide el cumplimiento. Ej.: `requests_exitosos / total_requests = 99.95%`.

> [!example] Anécdota del profe
> Trabajó en un **call center** con un SLA de tasa de respuesta de llamadas (~98%, promedio diario considerando horas pico). Y dio el ejemplo clásico: **Amazon** tiene un SLA de **99.99%** de disponibilidad. Si hay un incidente y el servicio queda *down* o degradado, ese SLA se afecta → resolverlo rápido sube el promedio.

**Error Budget:** la cantidad de tiempo/requests que **pueden fallar sin romper el SLO**. Con SLO 99.9% mensual → 0.1% = **~43.8 minutos/mes**. Sirve para decidir con **datos** (no subjetivamente): *¿puedo desplegar hoy?*, *¿es seguro este experimento?*, *¿hay que frenar features y pagar deuda técnica?* En empresas con políticas estrictas, si se agota el error budget **no se deja desplegar más** ese mes.

---

## Los 3 pilares de la observabilidad

| Pilar | Qué es | Responde | Cuándo usarlo |
|---|---|---|---|
| 📊 **Métricas** | Datos numéricos **agregados** en el tiempo. Baratas de almacenar, rápidas de consultar. | *¿Está bien mi sistema?* | SLOs, alertas, tendencias, capacity planning. |
| 📋 **Logs** | Registros de **eventos discretos** con contexto. Ricos pero **costosos** de almacenar/consultar. | *¿Qué pasó exactamente?* | Debugging puntual, auditoría/compliance, análisis forense post-incidente. |
| 🔍 **Trazas** | Seguimiento del **camino completo** de una request entre servicios. | *¿Dónde está el problema?* | Latencia, dependencias, cuellos de botella en microservicios. |

> [!note] No uses la herramienta equivocada
> - No uses `grep` en logs para calcular tasas de error en tiempo real → eso es un **Counter** en tu sistema de métricas.
> - Para entender **por qué** falló una request específica → **logs/traces**, no métricas.

> [!example] Trazas: el ejemplo del checkout
> Un `POST /checkout` tarda **52 ms** en total. Solo con la métrica ves los 52 ms; con la **traza** ves que **33 ms los consume un `INSERT` en la DB** → ahí está el cuello de botella. *Esa es la diferencia entre observabilidad y monitoreo tradicional.* En microservicios (ej. tipo Uber: API Gateway → Auth → Orders → DB; con load balancers e instancias), la traza dice **en qué servicio** falló o se demoró, usando un **trace_id** que se propaga entre servicios.
>
> 👉 *Por eso el profe recomienda para el TP* llamar a una **API gratuita externa** (Star Wars, Pokémon, Chuck Norris…) o tener una **base de datos** local: así, en el dashboard, podés **ver las trazas** y los tiempos de cada llamada encadenada.

---

## Tipos de métricas
> ⚠️ Los **tipos de métrica** NO son tipos de gráfica. Lo que importa es la **semántica del dato**, no cómo se visualiza.

| Tipo | Qué es | Ejemplos | Consulta típica |
|---|---|---|---|
| 📈 **Counter** | Solo **incrementa** (se resetea al reiniciar). | requests totales, errores totales, bytes transmitidos. | `rate(counter[5m])` → valor por segundo. |
| 🌡️ **Gauge** | Valor **instantáneo**, sube y baja (un termómetro). | CPU actual, memoria usada, conexiones activas, *queue depth*. | valor actual o promedio. |
| 📊 **Histogram** | **Distribución** en *buckets*. | latencia, tamaño de respuestas, duración de jobs. | percentiles `histogram_quantile(0.99, ...)` (p50/p95/p99). |

> [!warning] El promedio engaña
> Un promedio de latencia de 100 ms puede **esconder** que el **p99 es 2 segundos** (1 de cada 100 usuarios espera 2 s). El promedio oculta el problema; el **p99 lo revela**.

```python
# Instrumentación con OpenTelemetry (counter)
from opentelemetry import metrics
meter = metrics.get_meter("mi-servicio")
http_requests = meter.create_counter("http_requests_total", unit="1")
http_requests.add(1, {"method": "GET", "status": "200", "route": "/api/orders"})
```

---

## The Four Golden Signals (Google SRE)
Si solo pudieras medir 4 cosas, medí estas:
1. **Latency** — cuánto tarda en responder. SLO típico: p99 < 500 ms. *Distinguir latencia de requests exitosas vs fallidas* (una que falla en 1 ms distorsiona el p99 hacia abajo).
2. **Traffic** — demanda sobre el sistema (RPS, mensajes/s, transacciones/s). Define el *baseline* normal.
3. **Errors** — fracción de requests que falla (SLO típico < 0.1%). Tres categorías:
   - **Explícitos** — reconocidos por el protocolo (HTTP 5xx, excepciones no controladas, timeouts).
   - **Implícitos** — parecen exitosos pero no (HTTP 200 con JSON malformado o que **no respeta el contrato**, respuesta vacía). *Ej. del profe: sacás un campo del JSON que el consumidor esperaba → le devolvés un 200 "roto".*
   - **Por política** — éxitos que **violan el SLO** (responde bien pero fuera del error budget).
4. **Saturation** — qué tan "lleno" está (CPU, memoria, *connection pool*, *queue depth*, disco). Un recurso al 70% bajo carga normal ya es señal de alerta; la latencia sube exponencialmente cerca de la saturación.

---

## OpenTelemetry (OTel)
**Framework open source y *vendor-neutral*** para instrumentar, generar, recolectar y exportar telemetría. Estándar de la industria (parte de la **CNCF**, como Docker y Kubernetes).

> [!tip] Por qué importa (lo central de la unidad)
> Si instrumentás tu código con el **SDK propietario** de un vendor (ej. Datadog) y querés migrar, tenés que **re-instrumentar toda la app** → *vendor lock-in*. **OTel separa la instrumentación de la exportación:** instrumentás una sola vez y, para cambiar de backend, **solo cambiás el exporter** (en la capa de infra/variables de entorno), **sin tocar el código de negocio**.

**Arquitectura — OTel Collector:** tu app (SDK + auto-instrumentación) → **Receiver** (OTLP, Prometheus, Jaeger…) → **Processor** (filtrado, sampling, enriquecimiento, batching) → **Exporter** → backends. Podés exportar **al mismo tiempo** a varios destinos (ej. Prometheus gratis + Datadog pago) cambiando solo la config.

**Backends compatibles:** Métricas → Prometheus/Grafana, Datadog, New Relic. Trazas → Jaeger, Zipkin, Tempo. Logs → Loki, Elasticsearch. All-in-one → Datadog, New Relic, Honeycomb.

---

## Elegir las métricas correctas
Siempre **desde los SLOs**: 1) Definí el SLO → 2) Identificá el SLI → 3) Elegí el tipo (counter/gauge/histogram) → 4) Definí **etiquetas (labels)** → 5) ¿Qué acción requiere?

**Buena métrica = entendible** (¿la entiende un on-call a las 3am?) + **accionable** (sabés qué hacer cuando cambia) + **mejorable** + **multidimensional** (con labels).

**Labels recomendados:** `env` (prod/staging/dev), `service`, `region`, `version` (correlacionar con deploys), `status`.
> [!warning] Alta cardinalidad
> **Nunca** uses como label de métrica valores de alta cardinalidad (`user_id`, `request_id`, `session_id`) → crearías millones de series de tiempo. Para eso están **traces y logs**.

---

## Alertas y *Alerting Fatigue*
> [!danger] La regla de oro del alerting
> Una alerta válida cumple **los tres** criterios a la vez:
> 1. 🔴 **Urgente** — ¿requiere atención en los **próximos minutos**? (no horas, no mañana).
> 2. ⚡ **Accionable** — ¿hay algo concreto que hacer? ¿existe un **runbook**?
> 3. 👤 **Impacta al usuario** — ¿afecta o afectará el **SLO** comprometido?
>
> Si la respuesta a cualquiera es **NO → no es una alerta, es ruido** (y el ruido es el enemigo de la confiabilidad).

**Alerting Fatigue:** 200 alertas/día sin contexto → el equipo **aprende a ignorarlas** → se silencian "molestas" → un **incidente crítico pasa desapercibido** → pérdida de confianza y **burnout** del on-call. En cada Sprint hay que revisar qué alertas son realmente necesarias.

| Dimensión | 🚨 Alerta | 📢 Notificación |
|---|---|---|
| Canal | PagerDuty / Llamada / SMS | Slack / Email / Ticket / Dashboard |
| Respuesta esperada | Inmediata (< 5 min) | Próximas horas/días |
| ¿Despierta a alguien? | Sí (incluso a las 3am) | No |
| Ejemplo válido | Error rate supera el SLO en prod | Disco al 65% (tendencia de 2 semanas) |
| Runbook | ✅ Siempre | ❌ Opcional |

**Tipos de alerta, de peor a mejor:** 🔴 valor estático (`CPU > 80%`, muchos falsos positivos) → 🟡 por **anomalía** (desviaciones estándar) → 🟢 basada en **SLO / error budget burn rate** (*gold standard* de Google SRE: ej. "quemando el error budget 14× más rápido de lo normal"). **Toda alerta debe tener runbook**: si no podés escribir los pasos, no deberías crear la alerta.

---

## Logs — buenas prácticas
- **Log levels:** `DEBUG` (solo desarrollo) · `INFO` (eventos normales) · `WARN` (situación inesperada pero sigue) · `ERROR` (fallo que requiere revisión) · `FATAL` (irrecuperable, aborta). En **producción** no abusar de DEBUG/INFO (ocupan almacenamiento que se paga).
> **Anti-patrón:** loguear todo como `ERROR` "para no perderse nada" → misma pérdida de señal que el alerting fatigue: *cuando todo es error, nada es error*.
- **Logs estructurados (JSON)** — parseables, buscables y **correlacionables**. Incluir `timestamp`, `level`, `service`, `version`, `env`, `region`, **`trace_id`**, `request_id`, `user_id`, etc. El `trace_id` permite **saltar del log (Loki) a la traza (Jaeger/Tempo)** → correlación entre pilares.
- **Request correlation:** propagar un `request_id` en las cabeceras como un "pasamanos" entre servicios para reconstruir el camino punta a punta.
- **Políticas de retención** (cuánto guardar; cuesta dinero y/o lo exige una regulación), **centralización** (SIEM/agregación), **proteger el acceso** y **nunca loguear datos sensibles** (passwords, tokens, PII).

---

## Integración en el TP (New Relic / Datadog)
El profe mostró el repo modelo: se agrega la **librería** del proveedor (o de OTel) dentro de la app, que publica a un **agente** (ej. `datadog-agent` corriendo en la imagen) y este al backend. Cada proveedor tiene un **wizard paso a paso** según el lenguaje (Node, Python, Java/Spring, .NET…):
- **New Relic:** crear cuenta → *Add integration* → elegir lenguaje y entorno (Docker) → te da el `newrelic.ini`/config, la dependencia y las variables de entorno → *Test connection* (a veces dice que no conecta pero igual aparece en el dashboard).
- **Datadog / OTel:** librería de tracing + logs, o instrumentar con OpenTelemetry.

> [!tip] Truco para la presentación
> Tené un **script** (o un *runner* de Postman) que ejecute la API muchas veces (y genere algún error) **antes de presentar**: el monitoreo en el free tier **tarda** en mostrar datos, así llegás con un histórico de métricas y trazas listo.

---

## El loop completo (DevOps + observabilidad)
`Plan (SLOs, capacity) → Code (instrumentar con OTel) → Build/Test (confiabilidad, load testing, chaos eng.) → Release (feature flags, canary, blue-green) → Deploy (observar error budget) → Operate (on-call, runbooks) → Monitor (SLIs, alertas, post-mortems) → Feedback loop blameless y data-driven → Plan…`

> [!summary] Beneficios de una observabilidad madura
> **Técnicos:** MTTR reducido (minutos, no horas), deploys con confianza (el error budget dice cuándo es seguro), cultura *blameless* (datos > suposiciones), capacity planning.
> **Negocio:** menos incidentes (detección proactiva), SLAs basados en datos reales, **mismo lenguaje** entre Dev/Ops/negocio (SLOs), mejora continua medible. Esto **cierra el feedback loop** de la [[01 - Introducción a DevOps|Unidad 1]].
