---
title: "Preguntas Integradoras — Análisis de Sistemas"
tags:
  - analisis-sistemas
  - preguntas
  - parcial
aliases:
  - "Preguntas Análisis de Sistemas"
---

# 🧩 Preguntas Integradoras — Análisis de Sistemas

> [!tip] Cómo usar esta página
> Las respuestas están **plegadas**. Leé la pregunta, intentá responderla vos, y recién después abrí el callout (click en el ▸ a la izquierda de "Ver respuesta") para verificar. Pensada para repaso/active recall del parcial.

## 🧱 Pensando en Objetos

**1.** Definí qué es una **clase** y qué es un **objeto**, y explicá la relación entre ambos. ¿Qué tres cosas contiene una clase?
> [!success]- Ver respuesta
> Según Pressman, una **clase** es una descripción generalizada (plantilla o plano) de una colección de objetos similares; contiene **nombre, atributos y métodos**. Un **objeto** es una **instancia** de una clase específica, y hereda sus atributos y las operaciones disponibles para manipularlos.
> 
> Ejemplo del apunte: de la clase `persona` (atributos `nombre, documento, dirección, teléfono`; métodos `trabajar, comer, comunicarse, identificarse`) se **instancia** el objeto `juan`.

**2.** ¿Cuál es la regla mnemotécnica para pasar de un enunciado a clases? Conectala con cómo se detectan las clases en el [[Diagramas de Clases|diagrama de clases]].
> [!success]- Ver respuesta
> La regla (que "cae siempre"):
> 
> - **Sustantivos → clases**
> - **Verbos → métodos**
> - **Adjetivos → atributos**
> 
> En el [[Diagramas de Clases|diagrama de clases]] las clases se detectan como **sustantivos en singular**; los **métodos** suelen corresponder a verbos y los **atributos** a las características propias del objeto. Es la misma regla aplicada al modelado estructural.

**3.** Explicá el **encapsulamiento / ocultamiento de información** y dá el ejemplo del atributo `edad` de la clase `persona`.
> [!success]- Ver respuesta
> Encapsular consiste en **ocultar el estado** de los datos de un objeto, de modo que solo puedan cambiarse a través de las **operaciones definidas** para tal fin.
> 
> Ejemplo: el atributo `edad` de `persona` **no se accede directamente**: se declara **privado** y se crean dos métodos (uno para *ingresar* y otro para *obtener* la edad), idea de setter/getter. Así queda encapsulado.

**4.** ¿Qué tres niveles de acceso a la información existen y quién necesita cada uno?
> [!success]- Ver respuesta
> - **Privada:** solo el objeto la necesita para operar; es innecesaria para los demás.
> - **Pública:** la usa el resto de los objetos para interactuar con el objeto.
> - **Protegida:** la necesitan las clases heredadas.

**5.** Definí **retención de estado**.
> [!success]- Ver respuesta
> La información de un objeto se mantiene en el **mismo estado hasta que sea alterado** por las operaciones definidas para tal fin.

**6.** Explicá la **herencia** según Pressman y la terminología subclase/superclase.
> [!success]- Ver respuesta
> Una **subclase Y** hereda todos los atributos y operaciones de su **superclase X**. Todas las estructuras de datos y algoritmos de X están disponibles de inmediato para Y; no se necesita hacer más trabajo.
> 
> Terminología: **subclase (Y)** / **superclase (X)**.

**7.** ¿Qué es el **polimorfismo** y cuál es la diferencia entre **sobrecarga** y **sobreescritura** en cuanto a la firma del método y el comportamiento?
> [!success]- Ver respuesta
> Polimorfismo: capacidad de una clase de **comportarse de diferentes maneras** según el estímulo recibido. Se da por sobrecarga o sobreescritura.
> 
> - **Sobrecarga:** se **modifica** la firma del método; el comportamiento se **mantiene**.
> - **Sobreescritura (override):** se **mantiene** la firma; el comportamiento es **diferente**.

**8.** ¿Qué es la **generalización** y cómo se relaciona con la herencia y el polimorfismo? Usá el ejemplo de Persona Física / Persona Jurídica.
> [!success]- Ver respuesta
> La generalización es un **caso particular de herencia**: la relación entre una **clase general** y sus **clases hijas especializadas** (ej.: `animal` general → `humano, perro`, etc.).
> 
> En el ejemplo, de `persona` se baja el detalle a **Persona Física** (atributo `CUIL`) y **Persona Jurídica** (atributo `CUIT`). Ambas **sobreescriben** `identificarse()` (una devuelve CUIL y la otra CUIT). Así se conecta **generalización + polimorfismo**.

**9.** Más allá de identificar clases, ¿qué otras cosas hay que dejar claras al clasificar el enunciado y para qué sirven las simulaciones de situaciones reales?
> [!success]- Ver respuesta
> No solo se identifican clases, **también las relaciones** entre ellas. Las clases se **agrupan por funcionalidad** formando **jerarquías**. Deben quedar claros los **patrones jerárquicos** y el **flujo de mensajes**. Las **simulaciones de situaciones reales** se usan para lograr la **separación de responsabilidades**.

## 📐 Introducción a UML

**1.** ¿Qué significa UML, qué tipo de lenguaje es y quién lo impulsa?
> [!success]- Ver respuesta
> UML = **Unified Modeling Language** (Lenguaje Unificado de Modelado). Es un **lenguaje de propósito general para el modelado orientado a objetos**, impulsado por el **OMG (Object Management Group)**.

**2.** ⭐ Distinguí **método** de **lenguaje (de modelado)**. ¿UML es un método o un lenguaje?
> [!success]- Ver respuesta
> **UML es un LENGUAJE de modelado, NO un método.**
> 
> - **Método:** descripción de los **pasos** a seguir, indicando el **orden**, las técnicas y herramientas a emplear en cada paso para lograr un objetivo (conjunto de técnicas, herramientas y tareas).
> - **Lenguaje:** conjunto de señales que dan a entender una cosa; posee **elementos** y **reglas sintácticas** para combinarlos.
> - **Lenguaje de modelado:** se emplea para **expresar ideas por medio de modelos**.

**3.** ¿Qué notaciones combina UML?
> [!success]- Ver respuesta
> Combina notaciones provenientes de:
> 
> - Modelado Orientado a Objetos
> - Modelado de Datos
> - Modelado de Componentes
> - Modelado de Flujos de Trabajo (Workflows)

**4.** Definí qué es un **modelo**. ¿Cuándo se necesita uno y por qué a veces hace falta un conjunto de modelos?
> [!success]- Ver respuesta
> Un modelo es el **cuerpo de información** recabado acerca de un sistema con el fin de estudiarlo.
> 
> Se requiere cuando **no se puede estudiar el sistema en sí** (por complejidad, tamaño, costo, inexistencia, etc.). **El modelo NO es el sistema**: tiene necesariamente **diferencias** con él. A veces hace falta un **conjunto de modelos** para enfocar el sistema desde **distintas perspectivas** y **disminuir la brecha** modelo ↔ sistema.

**5.** Enumerá los **objetivos del UML**.
> [!success]- Ver respuesta
> 1. Establecer un **lenguaje visual** de modelado, expresivo y sencillo.
> 2. Mantener **independencia** de los procesos de modelado y de los lenguajes de programación.
> 3. Establecer **bases formales**.
> 4. Integrar las **mejores prácticas**.
> 5. Imponer un **estándar mundial**.

**6.** Describí la evolución de los enfoques de análisis (procesos, datos, objetos) con sus fechas/hitos.
> [!success]- Ver respuesta
> - **Orientación a procesos** (~'50–'90): qué debe hacer el sistema, sin importar el almacenamiento. Declina con las **bases de datos comerciales**.
> - **Orientación a datos** (~'70): nace con el **modelo relacional**; el más difundido.
> - **Orientación a objetos** (Smalltalk, **1969**): se habla de **diseño OO** a inicios de los '80 y de **análisis OO** a fines de esa década.

**7.** ¿Quiénes son los "tres amigos" y cómo fue el camino a la estandarización del UML?
> [!success]- Ver respuesta
> Los **tres amigos:** **Booch, Rumbaugh y Jacobson**.
> 
> - **1994:** Booch plantea unificar criterios; se le une **Rumbaugh** (oct.) → *Unified Method v0.8*.
> - **1995:** se suma **Jacobson** → **nace el UML**.
> - Se elaboran v0.9, v0.91, v1.0 (1996), v1.1 → presentada al **OMG**.
> - **Fines de 1997: el OMG adopta UML v1.1 como estándar.**

**8.** ¿Por qué UML predomina sobre otras notaciones?
> [!success]- Ver respuesta
> - Participación de **metodólogos influyentes**.
> - Participación de **importantes empresas**.
> - Es el **estándar del OMG**.

**9.** Conectá [[Pensando en Objetos]] con [[Introducción a UML]]: ¿qué rol cumple cada uno en el flujo de trabajo?
> [!success]- Ver respuesta
> Primero se piensa el mundo en términos de **objetos** (paradigma POO: clases, herencia, polimorfismo, etc.). Después, **UML es el lenguaje** con el que se expresan esos modelos. De UML salen las herramientas concretas: [[Casos de Uso]], [[Diagramas de Clases]], [[Diagramas de Secuencia]] y [[Diagramas de Actividades]].

## 🎭 Casos de Uso

**1.** Definí qué es un **caso de uso** y cómo se relaciona con los requerimientos.
> [!success]- Ver respuesta
> Un caso de uso es una **secuencia de interacciones** entre un sistema y alguien o algo que usa alguno de sus servicios. Los casos de uso son **las funcionalidades del sistema**.
> 
> Se relacionan con los **requerimientos** mediante trazabilidad (permite evaluar el impacto de cambios). **Cada requerimiento puede mapear a uno o más casos de uso.**

**2.** Diferenciá la **narrativa** del **diagrama** de casos de uso. ¿Cuál es estático? ¿Qué NO debe mencionar la narrativa?
> [!success]- Ver respuesta
> - **Narrativa (texto):** descripción detallada de la secuencia de interacciones (nombre, actores, precondición, flujos, poscondición).
> - **Diagrama:** representación gráfica de casos de uso + actores + relaciones. Es **estático**: NO muestra secuencia de ejecución.
> - Ambos sirven para **revisar el sistema con el usuario**.
> - ⚠️ Las narrativas **NO mencionan implementación técnica** (ni lenguaje ni base de datos).

**3.** ¿Qué es un **actor**? Distinguí actor principal, secundario y el actor "tiempo/chron/reloj".
> [!success]- Ver respuesta
> Un actor es una **agrupación uniforme** de personas, sistemas o máquinas que interactúan con el sistema **de la misma forma**.
> 
> - **Principal:** ejecuta el **primer paso**; aparece en el **nombre** del CU, en los **actores** y en el **primer paso**.
> - **Secundario:** otro que participa (raro). Ej.: una supervisora que autoriza una operación de la cajera.
> - **Tiempo / chron / reloj:** cuando la funcionalidad la dispara una **programación** (ej.: reporte diario a las 20 hs), no un humano.

**4.** ⭐ Enunciá las **reglas de oro** sobre actores. ¿Puede el sistema ser actor de sí mismo? ¿Y otro sistema?
> [!success]- Ver respuesta
> - **El SISTEMA nunca puede ser actor de sí mismo.**
> - **SÍ** puede ser actor **otro sistema** (ej.: el sistema de AFIP es actor de nuestro sistema bancario).
> - Si algo **no interactúa con el sistema**, no es actor (ej.: el cliente que mira a la cajera).

**5.** Enumerá las partes de la **estructura de la narrativa** y aclarar qué es raro y qué nunca falta.
> [!success]- Ver respuesta
> - **Nombre:** código + actor principal + acción en gerundio (ej.: *"CU 1.1 Usuario ingresando al sistema"*).
> - **Descripción:** breve descripción de lo que hace.
> - **Actores:** los que participan; al principal se le pone **(P)** o **(Principal)**.
> - **Precondición:** condición que se cumple antes y dispara el CU. **Es raro que un CU tenga precondición.**
> - **Flujo Normal (básico):** pasos para llegar al objetivo.
> - **Flujo Alternativo:** secuencia que surge de una **validación**, numerada respecto al paso (ej.: 4.1, 4.2).
> - **Poscondición:** estado del sistema tras el flujo normal.

**6.** ⭐ ¿Cuáles son las reglas de redacción de la narrativa? Pensá en la interacción, los botones, las validaciones y los mensajes del sistema.
> [!success]- Ver respuesta
> - Reflejar la **interacción actor ↔ sistema** (ida y vuelta), varias veces.
> - **No** decir que el usuario hace clic en un botón si antes el sistema no mostró/habilitó ese botón.
> - Se pueden **agrupar pasos** si los hace el **mismo** sujeto.
> - Las **validaciones** del sistema van en el **flujo normal**; si fallan, derivan al alternativo.
> - Los **mensajes del sistema van entre comillas** (ej.: *4.2 El sistema muestra "Usuario no existe"*).
> - Cada paso = **sujeto (Actor / Sistema) + acción**.

**7.** Describí la **notación del diagrama** de casos de uso (sistema/límite, actor, caso de uso, asociación).
> [!success]- Ver respuesta
> - **Sistema / límite (boundary):** **rectángulo**; el nombre va dentro arriba. Adentro van los CU; afuera los actores.
> - **Actor:** **monigote** (stick figure) fuera del rectángulo, con su nombre.
> - **Caso de uso:** **elipse/óvalo** con el nombre, dentro del rectángulo.
> - **Asociación actor–CU:** **línea sólida**.

**8.** ⭐ Compará `<<include>>` y `<<extends>>` en obligatoriedad, dirección de la flecha, origen y cómo se redacta en la narrativa. ¿La etiqueta `use` es válida?
> [!success]- Ver respuesta
> | | `<<include>>` (Inclusión) | `<<extends>>` (Extensión) |
> |---|---|---|
> | Obligatoriedad | **Siempre** se ejecuta cuando corre el CU base | **No siempre** (opcional/condicional) |
> | Dirección flecha | base **→** incluido | extensor **→** base |
> | Origen | funcionalidad **común** | no necesariamente error/excepción |
> | En narrativa | `Ejecutar CU…` | `Si [condición], ejecutar CU…` |
> 
> La flecha es punteada con punta abierta. La condición va **entre corchetes** y la acción tras la coma. La etiqueta **`use` es inválida**: solo valen `<<include>>` / `<<extends>>`.

**9.** (Integradora) ¿Cómo se vinculan los **sustantivos** de un caso de uso con las **clases** del [[Diagramas de Clases|diagrama de clases]], y la **narrativa** con el [[Diagramas de Actividades|diagrama de actividades]] y el [[Diagramas de Secuencia|de secuencia]]?
> [!success]- Ver respuesta
> El caso de uso es el origen del que se derivan los demás modelos:
> 
> - Los **sustantivos** de la narrativa se vuelven **clases** del diagrama de clases.
> - La **narrativa** (texto de la secuencia de interacciones) se vuelve un **diagrama de actividades**.
> - Cada **escenario** del CU se **realiza** con un **diagrama de secuencia**.

**10.** Mencioná al menos cuatro errores comunes marcados por la cátedra en los casos de uso (ejemplo radares CABA).
> [!success]- Ver respuesta
> - **Faltan actores** (el dispositivo que toma la foto, el sistema de la policía, el del Registro de Propiedad Automotor, el usuario web, el técnico que configura los radares).
> - **El propio sistema NO puede ser actor.**
> - **El primer paso debe ser de un actor**, nunca del sistema; y cada paso debe decir **quién** lo hace.
> - **Nombre mal** (debe ser actor + acción en gerundio), relación etiquetada **`use`** (inválida), **precondición mal** o **falta poscondición**, y **no reflejar la interacción** o faltar detalle.

## 🔀 Diagramas de Actividades

**1.** ¿Qué es un diagrama de actividades, de qué es la representación gráfica y qué tipo de diagrama es (estructura/comportamiento)?
> [!success]- Ver respuesta
> Es la **representación gráfica de la narrativa de un** [[Casos de Uso|caso de uso]] (con salvedades). Muestra la **lógica** que ocurre como respuesta a acciones internas y los **pasos** para llevar a cabo una operación. Es un diagrama de **comportamiento (dinámico)**.

**2.** ¿En qué se parece y en qué se diferencia clave de un diagrama de flujo?
> [!success]- Ver respuesta
> Se parece a un **diagrama de flujo**, pero su **diferencia clave** es que **puede mostrar procesamiento PARALELO**.

**3.** ¿Para qué tres cosas resulta adecuado el diagrama de actividades?
> [!success]- Ver respuesta
> 1. **Análisis de casos de uso** (entender qué acciones se necesitan y sus dependencias).
> 2. **Comprensión del flujo de trabajo** a través de varios casos de uso.
> 3. **Modelado de proceso de negocio (Workflow).**

**4.** ¿Cuál es la **ventaja** y la **desventaja** del DA? ¿Se puede revisar con el usuario?
> [!success]- Ver respuesta
> - ✅ **Ventaja:** soporta **comportamiento paralelo** (workflow, multi-thread).
> - ❌ **Desventaja:** NO muestra bien los enlaces entre **acciones y objetos** → para eso son mejores los [[Diagramas de Secuencia]].
> - Es de alto nivel y **NO tiene datos técnicos**, por eso **se puede revisar con el usuario**.

**5.** Describí los símbolos de **inicio, actividad y fin** y sus reglas de entradas/salidas.
> [!success]- Ver respuesta
> - **Inicio:** círculo **relleno** ●. Solo **UNO** por diagrama; 1 salida.
> - **Actividad / Acción:** caja de **extremos redondeados**; **1 entrada, 1 salida**.
> - **Fin:** círculo relleno **dentro de otro** ◉. Puede haber **varios**; 1 entrada.

**6.** ⭐ Diferenciá **bifurcación/unificación** (rombo) de **fork/join** (barra), en cuanto a entradas/salidas y a cuántas ramas se ejecutan. ¿Cuándo se usa fork?
> [!success]- Ver respuesta
> - **Bifurcación (rombo, decisión):** **1 entrada, 2 salidas**, cada salida con su guarda entre corchetes; se ejecuta **UNA SOLA** rama (mutuamente excluyentes). Se cierra con **unificación** (rombo: 2 entradas, 1 salida).
> - **Fork (barra gruesa):** **1 entrada, varias salidas**; todas las ramas se ejecutan **AL MISMO TIEMPO**. Se cierra con **join** (barra: varias entradas, 1 salida) que **espera TODAS** las entradas (sincroniza).
> - Usar **fork solo si el enunciado dice simultaneidad** ("al mismo tiempo que hago A, hago B").

**7.** ¿Qué son las **calles (swimlanes)** y para qué sirven?
> [!success]- Ver respuesta
> Son columnas con **líneas verticales**; cada calle indica **quién es responsable** de esas actividades (Actor / Sistema). Definen **quién hace qué**.

**8.** Respecto de la narrativa del CU, ¿qué partes NO incluye el diagrama de actividades? ¿Se pueden anidar estructuras?
> [!success]- Ver respuesta
> El DA **NO incluye precondición ni poscondición**. Sí se pueden **anidar** estructuras (ej.: una bifurcación dentro de un fork).

**9.** ⭐ Explicá el error del **deadlock** al mezclar una bifurcación con un join, y cómo se corrige.
> [!success]- Ver respuesta
> Si mandás ramas de una **bifurcación** directo a un **join**, el join espera **todas** sus entradas, pero la bifurcación solo ejecuta **una** → se produce **deadlock** (nunca llega al fin). ➡️ La corrección es cerrar la bifurcación con su **unificación** *antes* del join.

**10.** Mencioná otros errores comunes del DA (rótulo de la bifurcación, actividad colgada, fork, actores).
> [!success]- Ver respuesta
> - **Actividad sin salida** → queda colgada (toda actividad: 1 entrada + 1 salida).
> - **No indicar quién** realiza la actividad → ponerlo en el nombre o usar **calles**.
> - **Bifurcación mal rotulada:** el rombo va **SIN texto adentro**, con las condiciones **afuera y entre corchetes**, y **NUNCA "sí/no"**.
> - **Fork sin justificación** (la narrativa no dice "al mismo tiempo").
> - **Faltan actores / no figura el sistema** → usar calles.

## 🏛️ Diagramas de Clases

**1.** ¿Qué es un diagrama de clases, qué tipo es (estructural/comportamiento) y para qué sirve?
> [!success]- Ver respuesta
> Es un **diagrama de estructura estática**: muestra las clases y sus interrelaciones (herencia, agregación, asociación). **Tipo: ESTRUCTURAL.** Sirve para mostrar **qué hace** el sistema y **cómo se construye**. Una clase es una agrupación de cosas / plantilla para armar objetos, y se detecta como **sustantivo en singular**.

**2.** Describí las **tres secciones** de la representación de una clase.
> [!success]- Ver respuesta
> 1. **Superior:** nombre de la clase.
> 2. **Media:** atributos.
> 3. **Inferior:** operaciones (métodos).
> 
> Puede mostrarse esquemática: solo el rectángulo con el nombre.

**3.** ¿Qué símbolos de **visibilidad** existen y quién puede usar cada uno?
> [!success]- Ver respuesta
> - **+** public: cualquier clase externa con visibilidad hacia la clase.
> - **#** protected: descendientes o clases externas dentro del paquete.
> - **−** private: solo la propia clase.

**4.** Escribí la **sintaxis textual UML** de un atributo y de un método. ¿Qué determinan los atributos y qué representan los métodos?
> [!success]- Ver respuesta
> - **Atributo:** `visibilidad nombre : tipo = valor inicial`
> - **Método:** `visibilidad nombre(parámetros) : tipo devuelto`
> 
> Los **atributos** son características propias que determinan el **estado** del objeto (generalmente tipos simples; los compuestos se modelan como **relaciones**). Los **métodos** son responsabilidades/comportamiento (generalmente verbos) y definen cómo se comunican las clases.

**5.** ¿Qué es la **cardinalidad/multiplicidad** y cómo se leen `1`, `1..*`, `0..*` y `m`? Diferenciá cardinalidad de multiplicidad.
> [!success]- Ver respuesta
> Se anota **en cada extremo** de la relación e indica grado/nivel de dependencia:
> 
> - `1` = uno · `1..*` (1..n) = uno o muchos · `0..*` (0..n) = cero o muchos · `m` = número fijo.
> - Ej.: `Cliente "1" -- "1..*" OrdenDeCompra`: un cliente tiene muchas órdenes; una orden tiene un solo cliente.
> 
> En clases se habla de **Cardinalidad**; en objetos, de **Multiplicidad** (clases: atributos/métodos/cardinalidad ↔ objetos: estado/comportamiento/multiplicidad).

**6.** Diferenciá **asociación**, **agregación** y **generalización/herencia**. ¿Cómo se nota la herencia y la clase abstracta?
> [!success]- Ver respuesta
> - **Asociación:** línea que une dos clases; objetos que **colaboran**. **No es fuerte**: el tiempo de vida de un objeto **no depende** del otro (ej.: `Auto — Motor`).
> - **Agregación:** nombrada como tipo de relación (el material no desarrolla la diferencia gráfica rombo vacío vs lleno).
> - **Generalización / Herencia:** **triángulo** del lado de la clase **padre** (más general). Clase abstracta → nombre en **itálica**. Generalización: mirar de hijo → padre; especialización: de padre → hijo.

**7.** Según Liskov, ¿qué tipos de herencia hay? ¿Qué afirma Snyder sobre herencia y encapsulamiento?
> [!success]- Ver respuesta
> - **Herencia de tipo:** responde a **"¿es un?"** (ej.: `Moto es un Vehículo`).
> - **Herencia de uso:** **NO** responde a "¿es un?"; se usa para reutilizar código (ej.: `Silla` hereda de `Mesa`).
> 
> ⚠️ Según **Snyder**, la **herencia rompe el encapsulamiento**: el hijo accede a atributos e implementación del padre.

**8.** Enumerá los **9 pasos del método de Rumbaugh** para identificar clases.
> [!success]- Ver respuesta
> 1. **Identificar clases** de objetos (sustantivos; evitar estructuras de implementación; no preocuparse aún por herencia).
> 2. **Retener clases candidatas** (descartar redundantes, irrelevantes; una operación con características propias → clase).
> 3. **Diccionario de datos** (describir cada clase).
> 4. **Identificar asociaciones** (no perder tiempo distinguiendo asociación vs. agregación).
> 5. **Retener asociaciones correctas** (eliminar las de clases borradas, irrelevantes, acciones/sucesos transitorios, ternarias, derivadas).
> 6. **Identificar atributos** (propiedades individuales; nombre significativo; no exagerar).
> 7. **Retener atributos correctos** (si importa la existencia independiente → es objeto, no atributo).
> 8. **Refinamiento mediante herencia** (generalizar lo común / especializar).
> 9. **Iteración** del modelo (si hay error, volver a la etapa anterior).

**9.** ⭐ ¿Qué es el antipatrón "clase Dios" y por qué está mal? Mencioná los demás errores comunes del DC (radares).
> [!success]- Ver respuesta
> La **clase "Sistema" / "Procesador" que hace todo** es el antipatrón *clase Dios*: **el sistema NO es una clase**; hay que **repartir responsabilidades** entre las clases del dominio.
> 
> Otros errores: **inventar clases ajenas al dominio** (ej.: `Usuario` con login si no se pide), **olvidar clases del dominio** (Cámara, Infracción, Foto…), **olvidar atributos relevantes** (velocidad máxima, datos del vehículo, ubicación del radar) y **olvidar relaciones necesarias** (ej.: Radar ↔ Multa) con su cardinalidad.

**10.** (Integradora) ¿De dónde salen las **clases**, los **atributos** y los **métodos** combinando [[Pensando en Objetos]], [[Casos de Uso]] y [[Diagramas de Secuencia]]?
> [!success]- Ver respuesta
> - **Clases:** de los **sustantivos** (en singular) del enunciado / narrativa del caso de uso.
> - **Atributos:** de los **adjetivos** / características propias que determinan el estado.
> - **Métodos:** de los **verbos**, y se descubren con los **mensajes** que recibe cada objeto en el [[Diagramas de Secuencia|diagrama de secuencia]] (un mensaje recibido se transforma en un método de la clase receptora).

## ⏱️ Diagramas de Secuencia

**1.** ¿Qué modela un diagrama de secuencia, qué realiza respecto de un caso de uso y qué tipo de diagrama es?
> [!success]- Ver respuesta
> Modela los **aspectos dinámicos**: las interacciones entre objetos **organizadas en secuencia temporal**. **Realiza un escenario de un** [[Casos de Uso|caso de uso]]. Muestra los objetos participantes y la secuencia de mensajes. Es un diagrama de **comportamiento (dinámico)**.

**2.** ¿Qué representan las dos dimensiones (vertical y horizontal) del diagrama?
> [!success]- Ver respuesta
> - **Vertical → TIEMPO** (de arriba hacia abajo).
> - **Horizontal → OBJETOS** que participan.

**3.** ¿De qué depende su creación y qué muestra para un escenario de un CU?
> [!success]- Ver respuesta
> Su creación **depende de los casos de uso**. Para un **escenario** de un CU muestra: los eventos de los **actores externos**, su **orden** y los **eventos internos** del sistema.

**4.** Describí la notación: rol/objeto, línea de vida, activación y los tipos de mensaje (síncrono, retorno, auto-mensaje).
> [!success]- Ver respuesta
> - **Rol / objeto:** rectángulo etiquetado **`nombreRol : Clase`** (¡con los dos puntos!).
> - **Línea de vida (lifeline):** línea **vertical punteada** → tiempo de existencia del objeto.
> - **Activación / foco de control:** **rectángulo delgado** sobre la línea de vida → tiempo de ejecución de una acción.
> - **Mensaje:** línea horizontal entre líneas de vida (puede llevar parámetros y retorno).
> - **Síncrono:** flecha **llena**. **Retorno:** flecha **punteada** con el valor devuelto. **Auto-mensaje (self-call):** flecha que sale y vuelve al mismo objeto.

**5.** ¿Cómo se representan la **creación** y la **destrucción** de un objeto?
> [!success]- Ver respuesta
> - **Creación:** estereotipo **`<<create>>`** (el objeto aparece más abajo, a su altura temporal).
> - **Destrucción:** una **X** al final de la línea de vida + **`<<destroy>>`**.

**6.** ⭐ (Integradora) ¿Por qué se dice que "el mensaje es un método"? ¿Cómo ayuda el DS a descubrir las operaciones de las [[Diagramas de Clases|clases]]?
> [!success]- Ver respuesta
> Un **mensaje invoca una operación (método)** del objeto receptor. Los mensajes que **recibe** un objeto **se transforman en los métodos de su clase**, por eso el DS ayuda a **descubrir las operaciones** de cada clase. Es la conexión `mensajes ↔ métodos` entre el diagrama de secuencia y el de clases.

**7.** ¿Cómo se traducen las estructuras de control del código a **fragmentos combinados**? (if, if-else, switch, bucles, anidados)
> [!success]- Ver respuesta
> Cada estructura de control se traduce a un **marco** con guarda **entre corchetes**:
> 
> - `if` → **ALT** (`ALT [condición]`).
> - `if-else` → **ALT dividido** por una **línea punteada**; el `else` va debajo.
> - `switch` → **varios ALT** (un `ALT [x==caso]` por cada *case*).
> - `for` / `while` / `do-while` / `for-each` → **FOR (loop)** (`FOR [condición]`).
> - anidados (`for-if`) → un marco **dentro** de otro.

**8.** ¿Por qué una **búsqueda** que recorre elementos debe ir dentro de un `loop`? (analogía de la cátedra)
> [!success]- Ver respuesta
> Una búsqueda que recorre elementos es una **iteración**, no un único mensaje. La analogía de la cátedra: *"es como tener papeles en una caja: si buscás uno, tenés que mirar uno por uno"*. Por eso falta un `loop` si se modela como un solo mensaje.

**9.** Mencioná los errores comunes del DS (dos puntos, sistema como actor/clase, loop).
> [!success]- Ver respuesta
> - **Nombres de clases sin los dos puntos** → la sintaxis correcta es `nombre:Clase` o `:Clase`.
> - **El sistema no puede ser actor ni clase** → repartir sus responsabilidades entre clases concretas (Radar, Cámara, etc.).
> - **Falta un `loop`** tras una búsqueda que recorre elementos.

**10.** (Integradora) ¿Qué diferencia y qué complementariedad hay entre el [[Diagramas de Actividades|diagrama de actividades]] y el diagrama de secuencia para un mismo caso de uso?
> [!success]- Ver respuesta
> Ambos son diagramas de **comportamiento (dinámicos)** derivados del caso de uso, pero:
> 
> - El **DA** soporta **comportamiento paralelo** (workflow) y es de alto nivel, pero **NO muestra bien los enlaces entre acciones y objetos**.
> - El **DS** sí muestra bien la interacción **entre objetos** ordenada en el tiempo, y permite **descubrir los métodos** de las clases. Por eso para el detalle objeto-mensaje es mejor el DS.

## 🏗️ Arquitectura de Software

**1.** Definí qué es la arquitectura de software y qué tres cosas comprenden sus estructuras (vistas).
> [!success]- Ver respuesta
> Es el **set de estructuras (vistas)** del sistema que comprenden: **elementos de SW** (módulos, objetos, componentes, subsistemas, procesos), sus **propiedades externamente visibles** (servicios que provee, performance, reacción ante fallos) y las **relaciones entre ellos** (calls, send-data-to, synchronizes-with, uses, depends, instantiates).

**2.** ¿De qué se ocupa y de qué NO se ocupa la arquitectura? ¿Qué afirmación clave hay sobre todo sistema?
> [!success]- Ver respuesta
> Solo se ocupa del **costado público de las interfaces**; los **detalles de implementación NO son arquitectónicos**. La **abstracción** permite manejar la complejidad y las vistas son **selectivas**. ⭐ **"Todo sistema o programa tiene una arquitectura."**

**3.** ¿Por qué es importante la arquitectura?
> [!success]- Ver respuesta
> - Es **vehículo de comunicación** entre stakeholders → **lenguaje común**.
> - Provee un **blueprint técnico** para construir/modificar/analizar.
> - Es el **producto de las primeras decisiones de diseño** → **permite o impide casi todos los atributos de calidad**.
> - Es una abstracción **reutilizable y transferible**.

**4.** ¿Qué es un **estilo arquitectónico** y qué características lo definen? ¿Un sistema puede tener varios?
> [!success]- Ver respuesta
> Es la descripción de **elementos y tipos de relaciones** junto con un **set de restricciones** sobre cómo pueden usarse. Características:
> 
> - Set de **elementos** (componentes, procesos, módulos…).
> - Set de **conectores / mecanismos de interacción** (call, event, pipe).
> - **Topología** (distribución de elementos).
> - **Restricciones** sobre topología y comportamiento (ej.: pipelines acíclico).
> - **Trade-off** (costos/beneficios; ej.: pipe-and-filter prioriza reutilización sobre performance).
> - Un sistema puede **exhibir múltiples estilos a la vez**.

**5.** Enumerá los tres **viewtypes (tipos de vistas)** y un ejemplo de cada uno.
> [!success]- Ver respuesta
> - **Module** (estático): Decomposition, Uses, Layered, Class/generalization.
> - **Component & Connectors** (ejecución): Process, Concurrency, Shared-Data, Client-Server.
> - **Allocation** (distribución): Deployment, Implementation, Work Assignment.
> 
> Lo más cercano a "capas" es la vista **Layered**; a cliente-servidor, **Client-Server**. (El PPT no desarrolla MVC ni capas presentación/negocio/datos como tales.)

**6.** ¿Cuál es el **rol del arquitecto**? Relacioná con los stakeholders.
> [!success]- Ver respuesta
> El arquitecto es un **puente** entre los requerimientos del negocio y el sistema a construir: define el problema, los drivers, escenarios y stakeholders; identifica **atributos de calidad**; provee diseño y elige tecnologías. Cada stakeholder tiene intereses distintos: el **cliente** (funcionalidad, lifetime, time-to-market, flexibilidad), el **usuario** (facilidad de uso, velocidad, disponibilidad), los **administradores** (configuración, usuarios, seguridad, backup/recovery), los **managers** (costos bajos, objetivos estratégicos, plazos) y los **developers** (lenguaje, tecnología).

**7.** ⭐ ¿Qué son los **atributos de calidad** y por qué son tan importantes? Dá la definición y nombrá varios con lo que miden.
> [!success]- Ver respuesta
> Son una propiedad **medible o testeable** que indica qué tan bien el sistema satisface las necesidades (requerimientos no funcionales). Son *"el principal insumo de un arquitecto"*. Algunos:
> 
> - **Performance:** respuesta en un intervalo (métricas: **latencia** y **throughput**).
> - **Availability:** proporción de tiempo funcional (downtime). **Reliability:** probabilidad de no fallar en un intervalo.
> - **Usability, Reusability, Interoperability, Maintainability, Manageability, Scalability, Supportability, Security, Testability.**

**8.** Enunciá la **Ley de Conway** y la definición de **calidad (IEEE)**. ¿Existen arquitecturas "buenas" o "malas" en abstracto?
> [!success]- Ver respuesta
> - **Ley de Conway:** "Las organizaciones que diseñan sistemas están limitadas a producir diseños que **copian la estructura de comunicación** de sus organizaciones."
> - **Calidad (IEEE):** "El grado en el cual un software posee una **combinación deseada de atributos**."
> - **No hay arquitecturas buenas o malas** en abstracto: hay buenas arquitecturas que **resuelven un problema específico en un contexto específico**, y se evalúan según **si cumplen sus objetivos**.

**9.** Diferenciá **latencia** y **throughput** como métricas, y explicá por qué la arquitectura "permite o impide" atributos de calidad.
> [!success]- Ver respuesta
> **Latencia** y **throughput** son las dos métricas del atributo **Performance** (respuesta en un intervalo). Como la arquitectura es el **producto de las primeras decisiones de diseño**, esas decisiones **permiten o impiden casi todos los atributos de calidad**: una arquitectura mal elegida puede hacer imposible alcanzar, por ejemplo, la performance o la escalabilidad deseadas.

## ♟️ Patrones de Diseño

**1.** ¿Qué es un patrón de diseño y cuál es su propósito?
> [!success]- Ver respuesta
> Un **patrón es una solución para un problema en un contexto**. Su propósito es la **reutilización eficiente**. Los patrones de software son formas **"estandarizadas"** de resolver problemas comunes de diseño; sirven para identificar problemas típicos y sus soluciones.

**2.** Enumerá las **ventajas** de usar patrones.
> [!success]- Ver respuesta
> - Amplio **catálogo** de problemas y soluciones.
> - **Estandarizan** la resolución.
> - **Condensan y simplifican** el aprendizaje de buenas prácticas.
> - Dan un **vocabulario común** entre desarrolladores.
> - Evitan **"reinventar la rueda"**.

**3.** ¿Cuáles son los **cuatro elementos** de un patrón?
> [!success]- Ver respuesta
> 1. **Nombre** — identifica el patrón (de las partes más difíciles).
> 2. **Problema** — cuándo utilizarlo.
> 3. **Solución** — qué se obtiene al aplicarlo.
> 4. **Consecuencias** — ventajas y desventajas.

**4.** ⭐ ¿En qué tres clases se clasifican los patrones y para qué sirve cada una?
> [!success]- Ver respuesta
> - **Creacionales:** cómo **crear** objetos (problema de la instanciación; delegar/encapsular la creación).
> - **Estructurales:** cómo **agrupar y organizar** objetos (interconexiones en ejecución).
> - **De comportamiento:** cómo se **relacionan en ejecución** (dimensión temporal; llamadas entre objetos).

**5.** Nombrá patrones de cada clase según el material.
> [!success]- Ver respuesta
> - **Creacionales:** Factoría abstracta, Builder, Prototipo, **Singleton**.
> - **Estructurales:** Bridge, Adapter, Proxy, Facade, Flyweight.
> - **De comportamiento:** Command, Interpreter, Iterator, Mediator, Observer.

**6.** ¿Qué problema resuelve el patrón **Singleton** y cuáles son sus claves de implementación?
> [!success]- Ver respuesta
> En un Singleton **solo puede haber una instancia**. Claves de implementación:
> 
> - Constructor **`protected`** (no público).
> - Acceso vía un método **estático** `instance()`.
> - La instancia única se guarda en un atributo **estático privado** `Sinstance`.

**7.** ¿A qué clase de patrones pertenece Singleton y por qué?
> [!success]- Ver respuesta
> Pertenece a los patrones **Creacionales**, porque resuelve un problema de **creación/instanciación** de objetos (en este caso, garantizar una única instancia). Es el único patrón desarrollado con código en el material.

**8.** (Integradora) ¿Sobre qué se aplican los patrones y dentro de qué se enmarcan? Conectá con [[Diagramas de Clases]] y [[Arquitectura de Software]].
> [!success]- Ver respuesta
> Los patrones se aplican sobre el modelo de **clases** ([[Diagramas de Clases]]) y dentro de una **arquitectura** ([[Arquitectura de Software]]). Son soluciones probadas a problemas de diseño recurrentes; mientras la arquitectura define la estructura de alto nivel y sus atributos de calidad, los patrones resuelven problemas concretos de diseño a nivel de clases y objetos.

**9.** Una **aclaración de fidelidad** del material: ¿menciona explícitamente al GoF? ¿Qué patrón es el único desarrollado con código?
> [!success]- Ver respuesta
> El PPT **no menciona** explícitamente a **GoF / Gang of Four** ni al libro *Design Patterns*, aunque los patrones que lista son los clásicos de ese catálogo. **Singleton** es el único desarrollado con código.

## ⚠️ Errores comunes (estilo "encontrá el error")

> Basado en el [[Checklist de Errores Comunes]] (ejemplos corregidos de la cátedra — sistema de seguridad vial, radares CABA).

**1.** En un diagrama de casos de uso, el alumno puso "Sistema de seguridad vial" como uno de los **actores** y dibujó el **primer paso** del CU ejecutado por el sistema. ¿Qué está mal?
> [!success]- Ver respuesta
> Dos errores: **el sistema NO puede figurar como actor de sí mismo**, y **el primer paso lo ejecuta siempre un actor**, nunca el sistema. Además hay que verificar que se identificaron **todos** los actores (incluidos otros sistemas como policía o el Registro de Propiedad Automotor, y el rol que **configura** los radares).

**2.** Una narrativa nombra el caso de uso como *"Gestión de multas"* y dice *"4. El sistema guarda la multa en la base de datos MySQL"*. ¿Qué dos cosas corregirías?
> [!success]- Ver respuesta
> - El **nombre** debe ser **código + actor + acción en gerundio** (ej.: *"CU 4 Operador registrando multa"*), no un sustantivo genérico.
> - La narrativa **NO menciona implementación técnica** (ni lenguaje ni base de datos): "MySQL" sobra. Además los **mensajes del sistema van entre comillas** y cada paso debe decir **quién** lo hace.

**3.** Un diagrama de casos de uso usa una flecha etiquetada `use` entre dos CU, y otra relación `<<extends>>` apuntando del CU base al extensor. ¿Cuál es el error?
> [!success]- Ver respuesta
> La etiqueta **`use` es inválida**: solo valen `<<include>>` / `<<extends>>`. Además la **dirección** de `<<extends>>` está al revés: va del **extensor → base** (en cambio `<<include>>` va de **base → incluido**). Recordar: `<<include>>` = obligatorio, `<<extends>>` = opcional/condicional.

**4.** Una narrativa tiene como **precondición** "El usuario hace clic en el botón Guardar". ¿Está bien planteada?
> [!success]- Ver respuesta
> No. La **precondición** debe ser algo realmente **previo** que se cumple antes y dispara el CU; "hacer clic en Guardar" es un paso del flujo. Además **es raro que un CU tenga precondición**: si no la hay, **no se pone**. Y no se puede mencionar un botón si antes el sistema no lo mostró/habilitó.

**5.** En un diagrama de actividades, dos ramas que salen de una **bifurcación (rombo)** se vuelven a unir directamente en un **join (barra)**. ¿Qué pasa y cómo se arregla?
> [!success]- Ver respuesta
> Se produce un **deadlock**: el **join espera TODAS** sus entradas, pero la **bifurcación ejecuta solo UNA** rama, así que nunca llega al fin. La corrección es cerrar la bifurcación con su **unificación** *antes* del join (bifurcación↔unificación, fork↔join).

**6.** Un diagrama de actividades tiene un rombo con el texto "¿Aprobado?" adentro, y sus salidas etiquetadas "sí" y "no". ¿Qué corregirías?
> [!success]- Ver respuesta
> El rombo de **bifurcación** va **SIN texto adentro**: las condiciones van **afuera y entre corchetes**, y **NUNCA "sí/no"** (ej.: `[aprobado]` / `[no aprobado]`). También conviene revisar que haya **un solo inicio** y que cada actividad tenga **1 entrada y 1 salida**.

**7.** El enunciado dice "luego de validar, el sistema suma stock y envía un mail", sin indicar simultaneidad, pero el alumno usó un **fork**. ¿Procede?
> [!success]- Ver respuesta
> No. El **fork solo se usa si el enunciado dice "al mismo tiempo"** (simultaneidad). Si no hay paralelismo explícito, es un **fork sin justificación**: debe modelarse como flujo secuencial. Además hay que indicar **quién** hace cada actividad (en el nombre o con **calles**).

**8.** En un diagrama de clases aparece una clase `ProcesadorDeMultas` que valida, fotografía, calcula y notifica, mientras faltan las clases `Cámara`, `Infracción` y `Foto`. ¿Cómo se llama este error y qué falta?
> [!success]- Ver respuesta
> Es el antipatrón **clase Dios**: una clase "Sistema/Procesador" que hace todo. **El sistema NO es una clase**: hay que **repartir responsabilidades** entre las clases del dominio. Faltan **clases del dominio** (Cámara, Infracción, Foto…), probablemente también **atributos relevantes** (velocidad máxima, datos del vehículo, ubicación del radar) y **relaciones** necesarias (ej.: Radar ↔ Multa) con su cardinalidad.

**9.** Un diagrama de clases inventa una clase `Usuario` con `login` y `password`, pero el enunciado de los radares no pide gestión de usuarios. ¿Qué error es?
> [!success]- Ver respuesta
> Es **inventar clases ajenas al dominio**. No hay que agregar clases (ni login) que el enunciado no pide; el modelo debe ceñirse al **dominio del problema**.

**10.** En un diagrama de secuencia, un objeto se etiqueta `Radar` (sin dos puntos), el "Sistema" aparece como participante, y una búsqueda de la multa por patente se modela con **un solo mensaje**. ¿Qué tres cosas corregís?
> [!success]- Ver respuesta
> - **Falta la sintaxis con dos puntos:** debe ser `nombre:Clase` o `:Clase` (ej.: `:Radar`).
> - **El sistema no puede ser actor ni clase:** repartir sus responsabilidades en clases concretas (Radar, Cámara, etc.).
> - **Falta un `loop`:** una **búsqueda que recorre elementos** es una iteración ("mirar uno por uno"), no un único mensaje. Y cada mensaje debe corresponder a un **método** de la clase receptora.

---
🔙 Volver al índice: [[Análisis de Sistemas - Índice]]
