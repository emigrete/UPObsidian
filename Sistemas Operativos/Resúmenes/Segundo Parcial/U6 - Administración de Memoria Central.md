---
title: "U6 — Administración de Memoria Central"
aliases:
  - "Unidad 6"
  - "Memoria Central"
tags:
  - sistemas-operativos
  - segundo-parcial
  - unidad-6
parcial: "Segundo Parcial"
unidad: 6
clases: "Clases 6-7"
fuente: "Transcripciones Prof. Arzubi"
---

# U6 — Administración de Memoria Central

> [!info] De qué trata
> El sistema operativo administra la memoria RAM (memoria principal), que se puede ver como un gran vector de palabras. Esta unidad cubre cómo se organiza esa memoria, cómo se vinculan las direcciones lógicas con las físicas, las formas de vincular programas con sus librerías, y los distintos esquemas de administración de memoria: desde la asignación continua hasta la paginación, segmentación y segmentación paginada. **Premisa clave de esta unidad:** los programas se cargan completos en memoria y se ejecutan completos antes de sacarse (sin swapping parcial). El swapping y la memoria virtual se ven en la Unidad 7.

---

## 1. La memoria como vector de palabras

- La ==memoria principal== (RAM) se puede modelar como un gran **vector de palabras**.
- Cada punto de almacenamiento físico tiene 2 estados posibles (0 o 1), pero las **direcciones de memoria no apuntan a un bit sino a una palabra**.
- Una ==palabra== es una cantidad **fija de bytes** (por ejemplo, 2, 4 u 8 bytes). El sistema operativo, al instalarse en un equipo, establece un tamaño de palabra fijo para todo su funcionamiento.
- A mayor tamaño de palabra, mayor información por instrucción o dato que puede manejar el sistema en cada acceso.

> [!note] Definición
> **Palabra:** cantidad fija de bytes que establece el sistema operativo. Las direcciones de memoria apuntan a palabras, no a bits ni a bytes individuales.

---

## 2. Direcciones lógicas y físicas

- ==Dirección lógica==: la que el programador le asigna a las instrucciones cuando **escribe** el programa (antes de cargarlo en memoria). La CPU lee siempre direcciones lógicas.
- ==Dirección física==: la dirección real asignada en la memoria principal cuando el proceso es **cargado**. Puede coincidir o no con la lógica.

El problema central: la CPU lee la dirección lógica del programa, pero el proceso está ubicado en alguna dirección física de la RAM. Hay que **vincular** esas dos direcciones de alguna manera.

### 2.1 Tres momentos de vinculación

El profe explica que la vinculación dirección lógica → dirección física puede hacerse en tres momentos distintos:

#### a) Tiempo de compilación

- Al compilar el programa, se le asignan directamente las **direcciones físicas** donde va a residir.
- Consecuencia: el programa **siempre debe cargarse en el mismo lugar** de memoria (el que se le asignó al compilarlo).
- Limitación: solo funciona con **monoprogramación** (un único programa en memoria a la vez); no hay flexibilidad.

#### b) Tiempo de carga

- Al compilar, se usan **direcciones reubicables** (no fijas).
- Cuando el programa se va a cargar en memoria, el sistema le suma el ==registro base== (inicio del bloque asignado) a cada dirección lógica: $\text{dirección física} = \text{dirección lógica} + \text{registro base}$.
- Ventaja respecto al anterior: el programa puede cargarse en cualquier lugar de la memoria (no siempre en el mismo).
- Limitación: **no permite swapping**. Una vez cargado el proceso, no se lo puede mover ni sacar de la memoria principal antes de que termine, porque el contador de programa del bloque de control del proceso quedaría apuntando al lugar viejo.

#### c) Tiempo de ejecución

- La vinculación se realiza **mientras el proceso se está ejecutando**, instrucción por instrucción.
- Existe el ==MMU (Memory Management Unit)== — Unidad de Administración de Memoria — que toma la dirección lógica y le suma el registro base en tiempo real: $\text{dirección física} = \text{dirección lógica} + \text{registro base}$.
- Ventaja: **sí permite swapping**. El proceso puede entrar y salir de la memoria principal sin haber terminado, porque la vinculación se rehace cada vez que se carga, con el nuevo registro base.
- Es la forma más flexible de las tres y la que usan los sistemas operativos de propósito general modernos.

> [!important] El profe recalca
> El tiempo de ejecución es la opción más completa porque permite swapping. Los tres métodos son evolutivos: compilación → carga → ejecución, y el que más posibilidades da es el de ejecución.

---

## 3. Vinculación con librerías (linkedición)

Un programa fuente se compila y luego se **linkedita** (vincula con librerías). El profe describe tres formas de hacer esa vinculación:

### 3.1 Carga dinámica

- La realiza el **propio programador**: él sabe dónde están las librerías, las carga y las convoca cuando las necesita.
- No interviene el sistema operativo.
- Es rápido (no requiere interrupciones), pero el programador hace todo el trabajo manualmente.

### 3.2 Linkeo estático

- Al proceso ya compilado se le agregan **todas las librerías** que necesita, formando un ejecutable autónomo.
- Ventaja: el proceso se ejecuta solo, sin pedirle librerías al sistema operativo en tiempo de ejecución (administrativamente cómodo).
- Desventaja: las librerías de uso común quedan **repetidas en memoria** para cada proceso que las use → desperdicio de memoria principal.

> [!example] Ejemplo del profe
> Si tengo P1 y P2 cargados en memoria y ambos usan las mismas librerías, con linkeo estático esas librerías van a estar duplicadas en la memoria principal, una dentro de P1 y otra dentro de P2.

### 3.3 Linkeo dinámico

- Al proceso compilado se le agrega solo una **colilla o referencia** (no el código de la librería) en cada punto donde se necesite una librería.
- Las librerías residen en memoria una única vez y son usadas por todos los procesos que las necesitan.
- Ventaja: **no hay código repetido**, se ahorra memoria principal.
- Desventaja: el sistema operativo debe **sincronizar** el uso de esas librerías entre los distintos procesos (las librerías son rutinas/procesos con todas sus características de concurrencia, independencia, etc.).

| | Carga dinámica | Linkeo estático | Linkeo dinámico |
|---|---|---|---|
| Quién lo gestiona | El programador | SO (implícito) | SO |
| Librerías en memoria | Por demanda | Repetidas (una por proceso) | Una sola copia |
| Sincronización necesaria | No | No | Sí |
| Autonomía del proceso | Total | Total | Depende del SO |

---

## 4. Formas de administración de memoria

> [!important] El profe recalca
> En toda esta unidad (U6) se asume que el programa se carga **completo** en la memoria principal y se ejecuta completo. No se carga por partes ni se hace swapping parcial. Eso lo veremos en la Unidad 7 (memoria virtual).

### 4.1 Asignación continua

- No hay sistema operativo administrando la memoria: el programador mismo se encarga de cargar su programa.
- Monoprogramación: un solo programa en memoria a la vez.
- Técnica de **overlay (superposición)**: cuando el programa era más grande que la RAM disponible, el programador sacaba manualmente una parte de la memoria y cargaba otra en su lugar (siempre monoprogramación).
- Conviene cuando se necesita minimizar el costo de memoria (por ejemplo, un producto de software que se va a fabricar en un millón de unidades).

### 4.2 Partición simple

- Con los primeros sistemas operativos: la memoria se divide en **dos partes**: SO + un único programa.
- Sigue siendo monoprogramación.

### 4.3 Particiones múltiples fijas

- La memoria se divide en **varias particiones de tamaño fijo** (pueden ser iguales o distintas entre sí).
- Ya permite **multiprogramación**: hay un proceso por partición.
- Se puede tener una única cola de procesos esperando, o una cola por partición.

### 4.4 Particiones múltiples variables

- Las particiones **no tienen tamaño fijo**: se crean dinámicamente a medida que los procesos entran y salen.
- Los huecos que se van formando varían de tamaño y ubicación según lo que llegue y lo que salga.

#### Fragmentación externa

> [!note] Definición
> ==Fragmentación externa==: parte de la memoria que queda en poder del sistema operativo pero que es demasiado pequeña para alojar cualquier proceso de la cola. Es un desperdicio transitorio (los procesos van entrando y saliendo) pero que hay que tratar de minimizar.

#### Criterios de asignación de huecos

Cuando llega un proceso y hay varios huecos disponibles, el SO debe decidir en cuál ubicarlo. Hay tres criterios:

**Primer ajuste (First Fit)**
- Se recorre la lista de huecos con un **puntero** y se ubica el proceso en el **primer hueco** donde entre.
- Muy simple y económico administrativamente.
- El resultado es variable (puede ser bueno o malo).

**Mejor ajuste (Best Fit)**
- Se ubica el proceso en el hueco donde **entre más justo** (menor fragmentación residual).
- Requiere conocer el tamaño de todos los huecos disponibles.
- Conviene cuando el sistema trabaja predominantemente con **procesos grandes** (deja los huecos grandes libres para los que vendrán).

**Peor ajuste (Worst Fit)**
- Se ubica el proceso en el **hueco más grande** disponible, generando el mayor hueco residual posible.
- Conviene cuando el sistema trabaja predominantemente con **procesos pequeños** (el hueco grande que queda después puede recibir otro proceso pequeño).

> [!important] El profe recalca
> El criterio (primer/mejor/peor ajuste) se decide **al escribir el sistema operativo** y luego el SO se la banca. No es algo que cambie dinámicamente a medida que van llegando procesos. Es una decisión de diseño.

> [!warning] Ojo / suele tomar
> La fragmentación externa es inevitable con particiones variables. El objetivo es reducirla al mínimo posible eligiendo el criterio adecuado según el perfil de procesos del sistema.

#### Ejercicio numérico — Particiones múltiples variables (Clase 7)

El profe resuelve un ejercicio con los siguientes datos:

- Huecos disponibles (resto de memoria ocupado): 100 KB, 500 KB, 200 KB, 300 KB, 600 KB (y otros menores que no admiten procesos).
- Procesos que llegan en orden: A (190 KB), B (430 KB), C (125 KB), D (480 KB).

**Mejor ajuste:**
- A (190 KB) → hueco de 200 KB → resta 10 KB
- B (430 KB) → hueco de 500 KB → resta 70 KB
- C (125 KB) → hueco de 300 KB → resta 175 KB
- D (480 KB) → único que entra: hueco de 600 KB → resta 120 KB
- Todos los procesos entran.

**Primer ajuste** (puntero empieza en el primer hueco):
- A (190 KB) → primer hueco que cabe: 500 KB → resta 310 KB
- B (430 KB) → 310 KB no alcanza → siguiente que alcanza: 600 KB → resta 170 KB
- C (125 KB) → puntero está en 170 KB → entra → resta 45 KB
- D (480 KB) → recorre todos los huecos restantes y no entra en ninguno → **D queda afuera**.

**Peor ajuste:**
- A (190 KB) → hueco más grande: 600 KB → resta 410 KB
- B (430 KB) → solo entra en 500 KB → resta 70 KB
- C (125 KB) → hueco más grande disponible: 410 KB → resta 285 KB
- D (480 KB) → no entra en ningún hueco restante → **D queda afuera**.

> [!example] Ejemplo del profe
> "El criterio de mejor ajuste conviene cuando el sistema trabaja con procesos grandes, porque al poner el proceso en el hueco más justo, deja los huecos grandes disponibles para otros grandes que vengan. El peor ajuste conviene para procesos pequeños, porque el gran hueco residual puede alojar otro proceso pequeño."

---

## 5. Administración por paginación

> [!important] El profe recalca
> **La paginación es el sistema más importante de esta unidad** porque es el que se usa como base cuando se trabaja con memoria virtual (Unidad 7). Hay que tenerla bien clara.

### 5.1 Concepto básico

- La ==paginación== divide toda la memoria (principal y secundaria) en **bloques de tamaño fijo** e igual entre sí (por ejemplo 16 KB, 32 KB, etc.).
- Terminología:
  - **Página**: bloque en **memoria secundaria** (disco).
  - **Frame (marco)**: bloque en **memoria principal**. Es lo mismo físicamente; la distinción es solo para saber de qué memoria se habla.
- Al instalar el SO, se formatea la memoria con ese tamaño de bloque fijo.

### 5.2 Dirección lógica en paginación

Con los esquemas anteriores, una dirección era un único número (ej: 10525). Con paginación, la ==dirección lógica== tiene **dos componentes**:

$$\text{dirección lógica} = (\text{número de página},\ \text{desvío})$$

- **Número de página**: identifica en qué página del proceso está la instrucción o dato.
- **Desvío (offset)**: posición dentro de esa página (desde 0 hasta tamaño\_de\_página − 1).

> [!example] Ejemplo del profe
> Si las páginas tienen 512 palabras y quiero referenciar la instrucción en la posición absoluta 600, eso corresponde a: página 1, desvío $600 - 512 = 88$. La dirección lógica sería (1, 88).

### 5.3 Fragmentación interna

Como la paginación asigna un **número entero de páginas**, la última página del proceso puede no estar completamente utilizada.

> [!note] Definición
> ==Fragmentación interna==: parte de la memoria que el sistema operativo **asigna al proceso pero que el proceso no puede usar** porque le sobra (el final de su última página). En la mayoría de los casos existirá, ya que sería casualidad que el programa termine justo en el límite de la última página.

> [!warning] Ojo / suele tomar
> **Fragmentación interna** (paginación) ≠ **Fragmentación externa** (particiones variables). Son conceptos distintos. Con paginación aparece la interna; con particiones variables aparece la externa. El profe dice que siempre sale alguna pregunta sobre esto.

### 5.4 Tabla de páginas

Para traducir de dirección lógica a dirección física, **cada proceso tiene su propia ==tabla de páginas==**.

- La tabla de páginas mapea: número de página → número de frame (en qué frame de la memoria principal está guardada esa página).
- Estructura: una fila por página del proceso, con la columna del frame asignado.
- La tabla de páginas se almacena en el Bloque de Control del Proceso (BCP) al crear el proceso.

**Ejemplo (del profe):** proceso de 2 páginas.

| Página | Frame |
|--------|-------|
| 0 | 8 |
| 1 | 10 |

Traducción: CPU lee la dirección lógica (0, 200) → va a la tabla de páginas → página 0 está en frame 8 → dirección física = (frame 8, desvío 200). El desvío es el mismo en ambas direcciones porque páginas y frames tienen el mismo tamaño.

> [!note] Definición
> La búsqueda en la tabla de páginas es **secuencial** (no hay índice, no hay estructura de árbol). Se recorre de arriba a abajo hasta encontrar la página buscada.

### 5.5 Optimizaciones: tablas multinivel y TLB

Como la tabla de páginas puede ser muy larga y la búsqueda es secuencial, se usan dos mecanismos para acelerar la traducción:

#### Tablas multinivel

- En vez de una sola tabla con todas las páginas, se tienen **varias tablas más pequeñas** organizadas jerárquicamente.
- Hay una tabla de primer nivel con rangos de páginas, y cada entrada apunta a una tabla de segundo nivel con las páginas de ese rango.

> [!example] Ejemplo del profe
> Si tengo una tabla con 500 páginas y necesito la página 250: búsqueda simple → 250 lecturas. Con tablas multinivel (rangos de 100): 3 lecturas para llegar a la tabla correcta + 50 lecturas dentro de esa tabla = 53 lecturas. Mucho más eficiente.

#### Registros TLB (Translation Lookaside Buffer)

- ==TLB== (Translation Lookaside Buffer) — "buffer de traducción anticipada" — es una memoria muy rápida ubicada entre la CPU y la memoria principal.
- Guarda las **últimas traducciones página→frame** utilizadas.
- Funcionamiento: la CPU necesita traducir una dirección lógica → primero busca en el TLB.
  - Si la encuentra (**TLB hit**): obtiene el frame directamente, muy rápido.
  - Si no la encuentra (**TLB miss**): va a la tabla de páginas en memoria, recorre secuencialmente, obtiene el frame, y actualiza el TLB.
- Funciona igual que la **memoria caché**: aprovecha el principio de proximidad (si accedí a una dirección, es probable que pronto acceda a una cercana), por lo que lo que acaba de usarse está en el TLB.

### 5.6 Tabla de páginas invertida

- En vez de tener una tabla de páginas **por proceso**, hay una única tabla global **por frame**.
- Cada fila corresponde a un frame y contiene: ID del proceso + número de página que está guardada ahí.
- Se llama "invertida" porque está ordenada por frames, no por páginas.
- La dirección lógica pasa a tener 3 componentes: $(\text{ID proceso},\ \text{número de página},\ \text{desvío})$.
- Ventaja: una sola tabla para todo el sistema. Desventaja: la búsqueda debe identificar el proceso también.

---

## 6. Administración por segmentación

### 6.1 Concepto

- En la ==segmentación==, la memoria **no** se divide en bloques del mismo tamaño.
- El programa se divide en **segmentos de tamaño variable**, definidos de manera lógica (por ejemplo, según las secciones del programa).
- Elimina la fragmentación interna: al poder ajustar el tamaño de cada bloque, se le da al proceso exactamente lo que necesita.

> [!example] Ejemplo del profe (analogía de los ladrillos)
> "Si tenés que hacer una pared con ladrillos estándar, los vas poniendo sin pensar. Si los ladrillos son todos de distinto tamaño, cada vez que ponés uno tenés que buscar el que tenga la forma correcta para ese espacio. La administración de páginas (todos iguales) termina siendo más eficiente en la práctica que la de segmentos (todos distintos)."

### 6.2 Dirección lógica en segmentación

$$\text{dirección lógica} = (\text{número de segmento},\ \text{desvío})$$

### 6.3 Tabla de segmentos

Cada proceso tiene una **==tabla de segmentos==** con:
- Número de segmento
- Registro base: dirección de inicio del segmento en memoria principal
- Registro límite: tamaño del segmento

**Traducción:** para acceder al segmento $s$ con desvío $d$:
1. Ir a la tabla de segmentos, buscar el segmento $s$.
2. Verificar que $d < \text{límite}$ (el desvío no se "cae" fuera del segmento).
3. Dirección física $= \text{registro base} + d$.

> [!example] Ejemplo del profe
> Segmento 0 con registro base = 1000 y límite = 512. Si busco el desvío 200: verifico $200 < 512$ ✓ → dirección física $= 1000 + 200 = 1200$.

- Ventajas: elimina fragmentación interna; permite cortar el programa en lugares lógicamente convenientes.
- Desventaja: administrar bloques de distinto tamaño es más complejo que bloques iguales (más costoso computacionalmente).

---

## 7. Segmentación paginada

### 7.1 Motivación

La segmentación paginada busca **combinar las ventajas de ambos esquemas**: la división lógica de la segmentación + la uniformidad de la paginación.

- Cada **segmento** se divide internamente en **páginas de tamaño fijo**.
- Así la unidad mínima de asignación es la página → vuelve a aparecer la **fragmentación interna** (en la última página del último segmento).

### 7.2 Dirección lógica en segmentación paginada

$$\text{dirección lógica} = (\text{número de segmento},\ \text{número de página},\ \text{desvío})$$

### 7.3 Estructura de tablas

- Hay una **tabla de segmentos** que mapea cada segmento a su propia **tabla de páginas**.
- Cada tabla de páginas mapea páginas → frames.
- Para traducir: segmento → tabla de páginas del segmento → frame → dirección física con el desvío.

---

## 8. Ejercicios de paginación — cálculo de bits (Clase 7)

El profe plantea y resuelve en clase el siguiente ejercicio:

> **Enunciado:** Espacio de direcciones lógicas de 8 páginas, cada página con 512 palabras, cada palabra de 2 bytes. Espacio físico de 32 frames.

### Pregunta a) — Espacio que ocupa el programa (8 páginas)

$$8 \text{ páginas} \times 512 \text{ palabras/página} \times 2 \text{ bytes/palabra} = 8{.}192 \text{ bytes}$$

### Pregunta b) — Capacidad de memoria con 32 frames libres

Los frames tienen el mismo tamaño que las páginas:

$$32 \text{ frames} \times 512 \text{ palabras/frame} \times 2 \text{ bytes/palabra} = 32{.}768 \text{ bytes}$$

### Pregunta c) — Bits para especificar una dirección lógica

La dirección lógica tiene 2 partes: número de página + desvío.

- Para representar **8 páginas**: $2^3 = 8$ → se necesitan **3 bits** para el número de página.
- Para representar el **desvío dentro de una página** (512 palabras): $2^9 = 512$ → se necesitan **9 bits** para el desvío.

$$\text{Bits dirección lógica} = 3 + 9 = \boxed{12 \text{ bits}}$$

### Pregunta d) — Bits para especificar una dirección física

La dirección física tiene 2 partes: número de frame + desvío.

- Para representar **32 frames**: $2^5 = 32$ → se necesitan **5 bits** para el frame.
- El **desvío** es el mismo que antes: **9 bits**.

$$\text{Bits dirección física} = 5 + 9 = \boxed{14 \text{ bits}}$$

> [!important] El profe recalca
> El método para calcular los bits es: "¿Cuántas combinaciones necesito representar? → Busco la potencia de 2 que lo cubra → ese exponente es la cantidad de bits." Por ejemplo: $2^3 = 8$ páginas → 3 bits. Si tuviera 9 páginas necesitaría 4 bits ($2^4 = 16$, sobrando 7 combinaciones). También se puede aplicar $\log_2(n)$ y redondear hacia arriba.

> [!warning] Ojo / suele tomar
> El profe confirma que estos ejercicios de cálculo de bits entran en los parcialitos. También menciona una variante adicional: dada una dirección absoluta, determinar en qué página y con qué desvío se encuentra → dividir la dirección por el tamaño de página (512) para obtener el número de página, y el resto es el desvío.

---

## 9. Cuadro comparativo — Formas de administración

| Esquema | Bloques | Fragmentación | Dirección lógica |
|---|---|---|---|
| Partición simple | Variable (2 particiones) | Externa | Número simple |
| Particiones múltiples fijas | Fijo | Externa | Número simple |
| Particiones múltiples variables | Variable (dinámico) | Externa | Número simple |
| Paginación | Fijo (páginas = frames) | **Interna** | (página, desvío) |
| Segmentación | Variable | Ninguna | (segmento, desvío) |
| Segmentación paginada | Variable + fijo interno | **Interna** (última página) | (segmento, página, desvío) |

---

## Enlaces

- [[U5 - Abrazo Mortal (Deadlock)]]
- [[U7 - Memoria Virtual]]
