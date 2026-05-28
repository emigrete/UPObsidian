---
title: "Preguntas Integradoras — Testing y QA (2do Parcial)"
tags:
  - testing-qa
  - preguntas
  - 2do-parcial
aliases:
  - "Preguntas Testing 2do Parcial"
---

# 🧩 Preguntas Integradoras — Testing y QA (2do Parcial)

> [!tip] Cómo usar esta página
> Las respuestas están **plegadas**. Leé la pregunta, intentá responderla vos, y recién después abrí el callout (click en el ▸ a la izquierda de "Ver respuesta"). Entra Unidad 5 a 9.

## 🎯 Unidad 5 — Estrategias de testing

**1.** Diferenciá los conceptos de **riesgo** y **problema** en el contexto del testing, y enumerá las tres características que tiene todo riesgo.
> [!success]- Ver respuesta
> Un **riesgo** es un problema que **todavía no ocurrió**, mientras que un **problema** es un riesgo que **ya se manifestó**.
> 
> Las tres características de un riesgo son:
> 
> - Es **sin certeza** (no se sabe si va a ocurrir).
> - Tiene una **probabilidad** asociada.
> - Conlleva una **pérdida o impacto**.
> 
> El análisis de riesgos responde a: ¿qué puede ir mal?, ¿cuál es el impacto? y ¿qué se puede hacer? La administración de riesgos no trata con decisiones futuras, sino con las **futuras consecuencias de decisiones actuales**.

**2.** ¿Qué es una **prueba de unidad** y en qué documento se basa? ¿Qué aspectos del módulo cubre?
> [!success]- Ver respuesta
> La prueba de unidad prueba un **módulo aislado** y la realiza **quien lo construyó**. Se basa en el **diseño detallado**.
> 
> Cubre:
> 
> - La **interfaz** del módulo.
> - Las **estructuras de datos locales**.
> - Las **condiciones límite**.
> - Los **caminos independientes**.
> - Los caminos de **manejo de errores**.

**3.** Explicá la diferencia entre un **driver** y un **stub**. ¿Cuál es más costoso y qué advertencia hay sobre los stubs?
> [!success]- Ver respuesta
> Ambos sirven para probar un módulo de forma aislada:
> 
> - **Driver:** simula al módulo que **llama** al módulo bajo prueba y le **suministra los casos**. Es **menos costoso**.
> - **Stub:** simula a los módulos **llamados** por el módulo bajo prueba (con un comportamiento simplificado). Es **más costoso**.
> 
> **Advertencia sobre los stubs:** si un stub devuelve siempre lo mismo, el módulo no se testea adecuadamente.

**4.** Enumerá las estrategias de integración, clasificándolas en no incrementales e incrementales, e indicá qué necesita cada una.
> [!success]- Ver respuesta
> - **No incremental:** **Big-Bang** — se prueba cada módulo aislado y luego todos juntos de golpe. Pro: paralelismo. Contras: necesita stubs y drivers para todos, es difícil ubicar el origen de la falla y no es apto para sistemas grandes.
> - **Incrementales:**
>     - **Bottom-Up (ascendente):** empieza por los módulos de más abajo; requiere **Drivers**, no Stubs. Desventaja: el programa completo no existe hasta el último módulo.
>     - **Top-Down (descendente):** empieza por el módulo superior; requiere **Stubs**, no Drivers. Ventaja: demos tempranas (los superiores suelen ser GUI). Desventaja: stubs complejos.
>     - **Sándwich:** Top-Down y Bottom-Up a la vez; prueba primero los módulos **críticos**.
>     - **Por disponibilidad:** integra los módulos a medida que están disponibles.

**5.** ¿Qué desventajas tiene la integración **no incremental** frente a la incremental?
> [!success]- Ver respuesta
> La integración no incremental, comparada con la incremental:
> 
> - Requiere **más stubs y drivers**.
> - Detecta **más tarde** los errores de interfaz.
> - **Dificulta** encontrar la falta (ubicar el origen del error).
> - **Prueba menos** los módulos.

**6.** Describí cinco tipos de pruebas del sistema y distinguí entre prueba de **estrés** y prueba de **volumen**.
> [!success]- Ver respuesta
> Las pruebas del sistema se hacen en un ambiente **similar al real**. Algunos tipos:
> 
> - **Estrés (esfuerzo):** picos de carga por encima de los límites en poco tiempo.
> - **Volumen:** grandes volúmenes de datos.
> - **Facilidad de uso:** problemas de usabilidad (mensajes claros, sin jerga).
> - **Seguridad:** casos que burlan los controles de seguridad.
> - **Rendimiento:** verifica si el sistema cumple sus especificaciones de tiempo/respuesta bajo carga.
> - Otras: configuración, compatibilidad, documentación, instalación, recuperación, confiabilidad.
> 
> **Estrés ≠ Volumen** (analogía del dactilógrafo): **volumen** = escribir un documento enorme; **esfuerzo (estrés)** = escribir a 50 palabras por minuto.

**7.** ¿Quién realiza la **prueba de aceptación del usuario**, en qué se basan sus casos y por qué es relevante? Diferenciá prueba **alfa** y **beta**.
> [!success]- Ver respuesta
> La realizan **los usuarios**; sus casos se basan en la **especificación de requerimientos** (es de caja negra). Es la **última oportunidad real de testeo**.
> 
> Pasos: rol del usuario → criterios de aceptación → plan de aceptación → ejecución → decisión de aceptación.
> 
> - **Prueba alfa:** la hace el **equipo de desarrollo** sobre el producto final.
> - **Prueba beta:** la hacen **clientes elegidos**.
> 
> También existen las **pruebas piloto** (producción localizada, menor impacto de fallas) y las **pruebas en paralelo** (sistema viejo y nuevo funcionando a la vez, comparando resultados).

**8.** ¿Qué son las **pruebas de regresión** y cómo se relacionan con las pruebas de integración? ¿Qué requieren para poder ejecutarse?
> [!success]- Ver respuesta
> Las pruebas de regresión verifican que, tras un **cambio en el código**, la **funcionalidad original no se alteró** ("probar lo nuevo y lo viejo").
> 
> Son un **test funcional de integración** que se hace al final de un ciclo. Requieren casos **reusables**; se pueden correr todos o un subconjunto.

**9.** (Integradora) Una prueba de **unidad** y una prueba de **integración** atacan problemas distintos. Explicá qué prueba cada nivel y por qué la integración no se reduce a "sumar" las unidades ya probadas.
> [!success]- Ver respuesta
> - La **prueba de unidad** prueba un **módulo aislado** según su diseño detallado: interfaz, estructuras de datos locales, condiciones límite, caminos independientes y de manejo de errores.
> - La **prueba de integración** es una técnica sistemática para construir la estructura del programa probando las **interacciones entre módulos**.
> 
> Aunque cada módulo funcione bien por separado, los **errores de interfaz** entre ellos solo aparecen cuando interactúan. Por eso la integración no es sumar unidades: prueba precisamente lo que las pruebas unitarias (aisladas, con drivers y stubs) no pueden cubrir. De hecho, una crítica al enfoque no incremental (Big-Bang) es justamente que detecta tarde los errores de interfaz y dificulta hallar la falta.

## 📋 Unidad 6 — Plan de testing

**1.** ¿Cuál es la herramienta inicial de gestión del testing y qué estándar IEEE la regula?
> [!success]- Ver respuesta
> La herramienta inicial de gestión es el **Plan** (de pruebas). La **IEEE** propone como directriz el estándar **IEEE 829** (*Standard for Software Test Documentation*).

**2.** Enumerá al menos ocho ítems que debe contener un plan de pruebas estándar.
> [!success]- Ver respuesta
> Un plan de pruebas estándar incluye, entre otros:
> 
> - Alcance
> - Limitaciones
> - Estrategias
> - Criterios de aceptación
> - Roles
> - Riesgos
> - Definición del ambiente y equipamiento requerido
> - *Backup* y control de fuentes
> - Gestión de datos de prueba
> - Ejecución y control de casos
> - Seguimiento y corrección de errores
> - Test de requerimientos no funcionales (performance, seguridad, stress)
> - Políticas de datos de prueba

**3.** ¿En qué se basa la estimación de testing y qué condiciones debe cumplir? Mencioná las expresiones comunes para duración, cantidad de testers y costo.
> [!success]- Ver respuesta
> La estimación se basa en los **requisitos**, en la **opinión de expertos** y en **proyectos anteriores**; además debe ser **registrada, justificada y verificada**.
> 
> Expresiones comunes:
> 
> - Duración de testing = duración de desarrollo / 3
> - Cantidad de testers = cantidad de desarrolladores / 2
> - Costo de testing = duración × cantidad de testers × costo por día

**4.** Escribí la fórmula general del tiempo de testing y explicá qué representa cada término.
> [!success]- Ver respuesta
> La fórmula general del tiempo es:
> 
> $$T = t_d + t_y \cdot n + t_i \cdot m + t_p \cdot n + t_a$$
> 
> Donde:
> 
> - $t_d$ = diseño de casos
> - $t_y$ = deployment
> - $t_i$ = condiciones iniciales
> - $t_p$ = prueba de casos
> - $t_a$ = pre-aceptación
> - $n$ = cantidad de ciclos
> - $m$ = cantidad de rediseños del modelo de datos

**5.** ¿Qué es el análisis de **Puntos de Caso de Prueba (TCP)** y para qué se usa? Indicá los cuatro modelos de ejecución que combinan los proyectos.
> [!success]- Ver respuesta
> El TCP es un enfoque para **estimar proyectos de pruebas funcionales**, muy útil para **pruebas de regresión**. El **TCP total** es indicativo del **tamaño del proyecto** de prueba.
> 
> Los cuatro modelos de ejecución que combinan los proyectos son:
> 
> - **Modelo I — Generación de caso de prueba** → entregable: casos de prueba.
> - **Modelo II — Generación automática de scripts** → entregable: scripts.
> - **Modelo III — Ejecución manual** → entregable: defectos informados.
> - **Modelo IV — Ejecución automatizada** → entregable: defectos informados.

**6.** Explicá cómo se clasifican los casos en el TCP y escribí las fórmulas base (Previo TCP, TCP-G y TCP-T).
> [!success]- Ver respuesta
> Cada caso se clasifica en **simple / promedio / complejo**. Las fórmulas son:
> 
> $$\text{Previo TCP} = (\#\text{simples} \times 6) + (\#\text{promedio} \times 8) + (\#\text{complejos} \times 12)$$
> 
> $$\text{TCP-G} = \text{Previo TCP-G} \times \text{Factor de ajuste}$$
> 
> $$\text{TCP-T} = \text{TCP-G} + \text{TCP-A} + \text{TCP-ME} + \text{TCP-AE}$$
> 
> El proceso completo es de **7 pasos**: (1) identificar casos de uso, (2) identificar casos de prueba, (3) TCP generación (TCP-G), (4) TCP automatización (TCP-A), (5) TCP ejecución manual (TCP-ME), (6) TCP ejecución automatizada (TCP-AE), (7) TCP total.

**7.** Mencioná algunos factores de complejidad que se consideran para la automatización (TCP-A).
> [!success]- Ver respuesta
> Los factores de complejidad para automatización (TCP-A) incluyen:
> 
> - Interfaz con otros casos
> - Interfaz con app externa
> - Validación de app externa
> - Basado en datos
> - Vínculos/enlaces
> - Cálculos numéricos
> - Puntos de verificación
> - Verificación de BD
> - Componentes de UI

**8.** ¿Cuáles son las actividades del **ambiente de test** y qué contempla el **plan de mediciones**?
> [!success]- Ver respuesta
> **Ambiente de test** — actividades: definir **objetivos y alcance**, **creación**, **control** y **restauración** del ambiente. Equipamiento: puestos, servidores (casos de prueba, versiones, BD de prueba) y backup. Personal: definir las habilidades mínimas de cada rol.
> 
> **Plan de mediciones:** las métricas de testing son de **complejidad, tamaño, defectos y del producto**. Para cada métrica se define: objetivo, indicador y representación, fuente de datos, método de análisis y **dueño**. Se visualizan en un **tablero de control**.

**9.** (Integradora) En un plan de testing (U6) se documentan "estrategias" y "riesgos". Conectá esto con la Unidad 5: ¿cómo se plasma una estrategia de testing dentro del plan y qué se documenta sobre los riesgos?
> [!success]- Ver respuesta
> El plan de pruebas (IEEE 829) incluye explícitamente entre sus ítems las **estrategias** y los **riesgos**, además del alcance, criterios de aceptación, definición del ambiente, etc.
> 
> - Las **estrategias de testing** de la U5 (niveles de prueba, estrategias de integración como Bottom-Up/Top-Down/Sándwich, pruebas de sistema, aceptación y regresión) se vuelcan en el plan como la sección de "estrategias", definiendo cómo y en qué niveles se va a probar.
> - Sobre los **riesgos**, el plan recoge el análisis de la U5 usando la **plantilla de documentación de riesgos** (| Riesgo | Efectos que puede causar | Acciones preventivas | Acciones correctivas |). Ejemplos típicos: no tener un ambiente de testing separado, falta de tiempo, desarrollo no incremental ("catarata de errores") o no tener herramienta de seguimiento de errores.
> 
> Así, el plan (U6) es el documento donde se materializan y formalizan las decisiones de estrategia y gestión de riesgos planteadas en la U5.

## 🌀 Unidad 7 — Principios del testing ágil

**1.** Enunciá los **4 valores del Manifiesto Ágil** y la aclaración que los acompaña.
> [!success]- Ver respuesta
> Los 4 valores son:
> 
> 1. **Individuos e interacciones** sobre procesos y herramientas.
> 2. **Software funcionando** sobre documentación extensiva.
> 3. **Colaboración con el cliente** sobre negociación contractual.
> 4. **Respuesta ante el cambio** sobre seguir un plan.
> 
> Aclaración: "Aunque valoramos los elementos de la derecha, **valoramos más los de la izquierda**".

**2.** ¿Qué significa que "agilidad es calidad" y cuáles son algunos conceptos básicos del enfoque ágil?
> [!success]- Ver respuesta
> "Agilidad es calidad" significa **satisfacer las expectativas de los interesados priorizando lo que aporta más valor**.
> 
> Conceptos básicos:
> 
> - Ciclos rápidos y despliegue frecuente
> - **Diseño simple (YAGNI)**
> - **Refactoring**
> - **Programación por pares**
> - **Retrospectiva**
> - Conocimiento tácito
> - **Desarrollo basado en test (TDD)**

**3.** ¿Qué características tiene el **tester ágil** y qué cambios introduce respecto de una fase de pruebas tradicional?
> [!success]- Ver respuesta
> Dentro del equipo ágil hay **comunicación continua** y **roles múltiples** (todos tienen habilidades de probadores). El tester ágil, además de lo técnico, sabe: integrar las necesidades del interesado, definir **historias de usuario** como requisitos, transformarlas en **parámetros de prueba** y **colaborar** con todo el equipo.
> 
> Cambios clave:
> 
> - **Desaparece la fase de pruebas separada**: codificación y pruebas son **un solo proceso integrado**.
> - **Ágil no significa entregar rápido**: hay que entregar productos confiables.
> - **Factor de éxito:** automatizar las **pruebas de regresión** y las **pruebas de aceptación**.

**4.** Explicá el ciclo de **TDD** y diferenciá TDD, BDD y ATDD.
> [!success]- Ver respuesta
> **TDD (*Test-Driven Development*):** declarar el diseño como ejemplos ejecutables (pruebas) **antes** de programar. Ciclo:
> 
> 1. Escribir pruebas.
> 2. Verlas fallar.
> 3. Escribir el **código mínimo**.
> 4. Verlas pasar.
> 5. **Refactoring**.
> 
> Tiene origen en *Extreme Programming* y genera como subproducto pruebas automatizadas con alta cobertura.
> 
> - **BDD (*Behaviour-Driven Development*):** combina TDD con el diseño basado en el dominio; **no prueba la implementación, sino el comportamiento**.
> - **ATDD (*Acceptance Test-Driven Development*):** usuarios/PO, desarrolladores y testers analizan **juntos los criterios de aceptación antes** del desarrollo, creando escenarios/ejemplos que luego se convierten en **pruebas de aceptación automatizadas**.

**5.** Compará el **testing ágil** con el **QA tradicional** en al menos cuatro aspectos.
> [!success]- Ver respuesta
> | Agile testing | QA tradicional |
> |---|---|
> | Testers integrados como miembros plenos del equipo (planifican, estiman) | Pruebas en un grupo separado de QA |
> | Trabajan mano a mano con desarrollo | Interacciones limitadas y establecidas |
> | Requerimientos de a uno (para dar espacio al cambio) | Requerimientos por adelantado |
> | Foco en **automatizar aceptación y regresión** | QA escribe plan y casos para soportar requerimientos |
> | Feedback **continuo** en todas las etapas | Ciclo prueba-error: el bug vuelve al desarrollador |
> | Calidad **siempre lista** para entregar | La corrección se repite hasta agotar el tiempo |
> 
> En **cascada** el flujo es secuencial (Requerimientos → Especificaciones → Codificación → Testing → Release); en **ágil** cada historia se expande, codifica y testea, con posible *release* tras cada iteración, y el sistema bajo prueba es un **blanco móvil**.

**6.** ¿Qué es el **testing exploratorio** y por qué es valioso en ágil? ¿Se puede automatizar?
> [!success]- Ver respuesta
> El testing exploratorio es una prueba **no guionada**, con misión definida y *timeboxed*; la planificación, el diseño y la ejecución son **simultáneos** en ciclos cortos. **No puede automatizarse.**
> 
> Es invaluable en ágil porque responde rápido al cambio y necesita poca preparación: los sprints de 2 a 4 semanas no dan tiempo al testing guionado. Una alternativa es la **gestión de tests basada en sesiones** (James & Jon Bach).

**7.** ¿Por qué el respeto y la credibilidad son críticos para el tester y cómo se los gana dentro del equipo?
> [!success]- Ver respuesta
> El **respeto y la credibilidad** son críticos; el tester actúa como consejero del equipo y debe averiguar qué información valora el equipo. Formas de ganarse el respeto:
> 
> - Aportar una **perspectiva fresca** de usuario.
> - **Aislar bien los bugs** (reproducción simple, grabar con herramientas tipo Selenium).
> - **Hablar con los desarrolladores antes de cargar un reporte** (un defecto puede tener varios síntomas).
> 
> El **tour de aplicación** ayuda a crear un modelo mental del software.

**8.** Mencioná al menos cinco hitos de la **evolución de la agilidad** y por qué 2001 es clave.
> [!success]- Ver respuesta
> Algunos hitos:
> 
> - **1924:** Manufactura **Lean** (automatización "con toque humano").
> - **1939:** Mejora continua / **PDCA** (Deming).
> - **1948:** **Kanban** en Toyota.
> - **1970:** primera definición del **modelo cascada** (Winston Royce).
> - **1995:** **Scrum Development Process** (Sutherland y Schwaber); **Pair Programming**; DSDM.
> - **1996:** **eXtreme Programming** (Kent Beck); RUP.
> - **2002:** **Planning Poker**, Scrum Alliance y **TDD** (Kent Beck).
> 
> **2001 es clave** porque se publica el **Manifiesto Ágil** (17 firmantes), con sus **4 valores y 12 principios**, y aparece el primer libro de Scrum.

**9.** (Integradora) Conectá la prueba de aceptación del usuario de la U5 con ATDD (U7): ¿qué cambia en quién, cuándo y cómo se definen y ejecutan esos criterios?
> [!success]- Ver respuesta
> En el enfoque tradicional (U5), la **prueba de aceptación** la realizan **los usuarios** al final, basándose en la especificación de requerimientos (caja negra), y es la "última oportunidad real de testeo".
> 
> Con **ATDD** (U7) el momento y los actores cambian:
> 
> - **Quién:** usuarios/PO, desarrolladores y testers trabajan **juntos**.
> - **Cuándo:** los criterios de aceptación se analizan **antes** del desarrollo (no al final).
> - **Cómo:** crean escenarios/ejemplos que luego se convierten en **pruebas de aceptación automatizadas**.
> 
> Además, en ágil **desaparece la fase de pruebas separada** y la automatización de las pruebas de aceptación (y de regresión) es un **factor de éxito**, mientras que en el modelo tradicional la aceptación queda como una etapa final a cargo del usuario.

## ⚙️ Unidad 8 — DevOps y testing web y mobile

**1.** Definí **DevOps**, indicá dónde se sitúa y por qué surgió.
> [!success]- Ver respuesta
> **DevOps** es una **cultura** que promueve la **colaboración entre desarrollo y operaciones** para implementar código en producción más rápido, de forma **automatizada y repetible**. Se sitúa en la **intersección de Desarrollo, QA y Operaciones**.
> 
> Surgió porque antes desarrollo y operaciones trabajaban en **aislamiento**, y la implementación manual genera **errores humanos en producción**.

**2.** Mencioná beneficios de DevOps y cuándo conviene (y cuándo no) adoptarlo.
> [!success]- Ver respuesta
> Beneficios: previsibilidad, reproducibilidad (versiona todo), mantenibilidad (recuperación sin esfuerzo), **tiempo de comercialización (-50%)**, mayor calidad, riesgo reducido, resistencia (estado operativo estable), rentabilidad y división de la base de código.
> 
> - **¿Cuándo adoptarlo?** En grandes apps distribuidas (e-commerce, cloud).
> - **¿Cuándo no?** En apps de misión crítica (bancos, energía) con controles de acceso estrictos.

**3.** Explicá **CI** y **CD** y cuál es el objetivo de CI/CD.
> [!success]- Ver respuesta
> - **Integración Continua (CI):** los desarrolladores **fusionan su código en la rama central varias veces por día**; cada cambio dispara **pruebas rápidas**. Es el **primer paso** hacia la entrega continua.
> - **Entrega Continua (Continuous Delivery):** mantener el producto **siempre en estado de entrega**; liberar versiones más barato y con menor riesgo.
> 
> **Objetivo CI/CD:** hacer que liberar cambios sea un proceso rutinario y "aburrido", liberando tiempo del equipo para tareas de mayor valor.

**4.** Enumerá los **6 principios de DevOps** y explicá qué agrega **DevSecOps**.
> [!success]- Ver respuesta
> Los **6 principios de DevOps**:
> 
> - Acción centrada en el cliente.
> - Responsabilidad de extremo a extremo.
> - Mejora continua.
> - **Automatizar todo**.
> - Trabajar como un solo equipo.
> - **Monitorear y probar todo**.
> 
> **DevSecOps** integra la **seguridad desde el principio** del ciclo DevOps (la seguridad como capa **envolvente**) y se apoya en la **automatización** (auditorías de seguridad automatizadas). Buenas prácticas: educar a empleados (el usuario es el eslabón más débil), gestionar control de acceso, evaluar riesgos y plan de contingencia/DR, auditorías periódicas. En cloud rige el **modelo de responsabilidad compartida**.

**5.** ¿Qué son las pruebas automáticas y qué representan los **Cuadrantes de Agile Testing**?
> [!success]- Ver respuesta
> Las pruebas automáticas son un método de ejecutar una prueba **sin intervención humana**. Se automatiza principalmente por **tiempo** (libera al tester para pruebas más críticas).
> 
> Los **Cuadrantes de Agile Testing** (ejes negocio↔tecnología × ayuda a programar↔crítica al producto):
> 
> - **Q1** (tecnología / ayuda): unitarias, de componentes → **automatizadas**.
> - **Q2** (negocio / ayuda): funcionales, ejemplos, prototipos → automatizadas/manuales.
> - **Q3** (negocio / crítica): exploratorias, usabilidad, aceptación de usuario → **manuales**.
> - **Q4** (tecnología / crítica): carga, rendimiento, seguridad → herramientas especializadas.

**6.** ¿Qué conviene automatizar y cuáles son las desventajas de la automatización? Compará automatizado vs manual.
> [!success]- Ver respuesta
> **¿Qué automatizar?** Casos que corren en **cada versión** (sanity), **data driven**, funcionalidades críticas (smoke), funcionalidades estables y escenarios de performance. **Error común:** intentar automatizar **todo**.
> 
> **Desventajas:** no mide usabilidad, requiere conocimientos de programación y el **mantenimiento de scripts es costoso**.
> 
> **Automatizado vs Manual:**
> 
> - **Automatizado:** ventaja → ideal para pruebas repetidas y de **regresión**, matrices y paralelismo; desventaja → mayor esfuerzo inicial y difícil automatizar referencias visuales.
> - **Manual:** ventaja → el tester **explora** y descubre combinaciones impensadas (más bugs); desventaja → consume mucho tiempo y, con muchos cambios, baja la capacidad de hallar errores.

**7.** ¿Qué es una **web app** y cuáles son los principales tipos de prueba web?
> [!success]- Ver respuesta
> Una **web app** es una versión optimizada y adaptada a cualquier dispositivo/navegador. El proceso de pruebas va del **usuario** (capa exterior) a la **tecnología** (interior): diseño de interfaz, estético, de contenido, navegación, arquitectura y componentes.
> 
> Tipos de prueba web: **contenido**, **interfaz de usuario**, **funcional**, **usabilidad**, **formularios**, **navegación**, **base de datos**, **configuración/compatibilidad**, **rendimiento** y **seguridad**.

**8.** ¿Por qué el testing **mobile** es diferente del testing tradicional?
> [!success]- Ver respuesta
> Una **aplicación móvil** se ejecuta en dispositivos móviles y considera la **información contextual**. Es diferente porque:
> 
> - Hay **múltiples plataformas** (Android, iOS) frente a una sola dominante (Windows).
> - Existe la **fragmentación de Android** (muchas versiones del SO).
> - Hay gran variedad de **factores de forma** (tamaños de pantalla).
> - Los **lanzamientos son frecuentes** → ciclos de prueba continuos.
> - El entorno es único: cambios de red, alertas/notificaciones, pantalla táctil, batería.
> - Todo esto implica un **alto costo** de probar apps móviles.
> 
> Preguntas clave: duración de batería, comportamiento con red limitada/nula, velocidad, consumo de datos y periféricos.

**9.** Diferenciá el uso de **emulador/simulador** vs **dispositivo real** y mencioná métodos de prueba móvil.
> [!success]- Ver respuesta
> - **Emulador/Simulador:** más fácil y barato; se usa en la **etapa inicial** y para *features* independientes del dispositivo.
> - **Dispositivo real:** más complejo y costoso; se usa para **validar resultados** (es más preciso y confiable).
> 
> **Métodos de prueba móvil:** unitaria, integración, sistema (caja negra), **regresión** (a menudo automatizada), compatibilidad, rendimiento, funcional, caja blanca y **GUI** (coherencia entre dispositivos). En la estrategia son clave: selección de **dispositivos objetivo** (simulador + físico), automatización, entorno de red (Wi-Fi + simulación celular) y los tipos de prueba.

**10.** (Integradora) ¿Cómo cambia DevOps (U8) el rol del testing respecto del enfoque tradicional (cascada / QA tradicional de las U5–U7)?
> [!success]- Ver respuesta
> En el enfoque **tradicional/cascada**, el testing es una **fase secuencial y separada** que ocurre después de la codificación (Requerimientos → Especificaciones → Codificación → Testing → Release), con un grupo de QA aparte.
> 
> Con **DevOps**, el testing se transforma:
> 
> - Pasa a estar en la **intersección de Dev, QA y Operaciones**, integrado en una **cultura de colaboración**.
> - En **CI**, cada *merge* a la rama central (varias veces al día) dispara **pruebas rápidas automáticas**; el testing es **continuo**, no una etapa final.
> - Rigen los principios de **automatizar todo** y **monitorear y probar todo**, y con **DevSecOps** la seguridad se prueba desde el inicio.
> - Esto se alinea con el testing ágil (U7), donde desaparece la fase de pruebas separada y la **automatización de regresión y aceptación** es un factor de éxito; la diferencia es que DevOps lo extiende hasta el **despliegue y monitoreo en producción**.
> 
> En síntesis: de un testing tardío y manual al final, a un testing **automatizado, continuo y compartido** a lo largo de todo el ciclo de vida.

## 📐 Unidad 9 — Estándares de Testing (ISO 29119 + TMMi)

> [!warning] Material de referencia
> En el vault **faltan los apuntes oficiales** de los Módulos 12 (ISO 29119) y 13 (TMMi). Las respuestas siguientes se basan en el **repaso general de referencia** incluido en el resumen, que conviene reemplazar cuando aparezca el apunte oficial.

**1.** ¿Qué es la familia **ISO/IEC/IEEE 29119** y qué busca?
> [!success]- Ver respuesta
> La **ISO/IEC/IEEE 29119** es la familia de estándares internacionales de **pruebas de software**, pensada para **reemplazar/unificar** estándares previos (como IEEE 829, IEEE 1008 e ISO 29119 propiamente).

**2.** Enumerá las partes de la familia ISO 29119 y su contenido.
> [!success]- Ver respuesta
> - **29119-1** — Conceptos y definiciones.
> - **29119-2** — Procesos de prueba (organizacional, de gestión y dinámicos).
> - **29119-3** — Documentación de prueba (plantillas).
> - **29119-4** — Técnicas de prueba.
> - **29119-5** — Pruebas dirigidas por palabras clave (*keyword-driven testing*).

**3.** ¿Qué modelo de procesos de testing define la ISO 29119?
> [!success]- Ver respuesta
> Define un **modelo de procesos de testing en tres capas**:
> 
> - Procesos de la **organización**.
> - Procesos de **gestión** de pruebas.
> - Procesos **dinámicos** (diseño, implementación, ejecución, reporte de incidentes).

**4.** ¿Qué es **TMMi** y con qué modelo se alinea?
> [!success]- Ver respuesta
> **TMMi (*Test Maturity Model integration*)** es un modelo de madurez **específico del proceso de pruebas**, complementario y alineado con **CMMI**. Como CMMI, define **5 niveles de madurez**.

**5.** Enumerá los **5 niveles de madurez de TMMi** y una característica de cada uno.
> [!success]- Ver respuesta
> 1. **Inicial** — pruebas caóticas, sin proceso definido.
> 2. **Gestionado (Managed)** — política y estrategia de pruebas, planificación, monitoreo y control, diseño y ejecución, entorno de pruebas.
> 3. **Definido (Defined)** — organización de pruebas, programa de formación, ciclo de vida e integración, pruebas no funcionales, revisiones por pares.
> 4. **Medido (Measured)** — medición de pruebas, evaluación de calidad del producto, pruebas avanzadas/*peer reviews*.
> 5. **Optimización (Optimization)** — prevención de defectos, control de calidad, optimización del proceso de pruebas.

**6.** ¿Qué es la **CSTE** y en qué se diferencia de TMMi?
> [!success]- Ver respuesta
> La **CSTE (*Certified Software Tester*)** es una certificación **personal/profesional** del tester.
> 
> La diferencia clave con **TMMi** es el sujeto que evalúan: TMMi evalúa la **madurez de la organización** (su proceso de pruebas), mientras que CSTE certifica a **personas**. Idea de examen: distinguir **modelos de madurez de la organización** (TMMi, CMMI) frente a **certificaciones de personas** (CSTE).

**7.** (Integradora) ¿Cómo se relacionan los **niveles de TMMi** con la madurez del proceso y con CMMI?
> [!success]- Ver respuesta
> Los niveles de TMMi miden la **madurez del proceso de pruebas** de una organización, de forma escalonada: del **Nivel 1 (Inicial)**, con pruebas caóticas y sin proceso, hacia el **Nivel 5 (Optimización)**, centrado en prevención de defectos y mejora continua del proceso de pruebas.
> 
> Esta estructura es **complementaria y está alineada con CMMI**, que también define **5 niveles** (1 Inicial → 5 Optimizado) para la madurez del proceso de desarrollo en general. Así, a mayor nivel, el proceso pasa de reactivo e impredecible a definido, medido y finalmente optimizado: TMMi aplica esa misma lógica de madurez creciente, pero **específicamente al proceso de pruebas**.

**8.** (Integradora) La ISO 29119 busca reemplazar estándares como IEEE 829. ¿Qué rol cumplía IEEE 829 en la U6 y qué aporta 29119 frente a él?
> [!success]- Ver respuesta
> En la **U6**, el **IEEE 829** (*Standard for Software Test Documentation*) es la directriz que estructura el **plan de pruebas** y, en general, la documentación de testing.
> 
> La **ISO/IEC/IEEE 29119** busca **unificar y reemplazar** estándares previos como IEEE 829 (e IEEE 1008), ofreciendo un marco más abarcativo: no solo cubre la **documentación de prueba** (parte 29119-3, con sus plantillas, que es lo que daba IEEE 829), sino también **conceptos y definiciones** (29119-1), un **modelo de procesos en tres capas** (29119-2: organización, gestión y dinámicos), **técnicas de prueba** (29119-4) y **keyword-driven testing** (29119-5). Es decir, 29119 integra en una sola familia tanto la documentación que aportaba IEEE 829 como los procesos y técnicas de testing.

---
🔙 Volver al resumen: [[Resumen Completo - Testing y QA]]
