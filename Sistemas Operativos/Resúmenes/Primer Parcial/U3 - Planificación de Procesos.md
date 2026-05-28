---
title: "U3 — Planificación de Procesos y Algoritmos de Planificación"
aliases:
  - "Unidad 3"
  - "Planificación de Procesos"
tags:
  - sistemas-operativos
  - primer-parcial
  - unidad-3
parcial: "Primer Parcial"
unidad: 3
clases: "Clases 3-4"
fuente: "Transcripciones Prof. Arzubi"
---

# U3 — Planificación de Procesos y Algoritmos de Planificación

> [!info] De qué trata
> Esta unidad se centra en cómo el sistema operativo decide qué proceso se ejecuta, cuándo y con qué criterio. El profe hace hincapié en que el SO es un **software determinístico**: no puede "elegir lo que quiera", siempre necesita un algoritmo concreto que determine qué proceso poner a ejecutar. Hay tres planificadores (largo, mediano y corto plazo), varios criterios de selección conceptuales, y un conjunto de algoritmos concretos que se aplican según el tipo de proceso (batch o interactivo).

---

## Los Tres Planificadores

Un proceso va pasando por distintos estados a lo largo de su ciclo de vida. Los ==planificadores== son los módulos del SO que toman las decisiones en cada transición relevante. Hay tres:

### Planificador de Largo Plazo

- Actúa sobre la transición **nuevo → listo**: decide qué procesos pasan de la cola de nuevo a la cola de listo (es decir, cuáles se cargan en memoria principal para estar disponibles para ejecución).
- Se invoca con poca frecuencia (puede tardar segundos o minutos entre invocaciones). Por eso no es tan crítico en cuanto a velocidad.
- Además de elegir qué proceso poner en la cola de listo, **controla el grado de multiprogramación**: no puede sobrecargar ni subcargar el sistema. Si hay poca multiprogramación (muchos procesos bloqueados, pocos listos), puede optar por poner 2 procesos chicos en vez de 1 grande, o viceversa, para mantener un nivel adecuado.

### Planificador de Mediano Plazo

- Actúa sobre los procesos que están en estado **bloqueado**, realizando el *swapping* (intercambio): saca algunos procesos bloqueados de la memoria principal llevándolos a bloqueado-suspendido, y permite que otros procesos entren a la cola de listo.
- **¿Para qué?** Para mantener un adecuado grado de multiprogramación. Si hay muchos procesos bloqueados y pocos listos, la CPU queda ociosa. El planificador de mediano plazo resuelve esto liberando espacio en memoria para procesos que sí pueden ejecutarse.
- No actúa sobre todos los bloqueados, sino solo sobre algunos, según la necesidad.

### Planificador de Corto Plazo

- Actúa sobre la cola de **listo**, eligiendo qué proceso pasa a ejecución.
- Es el **más crítico y exigente** de los tres: repite su actividad permanentemente, potencialmente millones de veces en pocos minutos.
- Por eso los algoritmos que usa deben ser económicos (pocas líneas de código, poca memoria). No se busca la decisión "perfecta" sino una decisión buena, tomada muy rápido y repetida muchas veces.

> [!important] El profe recalca
> El planificador de corto plazo es el más crítico porque su actividad es permanente y repetitiva. Cada vez que se toma una decisión, se repite un millón de veces en el tiempo. El objetivo no es tomar la mejor decisión posible, sino tomar una decisión buena a bajo costo computacional.

---

## Concepto de Ráfaga (Burst)

Antes de hablar de criterios y algoritmos, el profe introduce la idea de **ráfaga**: cada vez que un proceso toma la CPU y ejecuta líneas de código hasta que hace una llamada al sistema (por entrada/salida u otro motivo), eso es una ráfaga de ejecución. Luego viene el tiempo de E/S, luego otra ráfaga, etc.

> [!example] Ejemplo del profe
> Proceso A: ráfaga de ejecución 50 ms → E/S 30 ms → ejecución 10 ms → termina.
> Proceso B: ejecución 10 ms → E/S 20 ms → ejecución 20 ms → termina.
> Estos son los datos con los que se trabajan los ejercicios de los algoritmos.

---

## Criterios de Planificación

Antes de ver los algoritmos concretos, el profe distingue entre **criterio** (qué se quiere lograr, nivel conceptual) y **algoritmo** (el procedimiento concreto para lograrlo). Repasa varios criterios posibles:

### Maximizar la utilización de la CPU

Si el objetivo es que la CPU ejecute la mayor cantidad de líneas de código posible, conviene elegir procesos con **mucha ejecución y poca E/S** (procesos batch que no interrumpen con llamadas al sistema frecuentemente). Un proceso que pide E/S todo el tiempo desperdicia tiempo de CPU.

### Maximizar la productividad (*throughput*)

==Productividad== = cantidad de producto (procesos completados) por unidad de tiempo. Si el objetivo es ejecutar más procesos por unidad de tiempo, conviene cargar **procesos cortos**, ya que se completan más rápido y el contador de procesos completados sube más.

### Minimizar el tiempo medio de espera

El ==tiempo de espera== de un proceso es cuánto tiempo estuvo esperando en la cola de listo sin ejecutarse. El tiempo medio de espera es el promedio de todos los procesos.

Dado el orden A, B, C con ráfagas de duración $a$, $b$, $c$:

$$\text{Tiempo medio de espera} = \frac{0 + a + (a+b)}{3} = \frac{2a + b}{3}$$

> [!example] Ejemplo del profe
> Si tengo 3 procesos con ráfagas A, B, C (en ese orden), el proceso A no espera nada, el B espera el tiempo de A, y el C espera el tiempo de A+B. La fórmula muestra que **A pesa el doble que B en el promedio**, porque aparece en dos términos. Conclusión: si quiero minimizar el tiempo medio de espera, conviene poner **primero el proceso más corto**. Eso llevaría al uso del algoritmo SJF.

> [!important] El profe recalca
> "Lean las fórmulas. Es básico para un profesional. Si una fórmula es simple, léanla e interpreten qué les está diciendo. La fórmula del tiempo medio de espera ya les está diciendo que el proceso que ponen primero influye más en el promedio."

### Minimizar el tiempo de retorno / tiempo de respuesta

El profe los menciona pero aclara que no los puede calcular todavía porque dependen de cómo se administra la memoria, el sistema de archivos y otros módulos. Los define brevemente:
- **Tiempo de retorno**: desde que un proceso entra nuevo hasta que sale por terminado.
- **Tiempo de respuesta**: desde que se hace la solicitud hasta que se empieza a recibir la respuesta.

> [!warning] Ojo / suele tomar
> El profe hace hincapié en que los términos como "tiempo de retorno" o "tiempo de respuesta" pueden tener distintas interpretaciones según quién los use. En una reunión de trabajo, hay que ponerse de acuerdo en qué significa cada término antes de asumir. **Nunca imaginar ni sobreentender.**

### Progreso Concurrente

> [!example] Ejemplo del profe
> Proceso A (ráfaga 50 ms → E/S 30 ms → ráfaga 10 ms) y Proceso B (ráfaga 10 ms → E/S 20 ms).
> Si se ejecutan en el orden A primero, B después → el tiempo total es 140 ms.
> Si se ejecutan en el orden B primero, A después → el tiempo total es 100 ms.
> Esto se llama **progreso concurrente**: el orden de ejecución de los procesos puede afectar significativamente el tiempo total, dependiendo de cuándo cada proceso hace E/S. No siempre se puede aprovechar (requeriría conocer de antemano las ráfagas y tiempos de E/S de cada proceso), pero es importante saber que existe.

---

## Planificación con y sin Desalojo

Antes de ver los algoritmos, el profe introduce una distinción fundamental:

> [!note] Definición
> - **Sin desalojo (non-preemptive / no apropiativo)**: una vez que un proceso está en ejecución, el sistema operativo **no lo interrumpe** por la llegada de otro proceso. Solo se libera la CPU cuando el proceso termina su ráfaga o va a bloqueado voluntariamente. Las interrupciones de llegada de nuevos procesos son **enmascarables** (se atienden en el próximo overhead, no en el momento).
> - **Con desalojo (preemptive / apropiativo)**: cuando llega un proceso a la cola de listo, el sistema operativo **interrumpe el proceso en ejecución** para reevaluar prioridades. La interrupción es no enmascarable. Implica una prioridad más estricta.

---

## Algoritmos de Planificación

### FIFO / FCFS — Primero en llegar, primero en ser atendido

- ==FIFO== (First In, First Out) / ==FCFS== (First Come, First Served): se atienden los procesos **en el orden en que llegaron** a la cola de listo.
- **Sin desalojo**: la llegada de un nuevo proceso genera una interrupción enmascarable; no se interrumpe el proceso en ejecución.
- **Ventaja**: extremadamente simple y económico. El SO solo saca el primero de la fila. Casi sin consumo de overhead para tomar la decisión.
- **Desventaja**: resultado impredecible. A veces da buenos tiempos, a veces malos. No optimiza ningún criterio en particular.

> [!important] El profe recalca
> "FIFO está siempre presente, en sistemas operativos y en cualquier situación. Con solo saber este algoritmo ya se anotan al menos un cuarto de punto. Hay que tenerlo siempre presente."

> [!example] Ejemplo del profe — Ejercicio FIFO
> - Proceso A llega en T=0: ráfaga 100 ms → E/S 30 ms → ráfaga 10 ms.
> - Proceso B llega en T=20: ráfaga 20 ms → E/S 20 ms → ráfaga 20 ms.
> - Overhead estándar: 10 ms.
>
> Mecánica: A se pone a ejecutar en T=0 (rutina 1 y 2). En T=20 llega B pero como es sin desalojo, la interrupción es enmascarable → A sigue. A completa su primera ráfaga de 100 ms y va a bloqueado (rutina 4). En el overhead se atiende la llegada de B: se carga en listo y se pone a ejecutar (rutinas 1 y 2). B ejecuta 20 ms, va a bloqueado (E/S 20 ms). Cuando A termina su E/S, pasa a listo y se ejecuta (10 ms, termina con rutina 6). Luego B sale de bloqueado, ejecuta 20 ms más y termina.

---

### SJF — Shortest Job First (proceso más corto primero)

- ==SJF== (Shortest Job First): elige el proceso que tiene la **próxima ráfaga más corta**.
- Para procesos **batch** (sin intervención del usuario).
- **Sin desalojo**: la prioridad se basa en la próxima ráfaga, pero no se interrumpe al proceso en ejecución.
- **Problema práctico**: requiere conocer de antemano el tamaño de la próxima ráfaga, lo cual generalmente no es posible. Solución: **estimarlo** a partir de la historia del proceso.

#### Estimación de la próxima ráfaga

El profe presenta una fórmula para estimar el tamaño de la próxima ráfaga ($T_{n+1}$) en función del historial:

$$T_{n+1} = T_n \cdot (1 - \alpha) + \bar{T} \cdot \alpha$$

Donde:
- $T_n$ = tiempo de la última ráfaga.
- $\bar{T}$ = promedio de todas las ráfagas anteriores.
- $\alpha$ = valor entre 0 y 1 que controla el peso relativo.
  - Si $\alpha = 0$: solo usa la última ráfaga ($(1-0) \cdot T_n + \bar{T} \cdot 0 = T_n$).
  - Si $\alpha = 1$: solo usa el promedio ($(1-1) \cdot T_n + \bar{T} \cdot 1 = \bar{T}$).
  - Si $\alpha = 0.8$: el promedio tiene peso 80%, la última ráfaga peso 20%.

> [!example] Ejemplo del profe
> "Es como si dijera: hay un alumno que viene aprobando todas las materias con 6, 7, 8. ¿Qué estimamos que sacará en Sistemas Operativos? Un 6, 7 u 8. Si viene aplazado y rindiendo 20 veces cada materia, estimamos que le va a pasar lo mismo. Eso es estimar en base a la historia de cada uno."

> [!example] Ejemplo del profe — Ejercicio SJF
> - Proceso A llega en T=0: ráfaga 20 ms → E/S 50 ms → ráfaga 10 ms → E/S → ráfaga 20 ms.
> - Proceso B llega en T=20: ráfaga 20 ms → E/S 20 ms → ráfaga 20 ms → E/S → ráfaga 10 ms.
>
> Mecánica: A empieza a ejecutar. En T=20 llega B pero es sin desalojo → se enmascara. A termina su ráfaga (20 ms) y va a bloqueado. En el overhead se carga B, se pone a ejecutar. B ejecuta 20 ms, va a bloqueado. Después los 2 salen de bloqueado al mismo tiempo → decisión SJF: ráfaga restante de A = 10 ms, de B = 20 ms → se elige A. A ejecuta 10 ms, va a bloqueado. B se ejecuta 20 ms, va a bloqueado. Y así sucesivamente, siempre eligiendo el de próxima ráfaga más corta.

---

### SRTF — Shortest Remaining Time First (menor tiempo restante primero)

- ==SRTF== (Shortest Remaining Time First): igual que SJF pero **con desalojo**.
- Cuando llega un proceso a la cola de listo, se interrumpe el proceso en ejecución para comparar: ¿la ráfaga **remanente** del proceso en ejecución es menor o mayor que la ráfaga del recién llegado?
- Se elige siempre el de menor ráfaga **restante** (lo que queda de ejecutar, no lo total).
- Al ser con desalojo: la llegada de un nuevo proceso genera una interrupción **no enmascarable** → se interrumpe inmediatamente, el proceso vuelve a la cola de listo con rutina 3.

> [!warning] Ojo / suele tomar
> La diferencia clave entre SJF y SRTF: SJF mira la próxima ráfaga completa (sin desalojo), SRTF mira el tiempo **restante** de la ráfaga actual (con desalojo). "La única forma de hablar de tiempo restante es que la ráfaga haya sido interrumpida, y la única forma de interrumpirla es con desalojo."

> [!example] Ejemplo del profe — Ejercicio SRTF
> - Proceso A llega en T=0: ráfaga 20 ms → E/S 30 ms → ráfaga 10 ms.
> - Proceso B llega en T=20: ráfaga 20 ms → E/S 20 ms → ráfaga 20 ms → E/S 10 ms → ráfaga.
>
> En T=0, A empieza a ejecutar. En T=20 llega B → es con desalojo → se interrumpe A. A ejecutó 10 ms, le quedan 10. B tiene ráfaga de 20. Menor ráfaga remanente = A (10 ms) → se pone a ejecutar A nuevamente. A completa los 10 ms, va a bloqueado (E/S 30 ms). Se pone B a ejecutar (20 ms), va a bloqueado. Cuando A sale de bloqueado, se pone a ejecutar (10 ms de ráfaga). Cuando B sale de bloqueado, vuelve a la cola, se compara con A si es necesario. Y así.

---

### SPN — Shortest Process Next (proceso total más corto primero)

- ==SPN== (Shortest Process Next): no mira la próxima ráfaga, sino el **tiempo total de ejecución** del proceso (suma de todas sus ráfagas de CPU, sin contar E/S).
- **Sin desalojo** y la prioridad asignada es **fija** durante todo el ejercicio: se calcula una sola vez al inicio sumando todas las ráfagas.
- El proceso con menor tiempo total tiene prioridad 1, el siguiente prioridad 2, etc.
- Como es un algoritmo por prioridades, también tiene **riesgo de inanición** → se debe combinar con envejecimiento.

> [!example] Ejemplo del profe — Ejercicio SPN (3 procesos)
> - Proceso A: ráfagas 30+30 = **60 ms** → prioridad 3.
> - Proceso B: ráfagas 10+20 = **30 ms** → prioridad 1.
> - Proceso C: ráfagas 20+20 = **40 ms** → prioridad 2.
>
> Mecánica: A llega en T=0, se ejecuta (sin desalojo, 30 ms), va a bloqueado. En el overhead llega B (T=20, enmascarado). B se ejecuta (prioridad 1, 10 ms), va a bloqueado. En el siguiente overhead aparecen A (bloqueado→listo) y C (cola nuevo→listo). Hay que elegir entre A (prioridad 3) y C (prioridad 2) → se elige C. C ejecuta 20 ms, va a bloqueado. B completa su E/S, pasa a listo → se compara A (prioridad 3) vs B (prioridad 1) → B ejecuta 20 ms, termina. Luego C (prioridad 2) completa, y finalmente A (prioridad 3).

---

### Algoritmo de Prioridades y el Problema de Inanición

Cualquier algoritmo que maneje prioridades (SJF, SRTF, SPN) comparte un problema:

> [!note] Definición — Inanición
> ==Inanición==: cuando un proceso de baja prioridad nunca (o muy tardíamente) recibe el uso de la CPU porque siempre llegan nuevos procesos de mayor prioridad. Ocurre porque el SO trabaja con actividades repetitivas: los procesos entran y salen permanentemente. Si siempre hay procesos de prioridad 1, los de prioridad 3 nunca entran.

> [!example] Ejemplo del profe
> "Si tengo 37 alumnos y establezco un orden alfabético para tomar horario, no hay riesgo de inanición: la cantidad es fija y todos serán atendidos. Pero si a medida que atiendo alumnos van entrando nuevos con nombre que empieza con A, los que empiezan con Z nunca llegan. Eso es inanición."

> [!note] Definición — Envejecimiento
> ==Envejecimiento== (aging): técnica para evitar la inanición. Se agrega al algoritmo de prioridades una condición: si un proceso lleva determinado tiempo sin ser atendido, se le sube la prioridad. Por ejemplo: si un proceso con prioridad 3 lleva 60 ms sin ejecutar, pasa a prioridad 2; si sigue sin ejecutar, pasa a prioridad 1. El umbral de tiempo y los criterios los define quien escribe el sistema operativo.

> [!important] El profe recalca
> La tasa de respuesta también sirve como mecanismo de envejecimiento:
> $$\text{Tasa de respuesta} = \frac{\text{tiempo de espera} + \text{tiempo de ejecución}}{\text{tiempo de ejecución}}$$
> El valor mínimo es 1 (cuando el tiempo de espera es 0). A medida que un proceso espera más, su tasa de respuesta sube. Un algoritmo que elige siempre el proceso con **mayor tasa de respuesta** asegura atender primero al que lleva más tiempo esperando, evitando así la inanición.

---

### Round Robin — Algoritmo circular con quantum

- ==Round Robin==: algoritmo para sistemas **multiusuarios / interactivos** (no batch).
- Se le asigna un ==quantum== de tiempo a cada proceso. El sistema va recorriendo la cola de listo de manera **rotativa** y le da un quantum a cada uno.
- Si el proceso termina su ráfaga antes de agotar el quantum, libera la CPU voluntariamente. Si no, el sistema operativo lo interrumpe cuando se cumple el quantum y lo manda de vuelta a la cola de listo con **rutina 3** (a diferencia de otros casos, aquí es el SO quien interrumpe al proceso, no el proceso al SO).
- **"Sin desalojo", salvo cuando se cumple el quantum**: la llegada de un nuevo proceso durante la ejecución genera una interrupción enmascarable. El único desalojo es por vencimiento del quantum.
- El quantum es un valor fijo configurado para el ejercicio (puede ser cualquier valor).

> [!example] Ejemplo del profe
> "Los sistemas multiusuarios surgen cuando hay programas interactivos y muchos usuarios compartiendo el sistema. El objetivo ya no es maximizar el uso de la CPU (como en los sistemas batch), sino **atender a todos los usuarios de manera equitativa**. Por eso se le da un quantum igual a cada uno de manera rotativa."

> [!example] Ejemplo del profe — Ejercicio Round Robin (quantum = 30 ms)
> - Proceso A llega en T=0: ráfaga 100 ms → E/S 30 ms → ráfaga 10 ms.
> - Proceso B llega en T=20: ráfaga 20 ms → E/S 20 ms → ráfaga 20 ms.
>
> T=0: A empieza. En T=20 llega B → enmascarado (sin desalojo dentro del quantum). En T=30 se cumple el quantum → el SO interrumpe a A (rutina 3), A vuelve a la cola de listo. Se atiende la llegada de B (rutinas 1 y 2), B ejecuta 20 ms (no necesitó los 30), va a bloqueado. Vuelve A a ejecutar con un nuevo quantum de 30 ms (le quedan 70). Al cumplirse, A vuelve a la cola. B sale de bloqueado, ejecuta 20 ms, termina. A ejecuta los últimos 10 ms de su primera ráfaga, va a E/S, luego regresa y ejecuta los 10 ms finales.

---

### Colas Multinivel

- En vez de tener una sola cola de listo, se pueden tener **múltiples colas**, cada una con su propio algoritmo.
- Ejemplo con 2 colas:
  - Cola 1: procesos batch → se aplica SJF/SPN/SRTF.
  - Cola 2: procesos interactivos → se aplica Round Robin.
- El SO agrega una decisión adicional: ¿a qué cola atender? Se puede asignar un porcentaje del tiempo: por ejemplo, 30% para la cola batch (nadie espera en pantalla) y 70% para la cola interactiva.
- Se puede extender a más de 2 colas con otros criterios.

---

### Colas Multinivel con Retroalimentación (Realimentación)

- Algoritmo para procesos **batch** que combina múltiples colas con tiempos de ejecución crecientes.
- Idea conceptual: **los procesos cortos salen rápido** (atiende más procesos por ciclo en la cola de mayor prioridad), y los procesos largos reciben cada vez **más tiempo de CPU** para no tener que entrar y salir veinte veces.

**Estructura típica del ejercicio del profe (3 colas):**

| Cola | Procesos atendidos por ciclo | Tiempo de CPU por proceso |
|------|------------------------------|--------------------------|
| 1    | 3                            | 40 ms                    |
| 2    | 2                            | 80 ms                    |
| 3    | 1                            | 160 ms                   |

- El algoritmo recorre siempre en el orden: Cola 1 → Cola 2 → Cola 3 → Cola 1 → Cola 2 → Cola 3 → ...
- En cada cola atiende la cantidad de procesos que le corresponde por el tiempo asignado.
- Si un proceso no terminó su ráfaga en la cola actual, vuelve a la cola de listo con **rutina 3** y pasa a la siguiente cola en el próximo ciclo.
- Si terminó la ráfaga, va a bloqueado (rutina 4) o a terminado (rutina 6).
- Si una cola está vacía al llegar el turno, el algoritmo sigue de largo a la siguiente.

> [!warning] Ojo / suele tomar
> "En los ejercicios de cola multinivel con retroalimentación, el proceso recorre las colas (Cola 1 → Cola 2 → Cola 3) y **vuelve a la Cola 1** si sigue sin terminar. Es un ciclo continuo. Si en una cola no hay nadie, se salta y sigue. Los procesos que terminan su ráfaga en una cola salen del ciclo temporalmente (van a bloqueado o terminado) y cuando regresan, vuelven a la Cola 1."

> [!example] Ejemplo del profe — Ejercicio Cola Multinivel con Retroalimentación
> Tres procesos: A (ráfaga 290 ms), B (ráfaga 340 ms), C (ráfaga 360 ms). Ciclo 1 (Cola 1, 3 procesos, 40 ms c/u): A ejecuta 40 ms (restan 250), B ejecuta 40 ms (restan 300), C ejecuta 40 ms (restan 320). Los tres pasan a Cola 2. Ciclo 1 (Cola 2, 2 procesos, 80 ms c/u): A ejecuta 80 ms (restan 170), B ejecuta 80 ms (restan 220), pasan a Cola 3. Ciclo 1 (Cola 3, 1 proceso, 160 ms): A ejecuta 160 ms (restan 10), pasa a Cola 1. Ciclo 2 comienza: Cola 1 con D, E, F (si hubiera) y también A con 10 ms restantes. Y así sucesivamente. Los procesos más cortos terminan antes; los más largos van acumulando tiempo por ciclo.

---

## Resumen Comparativo de Algoritmos

| Algoritmo        | Tipo de proceso | Desalojo | Criterio de selección              | Riesgo de inanición |
|-----------------|----------------|----------|-------------------------------------|----------------------|
| FIFO/FCFS        | Batch           | No       | Orden de llegada                    | No                  |
| SJF              | Batch           | No       | Próxima ráfaga más corta            | Sí                  |
| SRTF             | Batch           | Sí       | Ráfaga **remanente** más corta      | Sí                  |
| SPN              | Batch           | No       | Tiempo total de ejecución más corto | Sí                  |
| Round Robin      | Interactivo     | Por quantum | Orden rotativo con quantum         | No                  |
| Cola Multinivel  | Mixto           | Depende  | Por cola y algoritmo de cada cola   | Posible             |
| C. M. Retroalim. | Batch           | Por quantum/tiempo | Ciclo de colas con tiempo creciente | No (los largos avanzan) |

---

## Mecánica General para Resolver Ejercicios

El profe enfatiza que todos los ejercicios siguen la **misma mecánica** independientemente del algoritmo. Los números de rutina del diagrama de estados son:

- **Rutina 1**: nuevo → listo (proceso se carga en memoria principal y va a la cola de listo).
- **Rutina 2**: listo → ejecución (proceso pasa a usar la CPU).
- **Rutina 3**: ejecución → listo (proceso vuelve a la cola, por quantum o desalojo).
- **Rutina 4**: ejecución → bloqueado (proceso pide E/S o llamada al sistema).
- **Rutina 5**: bloqueado → listo (proceso completó su E/S, vuelve a la cola de listo).
- **Rutina 6**: ejecución → terminado.

Reglas clave para los ejercicios:
1. **Nunca puede haber 2 procesos en ejecución al mismo tiempo** (un solo procesador).
2. **Nunca puede haber ejecución durante un overhead** (cuando el SO está haciendo tareas de administración).
3. **Sí pueden haber varios procesos haciendo E/S al mismo tiempo** (el módulo de E/S puede manejar varias cosas simultáneamente en distintos dispositivos).
4. Cada cambio de estado tiene un overhead de 10 ms (en los ejercicios del profe).
5. En los algoritmos sin desalojo, la llegada de un nuevo proceso es una interrupción **enmascarable** → no se atiende hasta el próximo overhead.
6. En los algoritmos con desalojo (SRTF), la llegada de un nuevo proceso es una interrupción **no enmascarable** → se interrumpe inmediatamente.

> [!important] El profe recalca
> "Una vez que entienden la mecánica, pueden hacer cualquier ejercicio. No son problemas de razonamiento, son mecánicos: hay que aplicar el algoritmo A, B, C repetido. Háganlos solos, sin mirar la respuesta, porque en el examen surgen todas las dudas que no se vieron al mirar el ejercicio resuelto."

---

## El Sistema Operativo es Software Determinístico

El profe cierra el tema con un recordatorio conceptual importante:

> [!important] El profe recalca
> El SO es **software determinístico**: ante cada situación siempre sabe exactamente qué hacer. Quien escribe el SO elige un algoritmo y ese algoritmo se aplica siempre. No hay "que haga lo que quiera". Todos los criterios y algoritmos que vimos son formas de darle una instrucción precisa al planificador sobre qué proceso poner a ejecutar y cuándo.

---

## Enlaces

- [[U2 - Administración de Procesos]]
- [[U4 - Recursos Compartidos, Sincronización y Comunicación]]
