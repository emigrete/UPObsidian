---
title: "U9 — Administración de Archivos"
aliases:
  - "Unidad 9"
  - "Archivos"
tags:
  - sistemas-operativos
  - segundo-parcial
  - unidad-9
parcial: "Segundo Parcial"
unidad: 9
clases: "Clases 11-12"
fuente: "Transcripciones Prof. Arzubi"
---

# U9 — Administración de Archivos

> [!info] De qué trata
> Esta unidad cubre la administración de archivos desde la perspectiva del sistema operativo: qué es un archivo, sus atributos y operaciones, la clasificación en estructurados y no estructurados, las formas de direccionamiento dentro de un archivo, las tablas de archivos que mantiene el SO, los métodos de acceso a memoria secundaria (secuencial y directo), y los distintos tipos de archivo que se pueden implementar sobre acceso directo. El profe la considera una unidad corta y sencilla, con conceptos que los alumnos ya conocen de otros contextos.

---

## Qué es un archivo

> [!note] Definición
> Un archivo es una **visión lógica de información almacenada**. Antes se usaba el término "entidad": algo que tiene datos reunidos porque tiene lógica (sentido) de estar juntos.

El profe explica que todo lo que es software son archivos, sin excepción. No hay otra forma de software:
- Archivos fuente, objeto, ejecutables.
- Imágenes, videos, archivos de música.
- Librerías, directorios, archivos `.bat`.
- Bases de datos.

> [!example] Ejemplo del profe
> Un archivo de personal tiene nombre, DNI, fecha de nacimiento. Esos datos están juntos porque tiene sentido lógico que estén juntos. Una fotografía está formada por pixels que juntos le dan sentido a esa imagen. Un video, un archivo de música: todos son archivos porque sus elementos reunidos forman algo con sentido.

El profe aclara que algunos autores exigen que el archivo esté en ==memoria secundaria== como condición. Él no se anima a tanto porque podría haber archivos transitorios; sin embargo, coincide en que para que un archivo **perdure en el tiempo**, necesariamente tiene que estar en memoria secundaria.

---

## Atributos de un archivo

Todo archivo tiene ==atributos==, que son todas las características que permiten clasificar e identificar ese archivo:

- Nombre del archivo.
- Tipo de archivo.
- Dueño del archivo.
- Prioridad.
- Permisos: quién puede modificarlo, si es de lectura o escritura, etc.

---

## Operaciones sobre archivos

Las operaciones que se pueden realizar sobre un archivo son:

- Crear el archivo.
- Escribir en el archivo.
- Borrar / eliminar el archivo.
- Reubicar el archivo.
- Cerrar el archivo.

---

## Clasificación de archivos: estructurados vs. no estructurados

El profe introduce una clasificación central para entender de qué va a hablar en el resto de la unidad.

### Archivos no estructurados (flujo de bytes)

Son todos los archivos que **no** están organizados en registros: videos, imágenes, archivos `.bat`, directorios, etc. El profe los menciona para dejarlos de lado: **no son el foco de esta unidad**.

### Archivos estructurados (archivos tradicionales)

> [!note] Definición
> Un ==archivo estructurado== es aquel formado por **registros**, donde cada registro está constituido por **campos**. Son los archivos con los que se trabaja fundamentalmente en aplicaciones de gestión comercial, administrativa, etc.

> [!example] Ejemplo del profe
> Un archivo de personal tiene un registro por cada persona. Todos los registros tienen los mismos campos: DNI, nombre, fecha de nacimiento, etc. Un archivo de artículos tiene código del artículo, nombre, precio, volumen, etc. Cada fila es un registro; cada dato dentro de la fila es un campo.

> [!important] El profe recalca
> A partir de este punto, **todo lo que se ve en la unidad aplica a archivos estructurados**. El sistema operativo maneja algunos tipos de archivos estructurados y los pone a disposición. Estos son también llamados "archivos tradicionales".

---

## Formas de direccionamiento dentro de un archivo

El profe explica que hay 3 formas de direccionar (ubicar) un registro dentro de un archivo:

### 1. Direccionamiento físico

Se indica la ubicación del registro especificando **cuántos bytes hay que desplazarse** desde el inicio del archivo para llegar a él.

> [!example] Ejemplo del profe
> "El archivo empieza en la dirección 1000. Me corro 250 bytes y ahí está el registro de González." — Requiere conocer la estructura física del disco.

### 2. Direccionamiento relativo

Más evolucionado. Se indica simplemente el **número de registro** dentro del archivo, sin importar cuántos bytes ocupa cada uno.

> [!example] Ejemplo del profe
> "Dame el registro número 55 del archivo de personal." No se especifican bytes; el SO hace el cálculo internamente. Es más cómodo pero requiere más administración del sistema.

### 3. Direccionamiento simbólico

El más cómodo y el más usado en sistemas de gestión. El registro se identifica por su ==campo clave==.

> [!example] Ejemplo del profe
> "Buscame el registro con DNI 12.345.678." El campo clave es el DNI. Con ese valor, el SO ubica el registro. Es el método más independiente de la estructura física, pero es el que más recursos consume (más memoria, más tiempo de procesamiento).

> [!important] El profe recalca
> El direccionamiento simbólico es el que más esfuerzo le requiere al sistema operativo pero es el que más le gusta a todos cuando se trabaja con sistemas de gestión.

**Comparación de los tres tipos:**

| Tipo | Criterio de búsqueda | Esfuerzo del SO |
|---|---|---|
| Físico | Desplazamiento en bytes | Mínimo |
| Relativo | Número de registro | Intermedio |
| Simbólico | Valor del campo clave | Máximo |

---

## Tablas de archivos del sistema operativo

El SO mantiene **dos tipos de tablas** para gestionar los archivos en uso:

### Tabla única del sistema (de archivos abiertos)

Una sola tabla global donde figuran **todos los archivos que están siendo utilizados** por los distintos procesos en un momento dado. No contiene todos los archivos del sistema, solo los que están abiertos/en uso.

### Tabla de archivos por proceso

Cada proceso tiene **su propia tabla de archivos**. En ella se registran:
- Qué archivos está usando ese proceso.
- A qué posición apunta el ==puntero de archivo== de ese proceso dentro de cada archivo (por ejemplo, si está apuntando al registro de González, al de Pérez, etc.).

> [!note] Por qué existen las dos
> Un mismo archivo puede estar siendo usado por varios procesos simultáneamente. La tabla única del sistema lo registra una sola vez. Pero cada proceso necesita su propia tabla porque cada uno tiene su propio puntero (su propia posición de lectura/escritura dentro del archivo).

---

## Métodos de acceso a memoria secundaria

Hay exactamente **dos métodos de acceso** a memoria secundaria. No hay más.

### Acceso secuencial

Se aplica principalmente con ==cintas== como memoria secundaria. Para llegar a un registro, hay que recorrer la cinta desde el principio de forma lineal hasta encontrarlo.

> [!example] Ejemplo del profe
> "Imaginense la cinta físicamente. Si el registro que quiero está en el medio, tengo que hacer avanzar la cinta desde el principio hasta llegar ahí. No puedo saltar directamente."

Las cintas se siguen usando para backups e históricos.

### Acceso directo

Se aplica con ==discos== (y memorias secundarias direccionables). Permite acceder directamente a cualquier posición sin recorrer las anteriores.

> [!example] Ejemplo del profe
> "Imaginen el disco. Con acceso directo, puedo ir directamente a esta dirección sin pasar por las de antes."

---

## Tipos de archivo según el método de acceso

### Sobre acceso secuencial: solo un tipo posible

#### Archivo secuencial lineal (en cinta)

Es el único tipo de archivo que se puede implementar sobre acceso secuencial. Los registros están uno tras otro y no pueden insertarse ni eliminarse en el medio.

**El gran problema:** no puede crecer fácilmente. Si hay que insertar un registro, hay que reescribir el archivo completo en otro lugar (no se puede escribir "en el medio" de una cinta).

**Cómo se actualizaba históricamente:**

> [!example] Ejemplo del profe
> En la época en que solo existía este método, se trabajaba con un **archivo maestro** (por ejemplo, todos los alumnos de la Universidad de Palermo, armado el lunes) y un **archivo de transacciones** (las altas, bajas y modificaciones que ocurrían durante la semana). El lunes siguiente se hacía un proceso que leía ambos archivos en paralelo, intercalaba los cambios, y generaba un **nuevo archivo maestro** actualizado. Así era como se mantenían actualizados los archivos secuenciales.

---

### Sobre acceso directo: varios tipos posibles

El acceso directo permite implementar distintos tipos de archivo porque da flexibilidad para organizar los datos.

#### 1. Archivo secuencial lineal (en disco)

Se puede implementar un archivo secuencial sobre un disco. La ventaja es que es administrativamente simple (solo hay que saber dónde empieza). La desventaja es la misma que antes: para agregar un registro hay que copiar el archivo completo a otro lado y reescribirlo.

> [!important] El profe recalca
> No hay un tipo de archivo que sea mejor que otro. Depende siempre del problema que haya que resolver. El secuencial lineal sobre disco es barato y fácil de manejar; si alcanza para el problema, se usa.

#### 2. Archivo secuencial enlazado

Es un archivo secuencial, pero los registros están ==enlazados== entre sí mediante punteros (como una lista enlazada).

**Ventaja clave respecto al secuencial lineal:** puede **crecer sin reescribirse**. Si hay que agregar un registro, simplemente se ubica en cualquier lugar libre del disco y se ajustan los enlaces. Si hay que eliminar un registro, se redirige el enlace del anterior al siguiente.

**Desventaja:** el acceso sigue siendo secuencial. Para encontrar un registro hay que recorrer la cadena de enlaces desde el principio.

#### 3. Archivo secuencial indexado

Está formado por **dos partes**:
- La ==parte de datos==: contiene todos los registros con todos sus campos (incluido el campo clave).
- La ==parte de índice== (fichero de índice): una o varias tablas auxiliares, cada una con el campo clave y la dirección física donde está ese registro.

Se pueden tener varios ficheros de índice, uno por cada ordenamiento que interese (por DNI, por apellido, etc.).

**Los índices pueden ser:**
- **Densos:** contienen una entrada por cada registro del archivo.
- **No densos:** trabajan con rangos, similar a las tablas multinivel de memoria virtual; achican el espacio de búsqueda.

> [!example] Ejemplo del profe
> "Tengo el archivo de personal. En el fichero de índice por DNI busco el DNI que quiero, encuentro la dirección, y voy directo a ese registro en el archivo de datos. Si quiero recorrerlo por orden alfabético, uso el fichero de índice por apellido."

#### 4. Archivo de acceso calculado (Hashing)

Utiliza ==técnicas de hashing==: se aplica un algoritmo al campo clave del registro y el resultado es la dirección donde está (o debe guardarse) ese registro en el disco.

> [!important] El profe recalca
> Es muy rápido porque hacer un cálculo matemático es muy barato para la CPU. El problema es la generación de **sinónimos**.

> [!example] Ejemplo del profe (estúpido pero ilustrativo, según él)
> "Supongamos que la técnica de hashing consiste en truncar el DNI: me quedo con ciertos dígitos del medio y eso me da la dirección en disco. Ahora bien, si tengo dos DNIs distintos que al truncarlos dan el mismo resultado, eso es un sinónimo."

**El problema de los sinónimos:** cuando dos campos clave distintos producen la misma dirección al aplicarles el algoritmo. Hay tres formas de tratarlos:

1. **Primer espacio libre:** si la dirección calculada está ocupada, se busca hacia adelante hasta encontrar el primer espacio libre y se guarda ahí.
2. **Rehashing:** se aplica el algoritmo de hashing nuevamente al resultado obtenido para calcular una nueva dirección.
3. **Zona de overflow:** se reserva una zona especial dentro del archivo. Cuando aparece un sinónimo, se guarda directamente en esa zona. Al buscar, si no se encuentra en la dirección calculada, se busca secuencialmente en la zona de overflow.

> [!note] Objetivo del diseño de hashing
> Cuanto mejor sea la técnica de hashing, menor cantidad de sinónimos producirá. Eso es lo que se busca al elegir o diseñar el algoritmo.

#### 5. Archivo de acceso llave

El ==campo clave== del registro **coincide directamente con la dirección** en disco. Es el método de acceso más rápido de todos (no hay cálculo ni búsqueda en índice: el campo clave es la dirección).

**Limitación importante:** es muy difícil de implementar en la práctica, porque el rango posible de valores del campo clave puede ser enorme, y habría que reservar espacio en disco para toda esa cantidad de direcciones posibles, dejando mucho espacio vacío.

> [!example] Ejemplo del profe
> "Si el campo clave es el DNI, tendría que reservar en disco espacio para cada número de DNI posible. Es una cantidad impresionante de disco vacío para cubrir todos los DNIs posibles."

> [!important] El profe recalca
> Hay que saber que existe. Si alguna vez uno está desarrollando una aplicación con un archivo pequeño y cuyos valores de campo clave pueden hacerse coincidir con direcciones de disco (por ejemplo, en un sistema embebido con poca memoria), es el método más rápido que se puede tener.

---

## Estructura de directorios

El profe aclara que **directorios** es el término formal de lo que hoy se llama **carpetas** (y subcarpetas, sub-subcarpetas, etc.). Los directorios pueden ser de distinto tipo, y los presenta como una **evolución histórica**: cada tipo aparece para resolver las limitaciones del anterior.

> [!note] Cómo busca el sistema operativo
> Cuando uno pide "buscar" un archivo o una carpeta, el SO **recorre el árbol de directorios rama por rama** hasta encontrar lo que se busca. Cómo recorre ese árbol (y si puede o no volver hacia arriba) es lo que distingue a los últimos dos tipos.

### 1. Directorio de un solo nivel (raíz)

El primero de la historia. Hay un único directorio, el del **nivel raíz**, y todos los archivos cuelgan directamente de él, uno al lado del otro.

**Limitación:** no permite **ninguna organización**: no se pueden agrupar archivos por tipo, por usuario, ni por nada. Solo están todos juntos bajo la raíz.

> [!note] Nivel de abstracción
> El profe aclara que internamente el SO no lo guarda exactamente así (uno abajo del otro), pero conceptualmente está bien verlo de esa manera porque es lo que representa.

### 2. Directorio de dos niveles

Aparece un segundo nivel debajo de la raíz. Permite empezar a **organizar**: por ejemplo, una rama por cada usuario (usuario 1, usuario 2, usuario 3), o un mismo usuario separando sus archivos por tipo.

> [!example] Ejemplo del profe
> Es como armar **una carpeta y dentro una subcarpeta**: ya se puede agrupar, aunque la capacidad de organización todavía es limitada (solo dos niveles).

### 3. Directorio de N niveles (de árbol)

Es el que usan los sistemas actuales (Windows). Se pueden abrir y ramificar todos los directorios y subdirectorios que se quiera, dando una **capacidad de organización muy grande**.

> [!note] Por qué "árbol invertido"
> Es un **árbol invertido**: la raíz está arriba y las hojas van hacia abajo. El profe comenta que el nombre "árbol" no es el más adecuado por eso, pero hay que tener claro que está invertido.

**Limitación:** **no permite compartir** archivos o carpetas. Cada usuario solo tiene acceso a las carpetas que están en **su propia rama**.

### 4. Directorio de grafo acíclico

Ya no es un árbol, sino un **grafo**: un mismo archivo o carpeta puede ser alcanzado por **dos ramas distintas**.

**Novedad:** esto permite **compartir** archivos o carpetas. Si una rama es de un usuario y la otra de otro, ambos pueden acceder al mismo archivo o carpeta.

> [!note] Por qué "acíclico"
> Cuando el SO recorre el grafo buscando, **no puede subir** por la otra rama: llega al archivo compartido, vuelve hacia atrás y sigue recorriendo por el otro lado. Como nunca sube por otra rama, **no se pueden producir loops**.

### 5. Directorio de grafo general

Es como el acíclico (también permite compartir), pero con una diferencia clave: al recorrer, el sistema **sí puede subir** por otra rama.

> [!important] El profe recalca
> El problema del grafo general es que poder volver por otra rama puede hacer que el sistema entre en un **loop** durante la búsqueda. Por eso, si se usa este tipo de directorio, **hay que tomar medidas para evitar que se produzcan loops**. El grafo acíclico no tiene este problema.

**Resumen de qué permite cada tipo:**

| Tipo de directorio | Organización | ¿Comparte? | ¿Riesgo de loop? |
|---|---|---|---|
| Un solo nivel (raíz) | Ninguna | No | No |
| Dos niveles | Básica | No | No |
| N niveles (árbol) | Muy grande | No | No |
| Grafo acíclico | Muy grande | **Sí** | No |
| Grafo general | Muy grande | **Sí** | **Sí** (hay que evitarlo) |

---

## Consistencia en modo multiusuario

> [!note] Qué es la consistencia
> **Consistencia** es que **no se corrompan los datos** guardados en un archivo, en particular cuando **dos o más usuarios acceden a la misma información** simultáneamente.

Hay básicamente **dos formas** de asegurar la consistencia:

### Consistencia de sesión

El que **abre primero** el archivo es el que tiene la **autoridad para modificarlo**. Los demás que accedan **pueden verlo pero no modificarlo**, y **no ven los cambios** que hace el primero hasta que este **guarde y cierre** el archivo.

> [!example] Ejemplo del profe
> Un archivo de Word compartido: cuando uno lo está modificando, el otro lo puede ver pero no modificar. La prioridad la tiene **quien inicia la sesión**.

### Consistencia de archivos compartidos inmutables

Varios usuarios pueden acceder al mismo archivo y **cualquiera puede modificarlo**. Las modificaciones que hace uno son **vistas inmediatamente** por los demás. La restricción ya no es sobre el archivo completo, sino que se reduce **al registro (o campo) que se está modificando**: el control de acceso es de **grano mucho más fino**.

> [!example] Ejemplo del profe
> Una cuenta bancaria recíproca (de dos titulares): los dos pueden **mirar** la cuenta al mismo tiempo, pero solo **uno puede sacar el dinero** a la vez. El bloqueo es a nivel de registro/campo, no de todo el archivo.

---

## Métodos de asignación de espacio a los archivos en el disco

Trata de **cómo hace el SO (el módulo que administra el disco) para asignarle espacio a los archivos**.

> [!note] Todo se maneja por bloques
> La memoria secundaria siempre se administra a nivel de **bloques**, todos del mismo tamaño. No se transfiere un registro suelto, sino un bloque (que incluye una cantidad de registros según el tamaño de bloque del disco). Esto es **independiente** de si hay paginación o no.

Hay **tres formas** de administrar esos bloques:

### 1. Asignación contigua

Los bloques de un archivo se asignan **uno al lado del otro** (apegados). El directorio guarda el **bloque inicial** y la **longitud** (cantidad de bloques).

> [!example] Ejemplo del profe
> El archivo "cuenta" empieza en el bloque 0 con longitud 2 (ocupa los bloques 0 y 1); el archivo "correo" empieza en el bloque 3 con longitud 3 (bloques 3, 4, 5).

**Problema (el crecimiento):** los archivos crecen, y puede no haber espacio libre al lado. Como la asignación tiene que seguir siendo contigua, hay que **trasladar el archivo completo** a otro lugar donde quepa con su nuevo tamaño. Es un inconveniente importante.

### 2. Asignación enlazada

Usa **punteros**, igual que el archivo secuencial enlazado, pero aplicado al **espacio (bloques)** que el sistema le da al archivo, no a los registros. El directorio guarda el **bloque inicial**, y cada bloque apunta al siguiente; hay que recorrerlos de forma **secuencial**.

> [!example] Ejemplo del profe
> El archivo "cuenta" empieza en el bloque 1, que apunta al 5, que apunta al 11… los bloques están vinculados por punteros.

**Ventaja:** el archivo **puede crecer** sin trasladarlo. Si necesita otro bloque, se le da cualquier bloque libre y se ajustan los punteros (igual que el secuencial enlazado de registros).

### 3. Asignación indizada (indexada)

El archivo tiene un **bloque de índices** (son los famosos **i-nodos** / tablas de índice de bloques). El directorio apunta a ese bloque de índice, que contiene la **lista de todos los bloques que forman el archivo**.

> [!example] Ejemplo del profe
> El archivo "cuenta" tiene como bloque de índice el bloque 8; dentro del bloque 8 está la lista de los bloques que forman el archivo: 1, 6, 5, 11.

**Crecimiento (muy sencillo):** si el bloque de índice se llena (no entran más números de bloque), se **divide en dos**, y la mitad de los datos va con un bloque de índice y la otra mitad con otro. Si sigue creciendo, se vuelve a dividir, formando una especie de **árbol** que crece lateralmente. Permite el **crecimiento sin restricciones**.

---

## Gestión del espacio libre en el disco

Antes de asignar espacio (sección anterior), el SO necesita **saber cuáles son los bloques libres**. Hay **cuatro formas** de registrar el espacio libre:

### 1. Vector o mapa de bits

Los **primeros bloques del disco** se dedican a guardar un **bit por cada bloque** del disco: el bit vale **0 o 1** según ese bloque esté libre u ocupado. El mapa de bits tiene que tener **tantos bits como bloques** tiene el resto del disco.

> [!note] Tamaño del mapa
> El mapa puede ocupar 2, 4, 10, 50… bloques: depende del tamaño del disco (cuantos más bloques tenga el disco, más bits necesita el mapa para referenciarlos).

### 2. Lista enlazada

Una **lista enlazada de los bloques libres** que hay que recorrer **secuencialmente** para ir encontrándolos. Es muy económica, pero al ser un recorrido secuencial **no es muy eficiente**.

### 3. Agrupamiento

Los **primeros bloques** del disco guardan **directamente todas las direcciones de los bloques libres**. De ahí el sistema las analiza y resuelve cómo asignarlos.

### 4. Recuento

Se guarda la **dirección de un bloque** y **cuántos bloques libres hay a continuación**.

> [!example] Ejemplo del profe
> "Bloque 10 → 5" significa que a partir del bloque 10 hay 5 bloques libres; "bloque 30 → 20", "bloque 50 → 100", etc.

---

## Ejercicio resuelto: mapa de bits

> [!question] Enunciado del profe
> Un mapa de bits ocupa **1.024 bloques**. Cada bloque ocupa **8 KB**.
> a) ¿Cuántos bloques tiene el disco?
> b) ¿Cuál es el tamaño del disco en gigabytes?

> [!success]- Ver resolución
> **a) Cantidad de bloques del disco.** En el mapa de bits, **cada bit representa un bloque** del disco. Hay que calcular cuántos bits tiene el mapa:
> - El mapa ocupa 1.024 bloques × 8 KB = 8.192 KB.
> - Pasado a bits: 8.192 KB × 1.024 (bytes/KB) × 8 (bits/byte) = **67.108.864 bits**.
> - Como cada bit referencia un bloque → el disco tiene **67.108.864 bloques** (≈ 67 millones).
>
> **b) Tamaño del disco en GB.** Se multiplica la cantidad de bloques por el tamaño de cada bloque y se pasa a GB (como dijo Aarón en clase: ×8 KB, ÷1.024 y ÷1.024):
> - 67.108.864 bloques × 8 KB = 536.870.912 KB.
> - ÷ 1.024 = 524.288 MB.
> - ÷ 1.024 = **512 GB**.

---

## Enlaces

- [[U8 - Administración de Dispositivos de Entrada-Salida]]
- [[U10 - Introducción a los Sistemas Distribuidos]]
