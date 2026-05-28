---
title: Resumen Completo — Testing y QA
aliases:
  - Resumen Testing y QA
  - Resumen Final Testing y QA
  - Resumen Primer y Segundo Parcial
tags:
  - testing
  - qa
  - resumen
  - examen
  - universidad-palermo
date: 2026-05-24
---

# Resumen Completo — Testing y QA

> [!abstract] Sobre este resumen
> Resumen integral de **toda la materia** armado a partir de los 12 apuntes de `MaterialCursada/` y la **Hoja de Ruta** (`0193_HDR_261Q`). Está dividido según la estructura oficial del curso:
> - **🅰️ Primer parcial → Unidades 1 a 4 (Módulos 01–06).** Examen en la Semana 7, después de *Diseño de casos de test*.
> - **🅱️ Segundo parcial → Unidades 5 a 9 (Módulos 07–13).** Examen en la Semana 15, después de *TMMi*.
>
> Cada sección mantiene fidelidad al apunte de origen: se conservan definiciones, tablas, fórmulas y procesos.

## Mapa de la materia

| Unidad | Módulo(s) | Tema | Parcial |
|---|---|---|---|
| **U1** — Definición de calidad de software | 01, 02, 03 | Atributos de calidad · Calidad del software · Certificación | 🅰️ Primer |
| **U2** — Administración de Configuración | 04 | Gestión de configuración (CM) | 🅰️ Primer |
| **U3** — Fundamentos del testing | 05 | Error/defecto/falla, V&V, proceso, modelos de ciclo de vida | 🅰️ Primer |
| **U4** — Técnicas de diseño de casos de test | 06 | Caja negra / caja blanca, complejidad ciclomática | 🅰️ Primer |
| **U5** — Estrategias de testing | 07 | Niveles, estrategias de integración, pruebas de sistema | 🅱️ Segundo |
| **U6** — Plan de testing | 08 | Plan de pruebas (IEEE), estimación, TCP | 🅱️ Segundo |
| **U7** — Principios del testing ágil | 09 (+Evolución) | Manifiesto ágil, TDD/BDD/ATDD, evolución de la agilidad | 🅱️ Segundo |
| **U8** — DevOps y testing web y mobile | 10, 11 | DevOps, CI/CD, automatización, testing web y mobile | 🅱️ Segundo |
| **U9** — Estándares Testing | 12, 13 | ISO/IEC/IEEE 29119 · TMMi (modelos de madurez) | 🅱️ Segundo |

> [!warning] Material faltante
> En `MaterialCursada/` **no están los apuntes de la Unidad 9** (Módulo 12 *Estándares testing / ISO 29119* y Módulo 13 *TMMi*). La sección U9 incluye un repaso general marcado como tal, a completar cuando aparezca el apunte.

> [!question] Autoevaluación 2do Parcial
> [[Preguntas Integradoras - Testing y QA (2do Parcial)]] — preguntas estilo parcial con respuestas plegadas (U5–U9).

---

# 🅰️ PRIMER PARCIAL — Unidades 1 a 4

## Unidad 1 — Definición de calidad de software

#### Módulo 01 — Atributos de calidad del software

##### ¿Qué es la calidad?

La calidad se convierte hoy en un **objetivo fundamental para las empresas**: lo que prima es la adaptabilidad a las necesidades del cliente, que actúa como conductor de la propuesta de valor.

**Pirsig** sostiene que la calidad no es indefinible porque sea misteriosa, sino porque es "simple, inmediata y directa". Sin embargo, es necesario analizarla para incorporarla al concepto de software.

El término calidad **se interpreta mal** con frecuencia dentro del contexto del software: implica excelencia, pero ese no es el significado que le dan los profesionales de ingeniería de software.

**Definiciones formales:**
- *American Heritage Dictionary*: la calidad es "una característica o atributo de algo"; como atributo de un elemento, se refiere a **características mensurables** comparables con estándares conocidos (longitud, color, propiedades eléctricas, maleabilidad, etc.). El software, como entidad intelectual, es más difícil de caracterizar que los objetos físicos.
- **IEEE**: "Es cuando el software posee una buena combinación de atributos deseados y el cliente o usuario percibe que el producto cumple con sus expectativas."

##### ¿Qué es un producto de software con calidad?

Distintos actores tienen visiones distintas. Las frases más relevantes son:

| # | Frase | Relevante para... |
|---|-------|-------------------|
| 1 | **'Cero defectos es alta calidad'** | Usuarios afectados por defectos; gerentes criticados por su existencia |
| 2 | **'Mucha funcionalidad es alta calidad'** | Usuarios que se benefician de muchas opciones; vendedores que lo usan como argumento de venta |
| 3 | **'Alta performance es alta calidad'** | Usuarios cuyo trabajo depende del tiempo de respuesta; vendedores cuya venta se asocia al tiempo de respuesta |
| 4 | **'Costo de desarrollo bajo es alta calidad'** | Clientes que compran el software; PMs con presupuestos limitados |
| 5 | **'Rapidez en la construcción es alta calidad'** | Usuarios que esperan el producto; vendedores que quieren capturar el mercado primero |
| 6 | **'Amigabilidad es alta calidad'** | Usuarios que tardan en aprender a usar el producto |

##### Problemas asociados a la calidad de software

1. La especificación se orienta hacia características del producto que el consumidor quiere.
2. No se sabe cómo especificar ciertas características de calidad (por ejemplo, **mantenimiento**) de forma no ambigua.
3. No se puede especificar en forma concreta un software; aunque un producto se ajuste a su especificación, los usuarios pueden no considerarlo de alta calidad si no responde a sus expectativas.

> Se deben reconocer estos problemas y diseñar procedimientos de calidad que **no se basen en una especificación perfecta**. En la práctica, **es más importante la gestión de la calidad** que los estándares y la burocracia asociada.

##### Actividades principales en la administración de calidad

1. **Garantía de la calidad:** establecimiento de un marco de procedimientos y estándares organizacionales que conduce a software de alta calidad.
2. **Planificación de la calidad:** selección de procedimientos y estándares adecuados a partir de ese marco, y su adaptación a un proyecto específico.
3. **Control de la calidad:** definición y fomento de los procesos que garantizan que los procedimientos y estándares son seguidos por el equipo.
4. **Coste de calidad:** todos los costes acarreados en la búsqueda de la calidad o en las actividades relacionadas con su obtención.

##### Componentes del coste de calidad (costo de la no calidad)

| Etapa | Costo relativo de corregir un error |
|-------|-------------------------------------|
| Requisitos | 1 vez |
| Diseño | 3–6 veces |
| Código | 10 veces |
| Prueba de desarrollo | 15–40 veces |
| Prueba de sistema | 30–70 veces |
| En fase de explotación | 40–1000 veces |

> [!tip] Concepto clave de examen
> Cuanto más **cercano al despliegue** se encuentra el defecto, **más costosa** es su solución.

##### Principios de calidad de software (Watts Humphrey)

1. Si un cliente no demanda calidad, probablemente no la conseguirá.
2. Para obtener calidad de manera constante, los desarrolladores deben gestionarla en su trabajo.
3. Para gestionar la calidad, los desarrolladores deben **medirla** (registros históricos de errores).
4. La calidad de un producto la determina el **proceso** usado para desarrollarlo.
5. Como las pruebas solucionan solo una fracción de los defectos, se deben tener pruebas de calidad (funcionales y no funcionales).
6. La calidad sólo la producen profesionales **motivados** y orgullosos de su trabajo.

##### Calidad de producto y calidad de proceso

La **calidad del proceso de desarrollo afecta directamente a la calidad de los productos** derivados. Pero en software la relación es compleja porque:
- El software **no se manufactura, sino que se diseña** (proceso más creativo que mecánico).
- La calidad también se ve afectada por **factores externos** (novedad, presión comercial).
- Es difícil medir atributos como la **mantenibilidad**.

**La gestión de la calidad del proceso implica:** definir estándares de proceso, supervisar el desarrollo para asegurar su cumplimiento, e informar al gestor del proyecto y al comprador.

##### Atributos de calidad del software (definiciones)

- **Correctitud:** se comporta de acuerdo a la especificación de funciones que debería proveer.
- **Confiabilidad:** probabilidad de que el software opere como se esperaba dentro de un intervalo de tiempo especificado.
- **Robustez:** se comporta razonablemente **aún en circunstancias no anticipadas** en la especificación.
- **Performance (eficiencia):** usa los recursos (tiempo y memoria) **económicamente**.
- **Amigabilidad:** fácil de usar; es más que la interfaz (también la afectan correctitud y performance).
- **Verificabilidad:** sus propiedades pueden verificarse fácilmente (favorecida por diseño modular y buenas prácticas).
- **Mantenibilidad:** modificaciones tras la entrega. Subdivisiones: *reparabilidad* (corregir defectos) y *evolutividad* (nuevas funciones).
- **Reusabilidad:** aplicable más a las partes que al producto entero; debe pensarse durante el desarrollo.
- **Portabilidad:** puede correr en distintos ambientes (hardware, SO).
- **Interoperabilidad:** coexistir y cooperar con otros sistemas (clave: **estandarización de interfaces**).
- **Productividad:** cualidad del **proceso**; mide su eficiencia.
- **Oportunidad:** cualidad del **proceso**; entregar a tiempo.
- **Visibilidad:** todos los pasos del proceso y su estado están **documentados** y accesibles.
- **Integridad:** controla los **accesos no autorizados** a datos o software.

#### Módulo 02 — Calidad del software

##### QA vs QC (aseguramiento vs control)

- **Aseguramiento de la calidad (QA):** conjunto de actividades sistemáticas que dan al proceso la capacidad de producir un producto adecuado para el uso.
- **Control de calidad (QC):** evaluación independiente de la capacidad del proceso para producir un producto usable.

| **Quality Control (QC)** | **Quality Assurance (QA)** |
|--------------------------|---------------------------|
| **Objetivo:** detectar problemas en los productos de trabajo | **Objetivo:** asegurar adherencia a procesos, estándares y planes |
| **Foco:** contenido del producto | **Foco:** procesos del proyecto |
| **Actividades:** revisiones de productos de trabajo | **Actividades:** guía y monitoreo de procesos; control de que las revisiones se hacen |

En **CMMI** se usan dos conceptos: **Verificación** ("¿Estoy construyendo *correctamente* el producto?") y **Validación** ("¿Estoy construyendo el *producto correcto*?").

##### Activos del proceso

| Activo | Definición |
|--------|------------|
| **Proceso** (qué) | Secuencia de actividades de un conjunto de roles para un propósito dado |
| **Procedimientos** (cómo) | Pasos específicos, reglas y/o criterios para cumplir un objetivo |
| **Estándares** | Sabiduría y convenciones de la comunidad de ingeniería de software |
| ***Templates*** | Esbozo de un producto de trabajo con descripción de campos a completar |
| **Producto de trabajo** | Resultado tangible de un proceso o procedimiento |
| **Política** | Declaración de intención sobre un compromiso de la organización |
| **Actividad** | Acciones para crear o producir un producto, servicio o resultado |
| **Rol** | Persona, grupo o sistema que ejecuta una actividad |

##### Objetivos, funciones, rol y responsabilidades de QA

- **Objetivos:** mejorar la calidad monitoreando producto y proceso; asegurar cumplimiento de estándares; elevar desviaciones a gerencia; asistir a los equipos; dar seguimiento a **no-conformidades**.
- **Funciones:** controlar adherencia, evaluar nivel de calidad, dar *coaching*.
- **Rol:** garantiza a la gerencia que el proceso definido está implementado y en uso.
- **Responsabilidades:** participa en **todas las fases del ciclo de vida**.

##### Planificación de QA y *checklists*

El **plan de QA** institucionaliza el aseguramiento de calidad. El **IEEE 730** es el estándar para los planes de QA.

**Características de un *checklist* eficiente:** conciso y preciso (≤ 1 página), completo, genérico y consistente.

##### Tabla de severidad de defectos

| Severidad | Efecto (Clase) | Prioridad de resolución |
|-----------|--------|------------------------|
| **MAYOR** | VERTICAL (impacta a otros productos) | **ALTA** |
| **MAYOR** | HORIZONTAL (no impacta a otros) | **MEDIA** |
| **MENOR** | VERTICAL | **MEDIA** |
| **MENOR** | HORIZONTAL | **BAJA** |

**Criterios:** **Omisión** (falta un elemento que debería estar), **Excedente** (elemento innecesario), **Incorrecto** (elemento que no es correcto).

##### Modelo de calidad del producto (ISO 9126 / 25010)

| Característica | Sub-características |
|---|---|
| **Funcionalidad** | Completitud, Corrección, Idoneidad |
| **Rendimiento** | Comportamiento en el tiempo, Utilización de recursos |
| **Usabilidad** | Inteligibilidad, Aprendizaje, Operabilidad, Protección a errores |
| **Fiabilidad** | Madurez, Disponibilidad, Tolerancia a fallos, Recuperabilidad |
| **Seguridad** | Confidencialidad, Integridad, No repudio, Autenticidad, Responsabilidad |
| **Mantenibilidad** | Modularidad, Reusabilidad, Analizabilidad, Cambiabilidad, Capacidad de ser probado |
| **Portabilidad** | Adaptabilidad, Facilidad de instalación, Intercambiabilidad |
| **Compatibilidad** | Coexistencia, Interoperabilidad |

##### Calidad interna, externa y en uso

- **Calidad interna:** medida en base a los requerimientos de calidad interna.
- **Calidad externa:** medida del producto desde una perspectiva externa.
- **Calidad en uso:** perspectiva del usuario al usar el producto en un contexto específico. Dimensiones: **Eficacia, Eficiencia, Satisfacción** (utilidad, confianza, placer, comodidad), **Libertad de riesgo** (económico, salud/seguridad, ambiental) y **Cobertura de contexto** (integridad, flexibilidad).

Ciclo entre requerimientos y producto: *Req. Calidad Interna → Implementación → Calidad Interna → (Verificación) → Calidad Externa → (Validación) → Calidad en Uso*.

##### Modelos y métricas de calidad

Un **modelo de calidad** es un documento que recolecta buenas prácticas para dirigir procesos clave y medir avances.

**Cronología (orientativa):**
- *Nivel de proceso:* ITIL, Bootstrap, PSP, TSP, Cobit, IEEE 12207, **CMMI**, ISO 20000, ISO 90003, SQAE, **ISO 25000**.
- *Nivel de producto:* **McCall** (~1970), Boehm, GILB, **ISO 9126**, FURPS, GQM, ISO 15504, Dromey, WebQEM.

**Jerarquía Factor → Criterio → Métrica:** cada **factor de calidad** se descompone en **criterios**, y cada criterio se mide con **métricas** concretas (base del modelo McCall e ISO 9126).

#### Módulo 03 — Certificación del software

##### Introducción

La **Certificación del Modelo de Madurez de la Ingeniería del Software** mejora la calidad del desarrollo. El esquema se basa en un **modelo de procesos del ciclo de vida** según la **ISO/IEC 12207** y el método de evaluación **ISO/IEC 33000**. Está alineado con **ISO 27001** e **ISO 25000**.

**Mapa de convivencia de normas en la organización:**

| Norma | Área |
|---|---|
| **ISO 22301** | Continuidad de negocio |
| **ISO 38500** | Gobierno de IT |
| **ISO 15504** | Evaluación/mejora/madurez de software |
| **ISO 19770** | Gestión de activos |
| **ISO 20000** | Gestión de servicios TI |
| **ISO 12207** | Ciclo de vida de desarrollo |
| **ISO 25000** | Calidad del producto software |
| **ISO 27001 / 27002** | Seguridad de la información / controles |

##### Beneficios de la certificación

Diferenciarse de competidores · establecer **acuerdos de nivel de servicio** · detectar defectos antes de la entrega (ahorro en mantenimiento) · evaluar y controlar el rendimiento · asegurar características de seguridad (confidencialidad, integridad, autenticidad, no-repudio) · puesta en producción sin comprometer otros sistemas.

##### Evaluación del producto: ISO 9126 e ISO 14598

**ISO 9126 (modelo de calidad del producto):**

| Norma | Contenido |
|---|---|
| 9126-1 | Modelo de calidad |
| 9126-2 | Métricas externas |
| 9126-3 | Métricas internas |
| 9126-4 | Métricas de calidad en uso |

**ISO 14598 (evaluación del producto):**

| Norma | Contenido |
|---|---|
| 14598-1 | Visión general |
| 14598-2 | Planeamiento y gestión |
| 14598-3 | Proceso para desarrolladores |
| 14598-4 | Proceso para compradores |
| 14598-5 | Proceso para evaluadores |
| 14598-6 | Documentación de los módulos de evaluación |

**Cuatro problemas de la serie ISO 14598:** formulación insuficiente del objetivo · relaciones implícitas entre actividades · no atención al balanceo objetivos/recursos · atención insuficiente a la retroalimentación.

##### ISO/IEC 15504 (evaluación de procesos)

Componentes: **Modelo de Referencia del Proceso** (dominio/alcance, propósito, resultados), **Marco de Medición** (niveles de capacidad, atributos del proceso, escala de valoración) y **Modelo de Evaluación**. El **Proceso de Evaluación** incluye: planificación, recogida de datos, validación, valoración de atributos y generación de informes. Roles: **Patrocinador, Evaluador Competente, Evaluador(es)**.

##### Familia ISO 9000 (Sistema de Gestión de la Calidad)

La serie evolucionó hasta la **ISO 9001:2015** (la 9001:2008 perdió vigencia en 2018). Estructura según ciclo **PHVA**:

| Fase PHVA | Capítulos |
|---|---|
| **PLANIFICAR** | Cap. 5 Liderazgo / Cap. 6 Planificación |
| **HACER** | Cap. 7 Apoyo / Cap. 8 Operación |
| **VERIFICAR** | Cap. 9 Evaluación del desempeño |
| **ACTUAR** | Cap. 10 Mejora |
| (transversal) | Cap. 4 Contexto de la organización |

| **ISO 9000** | **ISO 9001** | **ISO 9004** | **ISO 19011** |
|---|---|---|---|
| Fundamentos y vocabulario | Requisitos (**CERTIFICABLE**) | Recomendaciones para mejorar el desempeño | Auditorías de calidad y medioambiental |

##### Auditorías (norma 19011)

**Definiciones:** *Auditoría* (proceso sistemático, independiente y documentado para obtener y evaluar evidencia frente a criterios) · *Criterios* (políticas/procedimientos/requisitos de referencia) · *Evidencia* (registros verificables) · *Hallazgos* (resultado de evaluar evidencia contra criterios) · *Programa* (conjunto de auditorías planificadas) · *Plan* (descripción de actividades de una auditoría).

**Etapas:** Preparación → Plan de auditoría → Lista de verificación → Reunión de apertura → Realización (auditoría de **suficiencia** y de **cumplimiento**) → ¿RNC? (sí → levantar Registro de No Conformidad; no → acción correctiva) → Reunión de cierre → Reporte → Seguimiento → Levante NC → Fin.

##### Herramientas para la gestión de calidad (5 pasos)

| Paso | Objetivo | Herramienta |
|---|---|---|
| 1 | Identificar el problema | **Brainstorming** |
| 2 | Encontrar causas principales | **Ishikawa** / **5 Por qué** |
| 3 | Elegir la mejor solución | **Diagrama de Pareto** |
| 4 | Implementar | **5W2H** |
| 5 | Medir resultados | **PDCA** |

- **Brainstorming (6 pasos):** Problema → Descripción → Proponer ideas → Selección → Riesgos y beneficios → Plan de acción. Regla: ninguna idea es mala, no criticar, sin límite de ideas.
- **5 Por qué:** popularizado por **Toyota** (1970); ~5 iteraciones de "¿por qué?" hasta la **causa raíz**; es herramienta de apoyo, no de resolución.
- **5W2H:** What, Who, Where, When, Why, How, How much. Auxiliar del **PDCA** en la planificación.
- **Pareto (regla 80-20):** el 80% de los efectos provienen del 20% de las causas; prioriza las pocas causas vitales.
- **PDCA:** Plan (analizar, planear) → Do (instalar el cambio) → Check (verificar resultados) → Act (estandarizar y desplegar).
- **Ishikawa (causa-efecto):** categorías comunes 4P (servicios) y 6M (fabricación: Máquinas, Métodos, Materiales, Medio Ambiente, Mano de obra, Medición).

##### CMMI (Capability Maturity Model Integration)

**Dos representaciones:**

| Representación | Descripción |
|---|---|
| **Continua** | Selecciona áreas de proceso específicas; usa **Niveles de Capacidad**; perfil por área |
| **Escalonada (Staged)** | Secuencia predefinida; usa **Niveles de Madurez**; agrupa áreas en niveles |

**Componentes:** Objetivos/Prácticas **Específicos** y **Genéricos** (requeridos/esperados), más componentes **informativos**. Objetivos genéricos GG1→GG5 institucionalizan el proceso (gestionado, definido, cuantitativamente gestionado, optimizado).

**Cinco niveles de madurez (Staged):**

| Nivel | Nombre | Característica |
|---|---|---|
| **1** | Ejecutado (Inicial) | Impredecible, reactivo, débilmente controlado |
| **2** | Gerenciado | Caracterizado en proyectos; frecuentemente reactivo |
| **3** | Definido | Visión **organizacional** y proactivo |
| **4** | Cuantitativamente gerenciado | Medido y controlado |
| **5** | Optimizado | Foco en **mejora continua** |

- **Nivel 2:** CM, MA, PMC, PP, PPQA, REQM, SAM.
- **Nivel 3:** DAR, IPM, OPD, OPF, OT, PI, RD, RSKM, TS, VAL, VER.
- **Nivel 4:** OPP, QPM.
- **Nivel 5:** CAR, OPM.

**SCAMPI** es el método estándar de evaluación CMMI; usos: mejora interna del proceso, selección de suministrador y monitorización del proceso.

##### ISO/IEC 25000 (SQuaRE)

Familia *System and Software Quality Requirements and Evaluation*. Certificaciones: **ISO 25010** (calidad de producto software) e **ISO/IEC 25012** (calidad de datos).

| División | Código | Normas |
|---|---|---|
| Gestión de calidad | 2500n | 25000, 25001 |
| Modelo de calidad | 2501n | 25010, 25012 |
| Medición de calidad | 2502n | 25020, 25021, 25022, 25023, 25024 |
| Requisitos de calidad | 2503n | 25030 |
| Evaluación de calidad | 2504n | 25040, 25041, 25042, 25045 |

---

## Unidad 2 — Administración de Configuración (CM)

#### Módulo 04 — Administración de Configuración (CM)

##### ¿Qué es la Administración de Configuración?

Disciplina que provee **estabilidad** a la producción de software controlando la **evolución del producto** y los cambios asociados.

> **Definición IEEE:** "Disciplina dedicada a identificar y documentar las características físicas y funcionales de un ítem de configuración, controlar cambios a esas características, registrar y reportar cambios y estados de la configuración, y verificar el cumplimiento de los requerimientos."

Se fundamenta en la **identificación de los componentes** y los cambios que sufren, registrando **motivos, quién y cuándo**, y si se permite o no la **realización concurrente** de cambios.

La CM es el **centro** de todas las actividades del proyecto (administración de requerimientos, planificación, seguimiento, mediciones, administración de proveedores, revisión de productos, QA giran a su alrededor).

##### Proceso de CM — disciplina de 5 partes

| Actividad | Artefacto resultante |
|---|---|
| **1. Gestión / Planificación** | Plan de Configuración |
| **2. Identificación** | Ítems de Configuración |
| **3. Control de configuración y cambios** | Pedido de Cambio |
| **4. Estado de la configuración** | Reporte de Configuración |
| **5. Auditoría (revisión)** | Auditoría de Configuración |

**1. Planificación:** define **qué, quién, cómo, dónde y cuándo**. Capítulos: ítems de configuración + esquema de identificación, roles y responsabilidades, políticas, herramientas y proceso, base de la configuración.

**2. Identificación de la configuración:** identifica los **ítems de configuración** (planes, especificaciones, diseños, programas, datos de prueba — todo lo necesario para el mantenimiento futuro). Esquema de nombres **único** que refleje tipo de elemento, parte del sistema y creador, y las relaciones entre elementos.

**3. Control de configuración y cambios:** almacenamiento seguro y aprobación de la **línea base**; prevención de cambios innecesarios. Los procedimientos se ocupan del **análisis costo/beneficio**, la **aprobación** y el **registro** de los componentes a cambiar.

Flujo: *Requerimiento de negocio → Planteo de necesidad → Análisis del cambio (costo/beneficio, impacto, componentes afectados) → Priorización → Desarrollo y control → Implantación.*

El **Consejo de Control de Cambios (CCB)** revisa y aprueba todas las peticiones (salvo correcciones menores), considera el impacto **estratégico y organizacional** (no técnico) y decide si el cambio se **justifica económicamente**.

> En **métodos ágiles**, el cliente decide cuándo implementar un cambio; las mejoras quedan a discreción de los programadores y la **reconstrucción continua** es parte normal del proceso.

**4. Estado de la configuración:** registra y sigue problemas y solicitudes de cambio. La **base de datos de configuraciones** ayuda a evaluar impacto y da información de gestión. Responde consultas como: ¿a qué clientes se entregó una versión?, ¿qué versiones afecta cambiar un componente?, ¿cuántas peticiones/fallos pendientes hay?

**5. Auditorías de configuración:** confirman que líneas base y documentación se ajustan a un estándar.
- **FCA (Funcional):** verifica que el desarrollo del CI se completó y cumple las características funcionales y de calidad.
- **PCA (Física):** verifica que el CI *tal como fue construido* es conforme con la documentación técnica.
- **De gestión:** confirma que registros y CIs son completos, consistentes y precisos.

##### Gestión de versiones y entregas

- **Versión:** instancia que difiere de otras (funcionalidad, rendimiento, reparaciones).
- **Entrega (*release*):** versión que se distribuye a clientes. **Siempre hay más versiones que entregas.**

Técnicas para identificar ítems: **numeración de versiones**, **identificación basada en atributos**, **identificación orientada al cambio**. La línea base es jerárquica: Sistema → Programas → Módulos.

Una entrega incluye además del ejecutable: archivos de configuración, archivos de datos, programa de instalación, documentación y embalaje/publicidad. Debe quedar **documentada para poder reconstruirse exactamente** en el futuro.

##### Herramientas SCM y repositorio

Entornos **abiertos** (herramientas integradas según procedimientos) vs **integrados** (versiones + construcción + seguimiento de cambios). Una herramienta SCM debe proveer: editor de peticiones, flujo de trabajo, mecanismo de alertas, base de conocimiento de cambios y generador de reportes.

**Repositorio:** contenido **inmutable**; se **extrae (check-out)** a un directorio de trabajo, se modifica y se **reintroduce (check-in)**, creando automáticamente una nueva versión. Capacidades: identificación de versiones, **deltas** para almacenamiento, registro de historial, desarrollo en paralelo y control de extracción.

##### Pasaje entre ambientes

| Paso | Descripción |
|---|---|
| 1 | Ingresa el pedido de cambio; CM analiza el impacto y comunica al líder/desarrollador |
| 2 | Desarrollador hace **check-out**, realiza el cambio y el **test de unidad**, hace **check-in**; CM crea un **release de testing** |
| 3 | QA/Testing prueba y crea un **release aprobado** |
| 4 | Usuarios hacen **test de aceptación** → CM crea **release de aceptación de usuario** |
| 5 | CM etiqueta el **release de producción** |

---

## Unidad 3 — Fundamentos del testing

#### Módulo 05 — Fundamentos del testing

##### Definición (IEEE)

> "Es el proceso de operar un sistema o componente en determinadas condiciones, observando y registrando los resultados, elaborando una evaluación y devolución de los mismos."

Las pruebas **no pueden garantizar** ausencia de fallos, pero **minimizan el riesgo**. La prueba identifica defectos en el código y diferencias entre lo que el software debería hacer y lo que hace.

##### Verificación y Validación (V&V)

| Concepto | Pregunta clave |
|---|---|
| **Verificación** | "¿Estoy construyendo **correctamente** el producto?" |
| **Validación** | "¿Estoy construyendo el **producto correcto**?" |

**Objetivos de V&V:** (1) **descubrir defectos** (provocando fallas y revisando productos) y (2) **evaluar la calidad** del producto.

##### Error, Defecto y Falla

```
Error (humano) → Defecto (en el código) → Falla (observable al ejecutar) → Incidente
```

| Término | Descripción |
|---|---|
| **Error** | Equivocación humana que introduce un defecto |
| **Bug / Defecto** | Manifestación del error en el código o documentación |
| **Falla / Fallo** | Comportamiento incorrecto observable al ejecutar |
| **Incidente** | Evento que ocurre al ejecutarse una falla |

- **Identificación de defectos:** determinar qué defecto(s) causaron la falla.
- **Corrección de defectos:** cambiar el sistema para remover los defectos.

##### Axiomas del testing

1. El software siempre se testea.
2. Un **buen caso** es el que muestra que el programa **no anda**, no que funciona.
3. Lo más difícil es saber **cuándo dejar de testear**.
4. Uno **no puede autotestearse**.
5. Parte ineludible es determinar el **resultado esperado**.
6. El test debe ser **reproducible**.
7. Casos para **condiciones válidas e inválidas**.
8. **Inspeccionar todos los resultados**.
9. A medida que avanza el testeo, **crece la probabilidad de no encontrar errores**.
10. El **mejor programador** hace el testeo de **caja blanca**.
11. El **mejor usuario** hace el testeo de **caja negra**.
12. El testeo es **parte del proceso de desarrollo**.
13. Nunca se modifica un programa solo para hacerlo más fácil de testear.

##### El proceso de la prueba

Las pruebas siempre se realizan contra un **resultado esperado**, definido por el **oráculo**. Se compara *esperado* vs *obtenido*.

**Fases:** Planificación y preparación → Diseño de pruebas → Ejecución → Evaluación → Depuración → Análisis de errores (genera estadísticas, predicción de fiabilidad y acciones preventivas).

##### Modelos de ciclo de vida y testing

**Modelo V** — correspondencia desarrollo (izquierda) ↔ niveles de prueba (derecha):

| Fase de desarrollo | Nivel de prueba |
|---|---|
| Requisitos de usuario | **Pruebas de Aceptación de Usuario** |
| Especificación de requisitos SW | **Pruebas de Sistema / Rendimiento** |
| Diseño de alto nivel | **Pruebas de Integración** |
| Diseño de bajo nivel | **Pruebas Unitarias** |
| Codificación | (base del modelo) |

La **planificación de cada nivel** se hace en paralelo a la fase de desarrollo correspondiente. El **Equipo de Desarrollo** cubre el lado izquierdo; el **Equipo de QC**, el derecho.

- **Modelo Prototipo:** ciclo iterativo *escuchar → construir/revisar maqueta → el cliente prueba*.
- **Modelo Espiral:** testing en cada vuelta con alcance creciente.
- **Modelo Incremental:** cada incremento repite Análisis → Diseño → Codificación → Pruebas y entrega una versión.
- **RUP:** roles **Tester** (implementa/ejecuta/analiza) y **Test Analyst** (identifica ideas, define detalles, determina resultados); artefactos como Test Case, Test Log, Test Results.

---

## Unidad 4 — Técnicas de diseño de casos de test

#### Módulo 06 — Técnicas de diseño de casos de test

##### Definición de casos de prueba

Cada ensayo consume **tiempo y dinero** → encontrar la mayor cantidad de defectos en la **menor cantidad de ensayos**.
- **Condiciones de prueba:** situaciones ante las que el sistema debe responder.
- **Casos de prueba:** lotes de datos para que se dé una condición de prueba.

> La **prueba exhaustiva es imposible**. Se usa un **criterio de selección**: elegir la menor cantidad de casos con mayor probabilidad de encontrar un defecto nuevo.

##### Técnicas estáticas (analíticas)

Analizan el producto **sin ejecutarlo**: análisis de código fuente, análisis automatizado y análisis formal.

- **Recorridas (*walkthroughs*):** simulan la ejecución con pocas personas; foco en **detectar faltas, no corregirlas**. Roles: **Autor, Moderador, Secretario**.
- **Inspecciones:** examinan el producto buscando faltas comunes con una *check-list*. Roles: moderador, secretario, lector, inspector, autor. **Proceso:** Planeación → Visión de conjunto → Preparación individual → Reunión de inspección → Corrección → Seguimiento.
- **Análisis automatizado:** herramienta que recorre el código (análisis sintáctico) y detecta anomalías sin ejecutarlo; complementa al compilador.
- **Análisis formal:** demostrar que el programa cumple una **especificación formal**. Objeciones: demostración más larga que el programa, puede ser incorrecta, demasiada matemática.

##### Técnicas dinámicas (pruebas)

| | **Caja Negra (funcional)** | **Caja Blanca (estructural)** |
|---|---|---|
| Origen de casos | Requerimiento / especificación | Código (estructura interna) |
| Conocimiento interno | No se conoce | Se conoce |
| Qué prueba | Lo que el software **debería hacer** | Lo que el software **hace** |
| Riesgo | Porciones de código sin ejercitar | Puede soslayar requerimientos no implementados |

##### Técnicas de Caja Negra

**Partición / clase de equivalencia:** un valor representativo de la clase equivale a cualquier otro de ese subconjunto. Pasos: (1) identificar clases (válidas e inválidas), (2) definir casos.

| Tipo de condición | Clases válidas | Clases inválidas |
|---|---|---|
| Rango de valores | 1 | 2 (por debajo y por encima) |
| Valor específico | 1 | 2 |
| Conjunto de valores | 1 | 1 |
| "Lógica" | 1 | 1 |

**Análisis de valores límite (borde/frontera):** los casos que exploran los **bordes** son más efectivos. Se prueban los extremos de la clase y los valores justo por fuera. Para conjuntos/archivos: vacío, primer registro, último registro, fin de archivo.

**Conjetura de errores (*error guessing*):** "prueba de sospechas" basada en intuición/experiencia e historia de defectos (listas/valores vacíos, cero, negativos, valores significativos).

**Pairwise:** reduce la combinatoria buscando combinaciones equivalentes y probando solo algunas.

##### Estáticas vs Dinámicas

| Aspecto | Estáticas | Dinámicas |
|---|---|---|
| Detección | Temprana | Durante/después de construcción |
| Aplicabilidad | Cualquier producto (reqs, diseño, código…) | Solo software construido |
| Conclusiones | Validez general | Solo para los casos probados |
| V&V | Solo verificación | Verificación y validación |

##### Complejidad ciclomática (McCabe)

Métrica de la **complejidad lógica**; mide la cantidad de **caminos independientes**.

$$V(G) = A - N + 2$$ (A = aristas, N = nodos)
$$V(G) = P + 1$$ (P = nodos predicado/decisión)
$$V(G) = \text{número de regiones del grafo}$$

> **Interpretación:** $V(G)$ indica el **número mínimo de casos de prueba** para cubrir todos los caminos independientes (cobertura de caja blanca).

---

# 🅱️ SEGUNDO PARCIAL — Unidades 5 a 9

## Unidad 5 — Estrategias de testing

#### Módulo 07 — Estrategias de testing

##### Gestión de riesgos en testing

- **Riesgo:** problema que **todavía no ocurrió**. **Problema:** riesgo que se manifestó.
- Características de un riesgo: **sin certeza**, **con probabilidad**, **con pérdida/impacto**.
- **Análisis de riesgos:** ¿qué puede ir mal?, ¿cuál es el impacto?, ¿qué se puede hacer?

> La administración de riesgos no trata con decisiones futuras, sino con las **futuras consecuencias de decisiones actuales**.

**Plantilla de documentación de riesgos:** | Riesgo | Efectos que puede causar | Acciones preventivas | Acciones correctivas |. Ejemplos típicos: no tener un ambiente de testing separado, falta de tiempo, desarrollo no incremental ("catarata de errores"), no tener herramienta de seguimiento de errores.

##### Niveles de prueba

**Prueba de unidad:** prueba un módulo **aislado** (la hace quien lo construyó), basada en el **diseño detallado**. Cubre: interfaz, estructuras de datos locales, condiciones límite, caminos independientes y de manejo de errores.

Para probar un módulo aislado se usan:
- **Driver:** simula al módulo que **llama** al módulo bajo prueba; **suministra los casos**. Menos costoso.
- **Stub:** simula a los módulos **llamados** por el módulo bajo prueba (comportamiento simplificado). Más costoso. *Advertencia:* si devuelve siempre lo mismo, el módulo no se testea adecuadamente.

**Prueba de integración:** técnica sistemática para construir la estructura del programa probando las **interacciones** entre módulos.

##### Estrategias de integración

| Tipo | Estrategias |
|---|---|
| **No incremental** | Big-Bang |
| **Incrementales** | Bottom-Up, Top-Down, Sándwich, Por disponibilidad |

- **Big-Bang:** se prueba cada módulo aislado y luego **todos juntos de golpe**. Pro: paralelismo. Contras: necesita stubs y drivers para todos, **difícil ubicar el origen de la falla**, no apto para sistemas grandes.
- **Bottom-Up (ascendente):** empieza por los módulos de **más abajo**; requiere **Drivers**, no Stubs. Desventaja: el programa completo no existe hasta el último módulo.
- **Top-Down (descendente):** empieza por el módulo **superior**; requiere **Stubs**, no Drivers. Ventaja: demos tempranas (los superiores suelen ser GUI). Desventaja: stubs complejos.
- **Sándwich:** Top-Down y Bottom-Up a la vez; prueba primero los módulos **críticos**.
- **Por disponibilidad:** integra los módulos a medida que están disponibles.

**No incremental vs incremental:** el no incremental requiere más stubs/drivers, detecta más tarde los errores de interfaz, dificulta encontrar la falta y prueba menos los módulos.

##### Pruebas del sistema

Se prueba en un ambiente **similar al real**. Tipos:

| Tipo | Qué busca |
|---|---|
| **Estrés (esfuerzo)** | Picos de carga por encima de los límites en poco tiempo |
| **Volumen** | Grandes volúmenes de datos |
| **Facilidad de uso** | Problemas de usabilidad (mensajes claros, sin jerga) |
| **Seguridad** | Casos que burlan los controles de seguridad |
| **Rendimiento** | El sistema no cumple sus especificaciones de tiempo/respuesta bajo carga |
| **Configuración** | Distintas configuraciones de HW/SW (mínima y máxima) |
| **Compatibilidad** | Interfaces con otros sistemas |
| **Documentación** | Exactitud/claridad; ejemplos del manual como casos de prueba |
| **Instalación** | Distintas formas de instalar |
| **Recuperación** | Se simulan fallas y se prueba la recuperación |
| **Confiabilidad** | Requerimientos específicos de confiabilidad (a veces estimada con modelos matemáticos) |

> *Estrés ≠ Volumen:* analogía del dactilógrafo — **volumen** = escribir un documento enorme; **esfuerzo** = escribir a 50 palabras por minuto.

##### Prueba de aceptación del usuario

La realizan **los usuarios**; casos basados en la **especificación de requerimientos** (caja negra). Es la **última oportunidad real de testeo**. Pasos: rol del usuario → criterios de aceptación → plan de aceptación → ejecución → decisión de aceptación.

- **Prueba alfa:** la hace el **equipo de desarrollo** sobre el producto final.
- **Prueba beta:** la hacen **clientes elegidos**.
- **Pruebas piloto:** producción localizada, menor impacto de fallas.
- **Pruebas en paralelo:** sistema viejo y nuevo funcionando a la vez, comparando resultados.

##### Pruebas de regresión

Verifican que tras un **cambio en el código** la **funcionalidad original no se alteró** ("probar lo nuevo y lo viejo"). Es un **test funcional de integración** al final de un ciclo. Requiere casos **reusables**; se pueden correr todos o un subconjunto.

---

## Unidad 6 — Plan de testing

#### Módulo 08 — Plan de testing (Plan de prueba)

##### El Plan de Pruebas (IEEE 829)

La herramienta inicial de gestión es el **Plan**. La **IEEE** propone el estándar **IEEE 829** (Standard for Software Test Documentation) como directriz.

**Ítems de un plan de pruebas estándar:** alcance · limitaciones · estrategias · criterios de aceptación · roles · riesgos · definición del ambiente · equipamiento requerido · *backup* y control de fuentes · gestión de datos de prueba · ejecución y control de casos · seguimiento y corrección de errores · test de requerimientos no funcionales (performance, seguridad, stress) · políticas de datos de prueba.

##### Estimación de testing

Reglas: se basa en los **requisitos**, en la **opinión de expertos** y en **proyectos anteriores**; debe ser **registrada, justificada y verificada**.

**Fórmula general del tiempo:**
$$T = t_d + t_y \cdot n + t_i \cdot m + t_p \cdot n + t_a$$
donde $t_d$=diseño de casos, $t_y$=deployment, $t_i$=condiciones iniciales, $t_p$=prueba de casos, $t_a$=pre-aceptación, $n$=ciclos, $m$=rediseños del modelo de datos.

**Expresiones comunes:**
- Duración de testing = duración de desarrollo / 3
- Cantidad de testers = cantidad de desarrolladores / 2
- Costo de testing = duración × cantidad de testers × costo por día

##### Análisis de Puntos de Caso de Prueba (TCP)

Enfoque para **estimar proyectos de pruebas funcionales**; muy útil para **pruebas de regresión**. Los proyectos combinan **cuatro modelos de ejecución**:

| Modelo | Nombre | Entregable |
|---|---|---|
| I | Generación de caso de prueba | Casos de prueba |
| II | Generación automática de scripts | Scripts |
| III | Ejecución manual | Defectos informados |
| IV | Ejecución automatizada | Defectos informados |

**Proceso de 7 pasos:** (1) identificar casos de uso, (2) identificar casos de prueba, (3) TCP generación (TCP-G), (4) TCP automatización (TCP-A), (5) TCP ejecución manual (TCP-ME), (6) TCP ejecución automatizada (TCP-AE), (7) TCP total.

Cada caso se clasifica en **simple / promedio / complejo** y se aplica la fórmula base:

$$\text{Previo TCP} = (\#\text{simples} \times 6) + (\#\text{promedio} \times 8) + (\#\text{complejos} \times 12)$$
$$\text{TCP-G} = \text{Previo TCP-G} \times \text{Factor de ajuste}$$
$$\text{TCP-T} = \text{TCP-G} + \text{TCP-A} + \text{TCP-ME} + \text{TCP-AE}$$

El **TCP total** es indicativo del **tamaño del proyecto** de prueba.

**Factores de complejidad para automatización (TCP-A):** interfaz con otros casos · interfaz con app externa · validación de app externa · basado en datos · vínculos/enlaces · cálculos numéricos · puntos de verificación · verificación de BD · componentes de UI.

##### Ambiente de test

Actividades: definir **objetivos y alcance**, **creación**, **control** y **restauración** del ambiente. Equipamiento: puestos, servidores (casos de prueba, versiones, BD de prueba), backup. Personal: definir habilidades mínimas de cada rol.

##### Plan de mediciones

Métricas de testing: **complejidad, tamaño, defectos y del producto**. Para cada métrica: objetivo, indicador y representación, fuente de datos, método de análisis y **dueño**. Se visualizan en un **tablero de control**.

---

## Unidad 7 — Principios del testing ágil

#### Módulo 09 — Principios del testing ágil

##### El Manifiesto Ágil — 4 valores

1. **Individuos e interacciones** sobre procesos y herramientas.
2. **Software funcionando** sobre documentación extensiva.
3. **Colaboración con el cliente** sobre negociación contractual.
4. **Respuesta ante el cambio** sobre seguir un plan.

> "Aunque valoramos los elementos de la derecha, valoramos más los de la izquierda."

**Agilidad es calidad:** satisfacer las expectativas de los interesados priorizando lo que aporta más valor. Conceptos básicos: ciclos rápidos y despliegue frecuente, **diseño simple (YAGNI)**, **refactoring**, **programación por pares**, **retrospectiva**, conocimiento tácito, **desarrollo basado en test (TDD)**.

##### El tester ágil

Dentro del equipo ágil hay **comunicación continua** y **roles múltiples** (todos tienen habilidades de probadores). El probador ágil, además de lo técnico, sabe: integrar las necesidades del interesado, definir **historias de usuario** como requisitos, transformarlas en **parámetros de prueba** y **colaborar** con todo el equipo.

- Desaparece la **fase de pruebas separada**: codificación y pruebas son **un solo proceso integrado**.
- **Ágil no significa entregar rápido**: hay que entregar productos confiables.
- **Factor de éxito:** automatizar las **pruebas de regresión** y las **pruebas de aceptación**.

##### TDD, BDD y ATDD

**TDD (*Test-Driven Development*):** declarar el diseño como ejemplos ejecutables (pruebas) **antes** de programar.
Ciclo: (1) escribir pruebas → (2) verlas fallar → (3) escribir el **código mínimo** → (4) verlas pasar → (5) **refactoring**. Origen en *Extreme Programming*; genera como subproducto pruebas automatizadas con alta cobertura.

**BDD (*Behaviour-Driven Development*):** combina TDD con el diseño basado en el dominio; **no prueba la implementación, sino el comportamiento**.

**ATDD (*Acceptance Test-Driven Development*):** usuarios/PO, desarrolladores y testers analizan **juntos los criterios de aceptación antes** del desarrollo, creando escenarios/ejemplos que luego se convierten en **pruebas de aceptación automatizadas**.

##### Testing ágil vs QA tradicional

| **Agile testing** | **QA tradicional** |
|---|---|
| Testers integrados como miembros plenos del equipo (planifican, estiman) | Pruebas en grupo separado de QA |
| Trabajan mano a mano con desarrollo | Interacciones limitadas y establecidas |
| Requerimientos de a uno (para dar espacio al cambio) | Requerimientos por adelantado |
| Foco en **automatizar aceptación y regresión** | QA escribe plan y casos para soportar requerimientos |
| Feedback **continuo** en todas las etapas | Ciclo prueba-error: el bug vuelve al desarrollador |
| Calidad **siempre lista** para entregar | Corrección se repite hasta agotar el tiempo |

**Cascada (tradicional):** Requerimientos → Especificaciones → Codificación → Testing → Release (secuencial).
**Ágil (iterativo e incremental):** cada historia se expande, codifica y testea; posible *release* tras cada iteración; el sistema bajo prueba es un **blanco móvil**.

##### Testing exploratorio en ágil

Prueba **no guionada**, con misión definida y *timeboxed*; planificación, diseño y ejecución simultáneos en ciclos cortos. **No puede automatizarse.** Es invaluable en ágil porque responde rápido al cambio y necesita poca preparación (los sprints de 2–4 semanas no dan tiempo al testing guionado). Alternativa: **gestión de tests basada en sesiones** (James & Jon Bach).

##### El tester como consejero del equipo

El **respeto y la credibilidad** son críticos. El tester debe averiguar qué información valora el equipo. Formas de ganarse el respeto: aportar una **perspectiva fresca** de usuario, **aislar bien los bugs** (reproducción simple, grabar con herramientas tipo Selenium), y **hablar con los desarrolladores antes de cargar un reporte** (un defecto puede tener varios síntomas). El **tour de aplicación** ayuda a crear un modelo mental del software.

##### Evolución de la agilidad en el software (hitos)

| Año | Hito |
|---|---|
| 1924 | **Manufactura Lean** (automatización "con toque humano") |
| 1939 | **Mejora continua / PDCA** — Deming |
| 1948 | **Kanban** en Toyota |
| 1950 | Primer proyecto con metodología **iterativa e incremental** (X-15) |
| 1957–59 | **Fortran**, **Cobol** |
| 1970 | Primera definición del **modelo cascada** (Winston Royce) |
| 1985 | *Evolutionary Delivery vs Waterfall* (Tom Gilb) — primer registro del término **Scrum** |
| 1986 | Kent Beck y Ward Cunningham experimentan con **patrones** |
| 1993 | **Refactorización** de código (Bill Opdyke) |
| 1995 | **Scrum Development Process** (Sutherland y Schwaber); **Pair Programming**; DSDM |
| 1996 | **eXtreme Programming** (Kent Beck); **RUP** |
| 1997 | **ASD** (Highsmith); **FDD** (De Luca) |
| 1998 | **Crystal** (Cockburn) |
| **2001** | **Manifiesto Ágil** — 17 firmantes; **4 valores y 12 principios**; primer libro de Scrum |
| 2002 | **Planning Poker**; **Scrum Alliance**; **TDD** (Kent Beck) |
| 2003 | **Lean Software Development** (Poppendieck) |
| 2009–2010 | **Scrum.org**; primera **Guía Oficial de Scrum** |

---

## Unidad 8 — DevOps y testing web y mobile

#### Módulo 10 — DevOps y pruebas automáticas

##### ¿Qué es DevOps?

**Cultura** que promueve la **colaboración entre desarrollo y operaciones** para implementar código en producción más rápido, de forma **automatizada y repetible**. Se sitúa en la **intersección de Desarrollo, QA y Operaciones**.

**¿Por qué?** Antes, desarrollo y operaciones trabajaban en **aislamiento**; la implementación manual genera **errores humanos en producción**.

##### Beneficios

Previsibilidad · reproducibilidad (versiona todo) · mantenibilidad (recuperación sin esfuerzo) · **tiempo de comercialización (-50%)** · mayor calidad · riesgo reducido · resistencia (estado operativo estable) · rentabilidad · divide la base de código.

**¿Cuándo adoptarlo?** Grandes apps distribuidas (e-commerce, cloud). **¿Cuándo no?** Apps de misión crítica (bancos, energía) con controles de acceso estrictos.

##### Ciclo de vida DevOps

Fases: **Desarrollo → Prueba → Integración → Despliegue → Seguimiento → Flujo de trabajo** (continuo).

Flujo (ciclo infinito): *Plan → Construcción → Testing → Release → Despliegue → Operación → Monitoreo → …*

##### CI / CD

- **Integración Continua (CI):** los desarrolladores **fusionan su código en la rama central varias veces por día**; cada cambio dispara **pruebas rápidas**. Es el **primer paso** hacia la entrega continua.
- **Entrega Continua (Continuous Delivery):** mantener el producto **siempre en estado de entrega**; liberar versiones más barato y con menor riesgo.

> **Objetivo CI/CD:** hacer que liberar cambios sea un proceso rutinario y "aburrido", liberando tiempo del equipo para tareas de mayor valor.

##### 6 principios de DevOps

Acción centrada en el cliente · responsabilidad de extremo a extremo · mejora continua · **automatizar todo** · trabajar como un solo equipo · **monitorear y probar todo**.

##### DevSecOps

Integrar la **seguridad desde el principio** del ciclo DevOps (la seguridad como capa **envolvente**). Se apoya en la **automatización** (auditorías de seguridad automatizadas). Buenas prácticas: educar a empleados (el usuario es el eslabón más débil), gestionar control de acceso, evaluar riesgos y plan de contingencia/DR, auditorías periódicas. En cloud, rige el **modelo de responsabilidad compartida**.

##### Pruebas automáticas

Método de ejecutar una prueba **sin intervención humana**.

**Cuadrantes de Agile Testing** (negocio↔tecnología × ayuda a programar↔crítica al producto):

| Cuadrante | Pruebas | Automatización |
|---|---|---|
| **Q1** (tecnología / ayuda) | Unitarias, de componentes | **Automatizadas** |
| **Q2** (negocio / ayuda) | Funcionales, ejemplos, prototipos | Automatizadas/Manuales |
| **Q3** (negocio / crítica) | Exploratorias, usabilidad, aceptación de usuario | **Manuales** |
| **Q4** (tecnología / crítica) | Carga, rendimiento, seguridad | Herramientas especializadas |

**¿Por qué automatizar?** Principalmente por **tiempo**: libera al tester para pruebas más críticas.

**Atributos de una buena automatización:** conciso, comprobación computacional, repetible, robusto, necesario, despejado, eficiente, específico, independiente, sustentable, trazable.

**¿Qué automatizar?** Casos que corren en **cada versión** (sanity), **data driven**, funcionalidades críticas (smoke), funcionalidades estables, escenarios de performance. **Error común:** intentar automatizar **todo**.

**Desventajas:** no mide usabilidad, requiere conocimientos de programación, el **mantenimiento de scripts es costoso**.

**Automatizado vs Manual:**

| | Automatizado | Manual |
|---|---|---|
| **Ventaja** | Ideal para correr pruebas repetidas y de **regresión**; matrices y paralelismo | El tester **explora** y descubre combinaciones impensadas (más bugs) |
| **Desventaja** | Mayor esfuerzo inicial; difícil automatizar referencias visuales | Consume mucho tiempo; con muchos cambios baja la capacidad de hallar errores |

#### Módulo 11 — Testing web y mobile

##### Pruebas web

Una **web app** es una versión optimizada y adaptada a cualquier dispositivo/navegador. El proceso de pruebas va del **usuario** (capa exterior) a la **tecnología** (interior): diseño de interfaz, estético, de contenido, navegación, arquitectura y componentes.

**Tipos de prueba web:**

| Tipo | Qué evalúa |
|---|---|
| **Contenido** | Contenido correcto desde la perspectiva del usuario |
| **Interfaz de usuario** | Reglas de diseño, estética y contenido visual sin errores |
| **Funcional** | Características que afectan las interacciones; contenido dinámico, conexiones a BD |
| **Usabilidad** | Facilidad de uso e idoneidad del sitio |
| **Formularios** | Cada campo funciona y publica los datos previstos |
| **Navegación** | Buena navegación, especialmente en sitios complejos |
| **Base de datos** | Traducción correcta de solicitudes a SQL; integridad de datos |
| **Configuración/Compatibilidad** | Funciona en distintos entornos HW/SW y navegadores |
| **Rendimiento** | Escalabilidad, carga, estrés; transacción end-to-end; post-implementación |
| **Seguridad** | Amenazas de Internet; protección de información confidencial |

##### Pruebas mobile

Una **aplicación móvil** se ejecuta en dispositivos móviles y considera la **información contextual**. Clasificación: **independientes** (no interactúan con sistemas externos) y **empresariales** (transacciones que consumen recursos, vía WAP/HTTP).

**¿Por qué es diferente del testing tradicional?**
- Múltiples plataformas (**Android, iOS**) vs una sola dominante (Windows).
- **Fragmentación de Android** (muchas versiones del SO).
- Gran variedad de **factores de forma** (tamaños de pantalla).
- **Lanzamientos frecuentes** → ciclos de prueba continuos.
- Entorno único: cambios de red, alertas/notificaciones, pantalla táctil, batería.
- → **alto costo** de probar apps móviles.

**Preguntas clave:** duración de batería, comportamiento con red limitada/nula, velocidad, consumo de datos, periféricos.

**Emulador vs dispositivo real:**

| Aspecto | Emulador/Simulador | Dispositivo real |
|---|---|---|
| Costo | Más fácil y barato | Complejo y costoso |
| Uso | Etapa inicial, features independientes del dispositivo | **Validar resultados** (más preciso y confiable) |

**Fragmentación de redes:** actualización de **2G→3G→4G/LTE** e **IPv4→IPv6**.

**Métodos de prueba móvil:** unitaria, integración, sistema (caja negra), **regresión** (a menudo automatizada), compatibilidad, rendimiento, funcional, caja blanca, GUI (coherencia entre dispositivos).

**Estrategia (resumen):** las pruebas estándar siempre se consideran; la **compatibilidad GUI** se incorpora por los desafíos de dispositivos. Elementos clave: selección de **dispositivos objetivo** (combinar simulador + físico), **automatización**, **entorno de red** (Wi-Fi + simulación celular) y **tipos de prueba** (funcional, rendimiento, seguridad, cumplimiento).

**Herramientas de automatización móvil:** basadas en **objetos** (mapean elementos de pantalla a objetos) o en **imágenes/coordenadas** (bitmap).

---

## Unidad 9 — Estándares Testing (ISO 29119 + TMMi)

> [!warning] Material faltante en el vault
> Los apuntes del **Módulo 12 (Estándares testing / ISO 29119)** y **Módulo 13 (TMMi)** **no están** en `MaterialCursada/`. Lo que sigue es un **repaso general de referencia** (no proviene del apunte oficial) para no dejar la unidad vacía. **Conviene reemplazarlo por el apunte cuando esté disponible.**

#### Módulo 12 — Estándares testing · ISO/IEC/IEEE 29119

- **ISO/IEC/IEEE 29119** es la familia de estándares internacionales de **pruebas de software**, pensada para reemplazar/unificar estándares previos (como IEEE 829, IEEE 1008, ISO 29119 propiamente).
- **Partes de la familia:**
  - **29119-1** — Conceptos y definiciones.
  - **29119-2** — Procesos de prueba (organizacional, de gestión y dinámicos).
  - **29119-3** — Documentación de prueba (plantillas).
  - **29119-4** — Técnicas de prueba.
  - **29119-5** — Pruebas dirigidas por palabras clave (*keyword-driven testing*).
- Define un **modelo de procesos de testing en tres capas**: procesos de la **organización**, de **gestión** de pruebas y **dinámicos** (diseño, implementación, ejecución, reporte de incidentes).

#### Módulo 13 — TMMi (modelos de madurez del testing)

- **TMMi (Test Maturity Model integration):** modelo de madurez **específico del proceso de pruebas**, complementario y alineado con **CMMI**. Como CMMI, define **5 niveles de madurez**:
  1. **Inicial** — pruebas caóticas, sin proceso definido.
  2. **Gestionado (Managed)** — política y estrategia de pruebas, planificación, monitoreo y control, diseño y ejecución, entorno de pruebas.
  3. **Definido (Defined)** — organización de pruebas, programa de formación, ciclo de vida e integración, pruebas no funcionales, revisiones por pares.
  4. **Medido (Measured)** — medición de pruebas, evaluación de calidad del producto, **pruebas avanzadas/peer reviews**.
  5. **Optimización (Optimization)** — prevención de defectos, control de calidad, optimización del proceso de pruebas.
- **CSTE (*Certified Software Tester*):** certificación **personal/profesional** del tester (a diferencia de TMMi, que evalúa la **organización**).
- **Idea de examen (Módulo 13):** distinguir **modelos de madurez de la organización** (TMMi, CMMI) vs **certificaciones de personas** (CSTE) — "Modelo de madurez vs Certificaciones de testing".

---

# 🧠 Repaso rápido (cheat-sheet)

> [!example] Conceptos must-know
> - **Costo de la no calidad:** corregir en explotación cuesta hasta **40–1000×** más que en requisitos.
> - **QA vs QC:** QA = procesos (preventivo); QC = producto (detección). **Verificación** = ¿bien construido? · **Validación** = ¿producto correcto?
> - **Error → Defecto → Falla → Incidente.**
> - **Caja negra** (requisitos, "qué debería hacer") vs **caja blanca** (código, "qué hace").
> - **Partición de equivalencia + valores límite** = las dos técnicas estrella de caja negra.
> - **Complejidad ciclomática:** $V(G)=A-N+2=P+1=$ nº de regiones = nº mínimo de casos para cubrir caminos.
> - **Driver** (simula al que llama, da los datos) vs **Stub** (simula al llamado).
> - **Integración:** Big-Bang (no incremental) · Bottom-Up (drivers) · Top-Down (stubs) · Sándwich.
> - **Modelo V:** cada fase de desarrollo ↔ un nivel de prueba (unidad, integración, sistema, aceptación).
> - **IEEE 829** = documentación de pruebas; **IEEE 730** = plan de QA.
> - **TCP:** simple×6 + promedio×8 + complejo×12; TCP-T = TCP-G + TCP-A + TCP-ME + TCP-AE.
> - **Manifiesto Ágil:** 4 valores, 12 principios, 2001, 17 firmantes.
> - **TDD:** red → green → refactor. **BDD:** prueba comportamiento. **ATDD:** criterios de aceptación antes de codificar.
> - **Cuadrantes ágiles:** Q1 unitarias (auto), Q2 funcionales, Q3 exploratorias (manual), Q4 rendimiento/seguridad.
> - **CI:** merge a la rama central varias veces/día con pruebas rápidas → **primer paso** del CD.
> - **DevOps** = Dev + Ops (cultura, automatización). **DevSecOps** = + seguridad desde el inicio.
> - **Mobile:** emulador para etapa inicial, **dispositivo real para validar**. Fragmentación Android = gran desafío.
> - **CMMI / TMMi:** 5 niveles (1 Inicial → 5 Optimizado). **TMMi** = madurez del *proceso de pruebas*; **CSTE** = certificación de *personas*.
> - **ISO clave:** 9001 (certificable), 9126/25010 (modelo de calidad de producto), 14598 (evaluación), 25000 (SQuaRE), **29119** (estándar de testing).

## Material fuente

- [[MaterialCursada/0193_HDR_261Q_v1-4.pdf|Hoja de Ruta (estructura del curso)]]
- **U1:** [[MaterialCursada/Atributos de Calidad del Software ok.pdf|Atributos de calidad]] · [[MaterialCursada/Calidad del Software.pdf|Calidad del software]] · [[MaterialCursada/Proceso de certificación.pdf|Certificación]]
- **U2:** [[MaterialCursada/0193_APU_Administracion_conf_182Q_v1-2.pdf|Administración de configuración]]
- **U3:** [[MaterialCursada/0193_APU_Fundamentos_testing_182Q_v1-2.pdf|Fundamentos del testing]]
- **U4:** [[MaterialCursada/0193_APU_Diseño_casos_test_182Q_v1-2.pdf|Diseño de casos de test]]
- **U5:** [[MaterialCursada/0193_APU_Estrategia_testing_182Q_v1-2.pdf|Estrategia de testing]]
- **U6:** [[MaterialCursada/0193_APU_Plan_prueba_182Q_v1-2.pdf|Plan de prueba]]
- **U7:** [[MaterialCursada/0193_APU_Principios_testing_182Q_v1-2.pdf|Principios del testing ágil]] · [[MaterialCursada/0193_APU_Evolucion_182Q_v1-2ok.pdf|Evolución de la agilidad]]
- **U8:** [[MaterialCursada/DevOps.pdf|DevOps]] · [[MaterialCursada/0193_APU_Testing_web_182Q_v1-2.pdf|Testing web y mobile]]
- **U9:** *(falta apunte de Estándares 29119 y TMMi)*
