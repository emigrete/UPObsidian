---
title: "Preguntas Integradoras — SO (2do Parcial)"
tags:
  - sistemas-operativos
  - preguntas
  - 2do-parcial
aliases:
  - "Preguntas SO 2do Parcial"
---

# 🧩 Preguntas Integradoras — Sistemas Operativos (2do Parcial)

> [!tip] Cómo usar esta página
> Las respuestas están **plegadas**. Leé la pregunta, intentá responderla vos, y recién después abrí el callout (click en el ▸ a la izquierda de "Ver respuesta") para verificar. Pensada para repaso/active recall del parcial. Entra **U6 a U10** (5 unidades).

## 🧠 Unidad 6 — Administración de Memoria Central

**1.** Distinga entre dirección lógica y dirección física, y explique los tres momentos en que puede realizarse la vinculación entre ambas. ¿Cuál de los tres permite swapping y por qué?
> [!success]- Ver respuesta
> La **dirección lógica** es la que el programador le asigna a las instrucciones cuando escribe el programa (antes de cargarlo en memoria); la CPU lee siempre direcciones lógicas. La **dirección física** es la dirección real asignada en la memoria principal cuando el proceso es cargado; puede coincidir o no con la lógica. Hay que vincular ambas.
> 
> Los tres momentos de vinculación son:
> 
> - **Tiempo de compilación**: al compilar se asignan directamente las direcciones físicas. El programa siempre debe cargarse en el mismo lugar; solo sirve para monoprogramación.
> - **Tiempo de carga**: al compilar se usan direcciones reubicables; al cargar, el SO suma el registro base a cada dirección lógica (física = lógica + registro base). El programa puede cargarse en cualquier lugar, pero **no permite swapping** (el contador de programa quedaría apuntando al lugar viejo).
> - **Tiempo de ejecución**: la vinculación se hace mientras el proceso se ejecuta, instrucción por instrucción, mediante la **MMU** (Memory Management Unit), que suma el registro base en tiempo real.
> 
> El que permite swapping es el de **tiempo de ejecución**, porque la vinculación se rehace cada vez que se carga el proceso, con el nuevo registro base. Es la opción más flexible y la que usan los SO de propósito general modernos.

**2.** Explique las tres formas de vinculación de un programa con sus librerías (linkedición) e indique ventajas y desventajas de cada una.
> [!success]- Ver respuesta
> - **Carga dinámica**: la realiza el propio programador, que sabe dónde están las librerías, las carga y las convoca cuando las necesita. No interviene el SO. Es rápido (sin interrupciones), pero el programador hace todo el trabajo manualmente.
> - **Linkeo estático**: al proceso ya compilado se le agregan todas las librerías que necesita, formando un ejecutable autónomo. Ventaja: el proceso se ejecuta solo, sin pedir librerías al SO. Desventaja: las librerías de uso común quedan repetidas en memoria para cada proceso → desperdicio de memoria principal.
> - **Linkeo dinámico**: al proceso se le agrega solo una colilla o referencia (no el código de la librería) en cada punto donde se la necesita. Las librerías residen en memoria una sola vez para todos los procesos. Ventaja: no hay código repetido, se ahorra memoria. Desventaja: el SO debe sincronizar el uso de esas librerías entre los procesos.

**3.** Compare la fragmentación interna y la fragmentación externa: en qué esquema de administración aparece cada una y a qué se debe.
> [!success]- Ver respuesta
> Son conceptos distintos (el profe dice que siempre sale una pregunta sobre esto):
> 
> - **Fragmentación externa**: aparece con **particiones múltiples variables**. Es la parte de la memoria que queda en poder del SO pero que es demasiado pequeña para alojar cualquier proceso de la cola. Es un desperdicio transitorio (los procesos entran y salen) pero inevitable con particiones variables; el objetivo es minimizarlo eligiendo el criterio de asignación adecuado.
> - **Fragmentación interna**: aparece con **paginación** (y también en la segmentación paginada, en la última página). Como la paginación asigna un número entero de páginas, la última página del proceso casi siempre queda parcialmente vacía. Es la parte que el SO asigna al proceso pero que este no puede usar porque le sobra.

**4.** Describa los tres criterios de asignación de huecos en particiones múltiples variables. ¿Cuál conviene según el perfil de procesos del sistema?
> [!success]- Ver respuesta
> Cuando llega un proceso y hay varios huecos, el SO decide en cuál ubicarlo:
> 
> - **Primer ajuste (First Fit)**: recorre la lista de huecos con un puntero y ubica el proceso en el primer hueco donde entre. Muy simple y económico; resultado variable.
> - **Mejor ajuste (Best Fit)**: ubica el proceso en el hueco donde entre más justo (menor fragmentación residual). Requiere conocer el tamaño de todos los huecos. Conviene cuando el sistema trabaja predominantemente con **procesos grandes** (deja los huecos grandes libres para los que vengan).
> - **Peor ajuste (Worst Fit)**: ubica el proceso en el hueco más grande disponible, generando el mayor hueco residual. Conviene cuando se trabaja con **procesos pequeños** (el hueco grande residual puede recibir otro proceso pequeño).
> 
> El criterio se decide al escribir el SO y se mantiene; no cambia dinámicamente. Es una decisión de diseño.

**5.** En paginación, ¿cómo está compuesta la dirección lógica? Explique el rol de la tabla de páginas en la traducción a dirección física y por qué el desvío es el mismo en ambas direcciones.
> [!success]- Ver respuesta
> En paginación la dirección lógica tiene **dos componentes**: (número de página, desvío). El número de página identifica en qué página del proceso está la instrucción/dato; el desvío (offset) es la posición dentro de esa página (de 0 hasta tamaño_de_página − 1).
> 
> Cada proceso tiene su propia **tabla de páginas**, almacenada en su BCP, que mapea número de página → número de frame. La búsqueda es **secuencial** (sin índice ni árbol). Para traducir: la CPU lee la dirección lógica, va a la tabla, encuentra en qué frame está esa página, y arma la dirección física (frame, desvío).
> 
> El **desvío es el mismo** en la dirección lógica y en la física porque las páginas y los frames tienen el mismo tamaño, así que no hay que recalcularlo: solo cambia el identificador de bloque (página → frame).

**6.** Explique dos mecanismos que aceleran la traducción de direcciones en paginación: las tablas multinivel y el TLB.
> [!success]- Ver respuesta
> Como la tabla de páginas puede ser muy larga y la búsqueda es secuencial, se usan dos mecanismos:
> 
> - **Tablas multinivel**: en lugar de una sola tabla con todas las páginas, se tienen varias tablas más pequeñas organizadas jerárquicamente. Una tabla de primer nivel con rangos de páginas, y cada entrada apunta a una tabla de segundo nivel con las páginas de ese rango. Ejemplo del profe: buscar la página 250 en una tabla de 500 son 250 lecturas; con rangos de 100 son 3 lecturas para llegar a la tabla correcta + 50 dentro de esa tabla = 53 lecturas.
> - **TLB (Translation Lookaside Buffer)**: memoria muy rápida entre la CPU y la memoria principal que guarda las últimas traducciones página→frame. Si la CPU encuentra la traducción en el TLB (**TLB hit**), obtiene el frame directamente; si no (**TLB miss**), va a la tabla de páginas en memoria, recorre secuencialmente y actualiza el TLB. Funciona igual que la memoria caché, aprovechando el principio de proximidad.

**7.** Compare paginación y segmentación: cómo divide cada una la memoria, qué tipo de fragmentación produce y cómo es su dirección lógica. Use la analogía de los ladrillos del profe.
> [!success]- Ver respuesta
> - **Paginación**: divide toda la memoria (principal y secundaria) en bloques de tamaño fijo e igual entre sí (páginas en disco, frames en RAM). Dirección lógica = (número de página, desvío). Produce **fragmentación interna** (la última página del proceso queda parcialmente vacía).
> - **Segmentación**: la memoria no se divide en bloques iguales; el programa se divide en **segmentos de tamaño variable**, definidos de manera lógica (según las secciones del programa). Dirección lógica = (número de segmento, desvío). **Elimina la fragmentación interna** porque se le da al proceso exactamente lo que necesita, pero administrar bloques de distinto tamaño es más complejo.
> 
> Analogía de los ladrillos: con ladrillos estándar (paginación) los ponés sin pensar; con ladrillos de distinto tamaño (segmentación) cada vez tenés que buscar el que coincide con el espacio. Por eso la paginación termina siendo más eficiente en la práctica.

**8.** Describa la traducción de direcciones en segmentación. ¿Qué contiene la tabla de segmentos y qué verificación adicional se hace respecto a la paginación?
> [!success]- Ver respuesta
> Cada proceso tiene una **tabla de segmentos** con: número de segmento, **registro base** (dirección de inicio del segmento en memoria principal) y **registro límite** (tamaño del segmento).
> 
> Para acceder al segmento s con desvío d:
> 
> - Ir a la tabla de segmentos y buscar el segmento s.
> - Verificar que d < límite (el desvío no se "cae" fuera del segmento) — esta es la verificación adicional respecto a la paginación.
> - Dirección física = registro base + d.
> 
> Ejemplo del profe: segmento 0 con base = 1000 y límite = 512, desvío 200 → verifica 200 < 512 ✓ → física = 1000 + 200 = 1200.

**9.** ¿Qué es la segmentación paginada y qué busca combinar? ¿Por qué reaparece la fragmentación interna y cómo es su dirección lógica?
> [!success]- Ver respuesta
> La **segmentación paginada** busca combinar las ventajas de ambos esquemas: la división lógica de la segmentación + la uniformidad de la paginación. Cada **segmento** se divide internamente en **páginas de tamaño fijo**.
> 
> Como la unidad mínima de asignación vuelve a ser la página, **reaparece la fragmentación interna** (en la última página del último segmento).
> 
> La dirección lógica tiene **tres componentes**: (número de segmento, número de página, desvío). En cuanto a tablas, hay una tabla de segmentos que mapea cada segmento a su propia tabla de páginas, y cada tabla de páginas mapea páginas → frames.

**10.** Dado un espacio lógico de 8 páginas de 512 palabras cada una (palabra de 2 bytes) y un espacio físico de 32 frames, calcule los bits necesarios para una dirección lógica y para una dirección física. Explique el método.
> [!success]- Ver respuesta
> El método es: "¿cuántas combinaciones necesito representar? → busco la potencia de 2 que lo cubra → ese exponente es la cantidad de bits".
> 
> **Dirección lógica** (número de página + desvío):
> 
> - 8 páginas: 2³ = 8 → **3 bits** para el número de página.
> - Desvío dentro de 512 palabras: 2⁹ = 512 → **9 bits** para el desvío.
> - Total = 3 + 9 = **12 bits**.
> 
> **Dirección física** (número de frame + desvío):
> 
> - 32 frames: 2⁵ = 32 → **5 bits** para el frame.
> - Desvío: el mismo, **9 bits**.
> - Total = 5 + 9 = **14 bits**.
> 
> (Datos extra del mismo ejercicio: el programa ocupa 8 × 512 × 2 = 8.192 bytes; la memoria con 32 frames libres es 32 × 512 × 2 = 32.768 bytes.)

## 🌀 Unidad 7 — Memoria Virtual

**1.** ¿Qué restricción de la [[U6 - Administración de Memoria Central]] elimina la memoria virtual y para qué sirve principalmente? ¿Sigue importando el tamaño de la RAM?
> [!success]- Ver respuesta
> En la [[U6 - Administración de Memoria Central]] el proceso debía cargarse **completo** en memoria principal, ejecutarse sin swap y recién al terminar se lo podía sacar. La **memoria virtual** elimina esa restricción: solo la parte del proceso que se está ejecutando debe estar en RAM; el resto puede quedar en memoria secundaria (disco) y se trae cuando hace falta. La CPU siempre trabaja con la memoria principal, nunca directamente con la secundaria.
> 
> Sirve principalmente para mejorar la **multiprogramación y el multiprocesamiento**: si cada proceso ocupa solo una parte de la RAM, se pueden tener muchos más procesos cargados a la vez, mejorando el aprovechamiento de la CPU.
> 
> Sí, el tamaño de la RAM **siempre importa**: aunque la memoria virtual extiende lógicamente el espacio con el disco, una RAM más grande permite cargar más procesos (aunque sea parcialmente). Pensar "no compro más RAM porque ya tengo memoria virtual" es un error conceptual.

**2.** ¿Por qué la memoria virtual adopta la paginación (y no la segmentación) como sistema de administración? Explique qué es la paginación por demanda y el rol del bit de validez.
> [!success]- Ver respuesta
> La memoria virtual trabaja con **paginación** porque maneja piezas de igual tamaño (frames), que son mucho más simples de administrar que las piezas de distinto tamaño de la segmentación (analogía de los ladrillos). La segmentación se usó en alguna medida, pero la paginación es lo más eficiente en la práctica.
> 
> La **paginación por demanda** es la variante usada en memoria virtual: el proceso no se carga completo, solo algunas páginas. La tabla de páginas existe completa (con todas las páginas del proceso), pero la mayoría no están en RAM. Cada entrada tiene un **bit de validez**: **1** si la página está en memoria principal, **0** si no está (está en disco o no fue cargada aún).

**3.** ¿Qué es un fallo de página y qué secuencia de eventos dispara? Explique el problema del reemplazo.
> [!success]- Ver respuesta
> Un **fallo de página (page fault)** ocurre cuando la CPU intenta acceder a una página cuyo bit de validez es 0 (no está en memoria principal). El proceso se bloquea, se hace una llamada al SO, y el módulo de E/S busca esa página en disco y la trae a un frame de la RAM.
> 
> El **problema del reemplazo**: una vez localizada la página, hay que decidir en qué frame ponerla. Si hay frames libres, no hay problema; pero si todos están ocupados, hay que **sacar** una de las páginas ya cargadas para hacer lugar. La página a traer es fácil de determinar (la que pidió la CPU); el problema interesante es decidir **cuál sacar**. El criterio ideal sería sacar la que no se va a usar más o la que se usará más tardíamente: eso es lo que buscan los algoritmos de reemplazo.

**4.** Explique la fórmula del Tiempo de Acceso Efectivo (TAE), el significado del factor 2 y dos formas de reducirlo.
> [!success]- Ver respuesta
> La fórmula es **TAE = T_mem + 2 · T_disco · p**, donde T_mem es el tiempo de acceso a memoria principal (orden de nanosegundos, 10⁻⁹ s), T_disco el tiempo de acceso al disco (orden de milisegundos, 10⁻³ s) y p la probabilidad de fallo de página.
> 
> El **factor 2** aparece porque en el peor caso hay dos accesos al disco: uno para guardar la página modificada que se reemplaza, y otro para traer la nueva. El segundo término "pesa brutal": la diferencia entre 10⁻⁹ y 10⁻³ es de seis órdenes de magnitud, así que cada fallo arrastra el rendimiento.
> 
> Dos formas de reducir el 2:
> 
> - **Usar el bit de modificación**: si se puede elegir reemplazar una página no modificada, se hace un solo viaje al disco (traer la nueva, pisar la vieja) y el 2 pasa a 1.
> - **Mantener frames libres**: si siempre hay un porcentaje de frames libre (ej. 10%), al producirse un fallo se ocupa uno libre de inmediato (un solo viaje) y la página a desalojar se manda al disco en un momento de poca actividad.

**5.** ¿Qué es el bit de modificación (dirty bit) y por qué afecta el costo del reemplazo? Relacione esto con el componente de tiempo de búsqueda del disco que se estudia en la [[U8 - Administración de Dispositivos de Entrada-Salida]].
> [!success]- Ver respuesta
> El **bit de modificación (dirty bit)** indica si la página fue modificada desde que se cargó: **0** = no modificada (su copia en disco es idéntica), **1** = modificada (los datos en RAM difieren del disco). Si se va a sacar una página modificada, hay que **escribirla en disco primero** para no perder los cambios y después traer la nueva (dos viajes); si no fue modificada, se la pisa directamente (un viaje). Por eso conviene reemplazar páginas no modificadas: reduce el factor 2 del TAE.
> 
> Relación con la [[U8 - Administración de Dispositivos de Entrada-Salida]]: el T_disco del TAE se descompone en cuatro tiempos (búsqueda, latencia, transferencia, gestión), y el **tiempo de búsqueda** (movimiento mecánico de la cabeza hasta la pista) es el que más pesa (~15 de 25 ms). Por eso en E/S se estudian los algoritmos de planificación del brazo del disco, que justamente buscan reducir ese tiempo de búsqueda y, en consecuencia, el costo de cada fallo de página.

**6.** ¿Qué es la anomalía de Belady? ¿Qué algoritmo la sufre y por qué? ¿Contradice la regla general de que más frames implican menos fallos?
> [!success]- Ver respuesta
> La **anomalía de Belady** es un fenómeno descubierto por Laszlo Belady al aplicar **FIFO**: en ciertos casos, al asignar **más frames** a un proceso, la cantidad de fallos de página **aumenta** en vez de disminuir, contradiciendo la lógica intuitiva.
> 
> La sufre **FIFO** porque solo mira **cuándo entró** la página, sin considerar con qué frecuencia se usa. Ningún otro algoritmo que considere el uso presenta esta anomalía.
> 
> No derrumba la regla general: en el 99,99% de los casos más frames implican menos fallos. Belady solo encontró una secuencia específica de referencias donde FIFO da el resultado anómalo. Ejemplo del profe: con la secuencia 3,2,1,0,3,2,4,3,2,1,2,0,1,7,0,1, FIFO da 9 fallos con 3 frames y 10 fallos con 4 frames.

**7.** Compare los algoritmos FIFO, Óptimo y LRU: criterio de reemplazo, ventajas y desventajas de cada uno.
> [!success]- Ver respuesta
> - **FIFO (First In, First Out)**: saca la página que lleva más tiempo cargada (la que entró primero). Ventaja: administrativamente muy sencillo y económico. Desventaja: impredecible, porque no considera el uso; una página vieja pero muy usada puede salir y causar un fallo inmediato. Es susceptible a la anomalía de Belady.
> - **Óptimo**: saca la página que no se va a usar más, o la que se usará más tardíamente en el futuro. Produce el mínimo posible de fallos. Desventaja: requiere conocer de antemano todas las páginas futuras → muy difícil de aplicar (solo en procesos batch calculatorios que ya se ejecutaron). Sirve como referencia teórica del mínimo de fallos.
> - **LRU (Least Recently Used)**: saca la página usada hace más tiempo (la menos recientemente usada); mira cuándo fue el último uso, no cuándo entró. Ventaja: tiene en cuenta el uso real, es más inteligente que FIFO y no sufre Belady. Desventaja: más costoso que FIFO porque requiere mantener y actualizar una pila.

**8.** Explique cómo se implementa LRU con pila y cómo lo aproxima el algoritmo de Aproximación LRU con bits.
> [!success]- Ver respuesta
> **LRU con pila** (vector de tamaño igual al número de frames): cuando se usa una página (haya o no fallo), ese número de página se coloca arriba de la pila, empujando los demás hacia abajo. Cuando hay fallo y hay que sacar, se saca la que está abajo de la pila (la usada hace más tiempo). Sin fallo, los frames quedan igual pero se actualiza la pila.
> 
> **Aproximación LRU con bits** (forma más económica): en vez de una pila, a cada página cargada se le asocia una cantidad fija de bits (ej. 6, 8, 10). Cada vez que se usa una página, se pone un 1 en el bit más significativo (más a la izquierda) de su registro. Periódicamente todos los bits se desplazan a la derecha (shift right), descartando el menos significativo. Al reemplazar, se saca la página con el **número binario más pequeño** (la que hace más tiempo no se usa). Cuantos más bits, más historia de uso se conserva.

**9.** Describa el algoritmo de Segunda Oportunidad (Clock) y en qué se diferencia el Segunda Oportunidad Mejorado.
> [!success]- Ver respuesta
> **Segunda Oportunidad (Clock)**: a cada frame se le asocia un solo bit (de uso) y hay un puntero que recorre los frames en orden circular. Al reemplazar: si el bit del frame apuntado es **1**, se pone en **0** y se pasa al siguiente (esa es la "segunda oportunidad"); si es **0**, se hace el reemplazo ahí. Cuando no hay fallo, los frames quedan igual pero el bit de la página recién usada se pone en 1. Ventaja: muy económico (un bit por frame es lo mínimo posible).
> 
> **Segunda Oportunidad Mejorado**: igual pero usa **2 bits por frame**: el bit de referencia (uso) y el bit de modificación (dirty bit). Las combinaciones son (0,0), (0,1), (1,0), (1,1), y al decidir se prefiere sacar páginas con bit de modificación en 0 (no modificadas), evitando el costoso viaje de escritura al disco. Reduce el factor 2 del TAE al costo de un bit extra por frame.

**10.** Explique el algoritmo de Conteo en sus dos variantes (LFU y MFU) y el problema que tiene cada una. ¿Por cuál se inclina el profe?
> [!success]- Ver respuesta
> En el **algoritmo de Conteo** cada página cargada tiene un contador que se incrementa en 1 cada vez que es referenciada. Hay dos criterios:
> 
> - **LFU (Least Frequently Used)**: saca la de contador más bajo, suponiendo que la de uso mínimo es la menos necesaria. Problema: una página con contador muy alto puede llevar mucho tiempo cargada y no usarse más, pero nunca se la saca → riesgo de que "viva eternamente" en memoria.
> - **MFU (Most Frequently Used)**: saca la de contador más alto, suponiendo que ya cumplió su función. Problema: puede sacar páginas que se usan frecuentemente y están activas.
> 
> El profe se inclinaría por **MFU** para evitar el problema de la "página que vive eternamente" de LFU, aunque reconoce que no hay respuesta definitiva: hay que elegir un criterio y mantenerse en él.

**11.** Explique las tres dimensiones de decisión para asignar frames a los procesos: equitativa vs. proporcional, local vs. global, fija vs. variable.
> [!success]- Ver respuesta
> En la práctica se elige una opción de cada dimensión:
> 
> - **Equitativa vs. Proporcional**: equitativa da la misma cantidad de frames a todos los procesos (ej. 10 procesos → 3 frames a cada uno); proporcional da a cada proceso en proporción a lo que necesita (el que necesita 10 páginas recibe el doble que el que necesita 5). El cálculo proporcional: sumar los frames requeridos por todos (total), calcular el porcentaje de cada proceso y aplicarlo a los frames disponibles.
> - **Local vs. Global**: local asigna frames fijos al proceso (siempre los mismos frames) y todos los reemplazos se hacen ahí; global mantiene la misma cantidad de frames pero el sistema puede cambiar cuáles usa (más flexible).
> - **Fija vs. Variable**: fija mantiene la cantidad asignada al inicio hasta que el proceso termina; variable permite al SO dar más o menos frames según las necesidades del momento.

**12.** **(Integradora U6–U7)** Un sistema tiene 19 MB disponibles para procesos, frames de 128 KB y procesos de 2,4 MB cada uno. Compare cuántos procesos entran con paginación simple (proceso completo) frente a paginación por demanda (asignación equitativa de 15 frames). ¿Cuál es la ventaja real de la memoria virtual que ilustra este caso?
> [!success]- Ver respuesta
> Hay 19 MB × 1024 / 128 = **152 frames** disponibles.
> 
> - **Paginación simple** (proceso completo): cada proceso necesita 2,4 MB × 1024 / 128 = 19,2 → **20 páginas/frames**. Entran ⌊152 / 20⌋ = **7 procesos** (usan 140 frames; quedan 12 de fragmentación externa). La última página de cada proceso se usa al 20%, dejando 0,8 × 128 = 102,4 KB de fragmentación interna por proceso.
> - **Paginación por demanda** (memoria virtual, 15 frames por proceso aunque cada uno necesite 20): entran ⌊152 / 15⌋ = **10 procesos** (usan 150 frames; quedan 2 de fragmentación externa).
> 
> La ventaja real: con paginación simple entran 7 procesos y con memoria virtual entran 10. Esa mayor multiprogramación es el beneficio concreto de la memoria virtual.

## 💾 Unidad 8 — Administración de Dispositivos de Entrada-Salida

**1.** ¿Cuál es el objetivo principal del módulo de E/S y cuáles sus dos objetivos concretos? Explique la analogía de los idiomas del profe.
> [!success]- Ver respuesta
> El módulo de E/S es la parte del SO que administra los distintos dispositivos periféricos, actuando como **interfaz entre esos dispositivos y el núcleo**. Su objetivo principal es reducir la complejidad que implica para el núcleo tratar con hardware tan heterogéneo. Sus dos objetivos concretos son:
> 
> - **Generalización de los periféricos**: los dispositivos son muy distintos entre sí (teclado, disco, pantalla, mouse, plotter, placa de red…). El módulo agrupa y homogeneiza esa diversidad para simplificar la comunicación con el núcleo.
> - **Eficiencia en el intercambio**: tiene dos componentes, la velocidad de acceso (que el intercambio sea lo más rápido posible) y la calidad (minimizar errores de transmisión, reenviando mensajes cuando se detecta un error, muchas veces de forma transparente).
> 
> Analogía: para hablar con todos los países del mundo harían falta 150-200 idiomas, pero agrupándolos por raíz (sajones, latinos, mandarines) con 5 o 6 alcanza. Igual hace el módulo: en vez de 200 variantes de comunicación en el núcleo, agrupa dispositivos similares.

**2.** ¿Qué es un driver y qué función cumple?
> [!success]- Ver respuesta
> Un **driver** es un **software** que hace que un dispositivo (impresora, disco, monitor, etc.) sea compatible con un SO determinado. Actúa como interfaz estandarizada entre el dispositivo de hardware y el SO, **ocultando las diferencias de hardware**. Cuando un fabricante construye un dispositivo, escribe un driver para cada SO que quiere que lo soporte. Antes se instalaban manualmente (disquete o CD); hoy los SO los buscan y descargan automáticamente (ej. al conectar un pendrive nuevo), de forma transparente para el usuario.

**3.** Describa la evolución histórica del módulo de E/S, desde el control directo por la CPU hasta el DMA. ¿En qué etapa aparece la multiprogramación?
> [!success]- Ver respuesta
> La evolución va de mayor intervención de la CPU hacia mayor autonomía:
> 
> - **Control directo por la CPU (sin módulo)**: la CPU controlaba directamente los dispositivos además de procesar el código. Solo permitía monoprogramación.
> - **E/S programada sin interrupciones**: aparece un módulo del SO que accede al dispositivo, pero sin interrupciones; la CPU sigue en **espera activa**. Sigue siendo monoprogramación.
> - **E/S programada con interrupciones**: con las interrupciones llega la **multiprogramación** (varios procesos cargados ejecutándose alternadamente). El módulo busca los datos del dispositivo, pero la CPU sigue siendo la que los carga en memoria principal.
> - **DMA (Acceso Directo a Memoria)**: un procesador dedicado que vincula periféricos con la memoria principal sin intervención de la CPU. El DMA modifica la RAM de forma autónoma y al terminar le envía un mensaje a la CPU. La libera completamente de esa actividad.
> 
> En toda la evolución previa al DMA, el único que puede escribir en la memoria principal es la CPU; esa restricción cambia con el DMA.

**4.** ¿Qué es el DMA y cuáles son sus tres formas de conexión, con su consecuencia respectiva?
> [!success]- Ver respuesta
> El **DMA (Acceso Directo a Memoria)** es un procesador dedicado cuya única función es vincular dispositivos periféricos con la memoria principal sin intervención de la CPU. Un equipo puede tener varios DMA. En su estado actual, modifica la RAM de forma autónoma y al terminar le avisa a la CPU con un mensaje.
> 
> Tres formas de conexión:
> 
> - **Un solo bus, un DMA**: DMA y periféricos comparten el bus del sistema → solo puede atender un periférico a la vez.
> - **Varios DMA, un bus**: cada DMA atiende un grupo de dispositivos pero comparten el bus → pueden atenderse varios, con limitaciones del bus compartido.
> - **Varios DMA, cada uno con su propio bus**: cada DMA tiene su bus independiente → todos los dispositivos pueden operar simultáneamente.

**5.** ¿En qué cuatro tiempos se descompone el tiempo de acceso a disco? ¿Cuál es el que más pesa y qué consecuencia tiene esto?
> [!success]- Ver respuesta
> El tiempo de acceso a disco se compone de:
> 
> - **Tiempo de búsqueda**: movimiento de la cabeza lectora hasta encontrar la pista (~15 ms).
> - **Tiempo de latencia**: rotación del disco hasta posicionarse sobre el sector (~8 ms).
> - **Tiempo de transferencia**: transferencia real de los datos (~1 ms).
> - **Tiempo de gestión**: overhead de gestión (~1 ms).
> 
> El **tiempo de búsqueda** es el que más pesa (15 ms vs. 8+1+1). Como consecuencia, hay que apuntar a reducirlo: para eso existen los algoritmos de planificación del movimiento del brazo del disco.

**6.** Compare los algoritmos FIFO, SSTF, SCAN y C-SCAN de planificación del brazo del disco. ¿Cuál da el menor movimiento y qué riesgo tiene?
> [!success]- Ver respuesta
> - **FIFO**: atiende los requerimientos en el orden en que llegaron, sin lógica de proximidad. Muy simple, pero el resultado es impredecible (en el ejercicio del profe dio 697 ms).
> - **SSTF (Shortest Seek Time First)**: atiende siempre la pista más cercana a la posición actual. Es un algoritmo por **prioridades** (prioridad = cercanía) y da el **menor movimiento total** (en el ejercicio, 230 ms). Riesgo: por ser por prioridades puede producir **inanición** (una pista en un extremo puede no atenderse nunca); la solución es el **envejecimiento (aging)**.
> - **SCAN (algoritmo del ascensor)**: la cabeza recorre el disco de extremo a extremo (0 a 199 y vuelta), atendiendo todo lo que encuentra. Hay que especificar la dirección inicial y llega siempre al extremo aunque no haya requerimiento allí.
> - **C-SCAN (Circular SCAN)**: como SCAN pero atiende solo en una dirección; al llegar al extremo superior baja directo al 0 sin atender nada y vuelve a subir atendiendo (la bajada puede darse como tiempo fijo, ej. 25 ms). En el ejercicio dio ~204 ms.
> 
> Regla del profe: cuando escuchan "algoritmo por prioridades" → semáforo de **inanición**; y la solución a la inanición siempre es el **envejecimiento**.

**7.** ¿En qué se diferencian LOOK y C-LOOK de SCAN y C-SCAN respectivamente?
> [!success]- Ver respuesta
> - **LOOK**: similar al SCAN, pero **no llega al extremo** físico del disco; se da vuelta en la última pista requerida (la más alta o más baja de la cola). Es más eficiente que SCAN porque no recorre pistas vacías hasta el extremo.
> - **C-LOOK**: combinación de C-SCAN y LOOK: atiende en una sola dirección y da vuelta en la última pista requerida, sin llegar al extremo físico del disco.

**8.** ¿Qué son los sistemas RAID y cuáles son sus dos objetivos? Explique la analogía del camión del profe.
> [!success]- Ver respuesta
> **RAID (Redundant Array of Independent Disks)** es un sistema que usa varios discos trabajando en conjunto, administrados por software, para lograr mayor rendimiento y/o seguridad. Los discos se dividen en bandas (strips); las bandas de la misma fila forman una lista (stripe).
> 
> Sus dos objetivos:
> 
> - **Mayor velocidad de acceso y capacidad de almacenamiento**: con varios discos se pueden tener varios accesos simultáneos donde antes solo había uno.
> - **Redundancia / seguridad**: si un disco o sector se estropea, el sistema puede recuperar la información perdida.
> 
> Analogía del camión: si necesito llevar 8 toneladas pero solo hay ruedas de 1 tonelada, en vez de buscar ruedas de 2 toneladas (que quizás no existen en el mercado), le pongo 8 ruedas de 1 tonelada. Igual con RAID: en vez de un disco enorme caro o inexistente, uso varios discos estándar.

**9.** Compare RAID 0 y RAID 1 en cuanto a redundancia y costo. ¿Cuál no cumple el objetivo de redundancia?
> [!success]- Ver respuesta
> - **RAID 0 (Striping)**: sin redundancia. Incrementa la velocidad de acceso (acceso paralelo a los discos) pero **no da ninguna seguridad**. Es el que **no cumple con el objetivo de redundancia** (solo incrementa velocidad); el profe lo recalca específicamente.
> - **RAID 1 (Mirror / espejo)**: duplica todos los discos. Alta seguridad (puede recuperarse de una falla), pero el costo es el doble de discos.
> 
> El profe aclara que lo importante es saber qué son los RAID y sus dos objetivos; los niveles 2, 3, 4, etc. los menciona para que se sepa que existen, pero no los pregunta en detalle. Sí importan RAID 0 y RAID 1.

## 📁 Unidad 9 — Administración de Archivos

**1.** ¿Qué es un archivo según el profe? ¿Qué condición plantean algunos autores y qué matiz hace él?
> [!success]- Ver respuesta
> Un archivo es una **visión lógica de información almacenada** (antes se usaba el término "entidad"): datos reunidos porque tiene lógica (sentido) que estén juntos. Todo lo que es software son archivos, sin excepción: fuente, objeto, ejecutables, imágenes, videos, música, librerías, directorios, `.bat`, bases de datos.
> 
> Algunos autores exigen que el archivo esté en **memoria secundaria** como condición. El profe no se anima a tanto, porque podría haber archivos transitorios; pero coincide en que para que un archivo **perdure en el tiempo** necesariamente tiene que estar en memoria secundaria.

**2.** Mencione los atributos de un archivo y las operaciones que se pueden realizar sobre él.
> [!success]- Ver respuesta
> **Atributos** (características que permiten clasificar e identificar el archivo): nombre del archivo, tipo de archivo, dueño del archivo, prioridad, y permisos (quién puede modificarlo, si es de lectura o escritura, etc.).
> 
> **Operaciones**: crear el archivo, escribir en el archivo, borrar/eliminar el archivo, reubicar el archivo y cerrar el archivo.

**3.** Distinga entre archivos estructurados y no estructurados. ¿Cuáles son el foco de la unidad y cómo están formados?
> [!success]- Ver respuesta
> - **No estructurados (flujo de bytes)**: archivos que no están organizados en registros (videos, imágenes, `.bat`, directorios, etc.). El profe los menciona solo para dejarlos de lado; **no son el foco** de la unidad.
> - **Estructurados (archivos tradicionales)**: formados por **registros**, donde cada registro está constituido por **campos**. Son los de las aplicaciones de gestión comercial/administrativa. Ejemplo: un archivo de personal tiene un registro por persona, todos con los mismos campos (DNI, nombre, fecha de nacimiento…); cada fila es un registro y cada dato es un campo.
> 
> A partir de la clasificación, **todo lo que se ve en la unidad aplica a archivos estructurados**.

**4.** Explique las tres formas de direccionamiento dentro de un archivo y compárelas según el esfuerzo que le exigen al SO.
> [!success]- Ver respuesta
> - **Direccionamiento físico**: indica cuántos bytes hay que desplazarse desde el inicio del archivo para llegar al registro (ej. "el archivo empieza en 1000, me corro 250 bytes"). Requiere conocer la estructura física. Esfuerzo del SO: **mínimo**.
> - **Direccionamiento relativo**: indica simplemente el número de registro dentro del archivo, sin importar cuántos bytes ocupa cada uno (ej. "dame el registro número 55"). El SO hace el cálculo internamente. Esfuerzo: **intermedio**.
> - **Direccionamiento simbólico**: el registro se identifica por su **campo clave** (ej. "buscame el registro con DNI 12.345.678"). Es el más cómodo e independiente de la estructura física, el más usado en sistemas de gestión, pero el que más recursos consume. Esfuerzo: **máximo**.

**5.** ¿Qué dos tipos de tablas de archivos mantiene el SO y por qué existen las dos?
> [!success]- Ver respuesta
> - **Tabla única del sistema (de archivos abiertos)**: una sola tabla global donde figuran todos los archivos que están siendo utilizados por los distintos procesos en un momento dado (solo los abiertos/en uso, no todos los del sistema).
> - **Tabla de archivos por proceso**: cada proceso tiene su propia tabla, donde se registran qué archivos usa y a qué posición apunta su **puntero de archivo** dentro de cada archivo.
> 
> Existen las dos porque un mismo archivo puede estar siendo usado por varios procesos a la vez: la tabla única lo registra una sola vez, pero cada proceso necesita su propia tabla porque cada uno tiene su propio puntero (su propia posición de lectura/escritura).

**6.** ¿Cuáles son los dos métodos de acceso a memoria secundaria? Asocie cada uno con su dispositivo típico.
> [!success]- Ver respuesta
> Hay exactamente **dos métodos** (no hay más):
> 
> - **Acceso secuencial**: se aplica principalmente con **cintas**. Para llegar a un registro hay que recorrer la cinta desde el principio de forma lineal; no se puede saltar directamente. Las cintas se siguen usando para backups e históricos.
> - **Acceso directo**: se aplica con **discos** (y memorias secundarias direccionables). Permite acceder directamente a cualquier posición sin recorrer las anteriores.

**7.** ¿Qué tipo de archivo se puede implementar sobre acceso secuencial y cuál es su gran problema? ¿Cómo se actualizaba históricamente?
> [!success]- Ver respuesta
> Sobre acceso secuencial solo se puede implementar el **archivo secuencial lineal (en cinta)**: los registros están uno tras otro y no pueden insertarse ni eliminarse en el medio. El gran problema es que **no puede crecer fácilmente**: para insertar un registro hay que reescribir el archivo completo en otro lugar.
> 
> Históricamente se actualizaba con un **archivo maestro** (ej. todos los alumnos, armado el lunes) y un **archivo de transacciones** (altas, bajas y modificaciones de la semana). El lunes siguiente un proceso leía ambos en paralelo, intercalaba los cambios y generaba un **nuevo archivo maestro** actualizado.

**8.** El acceso directo permite varios tipos de archivo. Compare el archivo secuencial lineal en disco con el archivo secuencial enlazado.
> [!success]- Ver respuesta
> - **Secuencial lineal en disco**: administrativamente simple (solo hay que saber dónde empieza) y barato. Desventaja: para agregar un registro hay que copiar el archivo completo a otro lado y reescribirlo. El profe aclara que ningún tipo es mejor que otro: depende del problema; si el lineal alcanza, se usa.
> - **Secuencial enlazado**: los registros están enlazados entre sí mediante punteros (como una lista enlazada). Ventaja clave: puede **crecer sin reescribirse** (un registro nuevo se ubica en cualquier lugar libre y se ajustan los enlaces; al eliminar, se redirige el enlace del anterior al siguiente). Desventaja: el acceso sigue siendo secuencial (hay que recorrer la cadena desde el principio).

**9.** Describa el archivo secuencial indexado: sus partes y los tipos de índice. **(Integradora U7)** ¿Con qué concepto de memoria virtual se relacionan los índices no densos?
> [!success]- Ver respuesta
> El **archivo secuencial indexado** tiene dos partes:
> 
> - **Parte de datos**: todos los registros con todos sus campos (incluido el campo clave).
> - **Parte de índice** (fichero de índice): una o varias tablas auxiliares, cada una con el campo clave y la dirección física del registro. Se pueden tener varios ficheros de índice, uno por cada ordenamiento que interese (por DNI, por apellido, etc.).
> 
> Los índices pueden ser:
> 
> - **Densos**: una entrada por cada registro del archivo.
> - **No densos**: trabajan con rangos, achicando el espacio de búsqueda. El propio profe los compara con las **tablas multinivel de memoria virtual** de la [[U7 - Memoria Virtual]]: igual que las tablas multinivel reducían las lecturas para llegar a la página buscada, los índices no densos reducen el espacio de búsqueda usando rangos.

**10.** Explique el archivo de acceso calculado (hashing): cómo funciona, qué problema tiene y las tres formas de tratarlo.
> [!success]- Ver respuesta
> El **archivo de acceso calculado (hashing)** aplica un algoritmo al campo clave del registro y el resultado es la dirección donde está (o debe guardarse) ese registro en disco. Es muy rápido porque un cálculo matemático es muy barato para la CPU.
> 
> El problema son los **sinónimos**: cuando dos campos clave distintos producen la misma dirección al aplicarles el algoritmo. Tres formas de tratarlos:
> 
> - **Primer espacio libre**: si la dirección calculada está ocupada, se busca hacia adelante hasta el primer espacio libre y se guarda ahí.
> - **Rehashing**: se aplica el algoritmo de hashing nuevamente al resultado obtenido para calcular una nueva dirección.
> - **Zona de overflow**: se reserva una zona especial; cuando aparece un sinónimo se guarda ahí, y al buscar, si no está en la dirección calculada, se busca secuencialmente en esa zona.
> 
> Cuanto mejor sea la técnica de hashing, menos sinónimos producirá: eso es lo que se busca al elegir o diseñar el algoritmo.

**11.** ¿Qué es el archivo de acceso llave? ¿Por qué es el más rápido y por qué es difícil de implementar?
> [!success]- Ver respuesta
> En el **archivo de acceso llave** el **campo clave del registro coincide directamente con la dirección** en disco. Es el método de acceso **más rápido de todos**: no hay cálculo (como en hashing) ni búsqueda en índice, el campo clave es la dirección.
> 
> Limitación: es muy difícil de implementar en la práctica, porque el rango posible de valores del campo clave puede ser enorme y habría que reservar espacio en disco para toda esa cantidad de direcciones posibles, dejando mucho espacio vacío (ej. si el campo clave es el DNI, habría que reservar espacio para cada número de DNI posible). Conviene saber que existe: en archivos pequeños con campo clave que pueda hacerse coincidir con direcciones (ej. un sistema embebido con poca memoria), es el método más rápido posible.

**12.** Describa los cinco tipos de estructura de directorios como evolución histórica. ¿Cuáles permiten compartir archivos y cuál puede producir loops en la búsqueda?
> [!success]- Ver respuesta
> Los directorios (hoy "carpetas") evolucionaron así:
> 
> - **Un solo nivel (raíz)**: todos los archivos cuelgan del directorio raíz, uno al lado del otro. No permite **ninguna organización**.
> - **Dos niveles**: un nivel debajo de la raíz (ej. una rama por usuario, o por tipo de archivo). Permite organización básica.
> - **N niveles (de árbol)**: árbol invertido (raíz arriba, hojas abajo); se ramifica sin límite → gran capacidad de organización (es el de Windows). **No permite compartir**: cada usuario accede solo a su rama.
> - **Grafo acíclico**: un mismo archivo/carpeta puede alcanzarse por **dos ramas distintas** → **permite compartir**. Es **acíclico** porque al recorrer el SO no puede subir por otra rama (vuelve hacia atrás), así que **no hay loops**.
> - **Grafo general**: también **permite compartir**, pero al recorrer el sistema **sí puede subir** por otra rama → **puede producir loops** en la búsqueda; hay que tomar medidas para evitarlos.
> 
> Resumen: **comparten** el grafo acíclico y el grafo general; **el único con riesgo de loop** es el grafo general. El SO busca un archivo **recorriendo el árbol rama por rama**.

**13.** Compare las dos formas de asegurar la consistencia en modo multiusuario. Use los ejemplos del profe.
> [!success]- Ver respuesta
> **Consistencia** = que no se corrompan los datos cuando dos o más usuarios acceden a la misma información a la vez. Dos formas:
> 
> - **Consistencia de sesión**: el que **abre primero** el archivo tiene la autoridad para modificarlo; los demás **pueden verlo pero no modificarlo**, y **no ven los cambios** hasta que el primero **guarde y cierre**. Ejemplo: un archivo de Word compartido (uno edita, los otros solo miran). La prioridad la tiene quien inicia la sesión.
> - **Consistencia de archivos compartidos inmutables**: varios usuarios acceden y **cualquiera puede modificar**; las modificaciones se **ven inmediatamente**, y la restricción se reduce **al registro o campo** que se está modificando (grano más fino, no el archivo completo). Ejemplo: una cuenta bancaria recíproca: los dos titulares pueden **mirar** la cuenta a la vez, pero solo **uno saca el dinero** por vez.

**14.** Explique los tres métodos de asignación de espacio a los archivos en disco. ¿Cómo resuelve cada uno el crecimiento del archivo?
> [!success]- Ver respuesta
> El disco se administra por **bloques** del mismo tamaño. Tres métodos:
> 
> - **Contigua**: los bloques del archivo van uno al lado del otro; el directorio guarda **bloque inicial + longitud**. Crecimiento: si no hay espacio libre al lado, hay que **trasladar el archivo completo** a otro lugar donde quepa → su principal inconveniente.
> - **Enlazada**: usa **punteros** (como el archivo secuencial enlazado, pero sobre los bloques de espacio). El directorio guarda el bloque inicial y cada bloque apunta al siguiente; recorrido secuencial. Crecimiento: se le da **cualquier bloque libre** y se ajustan los punteros, sin trasladar el archivo.
> - **Indizada (indexada)**: hay un **bloque de índices** (i-nodos) que contiene la lista de todos los bloques del archivo; el directorio apunta a él. Crecimiento: cuando el bloque de índice se llena, se **divide en dos** (y se vuelve a dividir si sigue creciendo), formando un árbol → crecimiento sin restricciones.

**15.** ¿Cuáles son las cuatro formas en que el SO registra el espacio libre del disco? **(Práctica)** Un mapa de bits ocupa 1.024 bloques de 8 KB cada uno: ¿cuántos bloques tiene el disco y cuál es su tamaño en GB?
> [!success]- Ver respuesta
> Cuatro formas de registrar el espacio libre:
> 
> - **Vector o mapa de bits**: los primeros bloques guardan **un bit por cada bloque** del disco (0 = libre, 1 = ocupado). Necesita tantos bits como bloques tenga el disco.
> - **Lista enlazada**: lista enlazada de bloques libres, recorrida **secuencialmente**. Económica pero poco eficiente.
> - **Agrupamiento**: los primeros bloques guardan **directamente todas las direcciones** de los bloques libres.
> - **Recuento**: guarda la **dirección de un bloque + cuántos bloques libres hay a continuación** (ej. "bloque 10 → 5 libres", "bloque 30 → 20").
> 
> **Cálculo (cada bit del mapa = un bloque del disco):**
> 
> - Bits del mapa = 1.024 bloques × 8 KB × 1.024 (bytes/KB) × 8 (bits/byte) = **67.108.864 bits** → el disco tiene **67.108.864 bloques** (≈ 67 millones).
> - Tamaño = 67.108.864 × 8 KB = 536.870.912 KB ÷ 1.024 = 524.288 MB ÷ 1.024 = **512 GB**.

## 🌐 Unidad 10 — Introducción a los Sistemas Distribuidos

**1.** ¿Qué es un sistema distribuido y cómo se comunican sus equipos? ¿En qué se diferencia de un cluster y del multiprocesamiento fuertemente acoplado?
> [!success]- Ver respuesta
> Un **sistema distribuido** es un sistema formado por **equipos completos ubicados geográficamente en distintos lugares** que trabajan en conjunto, porque tienen **objetivos comunes** y **comparten recursos**. No son equipos sueltos: están vinculados. Ejemplo: una empresa con equipos en distintas provincias o barrios. La **comunicación** se hace mediante **llamadas a procesos remotos** (paso de mensajes).
> 
> Diferencias clave (todo gira en torno a "¿están distribuidos geográficamente?"):
> 
> - **Multiprocesamiento fuertemente acoplado**: varios procesadores **dentro de un mismo equipo**. NO es distribuido.
> - **Cluster**: conjunto de equipos reunidos **en un mismo lugar** con una red muy veloz. NO es distribuido.
> - **Sistema distribuido** (MIMD **débilmente acoplado**): equipos **distribuidos geográficamente** e interconectados. Este sí lo es.

**2.** ¿Qué es la ISO y qué carácter tienen sus estándares y protocolos? ¿Qué pasa si una empresa decide no usarlos?
> [!success]- Ver respuesta
> La **ISO (International Standard Organization)** es una organización internacional que **establece estándares y protocolos** para muchos temas (desarrollo de software, programación, comunicaciones, etc.).
> 
> Sus estándares **no son de uso obligatorio** para nadie: sirven para que quienes **quieran comunicarse "hablen el mismo idioma"**. Una empresa puede usar **sus propios protocolos** (por ejemplo, para mantener reserva/secreto) sin problema; el único inconveniente es que **no va a poder interactuar con otros sistemas**.

**3.** Describa la idea conceptual del modelo OSI y enumere sus 7 capas en orden. ¿Cómo agrupa Internet las capas?
> [!success]- Ver respuesta
> El **OSI (Open System Interconnection)**, desarrollado por la ISO para la interconexión de **sistemas abiertos**, organiza la comunicación en **7 capas**. La idea conceptual es **dividir un problema complejo** en partes hasta llegar a problemas simples (analogía del profe: organizar un congreso de informática desgranándolo en tareas chicas). Al enviar de A a B, el mensaje (ej. una fotografía) se va dividiendo al **bajar** por las capas hasta el nivel físico (solo ceros y unos); en B se hace el **camino inverso** subiendo por las capas hasta reconstruirlo.
> 
> Las 7 capas (de abajo hacia arriba): **Física → Enlace → Red → Transporte → Sesión → Presentación → Aplicación**.
> 
> **Internet** considera de la capa de **red** hacia arriba: red (IP), transporte (TCP), y agrupa **sesión + presentación + aplicación** bajo "aplicación".

**4.** En la capa física, compare la transmisión analógica y la digital. ¿Por qué la digital puede reconstruirse exactamente igual en el destino?
> [!success]- Ver respuesta
> La capa física determina la **intensidad de la señal**; la transmisión puede ser analógica o digital (ambas válidas).
> 
> - **Analógica**: onda continua con **infinitos puntos** (el teorema de Shannon los reduce). Al recorrer, la señal **disminuye y recibe ruido**; los **regeneradores** la levantan tomando el punto medio de lo que llega, por lo que en el destino **puede no quedar exactamente igual** a la original.
> - **Digital**: usa la **onda cuadrada** con **2 estados** (0 y 1). Al regenerar, solo hay que decidir **si es 1 o 0** → la reconstrucción es **exactamente igual**. Esa es su ventaja, y es con lo que trabaja nuestro equipo de computación.

**5.** Explique los tres tipos de enlace de red (conmutación de circuito, de paquetes y ATM). ¿Cuáles requieren ordenar lo recibido en destino?
> [!success]- Ver respuesta
> - **Conmutación de circuito** (ej. telefonía fija): se establece un **circuito fijo** entre A y B (por donde haya menos tráfico) que se **mantiene durante toda la comunicación**. Al cortar y rellamar puede ser otro circuito. No hay que ordenar nada.
> - **Conmutación de paquetes**: el mensaje se **divide en paquetes** (con cabecera de destinatario/remitente). Los paquetes **viajan por distintos caminos** según el tráfico y **llegan en cualquier orden** → en destino **hay que ordenarlos**.
> - **ATM (Método de Transmisión Asincrónica)**: combina los dos: usa **celdas** (paquetes mucho más chicos) pero **establece un circuito** y lo mantiene durante la comunicación → las celdas **llegan ordenadas** (no hay que ordenarlas).
> 
> Solo la **conmutación de paquetes** obliga a ordenar en destino.

**6.** ¿De qué se encargan las capas de red, transporte, sesión, presentación y aplicación? Dé el protocolo o concepto de Internet asociado a cada una donde corresponda.
> [!success]- Ver respuesta
> - **Red**: identifica **unívocamente** a cada usuario para que no se pierda el destinatario. En Internet (redes de redes): el **IP** (único en el mundo); siglas `.com`, `.ar`, `.gov`, `.edu`.
> - **Transporte**: define el **tamaño de los paquetes** y lo normaliza/protocoliza. En Internet: **TCP**.
> - **Sesión**: si la comunicación es **Full Duplex o Half Duplex**.
> - **Presentación**: **codificación** de los caracteres → **ASCII** (8 bits, el más conocido) o **EBCDIC**.
> - **Aplicación**: servicios al usuario → **SMTP, FTP, TELNET** (mails, mensajes, etc.).

**7.** Explique la taxonomía de Flynn (las 4 combinaciones) con un ejemplo de cada una.
> [!success]- Ver respuesta
> Flynn combina **flujo de instrucciones × flujo de datos** (cada uno único o múltiple):
> 
> - **SISD** (instrucción simple, dato simple): computadora con **un solo procesador**. Ejecuta una instrucción sobre un dato.
> - **SIMD** (instrucción simple, múltiples datos): **procesadores vectoriales**. Ej.: sumar 1 a cada posición de un vector (una instrucción, muchos datos).
> - **MISD** (múltiples instrucciones, un dato): **paralelismo redundante** (ver pregunta 8).
> - **MIMD** (múltiples instrucciones, múltiples datos): **multiprocesamiento**. Es la estructura de los sistemas distribuidos. Puede ser **fuertemente acoplado** (un equipo, NO distribuido) o **débilmente acoplado** (sistemas distribuidos).

**8.** ¿Qué es el paralelismo redundante (MISD), para qué se usa y cómo funciona? Relaciónelo con el hecho de que el software no se puede probar al 100%.
> [!success]- Ver respuesta
> El **paralelismo redundante (MISD)** se usa en aplicaciones **muy exigentes en tiempo que no pueden fallar** (ej. el sistema de un avión de combate, donde "se va la vida del piloto" y se trabaja en nanosegundos).
> 
> Como el **software nunca se puede probar al 100%** (sería necesario cargar todos los datos posibles y recorrer todos los caminos internos, y los números son infinitos → conceptualmente imposible; las pruebas de caja negra/blanca nunca son exhaustivas), se recurre a la redundancia: se le da el **mismo algoritmo a 3 programadores** que lo desarrollan y prueban por separado. Luego se ejecutan los **3 programas en 3 procesadores** y se **comparan los resultados**, eligiendo el de la **mayoría** (si dos dan 5 y uno da 4, vale 5). Así se evita que un error de software provoque una catástrofe.

**9.** ¿Qué es el middleware, dónde se ubica y qué problema resuelve?
> [!success]- Ver respuesta
> El **middleware** es **software**: una **interfaz de programación y protocolos estándares** que se ubica **entre la plataforma y las aplicaciones** (la **plataforma** = hardware del equipo + sistema operativo).
> 
> Resuelve el problema de la **portabilidad entre plataformas**: antes había que desarrollar **una versión de la aplicación por cada plataforma**. Con el middleware, la aplicación se desarrolla **una sola vez** sobre él, y como el middleware funciona sobre varias plataformas, esa misma aplicación puede usarse en todas (el middleware hace la **compatibilidad**).

**10.** ¿Qué es un cluster, cuál es su objetivo y por qué NO es un sistema distribuido? Use la analogía del profe.
> [!success]- Ver respuesta
> Un **cluster** ("racimo") es un **conjunto de equipos reunidos en un mismo lugar**, conectados por una **red de comunicaciones extremadamente veloz**, con un **software que lo administra**. Su objetivo es **potenciar determinadas capacidades** (almacenamiento, procesamiento, impresión, etc.).
> 
> **No es un sistema distribuido** porque los equipos están **todos en el mismo lugar**, no distribuidos geográficamente. Analogía del profe: es como un sistema **RAID** (conjunto de discos + software que los administra), pero con equipos completos en lugar de discos.

---
🔙 Volver al índice: [[Sistemas Operativos — Índice]]
