---
title: "U8 — Administración de Dispositivos de Entrada-Salida"
aliases:
  - "Unidad 8"
  - "E/S"
tags:
  - sistemas-operativos
  - segundo-parcial
  - unidad-8
parcial: "Segundo Parcial"
unidad: 8
clases: "Clases 10-11"
fuente: "Transcripciones Prof. Arzubi"
---

# U8 — Administración de Dispositivos de Entrada-Salida

> [!info] De qué trata
> Esta unidad estudia cómo el sistema operativo gestiona la enorme variedad de dispositivos periféricos a través de un módulo especializado. El foco está en entender qué hace ese módulo, cómo evolucionó históricamente, las técnicas para realizar operaciones de E/S, y —especialmente— los algoritmos de planificación del movimiento del brazo del disco, que son ejercicio de parcial. También se introducen los sistemas RAID.

---

## 1. El módulo de administración de E/S

### Objetivo principal

> [!note] Definición
> El ==módulo de entrada/salida== es la parte del sistema operativo que administra los distintos dispositivos periféricos, actuando como **interfaz entre esos dispositivos y el núcleo** del sistema operativo. Su función es reducir la complejidad que implica para el núcleo tratar con hardware tan heterogéneo.

El módulo persigue dos objetivos:

1. **Generalización de los periféricos**: los dispositivos son muy distintos entre sí (heterogéneos): un teclado, un disco, una pantalla, un mouse, un plotter, una placa de red... Todos se comunican de forma diferente. El módulo trata de agrupar y homogeneizar esa diversidad para que la comunicación con el núcleo sea más sencilla.

> [!example] Ejemplo del profe
> "Si yo tuviera que hablar con todos los países del mundo, tendría que saber 150 o 200 idiomas. Pero si logro agrupar esos idiomas por raíz (sajones, latinos, mandarines), con 5 o 6 idiomas me manejo." Lo mismo aplica al módulo: en vez de implementar 200 variantes de comunicación distintas en el núcleo, agrupa dispositivos similares y reduce esa diversidad.

2. **Eficiencia en el intercambio**: tiene dos componentes:
   - **Velocidad de acceso**: lograr que el intercambio con los dispositivos sea lo más rápido posible.
   - **Calidad**: minimizar la cantidad de errores en la transmisión. Todos los sistemas tienen mecanismos de verificación que reenvían mensajes cuando detectan un error; muchas veces eso es transparente para el usuario.

> [!important] El profe recalca
> El objetivo *principal* del módulo es administrar los dispositivos para ser una interfaz que le facilite al núcleo la complejidad de tratar con hardware heterogéneo. Los dos objetivos concretos son **generalización de los periféricos** y **eficiencia en el intercambio**.

---

## 2. Clasificación de los dispositivos

El profe da una clasificación sencilla a modo de referencia (aclara que no es la única posible y que no la va a tomar en el parcial):

| Tipo | Ejemplos |
|---|---|
| **Dispositivos generales** | Discos, sensores, controladores |
| **Dispositivos de transmisión** | Placas de red, adaptadores de línea, módems |
| **Enlace con el usuario** | Terminales de video, teclado, pantalla, mouse, impresora |

> [!warning] Ojo / suele tomar
> El profe aclara explícitamente que esta clasificación es a modo informativo y **no la va a tomar en el parcial**. Sí es importante la heterogeneidad como concepto (que justifica la necesidad del módulo).

---

## 3. Drivers

Los ==drivers== son programas que hacen que un dispositivo (impresora, disco, monitor, etc.) sea compatible con un sistema operativo determinado. Cuando un fabricante construye un dispositivo, escribe un driver para cada sistema operativo que quiere que lo soporte.

Antes era necesario instalarlos manualmente (venían en disquete o CD). Hoy los sistemas operativos los buscan y descargan automáticamente: cuando conectás un pendrive nuevo, el sistema operativo está buscando e instalando los drivers correspondientes — aunque el proceso sea transparente para el usuario.

> [!note] Definición
> Un driver es un **software** que actúa como interfaz estandarizada entre un dispositivo de hardware y el sistema operativo, ocultando las diferencias de hardware.

---

## 4. Contenido de un puerto de E/S

Un puerto de E/S contiene (esquemáticamente):
- **Registro de estado**: un bit que indica si el puerto está libre u ocupado, y un bit de error.
- **Registro de control**: un bit para indicar si se lee o escribe, y un bit de "listo/no listo".
- **Registro de salida**: byte de datos de salida.
- **Registro de entrada**: byte de datos de entrada.

---

## 5. Evolución del módulo de E/S

La evolución histórica va de mayor intervención de la CPU hacia mayor autonomía:

### 5.1 Control directo por la CPU (sin módulo)
En los primeros sistemas, la ==CPU== era la que controlaba directamente los dispositivos (cintas, etc.). Procesaba el código Y administraba los periféricos. Solo era posible la **monoprogramación** (un proceso a la vez).

> [!important] El profe recalca
> En toda esta evolución, el único que puede escribir en la **memoria principal** es la CPU. Eso es una restricción fundamental que luego cambia con el DMA.

### 5.2 E/S programada sin interrupciones
Apareció un módulo de SO que se encargaba del acceso al dispositivo, pero **sin interrupciones**. La CPU seguía esperando activamente a que terminara la operación de E/S (**espera activa**). Seguía siendo monoprogramación.

### 5.3 E/S programada con interrupciones (dirigida por interrupciones)
Con la aparición de las interrupciones llegó la **multiprogramación**: varios procesos cargados en memoria, ejecutándose de forma alternada o simultánea. El módulo de E/S busca los datos del dispositivo, pero sigue siendo la CPU la que los toma y los carga en la memoria principal.

### 5.4 DMA — Acceso Directo a Memoria
> [!note] Definición
> El ==DMA (Acceso Directo a Memoria)== es un **procesador dedicado** cuya única función es vincular dispositivos periféricos con la memoria principal, sin intervención de la CPU. Un equipo puede tener varios DMA.

Con el DMA:
- La CPU ya no tiene que cargar los datos en la memoria principal; lo hace el DMA.
- En versiones intermedias, la CPU chequeaba periódicamente si el DMA terminó.
- En el estado actual (DMA como procesador dedicado): el DMA modifica la memoria principal de forma autónoma y, al terminar, **le envía un mensaje a la CPU** avisando que la tarea está hecha. La CPU se libera completamente de esa actividad.

**Formas de conexión del DMA:**

| Forma | Descripción | Consecuencia |
|---|---|---|
| Un solo bus, un DMA | DMA y periféricos comparten el bus del sistema | Solo puede atender un periférico a la vez |
| Varios DMA, un bus | Cada DMA atiende un grupo de dispositivos, comparten el bus | Pueden atenderse varios dispositivos, pero con limitaciones del bus compartido |
| Varios DMA, cada uno con su propio bus | Cada DMA tiene su bus independiente | Todos los dispositivos pueden operar **simultáneamente** |

---

## 6. Tiempo de acceso a disco

El ==tiempo de acceso a disco== se compone de 4 tiempos:

| Tiempo | Descripción | Valor aprox. |
|---|---|---|
| **Tiempo de búsqueda** | Movimiento de la cabeza lectora hasta encontrar la **pista** | ~15 ms |
| **Tiempo de latencia** | Tiempo de rotación hasta posicionarse sobre el **sector** | ~8 ms |
| **Tiempo de transferencia** | Transferencia real de los datos | ~1 ms |
| **Tiempo de gestión** | Overhead de gestión | ~1 ms |

> [!important] El profe recalca
> El **tiempo de búsqueda** es el que más pesa (15 ms vs. 8+1+1). Por lo tanto, hay que apuntar a reducirlo. Para eso existen los algoritmos de planificación del movimiento del brazo del disco.

---

## 7. Algoritmos de planificación del brazo del disco

> [!warning] Ojo / suele tomar
> El profe dice explícitamente que estos ejercicios **entran en el parcialito** y que "son extremadamente sencillos", buenos para "lucirse y levantar nota". Hay que saber cuál es cada algoritmo y resolver el ejercicio con la tabla: columna de pista origen, pista destino, cantidad de pistas recorridas.

**Convención para los ejercicios**: cada movimiento de una pista a la siguiente equivale a **1 ms**. La suma total de movimientos es el resultado.

**Ejemplo base usado por el profe** (mismo para todos los algoritmos):
- Secuencia de requerimientos: 98, 183, 37, 122, 14, 124, 65, 67 *(nota: algunos números son reconstruidos del ejercicio según la transcripción)*
- Posición inicial de la cabeza: pista **53**
- Disco de **200 pistas** (0 a 199)

> [!warning] Ojo / suele tomar
> Si dos requerimientos apuntan a la **misma pista**, se atienden ambos en un solo movimiento (no hay que ir dos veces). El profe lo puso a propósito en el ejercicio para aclararlo.

---

### 7.1 FIFO (First In, First Out)

Atiende los requerimientos **en el orden en que llegaron**, sin ninguna lógica de proximidad.

- Muy simple administrativamente: solo mira el primero de la fila.
- El resultado es **impredecible**: a veces sale bien, a veces muy mal.
- En el ejercicio del profe dio **697 ms** de movimiento total.

> [!example] Ejemplo del profe
> Partiendo de la pista 53: va a 98 (45 pistas), luego a 183 (85), luego a 37 (146), luego a 122 (85)... suma total: 697.

---

### 7.2 SSTF — Shortest Seek Time First

Atiende siempre la pista **más cercana** a la posición actual de la cabeza lectora.

- Requiere conocer **todas las pistas pendientes** simultáneamente para poder comparar.
- Es un algoritmo por **prioridades** (prioridad = cercanía).
- Es el que da el **menor movimiento total** (mejor resultado en velocidad).
- En el ejercicio del profe dio **230 ms**.

> [!warning] Ojo / suele tomar
> Por ser un algoritmo de prioridades, puede producir **inanición**: una pista que queda en un extremo puede no ser atendida nunca si siempre aparece una más cercana. La solución es aplicar **envejecimiento** (aging): a medida que pasa el tiempo, aumentar la prioridad de esa pista para que finalmente sea atendida.

> [!important] El profe recalca
> Cuando escuchan "algoritmo por prioridades" → se les tiene que encender el semáforo de **inanición**. Y la solución a la inanición siempre es el **envejecimiento**.

---

### 7.3 SCAN (algoritmo del ascensor)

La cabeza lectora recorre el disco **de extremo a extremo** (de la pista 0 a la 199 y vuelta), atendiendo todos los requerimientos que encuentra en el camino.

- Hay que especificar la **dirección inicial** (si está subiendo o bajando).
- Llega siempre al **extremo** aunque no haya ningún requerimiento allí.
- En el ejercicio del profe: sube desde 53, atiende 54, 98, 122, 124, 183, llega hasta 199 (17 pistas extra), baja y atiende 37, 15 *(según el ejercicio)*, 4.

> [!example] Ejemplo del profe
> Partiendo en 53 subiendo: 53→54 (1), 54→98 (44), 98→122 (24), 122→124 (2), 124→183 (59), 183→199 (17), 199→37 (baja atendiendo), 37→15, 15→4.

---

### 7.4 C-SCAN (Circular SCAN)

Similar al SCAN pero atiende requerimientos **solo en una dirección** (por ejemplo, solo en subida). Cuando llega al extremo superior, **baja directo al 0 sin atender nada** y vuelve a subir atendiendo.

- La bajada de 199 a 0 puede darse como un tiempo fijo (ej.: el enunciado puede decir "de 200 a 0 tarda 25 ms").
- En el ejercicio del profe dio aproximadamente **204 ms**.

> [!example] Ejemplo del profe
> Si el enunciado dice que de 199 a 0 tarda 25 ms, en vez de contar 200 movimientos se pone directamente 25 ms en esa fila de la tabla.

---

### 7.5 LOOK

Similar al SCAN, pero **no llega al extremo** del disco. Se da vuelta en la última pista requerida (la más alta o más baja que haya en la cola).

- Más eficiente que SCAN porque no recorre pistas vacías hasta el extremo.

---

### 7.6 C-LOOK

Combinación de C-SCAN y LOOK: atiende en una sola dirección y da vuelta en la **última pista requerida** (no llega al extremo físico del disco).

---

### Tabla comparativa de algoritmos

| Algoritmo | Criterio | Extremo | Inanición | Complejidad |
|---|---|---|---|---|
| FIFO | Orden de llegada | No aplica | No | Mínima |
| SSTF | Más cercana primero | No aplica | Sí (por prioridades) | Media |
| SCAN | Barrer de extremo a extremo | Llega al extremo | No | Baja |
| C-SCAN | Solo una dirección, vuelta al 0 sin atender | Llega al extremo | No | Baja |
| LOOK | Como SCAN, sin llegar al extremo | No llega | No | Baja |
| C-LOOK | Como C-SCAN, sin llegar al extremo | No llega | No | Baja |

---

## 8. Sistemas RAID

> [!note] Definición
> ==RAID (Redundant Array of Independent Disks)== — conjunto redundante de discos independientes — es un sistema que utiliza **varios discos trabajando en conjunto**, administrados por un software, para lograr mayor rendimiento y/o seguridad.

**Dos objetivos de los sistemas RAID:**
1. **Mayor velocidad de acceso y capacidad de almacenamiento**: con varios discos puedo tener varios accesos simultáneos (donde antes solo podía acceder a uno).
2. **Redundancia / seguridad**: si un disco (o sector) se estropea, el sistema puede recuperar la información perdida.

> [!example] Ejemplo del profe (el camión)
> "Si un camión con 4 ruedas que bancán 1 tonelada cada una puede llevar 4 toneladas, y yo necesito 8, en vez de buscar ruedas de 2 toneladas (que quizás no hay en el mercado), le pongo 8 ruedas de 1 tonelada." Así funciona RAID: en vez de un disco enorme que quizás no existe o sale muy caro, uso varios discos estándar del mercado.

Los discos RAID se dividen en **bandas** (strips). Las bandas que están en la misma fila forman una **lista** (stripe).

### Niveles de RAID

| Nivel | Nombre | Característica principal |
|---|---|---|
| **RAID 0** | Striping | Sin redundancia. Incrementa velocidad de acceso (acceso paralelo a los discos), pero no da ninguna seguridad. |
| **RAID 1** | Mirror (espejo) | Duplica todos los discos. Alta seguridad (puede recuperarse de una falla), pero el costo es el doble de discos. |
| **RAID 2** | Código de Hamming | Menos discos que RAID 1 (ej.: 4 discos de datos + 3 de paridad). Bandas muy chicas. Para escribir hay que acceder a todos los discos simultáneamente (cuello de botella en escritura). |
| **RAID 3** | Paridad simple | Similar a RAID 2 pero con un solo bit de paridad (más sencillo). También requiere acceso simultáneo a todos los discos en escritura. |
| **RAID 4** | Paridad en un disco | Bandas de paridad más grandes, en un disco dedicado. Permite lectura independiente de cada disco (rápida). En escritura el cuello de botella es el disco de paridad (todos lo necesitan). |
| **RAID 5** | Paridad distribuida | Igual que RAID 4, pero las bandas de paridad se distribuyen entre todos los discos. Elimina el cuello de botella. Mejor rendimiento general. |
| **RAID 6+** | Comerciales | Tolerancia a 2 o más fallas. Depende del fabricante. |

> [!important] El profe recalca
> Lo que importa saber: **qué son los sistemas RAID** y **cuáles son sus dos objetivos** (mayor velocidad/almacenamiento + redundancia). Los detalles de cada nivel (RAID 2, 3, 4...) los menciona para que sepan que existen, pero **no los va a preguntar en detalle**. Sí es importante RAID 0 (sin redundancia, solo velocidad) y RAID 1 (mirror, duplica discos).

> [!warning] Ojo / suele tomar
> RAID 0 **no cumple con el objetivo de redundancia**: solo incrementa la velocidad. Lo mencionó específicamente.

---

## 9. Buffering y Spooling

> [!warning] Ojo / suele tomar
> El profe menciona buffering y spooling de pasada en la evolución del módulo de E/S (en el contexto de las técnicas de manejo de E/S) pero **no los desarrolló en profundidad** en la transcripción de esta unidad. Son técnicas conocidas pero no formaron parte de la explicación detallada. Si aparecen en el parcial será a nivel conceptual.

---

## Enlaces

- [[U7 - Memoria Virtual]]
- [[U9 - Administración de Archivos]]
