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
clases: "Clase 11"
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

## Estado de la unidad al final de la clase

> [!warning] Unidad incompleta — continúa en la clase siguiente
> El profe explicitamente cortó la clase antes de terminar la unidad porque tenían que hacer el segundo parcialito. Al final de la clase dijo: "Me falta un poquito. La clase que viene terminamos de ver esta unidad. Me queda ver algo de **directorios** y **consistencia**." Esos temas (estructura de directorios y consistencia del sistema de archivos) **no fueron cubiertos en la Clase 11** y serán parte de la Clase 12 junto con la Unidad 10.

---

## Enlaces

- [[U8 - Administración de Dispositivos de Entrada-Salida]]
- [[U10 - Introducción a los Sistemas Distribuidos]]
