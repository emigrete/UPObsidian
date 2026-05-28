---
title: "U7 — Memoria Virtual y Algoritmos de Reemplazo de Páginas"
aliases:
  - "Unidad 7"
  - "Memoria Virtual"
tags:
  - sistemas-operativos
  - segundo-parcial
  - unidad-7
parcial: "Segundo Parcial"
unidad: 7
clases: "Clases 8-10"
fuente: "Transcripciones Prof. Arzubi"
---

# U7 — Memoria Virtual y Algoritmos de Reemplazo de Páginas

> [!info] De qué trata
> La Unidad 7 elimina la restricción de la Unidad 6: ya no es necesario cargar el proceso **completo** en memoria principal para ejecutarlo. Con memoria virtual, se carga solo la parte que se está usando en cada momento; el resto permanece en disco. La unidad cubre el concepto de memoria virtual, la paginación por demanda, el fallo de página, la fórmula de tiempo de acceso efectivo, los algoritmos de reemplazo de páginas (FIFO, Óptimo, LRU, Aproximación LRU, Segunda Oportunidad, Segunda Oportunidad Mejorado y Conteo) y las formas de asignar frames a los procesos.

---

## 1. Contraste con la Unidad 6 — ¿Qué cambia?

En la Unidad 6, para cualquier esquema de administración de memoria (particiones múltiples fijas, variables, paginación, segmentación), la condición era:

> El proceso debe cargarse **completo** en memoria principal, ejecutarse sin hacer swap, y recién al terminar se lo puede sacar.

Con memoria virtual esa restricción desaparece.

> [!note] Definición
> **Memoria virtual**: mecanismo por el cual los procesos no necesitan estar cargados completos en la RAM para ejecutarse. Solo la parte que se está ejecutando en cada momento debe estar en memoria principal; el resto puede estar en memoria secundaria (disco). A medida que se necesita, se va cargando.

> [!important] El profe recalca
> La CPU **siempre** trabaja con la memoria principal. La CPU no trabaja directamente con la memoria secundaria. Lo que cambia es que yo cargo en RAM solo la parte del proceso que estoy ejecutando; lo que no necesito queda en disco y se trae cuando haga falta.

**¿Para qué sirve esto?** El beneficio principal está en la **multiprogramación y el multiprocesamiento**: si cada proceso ocupa solo una parte de la RAM, puedo tener muchos más procesos cargados simultáneamente. Esto mejora el aprovechamiento de la CPU.

> [!important] El profe recalca
> El tamaño de la RAM **siempre importa**. Si bien la memoria virtual extiende "lógicamente" el espacio disponible con el disco, una RAM más grande permite cargar más procesos (aunque sea parcialmente), lo que mejora la multiprogramación y el multiprocesamiento. No comprar más RAM porque "ya tengo memoria virtual" es un error conceptual.

---

## 2. Sistema de administración utilizado: Paginación

La memoria virtual trabaja fundamentalmente con el sistema de administración de memoria por **paginación**. El profe explica por qué:

> [!example] Ejemplo del profe
> Imaginen que tienen que construir una pared con ladrillos. Si usan ladrillos normalizados (todos iguales), agarran cualquiera y lo ponen. Si usan ladrillos de distinto tamaño (como en segmentación), cada vez que agregan uno tienen que encontrar el que coincide con el hueco que quedó. La administración de piezas iguales es mucho más simple. Por eso la paginación (piezas iguales) es la que adoptó la memoria virtual.

La segmentación también se ha usado en alguna medida, pero la paginación es lo más eficiente y lo que se utiliza en la práctica.

### Repaso rápido de paginación (base para memoria virtual)

- La memoria se divide en bloques del **mismo tamaño**, llamados ==frames== en memoria principal y ==páginas== en disco/memoria secundaria. Es lo mismo en tamaño; el nombre cambia solo para saber de qué se habla.
- Cada proceso tiene su propia ==tabla de páginas==, que vincula el número de página (dirección lógica) con el número de frame donde está cargada (dirección física).
- La ==dirección lógica== tiene 2 partes: **número de página** y **desvío** (desplazamiento dentro de la página).
- La ==dirección física== también tiene 2 partes: **número de frame** y **desvío**. El desvío es el mismo en la dirección lógica y física (porque página y frame tienen el mismo tamaño, no hay que calcularlo).
- La ==fragmentación interna== es inevitable: como los bloques son de tamaño fijo y se asignan enteros, la última página de un proceso casi siempre queda parcialmente vacía. Ese espacio sobrante es fragmentación interna: fue asignado al proceso pero el proceso no lo puede usar.

---

## 3. Paginación por demanda (Demand Paging)

En la Unidad 6 (paginación simple), el proceso se cargaba completo: todas sus páginas tenían entrada en la tabla de páginas y cada una apuntaba a un frame. En la Unidad 7:

> [!note] Definición
> **Paginación por demanda**: variante de paginación utilizada en memoria virtual. El proceso **no se carga completo**: solo se cargan algunas páginas. La tabla de páginas existe completa (tiene todas las páginas del proceso), pero la mayoría de ellas no están cargadas en RAM. Cada entrada de la tabla tiene un ==bit de validez==: **1** si la página está en memoria principal, **0** si no está (está en disco o no fue cargada aún).

Cuando el proceso intenta acceder a una página cuyo bit de validez es 0:

> [!note] Definición
> **Fallo de página (page fault)**: evento que ocurre cuando la CPU intenta acceder a una página que no está cargada en memoria principal. El proceso se bloquea, se hace una llamada al sistema operativo, y el módulo de E/S busca esa página en disco y la trae a un frame de la memoria principal.

### ¿Dónde se carga la nueva página? — El problema del reemplazo

Una vez localizada la página en disco, hay que decidir **en qué frame ponerla**. Si hay frames libres, no hay problema. Pero si todos están ocupados, hay que **sacar** una de las páginas que ya está en memoria para hacer lugar. Esa decisión es el núcleo de esta unidad.

> [!important] El profe recalca
> La página que hay que traer es fácil de determinar: la que pidió la CPU. El problema interesante es decidir **cuál página sacar** para hacerle lugar. El criterio ideal sería sacar la que no se va a usar más, o la que se va a usar más tardíamente. Eso es lo que buscan los algoritmos de reemplazo.

---

## 4. Bit de modificación (Dirty Bit)

Además del bit de validez, la tabla de páginas puede tener un ==bit de modificación== (también llamado "dirty bit"):

- **0**: la página no fue modificada desde que se cargó (su copia en disco es idéntica).
- **1**: la página fue modificada (los datos en RAM difieren de los del disco).

Esto importa para el costo del reemplazo: si se va a sacar una página **modificada**, hay que **escribirla en disco primero** (para no perder los cambios) y después traer la nueva. Si no fue modificada, se la puede pisar directamente.

---

## 5. Fórmula del Tiempo de Acceso Efectivo (TAE)

$$TAE = T_{mem} + 2 \cdot T_{disco} \cdot p$$

donde:
- $T_{mem}$: tiempo de acceso a la memoria principal (orden de $10^{-9}$ s, nanosegundos).
- $T_{disco}$: tiempo de acceso al disco (orden de $10^{-3}$ s, milisegundos).
- $p$: probabilidad de fallo de página.
- El **factor 2** aparece porque en el peor caso hay que hacer **dos accesos al disco**: uno para guardar la página modificada que se reemplaza, y otro para traer la nueva del disco.

> [!important] El profe recalca
> El segundo término "pesa brutal". La diferencia entre $10^{-9}$ s y $10^{-3}$ s es de **seis órdenes de magnitud**. Cada fallo de página arrastra el rendimiento del sistema. Por eso reducir la cantidad de fallos de página es la gran preocupación cuando se trabaja con memoria virtual.

### Componentes del tiempo de acceso al disco

El profe detalla los 4 tiempos que forman $T_{disco}$ (los números son orientativos para ver el **peso relativo**, no actualizados):

| Componente | Descripción | Ejemplo |
|---|---|---|
| ==Tiempo de búsqueda== | Movimiento mecánico de la cabeza lectora hasta la pista | 15 ms |
| ==Tiempo de latencia== | Rotación del disco hasta que el sector queda bajo la cabeza | 8 ms |
| Tiempo de transferencia | Transmisión efectiva de los datos | 1 ms |
| Tiempo de gestión | Overhead de gestión | 1 ms |
| **Total** | | **25 ms** |

El **tiempo de búsqueda** es el que más pesa (15 de 25 ms). Cuando se vea la Unidad 8 (E/S) se verán algoritmos específicos para reducirlo.

### Cómo reducir el factor 2

El profe da dos formas de bajar ese 2:

1. **Usar el bit de modificación**: si se puede elegir entre reemplazar una página modificada y una no modificada, conviene la no modificada. En ese caso se hace un solo viaje al disco (traer la nueva, pisar la vieja), y el 2 se convierte en 1.
2. **Mantener frames libres**: si siempre hay un porcentaje de frames libre (ej: 10%), al producirse un fallo se ocupa uno libre inmediatamente (un solo viaje al disco), y la página a desalojar se manda al disco en un momento de poca actividad.

---

## 6. Factores que influyen en la probabilidad de fallo de página

### 6.1 Cantidad de frames asignados al proceso

Existe una curva (conceptual/intuitiva): a más frames asignados al proceso, menor probabilidad de fallo. El profe dice que si un proceso necesita 10 páginas y se le dan 10 frames, la probabilidad es 0 (pero eso equivale a cargarlo completo, perdiendo la ventaja de la memoria virtual).

La idea es encontrar un punto intermedio: asignar suficientes frames para que los fallos sean tolerables, pero no tantos que se reduzca la multiprogramación.

> [!warning] Ojo / suele tomar — Anomalía de Belady
> ==Anomalía de Belady==: fenómeno descubierto por Laszlo Belady al aplicar el algoritmo FIFO. Muestra que en ciertos casos, **al asignar más frames a un proceso, la cantidad de fallos de página puede aumentar** en vez de disminuir. Esto contradice la lógica intuitiva.
>
> El profe aclara que Belady no "derrumbó" la teoría: en el 99,99% de los casos más frames implica menos fallos. Simplemente encontró una secuencia específica de referencias donde FIFO (que no tiene en cuenta el uso) produce este resultado anómalo. La explicación es que FIFO solo mira cuándo entró la página, sin considerar con qué frecuencia se usa.

**Ejemplo del profe (Anomalía de Belady con FIFO):**

Secuencia de referencias: `3, 2, 1, 0, 3, 2, 4, 3, 2, 1, 2, 0, 1, 7, 0, 1`.

- Con **3 frames** → **9 fallos de página**.
- Con **4 frames** → **10 fallos de página** (¡uno más!).

### 6.2 Tamaño de la página

- Páginas **más grandes**: menos fallos de página (hay más datos próximos en la misma página), pero **más fragmentación interna** (desperdicio de la última página).
- Páginas **más pequeñas**: más fallos (hay que cambiar de página más seguido), pero menos fragmentación interna.

El profe aclara que si el tamaño de página es gigante en el límite (un proceso cabe en una sola página), los fallos son 0, pero es equivalente a cargarlo completo. Es una decisión de diseño del sistema operativo con tradeoffs inevitables.

---

## 7. Prepaginación vs. Sin prepaginación

- **Sin prepaginación**: se van cargando páginas solo cuando se producen fallos de página (no se sabe de antemano qué páginas se pedirán). Es el método que se usa en todos los ejercicios del profe.
- **Con prepaginación**: el sistema ya sabe qué páginas va a necesitar el proceso (p. ej., porque se ejecutó antes y se guardó esa información) y las carga de antemano, evitando fallos iniciales. Solo aplicable en situaciones muy específicas.

---

## 8. Algoritmos de Reemplazo de Páginas

El profe presenta los algoritmos en orden. Para cada uno describe el criterio, las ventajas, las desventajas y hace ejercicios de simulación.

**Mecánica de simulación** (idéntica para todos): se tiene una tabla donde las filas son los frames asignados al proceso, las columnas son los instantes de tiempo (cada requerimiento de página), y se marca una fila extra de fallos de página. El tiempo avanza hacia la derecha.

---

### 8.1 FIFO (First In, First Out)

> [!note] Definición
> **Algoritmo FIFO**: cuando hay fallo de página y se necesita reemplazar, se saca la página que **lleva más tiempo cargada** en memoria (la que entró primero).

**Ventaja**: administrativamente muy sencillo y económico. Solo hay que saber en qué orden entraron las páginas.

**Desventaja**: el resultado es completamente impredecible e incierto, porque no tiene en cuenta el uso. Una página puede haber entrado hace mucho tiempo pero seguir siendo muy utilizada; sacarla puede causar inmediatamente un nuevo fallo. Algunas veces saldrá bien, otras muy mal.

> [!warning] Ojo / suele tomar
> FIFO es susceptible a la **anomalía de Belady** (ver sección 6.1). Ningún otro algoritmo que considere el uso presenta esta anomalía.

**Ejercicio del profe — FIFO con 3 frames:**

Secuencia: `7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1`

Mecánica: cuando hay fallo y todos los frames están ocupados, se reemplaza el que lleva más tiempo. Para identificarlo fácilmente, se mira la columna más a la izquierda de las que están cargadas (la que lleva más ciclos).

El profe no da el total exacto de este ejercicio en clase pero lo resuelve paso a paso mostrando la mecánica.

---

### 8.2 Algoritmo Óptimo

> [!note] Definición
> **Algoritmo Óptimo**: cuando hay que reemplazar, se saca la página que **no se va a usar más** o la que se va a usar **más tardíamente** en el futuro. Como su nombre indica, produce la menor cantidad posible de fallos de página.

**Limitación**: para aplicarlo hay que conocer de antemano **todas las páginas que el proceso va a requerir** en el futuro. En la práctica, esto es prácticamente imposible:
- Solo es aplicable a procesos batch de tipo calculatorio (no interactivos, porque el usuario los lleva por caminos imprevisibles).
- Incluso en procesos batch, habría que haberlo ejecutado antes y guardado la secuencia exacta de referencias.

> [!important] El profe recalca
> Algunas bibliografías dicen que el algoritmo óptimo es "imposible de aplicar". El profe se resiste a esa palabra: prefiere decir que es **muy difícil** de aplicar. "Si fuera imposible, no sería un algoritmo; ¿para qué hacer un algoritmo que no se puede usar?" Sirve como **punto de referencia teórico**: su resultado indica el mínimo de fallos posible para una dada secuencia de referencias.

**Ejercicio del profe — Óptimo con 3 frames:**

Misma secuencia. Al producirse un fallo hay que mirar **hacia la derecha** en la secuencia para ver cuál de las páginas en memoria aparece más tarde (o no aparece). Esa es la que se saca.

El profe obtiene **9 fallos de página** (contra los del FIFO, mostrando que el óptimo produce menos).

---

### 8.3 LRU — Least Recently Used (Menos Recientemente Usado)

> [!note] Definición
> ==LRU (Least Recently Used)==: cuando hay que reemplazar, se saca la página que fue **utilizada hace más tiempo** (la menos recientemente usada). No mira cuándo entró, sino cuándo fue el último uso.

**Implementación con pila**: el algoritmo mantiene una "pila" (vector de tamaño igual al número de frames):
- Cuando se usa una página (haya o no fallo), ese número de página se coloca **arriba** de la pila, empujando los demás hacia abajo.
- Cuando hay fallo y hay que sacar: se saca la que está **abajo de la pila** (la usada hace más tiempo).
- Cuando **no** hay fallo: los frames quedan igual, pero en la pila se pone arriba la página que se acaba de usar.

**Ventaja**: tiene en cuenta el uso real, no el tiempo de entrada. Es mucho más inteligente que FIFO.

**Desventaja**: más costoso que FIFO porque requiere mantener la pila en memoria y actualizarla. Ocupa un poco más de espacio.

---

### 8.4 Algoritmo de Aproximación LRU (LRU Aproximado con Bits)

> [!note] Definición
> **Aproximación LRU**: forma más económica de implementar LRU. En vez de mantener una pila con números de páginas, se asocia a cada página cargada una cantidad fija de **bits** (ej: 6, 8, 10 bits).

**Funcionamiento**:
- Cada vez que se ejecuta una página, se pone un **1** en el bit más significativo (más a la izquierda) del registro de esa página.
- Periódicamente, todos los bits se **desplazan a la derecha** (shift right), descartando el bit menos significativo.
- Cuando hay que reemplazar, se saca la página con el **número binario más pequeño** (la que hace más tiempo no se usa, porque sus bits de uso ya se corrieron a la derecha y quedaron en cero).
- Cuantos más bits se asignen, más "historia" de uso se conserva.

> [!example] Ejemplo del profe
> Tres páginas cargadas: pág. 1, pág. 6, pág. 7. En el ciclo 1 se usa pág. 1 → registro: `100000 / 000000 / 000000`. En el ciclo 2 se usa pág. 6 → se corre todo a la derecha y se marca: `010000 / 100000 / 000000`. En el ciclo 3 se usa pág. 7 → `001000 / 010000 / 100000`. Si hay que traer una nueva página, se saca la pág. 1 porque tiene el número binario más chico (`001000` < … espera, en el ejemplo la 1 sería la que tiene el número más chico porque se usó primero). La que se usa más recientemente tiene el número binario más grande (el 1 está más a la izquierda → número mayor).

---

### 8.5 Algoritmo de Segunda Oportunidad (Clock)

> [!note] Definición
> **Algoritmo de Segunda Oportunidad**: a cada frame se le asocia **un solo bit** (bit de uso). Hay un **puntero** que recorre los frames en orden circular. Cuando hay que reemplazar:
> - Si el bit del frame apuntado es **1** (fue usado): se pone en **0** y se pasa al siguiente frame (esa es la "segunda oportunidad": esta vez te perdono, pero te bajo el bit).
> - Si el bit del frame apuntado es **0** (no fue usado desde la última vez que lo revisó el puntero): se hace el reemplazo ahí.
>
> Cuando **no** hay fallo de página: los frames quedan igual, pero el bit del frame de la página que se acaba de usar se pone en **1** (si ya estaba en 1, se deja en 1).

**Ventaja**: mucho más económico que LRU con pila. Un bit por frame es lo mínimo posible.

**Mecánica de ejercicio** (el profe la detalla paso a paso):
- Estado inicial: todos los bits en 0, puntero en frame 1.
- Al cargar una página: bit del frame usado → 1; puntero avanza al siguiente.
- Al haber fallo y necesitar reemplazar: el puntero recorre desde su posición actual buscando un 0. Cuando encuentra un 1, lo pone en 0 y avanza (segunda oportunidad). Cuando encuentra un 0, hace el reemplazo ahí, pone el bit en 1, y avanza el puntero.
- Sin fallo: las páginas quedan en los mismos frames; solo se actualiza el bit del frame de la página usada a 1.

---

### 8.6 Algoritmo de Segunda Oportunidad Mejorado

> [!note] Definición
> **Segunda Oportunidad Mejorado**: igual que el anterior pero usa **2 bits por frame** en vez de 1:
> - **Bit de referencia (uso)**: igual que en segunda oportunidad.
> - **Bit de modificación**: indica si la página fue modificada (dirty bit).
>
> Las combinaciones posibles son: (0,0), (0,1), (1,0), (1,1). Al tomar la decisión de reemplazo, se prefiere sacar páginas con bit de modificación en 0 (no modificadas), para evitar el costoso viaje de escritura al disco.

**Ventaja**: toma la decisión con más información (uso + modificación), reduciendo el factor 2 de la fórmula TAE. Solo cuesta un bit extra por frame.

El profe menciona este algoritmo conceptualmente, sin hacer ejercicio práctico.

---

### 8.7 Algoritmo de Conteo (LFU / MFU)

> [!note] Definición
> **Algoritmo de conteo**: a cada página cargada en memoria principal se le asocia un **contador**. Cada vez que la página es referenciada, el contador se incrementa en 1.

Hay **dos criterios** para elegir qué página sacar:

| Criterio | Nombre | Acción |
|---|---|---|
| ==LFU== (Least Frequently Used) | Menos frecuentemente usado | Saca la de **contador más bajo** |
| ==MFU== (Most Frequently Used) | Más frecuentemente usado | Saca la de **contador más alto** |

**Debate del profe (importante):**

El profe deliberadamente no dice cuál es mejor, porque ambos tienen razonamiento válido y fallas:

- **LFU** (sacar la de contador más bajo): supone que la de uso mínimo es la menos necesaria. **Problema**: una página con contador muy alto puede llevar mucho tiempo cargada y nunca más usarse, pero nunca se la saca porque su número sigue siendo el mayor. Riesgo de que una página "viva eternamente" en memoria aunque ya no se use.
- **MFU** (sacar la de contador más alto): supone que la de uso máximo es la que llevas más tiempo cargada y ya cumplió su función. **Problema**: puede sacar páginas que se usan frecuentemente y están activas.

> [!important] El profe recalca
> El profe se inclinaría por MFU (sacar la de contador más alto) para evitar el problema de la "página que vive eternamente" que describe LFU. Pero reconoce que no hay respuesta definitiva. Lo importante es saber que poner un contador es la solución más natural, pero que aun así tiene sus pros y contras, y que al implementarlo hay que elegir **un criterio** y mantenerse en él.

---

## 9. Asignación de Frames a los Procesos

Una vez vistos los algoritmos de reemplazo, el profe explica las **formas** en que el sistema operativo puede asignar frames a los procesos. Hay tres dimensiones de decisión:

### 9.1 Equitativa vs. Proporcional

| | ==Equitativa== | ==Proporcional== |
|---|---|---|
| Criterio | A todos los procesos se les da la **misma cantidad** de frames | A cada proceso se le da en **proporción a lo que necesita** |
| Ejemplo | 10 procesos → 3 frames a cada uno | El proceso que necesita 10 páginas recibe el doble de frames que el que necesita 5 |

Para la asignación proporcional, el cálculo es: calcular qué porcentaje representa cada proceso del requerimiento total, y aplicar ese porcentaje a los frames disponibles.

### 9.2 Local vs. Global

| | ==Local== | ==Global== |
|---|---|---|
| Criterio | Al proceso se le asignan frames **fijos** (ej: frames 10, 15 y 20) y todos los reemplazos se hacen siempre en esos frames | Al proceso siempre se le mantiene la **misma cantidad** de frames, pero el sistema puede cambiar qué frames usa |
| Flexibilidad | Baja (siempre los mismos frames) | Alta (la cantidad es fija pero la ubicación varía) |

### 9.3 Fija vs. Variable

| | ==Fija== | ==Variable== |
|---|---|---|
| Criterio | La cantidad de frames asignada al inicio no cambia hasta que el proceso termina | El sistema operativo puede dar más o menos frames según las necesidades del momento |

En la práctica se elige una opción de cada dimensión (equitativa o proporcional + local o global + fija o variable).

---

## 10. Ejercicios de Cálculo — Modelo

El profe resuelve ejercicios de cálculo para comparar las tres formas de administración de memoria (particiones múltiples variables, paginación simple, paginación por demanda).

### Ejercicio 1 (Clase 9)

**Enunciado**: Sistema de multiprogramación con 24 MB de RAM. El sistema operativo y librerías ocupan 5 MB. Memoria disponible para procesos: $24 - 5 = 19$ MB. Todos los procesos son del mismo tamaño: 2,4 MB. Tamaño del frame: 128 KB. En memoria virtual, asignación equitativa de 15 frames por proceso.

**A) Particiones múltiples variables** (proceso cargado completo, sin paginación):

$$\text{Procesos que caben} = \lfloor 19 \div 2{,}4 \rfloor = \lfloor 7{,}91 \rfloor = 7 \text{ procesos}$$

$$\text{Fragmentación externa} = 19 - (7 \times 2{,}4) = 19 - 16{,}8 = 2{,}2 \text{ MB}$$

**B) Paginación simple** (proceso cargado completo con paginación, sin memoria virtual):

Primero, cuántas páginas/frames necesita cada proceso:

$$2{,}4 \text{ MB} \times 1024 = 2457{,}6 \text{ KB} \div 128 \text{ KB/página} = 19{,}2 \text{ páginas} \Rightarrow 20 \text{ páginas}$$

La última página se usa al 20%, queda libre el 80%:

$$\text{Fragmentación interna por proceso} = 0{,}8 \times 128 = 102{,}4 \text{ KB}$$

Cuántos frames hay en memoria disponible:

$$19 \text{ MB} \times 1024 = 19456 \text{ KB} \div 128 = 152 \text{ frames}$$

$$\text{Procesos que caben} = \lfloor 152 \div 20 \rfloor = 7 \text{ procesos (usan } 140 \text{ frames)}$$

$$\text{Fragmentación externa} = 152 - 140 = 12 \text{ frames}$$

**C) Paginación por demanda** (memoria virtual, asignación equitativa de 15 frames):

Frames disponibles: 152. A cada proceso se le dan 15 frames (aunque necesite 20, en memoria virtual no se carga completo).

$$\text{Procesos que caben} = \lfloor 152 \div 15 \rfloor = 10 \text{ procesos (usan } 150 \text{ frames)}$$

$$\text{Fragmentación externa} = 152 - 150 = 2 \text{ frames}$$

> [!important] El profe recalca
> Esto es clave: con paginación simple entran **7 procesos**; con memoria virtual (paginación por demanda) entran **10 procesos**. Esa es la ventaja real de la memoria virtual en términos de multiprogramación.

---

### Ejercicio 2 (Clase 9)

**Enunciado**: Sistema de paginación por demanda. Memoria principal con 8000 frames. Cada frame direcciona 2048 palabras. Cada palabra: 64 bits (= 8 bytes). Cuatro procesos A, B, C, D con distintos requerimientos en KB.

El profe da los resultados de la columna "frames requeridos":

- Proceso A: **424 frames** (fragmentación interna: 4 KB)
- Proceso B: **1004 frames** (fragmentación interna: 72 KB → el profe dice 0, según los alumnos; hay ligera discusión)
- Proceso C: **1170 frames**
- Proceso D: **5360 frames** (fragmentación interna: 8 KB)

**Asignación equitativa** (8000 frames ÷ 4 procesos):

$$\text{Frames por proceso} = \frac{8000}{4} = 2000 \text{ frames a cada uno}$$

**Asignación proporcional**: calcular el porcentaje que cada proceso representa del requerimiento total, y asignarlo sobre los 8000 frames disponibles. El proceso A representa aproximadamente el 5% del total, por lo tanto recibe el 5% de 8000 = ~400 frames. Los demás se calculan análogamente.

> [!note] Definición — Método de asignación proporcional
> Paso 1: sumar los frames requeridos por todos los procesos (total). Paso 2: calcular el porcentaje que cada proceso representa de ese total. Paso 3: aplicar ese porcentaje a los 8000 frames disponibles.

---

## 11. Resumen comparativo de algoritmos de reemplazo

| Algoritmo | Criterio de reemplazo | Costo de implementación | Predice uso futuro | Observaciones |
|---|---|---|---|---|
| **FIFO** | La que lleva más tiempo en memoria | Muy bajo | No | Impredecible; susceptible a anomalía de Belady |
| **Óptimo** | La que se usará más tarde en el futuro | Impracticable | Sí (requiere conocimiento a priori) | Mínimo de fallos posible; referencia teórica |
| **LRU** | La menos recientemente usada | Medio (pila de números) | No (usa pasado) | Generalmente bueno; no sufre anomalía de Belady |
| **Aprox. LRU** | La de menor número binario en sus bits | Bajo (n bits por página) | No (usa pasado) | Más económico que LRU; calidad ↑ con más bits |
| **2ª Oportunidad** | Primera con bit de uso = 0 | Muy bajo (1 bit/frame + puntero) | No | Económico; similar a LRU pero simplificado |
| **2ª Oport. Mejorado** | Primera con (uso=0, modif=0) preferida | Muy bajo (2 bits/frame) | No | Reduce factor 2 de la fórmula TAE |
| **LFU** | La de contador más bajo | Bajo (contador/página) | No | Riesgo de inanición para páginas con contador alto |
| **MFU** | La de contador más alto | Bajo (contador/página) | No | Puede sacar páginas activas; el profe lo prefiere sobre LFU |

---

## 12. Sistemas determinísticos (nota del profe al cerrar U7)

Al concluir la unidad, el profe hace énfasis en que los sistemas operativos son **sistemas determinísticos**: ante cada situación tienen predefinido qué hacer. No toman decisiones con incertidumbre; cada criterio (qué algoritmo usar, si la asignación es equitativa o proporcional, etc.) está definido al escribir el sistema operativo. Eso diferencia a los SO de los sistemas de apoyo a la toma de decisiones.

---

## 13. Lo que el profe menciona al pasar (sin desarrollar)

- **Thrashing/hiperpaginación**: el profe no desarrolla este concepto explícitamente en las transcripciones disponibles. Se menciona implícitamente al hablar de que asignar muy pocos frames causa muchos fallos y degrada el rendimiento, pero el término no aparece textualmente.
- **Conjunto de trabajo (working set)**: tampoco aparece explícitamente en las transcripciones. Los conceptos relacionados (cuántos frames asignar para minimizar fallos) se tratan de forma intuitiva.
- El profe menciona al final de la Clase 8 que al terminar U7 se verán las formas de asignación de frames, lo que efectivamente se desarrolla en Clase 9.

---

## Referencias cruzadas entre unidades

- La fórmula de TAE menciona el **tiempo de acceso al disco** (4 componentes). Los algoritmos para optimizar el tiempo de búsqueda del disco (FIFO, SSTF, SCAN, C-SCAN, LOOK, C-LOOK) se ven en la Unidad 8.
- Los conceptos de paginación, tabla de páginas, fragmentación interna, frames y páginas vienen de la **Unidad 6**.

---

## Enlaces

- [[U6 - Administración de Memoria Central]]
- [[U8 - Administración de Dispositivos de Entrada-Salida]]
