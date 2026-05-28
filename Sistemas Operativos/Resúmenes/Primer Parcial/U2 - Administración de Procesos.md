---
title: "U2 — Administración de Procesos"
aliases:
  - "Unidad 2"
  - "Procesos"
tags:
  - sistemas-operativos
  - primer-parcial
  - unidad-2
parcial: "Primer Parcial"
unidad: 2
clases: "Clases 2-3"
fuente: "Transcripciones Prof. Arzubi"
---

# U2 — Administración de Procesos

> [!info] De qué trata
> Esta unidad explica qué es un proceso, cómo se diferencia de un programa, y cuál es el ciclo de vida completo que atraviesa un proceso desde que nace hasta que muere. También cubre los hilos (threads) como unidad mínima de ejecución dentro de un proceso, y los planificadores que toma decisiones sobre qué proceso pasa a cada estado. Todo esto sienta las bases para entender los algoritmos de planificación de la Unidad 3.

---

## Programa vs. Proceso

> [!note] Definición
> Un ==programa== es una secuencia de líneas de código almacenada en la **memoria secundaria** (disco). El programa está siempre ahí y no desaparece.
> Un ==proceso== es ese mismo programa al que el sistema operativo le **agrega el Bloque de Control de Proceso (BCP)**. Recién ahí, con BCP incluido, hablamos de proceso. Los procesos **nacen y mueren**; el programa permanece.

El profe lo dice así: el programa está siempre en el disco. Cuando alguien quiere ejecutarlo, el sistema operativo toma ese programa, le coloca el BCP y lo pone en la cola de **Nuevo**. Ahí nació el proceso. Cuando termina de ejecutarse, el sistema le saca la información estadística y de uso que necesite, y el proceso muere. El programa sigue intacto en la memoria secundaria.

> [!important] El profe recalca
> No es lo mismo un programa que un proceso. Un programa es una entidad pasiva guardada en disco. Un proceso es una entidad activa: el programa más todo lo que el sistema operativo necesita para administrarlo (el BCP).

---

## Bloque de Control de Proceso (BCP)

> [!note] Definición
> El ==Bloque de Control de Proceso (BCP)== es el espacio que el sistema operativo agrega al programa para convertirlo en proceso. Contiene toda la información que el SO necesita para administrar ese proceso.

Información que puede contener el BCP (el profe menciona estos como ejemplos):

- Nombre del proceso
- Tamaño del proceso
- Dueño del proceso
- Prioridad del proceso
- Memoria que ocupa
- Archivos que necesita
- Información de uso durante la ejecución (tiempo de CPU consumido, cantidad de transacciones, memoria usada, etc.) — útil para estadísticas, facturación de servicios, análisis de performance

> [!note] Definición
> El que escribe el sistema operativo decide qué información pone en el BCP según cómo imagina administrar los procesos. No hay un BCP único universal: cada SO lo diseña según sus objetivos.

---

## Hilos (Threads)

> [!note] Definición
> Un ==hilo== (thread) es la **menor unidad de ejecución**. Un proceso puede estar formado por uno o varios hilos. Cada hilo tiene su propio **Bloque de Control de Hilo**, que contiene información mucho más acotada que el BCP: registros, contador de programa (próxima instrucción a ejecutar) y pila.

### ¿Qué comparten los hilos de un mismo proceso?

Los hilos de un mismo proceso comparten **código, datos y recursos**. Esto es lo que los hace diferentes a los procesos entre sí: dos procesos distintos no comparten nada.

> [!important] El profe recalca
> Esto es importante: los hilos comparten código, datos y recursos **dentro del mismo proceso**. No hay que confundir hilos de distintos procesos.

### ¿Por qué usar hilos?

- El cambio de contexto entre un hilo y otro es **más económico** que entre procesos, porque ya tienen código, datos y recursos compartidos. Al cambiar de hilo solo hay que cargar los registros y el contador de programa.
- Un proceso con hilos permite ejecutar partes del código en **cualquier orden o simultáneamente** (si el procesador lo soporta), lo que acelera la ejecución. Condición: esas partes del código tienen que poder ejecutarse independientemente, sin orden forzado.

> [!example] Ejemplo del profe
> Si el proceso tiene 3 hilos, es porque el código puede dividirse en 3 grupos que se pueden ejecutar en cualquier momento, incluso haciendo multiprogramación entre los 3. Si hubiera que ejecutarlos en orden secuencial estricto, no tendría sentido usar hilos.

### Implementación de hilos: ¿quién los administra?

Los hilos se pueden implementar a **nivel de usuario** o a **nivel del sistema operativo**:

| | Nivel de usuario | Nivel del SO |
|---|---|---|
| **¿Quién gestiona?** | El propio programador | El sistema operativo |
| **Velocidad de cambio** | Muy rápido (no hay llamada al SO) | Más lento (hay overhead del SO) |
| **En sistema multiusuario (quantum)** | El proceso completo recibe **un solo quantum** (el SO no ve los hilos) | Cada hilo recibe **su propio quantum** |

> [!important] El profe recalca
> Si lo administra el usuario: el cambio de hilo es extremadamente rápido porque no interrumpe nada. Pero en un sistema multiusuario, el SO le da un solo quantum al proceso y el usuario tiene que repartirlo entre todos sus hilos.
> Si lo administra el SO: a cada hilo le da un quantum de tiempo, como si fuera un proceso independiente. Más tiempo de CPU total para el proceso, pero con overhead en cada cambio.

El profe aclara que en la práctica esto lo deciden los lenguajes de programación y el SO que uno tenga. Lo importante es saber que existe esta diferencia.

> [!note] Definición
> A partir de acá el profe dice que va a hablar de "procesos" para simplificar, englobando también a los hilos. El concepto y los estados que siguen aplican igual para ambos.

---

## Estados de un proceso y ciclo de vida

El profe plantea el ciclo de vida como un diagrama de estados. Todo proceso nace en **Nuevo** y muere en **Terminado**.

### Los estados

| Estado | Descripción |
|---|---|
| **Nuevo** | El proceso acaba de crearse (el SO le agregó el BCP). Está en la cola de nuevo, en la memoria secundaria, todavía no cargado en RAM. |
| **Listo** | El proceso está en memoria principal y **en condiciones de ejecutarse**. Está esperando que el SO le asigne la CPU. |
| **En ejecución** | El proceso está usando la CPU. Con un solo procesador, solo puede haber un proceso en este estado a la vez. |
| **Bloqueado** | El proceso necesita algo (datos de E/S, recursos) y no puede seguir ejecutándose hasta que llegue. Se saca de la CPU pero **no** va a la cola de Listo, porque no está en condiciones de ejecutarse. |
| **Terminado** | El proceso terminó su ejecución. El SO extrae toda la información estadística y de uso, y luego el proceso muere definitivamente. |

### Transiciones posibles

```
Nuevo ──(planificador largo plazo)──► Listo
Listo ──(planificador corto plazo / despachador)──► En ejecución
En ejecución ──(proceso termina)──► Terminado
En ejecución ──(necesita E/S o recurso)──► Bloqueado
En ejecución ──(se agotó el quantum / llega proceso de mayor prioridad)──► Listo
Bloqueado ──(llegaron los datos / se satisfizo el recurso)──► Listo
```

> [!example] Ejemplo del profe
> El proceso está en ejecución y necesita "los datos de González". Hace una llamada al SO (interrupción no enmascarable). El SO lo saca de la CPU y lo manda a **Bloqueado** porque no puede ejecutarse sin esos datos. Toma otro proceso de Listo y lo pone a ejecutar. Cuando los datos de González llegan a memoria principal, el proceso bloqueado vuelve a **Listo** (ya está en condiciones de ejecutarse).

> [!warning] Ojo / suele tomar
> Un proceso bloqueado **no va a Listo** directamente mientras espera. Recién vuelve a Listo cuando se satisface lo que estaba esperando. Esta distinción entre Bloqueado y Listo es fundamental.

### Estados extendidos: Suspendido Bloqueado y Suspendido Listo

No todos los SO los tienen, pero los principales sí. El problema que resuelven: si tengo muchos procesos bloqueados y pocos en Listo, el **grado de multiprogramación** baja (no hay procesos en condiciones de ejecutarse, la CPU se desaprovecha).

Solución — el SO puede hacer **swap** (intercambio): sacar procesos bloqueados de la memoria principal y mandarlos a la memoria secundaria, liberando espacio para cargar procesos que sí puedan ejecutarse.

| Estado adicional | Descripción |
|---|---|
| **Suspendido Bloqueado** | Proceso que estaba bloqueado y fue sacado a memoria secundaria (swap out). Sigue esperando que llegue lo que necesitaba. |
| **Suspendido Listo** | Cuando llega lo que el proceso necesitaba (ej: los datos de González), pasa de Suspendido Bloqueado a Suspendido Listo. Desde acá, cuando haya lugar en memoria principal, entra a la cola de Listo con **prioridad sobre los que vienen de Nuevo** (ya estaba en ejecución antes). |

> [!important] El profe recalca
> El objetivo del swap y de los estados suspendidos es mantener un **adecuado grado de multiprogramación**: tener siempre suficientes procesos en condiciones de ejecutarse para que la CPU no quede ociosa.

---

## Los tres planificadores

El SO usa planificadores (schedulers) para tomar las decisiones de qué proceso pasa a cada estado. Son tres:

### Planificador de largo plazo

- Decide qué proceso de la cola de **Nuevo** pasa a la cola de **Listo** (es decir, cuál se carga en memoria principal).
- Se llama "de largo plazo" porque actúa **con menos frecuencia** que los otros.
- También participa en el **grado de multiprogramación**: evalúa cuántos procesos hay en Listo y decide si carga uno grande o dos chicos, para mantener un grado adecuado.

### Planificador de corto plazo (el más importante)

- Decide qué proceso de la cola de **Listo** pasa a **Ejecución**.
- Es el **más exigente y crítico** porque actúa permanentemente: está cargando y sacando procesos del procesador todo el tiempo.
- Aplica el algoritmo de planificación que haya determinado el que diseñó el SO.
- Actúa a través del **despachador** (dispatcher), que es quien efectivamente coloca y saca procesos del estado de ejecución.

### Planificador de mediano plazo

- Administra el **swap** (intercambio entre memoria principal y secundaria).
- Actúa sobre los procesos **bloqueados**: cuando el grado de multiprogramación baja demasiado, saca procesos bloqueados a memoria secundaria (Suspendido Bloqueado) para liberar espacio y cargar procesos que sí pueden ejecutarse.
- Su objetivo: mantener un **adecuado grado de multiprogramación**.

> [!important] El profe recalca
> El planificador de corto plazo es el más exigente porque repite su actividad permanentemente. El de largo plazo es el más tranquilo porque interviene con menos frecuencia.

> [!warning] Ojo / suele tomar
> Los tres planificadores son **determinísticos**: no eligen al azar. El que escribe el SO define un criterio y el planificador lo aplica siempre. No existe el "hace lo que quiere".

---

## Estructuras de control del SO

Todo SO tiene tablas obligatorias para poder funcionar. El profe las menciona como algo obvio que cualquier SO debe tener:

- **Tablas de procesos** — información de todos los procesos activos
- **Tablas de memoria** — qué hay cargado en memoria principal
- **Tablas de archivos** — archivos abiertos / en uso
- **Tablas de E/S** — estado de los dispositivos de entrada/salida

El BCP de cada proceso, las pilas, las colas y demás estructuras internas dependen de cómo lo haya diseñado quien escribió el SO.

---

## Comunicación entre procesos

> [!warning] Ojo / suele tomar
> El profe menciona que "comunicación entre procesos" **no se va a ver en esta unidad**: quedó para la siguiente clase junto con los criterios y algoritmos de planificación. No forma parte del contenido de la U2 tal como fue desarrollado en clase.

---

## Grado de multiprogramación

> [!note] Definición
> El ==grado de multiprogramación== es la cantidad de procesos que están alternando en el uso del procesador (es decir, los que están en condiciones de ejecutarse: en Listo o en Ejecución). No cuentan los bloqueados porque no pueden ejecutarse.

- A mayor cantidad de procesos en condiciones de ejecutarse, mayor aprovechamiento de la CPU.
- La memoria principal limita el grado: si la RAM es chica, caben pocos procesos.
- Demasiados procesos bloqueados = bajo grado de multiprogramación = CPU desaprovechada. Para esto existe el planificador de mediano plazo y el swap.

---

## Resumen del ciclo de vida (diagrama conceptual)

```
[Memoria secundaria]
     Programa
        │
        │ (SO agrega BCP)
        ▼
    [NUEVO] ──(planif. largo plazo)──► [LISTO]
                                          │
                                          │ (planif. corto plazo / despachador)
                                          ▼
                                    [EN EJECUCIÓN]
                                    /      |      \
                          (termina) |  (quantum   | (necesita
                                    |   agotado   |  E/S o
                                    ▼   o prio.)  |  recurso)
                               [TERMINADO]  │     ▼
                                    ↑       └► [LISTO]   [BLOQUEADO]
                                    │                        │
                           (SO extrae                        │ (llegan datos)
                            estadísticas                     ▼
                            y muere)                     [LISTO]

  (swap out) ──────────────────────────────────────► [SUSPENDIDO BLOQUEADO]
                                                              │ (llegan datos)
                                                              ▼
                                                      [SUSPENDIDO LISTO]
                                                              │ (hay lugar en RAM)
                                                              ▼
                                                          [LISTO]
```

---

## Enlaces

- [[U1 - Introducción a los Sistemas Operativos]]
- [[U3 - Planificación de Procesos]]
