---
title: "U1 — Introducción a los Sistemas Operativos"
aliases:
  - "Unidad 1"
  - "Introducción SO"
tags:
  - sistemas-operativos
  - primer-parcial
  - unidad-1
parcial: "Primer Parcial"
unidad: 1
clases: "Clases 1-2"
fuente: "Transcripciones Prof. Arzubi"
---

# U1 — Introducción a los Sistemas Operativos

> [!info] De qué trata
> Esta unidad presenta qué es un sistema operativo, para qué sirve y cómo funciona conceptualmente. El profe hace hincapié en entenderlo desde un **nivel de abstracción alto** y después ir bajando hasta los algoritmos. No se estudia ningún SO en particular (nada de Windows, Linux, etc.): se estudia el funcionamiento conceptual que es común a todos.

---

## Definición de sistema operativo

> [!note] Definición
> El ==sistema operativo== es un **conjunto de programas** que controla la ejecución de las aplicaciones y actúa como **interfaz** entre ellas y el hardware de la computadora.

El profe lo explica así: esquemáticamente, tenés el hardware (CPU, memoria, dispositivos) en la capa de abajo, encima el sistema operativo, y encima de todo las aplicaciones. El SO es la interfaz que está en el medio. Si no existiera ese intermediario, cada aplicación tendría que administrar la memoria, la impresora, los recursos, todo sola. Al existir el SO, todo eso es **transparente** para quien programa.

> [!important] El profe recalca
> El sistema operativo es **software** igual que cualquier programa de usuario. Se ejecuta en la misma CPU y ocupa lugar en la memoria principal. Eso tiene una consecuencia particular que se explica más adelante.

---

## Objetivos del sistema operativo

El profe menciona **3 objetivos**:

1. **Comodidad** — el SO debe ser una interfaz amigable que facilite el uso del hardware y las comunicaciones a las aplicaciones y a los usuarios.
2. **Eficiencia** — importante distinguir eficacia de eficiencia:
   - *Eficacia*: cumplir el objetivo (hacer la tarea, punto).
   - *Eficiencia*: cumplirlo sacándole el **máximo provecho a los recursos**. Los recursos siempre son escasos (como el dinero, dice el profe). El SO debe asignarlos de la manera más eficiente posible.
3. **Capacidad de evolución** — todo software cambia. El que hace un SO tiene que construirlo de modo que permita incorporar nuevas funciones sin romper lo que ya funciona.

> [!example] Ejemplo del profe
> La eficiencia se entiende igual que en cualquier industria: yo puedo haber terminado el trabajo (fui eficaz), pero si usé el doble de recursos de los necesarios, no fui eficiente. En SO, el recurso más caro e importante es la CPU, y hay que usarla al máximo.

---

## Funciones del sistema operativo

El profe dice que originalmente tenía 3 funciones, pero "siempre hay gente que piensa y piensa" y ahora son 5. Las funciones son más precisas que la definición para entender qué hace el SO.

| # | Función | Qué hace |
|---|---------|----------|
| 1 | ==Gestión de recursos== | Administra y asigna todos los recursos del sistema (CPU, memoria, dispositivos) a los procesos. |
| 2 | ==Ejecución de servicios para los programas== | Todo lo que un programa necesita (traer datos del disco, imprimir, etc.) se lo pide al SO. El programa nunca accede directamente al hardware. |
| 3 | ==Ejecución de los mandatos de usuario== (Shell) | Son los comandos con los que el usuario interactúa directamente con el SO: crear una carpeta, copiar un archivo, hacer un backup. En Windows, todo lo que hacés fuera de una aplicación son mandatos de usuario. |
| 4 | ==Abstracción== | El SO le genera a cada usuario una **visión particular** del sistema de cómputo: el usuario trabaja como si estuviera solo, con los recursos que le corresponden. |
| 5 | ==Aislamiento== | Evita que un usuario entre en los recursos o datos de otro. Mantiene la privacidad y la "propiedad" de los datos de cada usuario. |

> [!note] Definición
> El **Shell** es el módulo del SO que implementa la función de ejecución de mandatos. Actúa como intérprete entre el usuario y el sistema.

> [!important] El profe recalca
> Las funciones de abstracción y aislamiento se volvieron necesarias a medida que aumentó la cantidad de usuarios que participan en un sistema. En un sistema con un solo usuario, no eran tan relevantes.

---

## El SO es software: una situación única

Esta es una reflexión conceptual que el profe considera clave ("hay cosas que no pueden no saber").

En cualquier industria convencional, el **elemento de control está fuera** de lo que controla (sensores en una línea de producción, etc.). Con el SO pasa algo diferente: el SO es software igual que las aplicaciones, así que **coexisten en la misma memoria principal**.

Lo que ocurre:

- Para ejecutarse, tanto el SO como las aplicaciones deben estar cargados en la **memoria principal** (RAM). La CPU solo trabaja sobre la memoria principal.
- Cuando el SO le entrega el uso de la CPU a un proceso, el **SO queda "dormido"** — no se ejecuta.
- El SO debe dejar todo preparado de antemano para poder **recuperar el control** ante cualquier evento: que el proceso pida algo (llamada al sistema), que ocurra una interrupción, que el proceso termine, etc.
- Cuando recupera el control, la CPU ejecuta el SO, que hace todas sus tareas administrativas. Una vez terminadas, pone a ejecutar el siguiente proceso.

> [!example] Ejemplo del profe
> Es como en el cine cuando dicen "corten". En ese momento hay que guardar exactamente cómo quedó cada cosa (cada actor con la mano en el lugar correcto, los anteojos puestos, todo igual) para poder continuar la escena después. El SO hace lo mismo: guarda todos los valores y punteros del proceso interrumpido para poder restaurarlo exactamente igual más adelante.

> [!important] El profe recalca
> **El sistema operativo se ejecuta igual que cualquier otro programa.** Cuando pone a ejecutar un proceso, el SO queda dormido. Cuando hay un evento (interrupción o llamada al sistema), el SO retoma la CPU, ejecuta sus tareas y vuelve a poner a ejecutar un proceso. Esta particularidad es exclusiva del software y no tiene equivalente en la industria convencional.

---

## Estructura básica del sistema de cómputo

Esquema que el profe usa en toda la materia:

```
[ CPU ] <---> [ Memoria Principal (RAM) ]
                        ^
                        |
              [ Módulo de E/S ] <---> [ Disco / Impresora / etc. ]
```

Puntos clave:

- **Los programas** están almacenados en la **memoria secundaria** (disco) cuando la computadora está apagada o cuando no se ejecutan.
- Para ejecutarse, un programa **debe estar cargado en la memoria principal**. La CPU solo actúa sobre la memoria principal.
- Los datos (ej: los datos de un registro en base de datos) también deben estar en memoria principal para ser procesados.
- El SO también debe tener **al menos una parte cargada en memoria principal**, porque es software que se ejecuta.

### Jerarquía de memorias

El profe menciona una pirámide de memorias:

| Nivel | Tipo | Velocidad | Costo |
|-------|------|-----------|-------|
| Arriba | Registros de CPU | Máxima | Máximo |
| | Caché | Muy alta | Alto |
| | RAM (memoria principal) | Alta | Medio |
| | Disco / cinta | Lenta | Bajo |
| Abajo | Cinta magnética | Mínima | Mínimo |

- La **caché** guarda lo último leído de memoria principal. La próxima vez que se busca ese dato, si está en caché, se ahorra el acceso a RAM (más lento). Esto funciona porque se cumple el **principio de proximidad de Denning**: cuando se busca una dirección, la próxima búsqueda muy probablemente esté cerca de esa. Por eso mover datos en bloques (no de a una palabra) es eficiente.

### Compilación y enlace (linkedición)

Para que un programa escrito en lenguaje de alto nivel pueda ejecutarse:

1. **Programa fuente** (lenguaje de alto nivel) → no puede ejecutarse directamente.
2. **Compilación** → genera el **programa objeto** (lenguaje de máquina).
3. **Linkedición (enlace)** → agrega las **librerías** (rutinas ya escritas que el lenguaje pone a disposición). Recién aquí el programa está listo para cargarse en memoria principal.

---

## Evolución de los sistemas operativos

El profe aclara que no lo ve por razones históricas, sino porque **los SO fueron agregando funciones pero el funcionamiento básico no cambió**. La computadora más moderna sigue funcionando igual que las primeras (con 0 y 1); cambió la velocidad y el hardware, no el concepto.

> [!example] Ejemplo del profe
> Es como el motor de combustión: mejora la inyección, mejora esto o aquello, pero los cuatro tiempos del ciclo siguen siendo los mismos. Por eso cualquier libro de SO, por viejo que sea, sigue siendo útil conceptualmente.

### Etapa 1: Procesamiento en serie (sin SO)

- No había sistema operativo.
- El centro de cómputo se asignaba por horarios a las distintas áreas de la empresa.
- El operador llegaba con las **tarjetas perforadas** (donde estaba el programa), ponía las cintas, arrancaba la máquina manualmente.
- El hardware era muy inestable (válvulas que se quemaban), la RAM era extremadamente chica.
- La CPU ejecutaba a la velocidad de la impresora o del dispositivo más lento: si había que imprimir recibos, la CPU esperaba que se imprimiera cada línea. **Aprovechamiento de la CPU: muy bajo.**

### Etapa 2: Sistema por lotes monoprogramado

- Apareció el primer SO rudimentario, llamado **Monitor**, con algunos comandos básicos.
- Un jefe de turno operaba la consola y el Monitor respondía a comandos.
- Todavía **un solo proceso en memoria** a la vez.
- El problema de la CPU esperando la E/S seguía existiendo. El gráfico del profe: en el eje del tiempo, el proceso ejecuta un rato → pide datos de González → espera la E/S un buen rato → vuelve a ejecutar. Todo ese tiempo de espera la CPU está ociosa. **Aprovechamiento de la CPU: precario.**

### Etapa 3: Sistema por lotes multiprogramado

- Aparición de los **discos**: permiten acceso directo (no secuencial como la cinta). También tienen más capacidad y velocidad.
- El SO incorporó el **manejo de interrupciones**: puede interrumpir un proceso y retomarlo después.
- Ahora se pueden cargar **varios procesos en memoria principal** al mismo tiempo.
- Cuando el proceso P1 se bloquea esperando E/S, el SO lo saca de la CPU, guarda su estado, y pone a ejecutar P2. Cuando P2 también se bloquea, pone P3, etc.
- Los procesos se van alternando en el uso del procesador. **No es ejecución simultánea** — es ejecución **alternada**. La sensación de simultaneidad se debe a que la CPU cambia tan rápido que nuestros sentidos no lo perciben.

> [!important] El profe recalca
> **Cuidado con usar la palabra "simultáneo"** cuando hay un solo procesador. Los procesos se ejecutan de manera **alternada**, no simultánea. Esto es conceptualmente importante.

- **Grado de multiprogramación**: cuántos procesos están alternando en el uso del procesador. A mayor RAM → más procesos cargados → mayor grado de multiprogramación → mejor aprovechamiento de la CPU.
- **Objetivo**: maximizar el uso de la CPU.
- A estos programas que se lanzan y se ejecutan solos sin intervención del usuario se los llama **programas batch (por lotes)**. Ej: imprimir recibos de sueldos.

#### Overhead

> [!note] Definición
> El ==overhead== es el tiempo que ocupa la CPU el propio sistema operativo entre un proceso y otro. Es el tiempo en que el SO ejecuta sus tareas administrativas: guarda el estado del proceso saliente, busca el siguiente proceso listo, lo carga, etc.

- El overhead **siempre existe** cada vez que hay un cambio de proceso.
- No es fijo: varía según las tareas que el SO tenga que hacer en ese momento.
- El overhead no es parte del tiempo de ejecución de ningún proceso de usuario.

### Etapa 4: Sistemas de tiempo compartido (multiusuario)

- Aparecen los **programas interactivos**: necesitan la participación del usuario durante su ejecución (le preguntan cosas, muestran opciones). Hoy la mayoría de las aplicaciones web son interactivas.
- Con usuarios interactivos, el objetivo del SO **cambia**: ya no es maximizar el uso de la CPU, sino **atender a todos los usuarios de manera equitativa**.
- Mecanismo: el SO divide el tiempo en **quantums** (rodajas de tiempo iguales para cada usuario/proceso) y los asigna de manera rotativa.

> [!example] Ejemplo del profe
> Supongamos un quantum de 20 ms. El SO le da 20 ms al proceso 1, luego 20 ms al proceso 2, luego al 3, y vuelve al 1. Entre cada cambio hay overhead. La velocidad de la CPU es tan alta que los usuarios perciben que están todos atendidos "al mismo tiempo".

#### Análisis de los extremos del quantum

El profe hace este análisis para entender el concepto:

- **Quantum muy chico**: se hace tanto overhead entre proceso y proceso que se llega a un punto donde el sistema pasa más tiempo haciendo overhead que ejecutando. En el extremo: se pone y saca el proceso sin que llegue a ejecutar nada. Por eso, **el quantum tiene un límite mínimo**. Esto también explica por qué los sistemas tienen un límite de usuarios concurrentes: si hay demasiados, el quantum efectivo se vuelve demasiado chico.
- **Quantum muy grande**: el sistema se comporta más como un multiprogramado que como un multiusuario (porque antes de que se acabe el quantum, el proceso ya pidió una E/S y cedió la CPU por su propia cuenta).

> [!warning] Ojo / suele tomar
> Si a un proceso le hacen falta 60 ms y el quantum es 20 ms, el proceso **no se bloquea ni se descarta**: simplemente recibe 20 ms, se pausa, y cuando le toca el turno recibe otros 20, y así hasta completar sus 60. Si en el último quantum termina antes (por ej. a los 10 ms), el SO recupera la CPU sin esperar los 10 ms restantes.

### Etapa 5: Sistemas de multiprocesamiento (procesamiento en paralelo)

- Ya no es multiprogramación (un solo procesador), sino **múltiples CPUs**.
- Con 2 o más CPUs, sí se puede hablar de **ejecución simultánea** real de procesos.

#### Fuertemente acoplado

- Varios procesadores **en la misma computadora**, compartiendo el mismo reloj, la misma memoria principal y el mismo bus.
- Puede ser **simétrico**: cualquier procesador puede cumplir cualquier función (se usan indistintamente).
- Puede ser **asimétrico**: uno o más procesadores se dedican a tareas específicas (ej: uno se encarga de administrar memoria).
- La comunicación entre procesos que corren en distintos procesadores se hace a través de la **memoria principal** (que es compartida).

#### Débilmente acoplado (sistemas distribuidos)

- Computadoras completas distribuidas geográficamente, interconectadas por una red, con un **objetivo común** (ej: sucursales de una empresa).
- Cada equipo tiene su propia memoria, su propio SO.
- La comunicación entre procesos se hace mediante **llamadas a procesos remotos** (más compleja: hay que sincronizar relojes, establecer protocolos, etc.).
- Primero surgió el **software de red**: cada equipo con su propio SO más un software de red compatible.
- Luego surgieron los **sistemas operativos distribuidos**: un único SO que cubre todos los equipos y se encarga también de las comunicaciones. Ventaja para el usuario: los recursos remotos aparecen en su escritorio como si fueran locales (no necesita saber que una base de datos está en Córdoba).

### Etapa 6: Sistemas de tiempo real

- Para aplicaciones **extremadamente exigentes en tiempo de respuesta**: control de aviones supersónicos, centrales nucleares, robots industriales, etc.
- Usan lenguajes de programación de tiempo real (más caros, menos difundidos).

> [!example] Ejemplo del profe
> Software de seguridad de una central nuclear con sensores distribuidos. Si se detecta derrame de agua pesada y al mismo tiempo sube la temperatura del reactor, un SO convencional terminaría de atender el primer evento y recién después atendería el segundo. Un SO de tiempo real puede **interrumpir interrupciones**: tiene manejo de prioridades tan fino que puede detener la rutina que está ejecutando si llega una de mayor prioridad. Cada milisegundo puede ser crítico.

#### Características especiales de los SO de tiempo real

1. **Tolerancia a fallos**: ante una falla en el hardware o en la comunicación, el SO la resuelve solo (cambia a un canal alternativo, reasigna memoria, etc.) sin intervención humana.
2. **Degradación parcial**: si el sistema está perdiendo recursos (ej: se va quedando sin memoria), el SO decide qué funciones mantiene y cuáles sacrifica, preservando siempre las más críticas.

> [!example] Ejemplo del profe
> La degradación parcial es como si te dijeran que tenés que entregar una parte del cuerpo: no vas a entregar la cabeza, vas a entregar un pie o un brazo. El sistema tiene que tener programado de antemano qué es lo más importante de preservar.

> [!warning] Ojo / suele tomar
> "Tiempo real" en informática técnica no es lo mismo que "en línea". Que una empresa diga "te atendemos en tiempo real" y te hagan esperar 4 segundos **no es tiempo real**. Tiempo real implica tiempos de respuesta garantizados, extremadamente cortos, críticos.

---

## Componentes (capas) de un sistema operativo

El SO tiene básicamente **3 capas**:

1. **Núcleo (kernel)**: interactúa directamente con el hardware. Gestión de recursos, manejo de interrupciones, funciones básicas de memoria.
2. **Servicios**: gestión de procesos, gestión de memoria, operaciones de E/S, gestión de almacenamiento secundario, funciones de soporte.
3. **Mandatos de usuario (Shell)**: la interfaz entre el usuario y el sistema.

---

## Arquitectura de los sistemas operativos

El profe señala que, así como las aplicaciones tienen arquitecturas de software, el SO también. Es importante entender los tipos básicos.

### Monolítica

- Todo el SO en **un gran programa sin estructura clara**: todas las funciones juntas, fuertemente acopladas.
- Se ejecuta en **modo núcleo**.
- **Ventaja**: muy rápido (todo está cargado, no hay llamadas a módulos externos).
- **Desventaja**: difícil de mantener. Si cambio un módulo para corregir un error, puedo introducir errores en otro módulo sin darme cuenta, porque todo está muy mezclado.

El profe lo relaciona con los principios de diseño de software:
- **Máxima cohesión**: cada módulo debe cumplir una sola función (ej: un módulo para calcular promedios, otro para calcular becas, no ambos juntos).
- **Mínimo acoplamiento**: los módulos deben depender lo menos posible entre sí. En la arquitectura monolítica el acoplamiento es máximo, lo opuesto al ideal.

### Por capas

- El SO se organiza en **capas superpuestas**, cada una con una función específica.
- Todas se ejecutan en **modo núcleo**.
- Las capas se comunican entre sí solo a través de **interfaces definidas** (puntos de entrada y salida controlados).
- Cada capa se construye **sobre la capa inferior** ya depurada.
- **Ventaja de mantenimiento**: si modifico una capa y el sistema falla, sé exactamente que el problema está en esa capa. No tengo que buscar en otro lado.
- También se la llama "débilmente acoplada" en contraposición a la monolítica.

### Microkernel

- A nivel núcleo se ejecutan **solo las operaciones básicas**: manejo de interrupciones y carga/descarga de procesos.
- El resto de las funciones del SO (módulo de E/S, gestión de memoria, etc.) se ejecutan en **modo usuario**, como si fueran aplicaciones comunes.
- Esas funciones externas se llaman **servidores**.
- **Ventaja**: núcleo muy pequeño, flexible, más fácil de portar.
- **Desventaja relativa**: al llamar a funciones que están fuera del núcleo, hay algo más de overhead que en la monolítica.

### Módulos cargables al núcleo (modular)

- Se usan **módulos** que se pueden cargar y descargar del núcleo dinámicamente.
- Más flexible que el sistema por capas: un módulo puede llamar a cualquier otro módulo (sin tener que seguir el orden de las capas).
- Se usan mucho en la actualidad.

### Híbridos

- Combinan 2 o más arquitecturas. Ej: parte monolítica + parte modular.
- Son los más comunes hoy en día. El criterio es que **la cosa funcione bien**, no hacer una obra de arte arquitectónica.

### Máquinas virtuales

- Permiten que el mismo hardware físico presente **múltiples instancias del sistema de cómputo**.
- Un usuario puede ver un SO y otro usuario puede ver un SO diferente, en el mismo equipo.
- Es como una "clonación" del sistema donde cada clon se comporta de manera independiente.

---

## Interrupciones

> [!note] Definición
> Una ==interrupción== es una **señal eléctrica** producida por hardware o software que indica que se ha producido un **evento**. El SO la interpreta (de acuerdo a la intensidad/tipo de señal) y ejecuta la rutina correspondiente.

Eventos que generan interrupciones: un proceso pide datos del disco, hay un error de hardware, se mueve el mouse, llega un paquete de red, se termina el quantum de tiempo, etc.

### Vector de interrupciones

- Todos los SO tienen un **vector de interrupciones**: una tabla donde cada posición contiene la **dirección en memoria** de la primera instrucción de la rutina que atiende ese tipo de interrupción.
- Cuando ocurre la interrupción N, el SO consulta la posición N del vector y salta a ejecutar esa rutina.

### Operaciones atómicas

- Las rutinas que ejecuta el SO para atender interrupciones son **operaciones atómicas**: una vez que empiezan, **no se interrumpen** hasta que terminan.
- Los procesos de usuario sí pueden interrumpirse (ej: cuando se les acaba el quantum). Las rutinas del SO, no.

### Enmascarables vs. no enmascarables

| Tipo | Descripción |
|------|-------------|
| ==Enmascarable== | El SO **puede esperar** para atenderla. La atiende en el próximo overhead (cuando ya controla la CPU). Ej: llega una comunicación no urgente mientras se ejecuta un proceso. |
| ==No enmascarable== | El SO **está obligado a atenderla de inmediato**. Ej: el proceso en ejecución necesita datos del disco y no puede continuar sin ellos. Si el SO no la atiende, el proceso queda colgado en la CPU y todo el sistema se detiene. |

> [!example] Ejemplo del profe
> Otro caso de interrupción no enmascarable: si el sistema usa prioridades y llega un proceso con mayor prioridad que el que está ejecutando, el SO puede estar obligado a sacar al que está corriendo y poner al de mayor prioridad. Esto depende del algoritmo de planificación utilizado (tema de la unidad siguiente).

---

## Un criterio transversal: simplicidad de los algoritmos

El profe lo menciona varias veces como una idea central para entender por qué los SO funcionan como funcionan:

> [!important] El profe recalca
> En un SO las operaciones se repiten **millones de veces por minuto**. Por eso, el esfuerzo de tomar una decisión tiene que ser el **mínimo posible**: no conviene un algoritmo que dé la decisión "óptima" si ese cálculo cuesta mucho tiempo, porque ese costo se multiplica por millones. En SO se valora que el algoritmo sea **simple y rápido**, aunque no sea el mejor teóricamente. La performance global del sistema importa más que la perfección de cada decisión individual.

---

## Consideración sobre los sistemas operativos específicos

> [!important] El profe recalca
> **En esta materia no se estudia ningún SO en particular.** No importa Windows, Linux, Mac ni ningún otro. Se estudia el funcionamiento conceptual que es común a todos. Cada SO elige cómo implementar cada función; lo importante es entender las opciones disponibles, sus ventajas y desventajas, para poder elegir con criterio.

---

## Resumen de la evolución

| Etapa | Nombre | Característica clave | Objetivo del SO |
|-------|--------|---------------------|-----------------|
| 1 | Procesamiento en serie | Sin SO. Operador manual. | — |
| 2 | Por lotes monoprogramado | Primer SO (Monitor). Un proceso a la vez. | Automatizar la operación |
| 3 | Por lotes multiprogramado | Varios procesos en RAM. Alternancia. | Maximizar uso de CPU |
| 4 | Tiempo compartido / multiusuario | Quantum de tiempo. Programas interactivos. | Equidad entre usuarios |
| 5 | Multiprocesamiento | Varias CPUs. Ejecución simultánea real. | Más throughput |
| 6 | Tiempo real | Respuesta garantizada. Tolerancia a fallos. | Cumplir restricciones de tiempo |

---

## Enlaces

- [[U2 - Administración de Procesos]]
