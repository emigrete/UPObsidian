---
title: "U5 — Abrazo Mortal (Deadlock)"
aliases:
  - "Unidad 5"
  - "Deadlock"
  - "Interbloqueo"
tags:
  - sistemas-operativos
  - primer-parcial
  - unidad-5
parcial: "Primer Parcial"
unidad: 5
clases: "Clases 5-6"
fuente: "Transcripciones Prof. Arzubi"
---

# U5 — Abrazo Mortal (Deadlock)

> [!info] De qué trata
> El abrazo mortal (deadlock) es una situación en la que un conjunto de procesos queda bloqueado permanentemente porque cada uno retiene recursos que los demás necesitan y espera recursos que los demás retienen. Nadie larga lo que tiene y nadie puede avanzar. Esta unidad explica las condiciones para que ocurra, cómo representar el estado del sistema, y las estrategias disponibles para tratarlo.

---

## Qué es el abrazo mortal

> [!note] Definición
> El ==abrazo mortal== (también llamado *deadlock* o interbloqueo) es el bloqueo permanente de un conjunto de procesos donde cada proceso retiene al menos un recurso y espera obtener otro recurso que está retenido por otro proceso del mismo conjunto. Como ninguno libera lo que tiene, ninguno puede ejecutarse.

El profe lo relaciona con la unidad anterior: en U4 hablábamos de sincronizar **procesos**; acá pasamos a sincronizar la **asignación de recursos** para evitar este tipo de conflicto que bloquea todo el equipo.

> [!example] Ejemplo del profe — las dos manos
> El profe usa sus manos: la mano derecha es un proceso que tiene el lápiz y espera la goma; la mano izquierda tiene la goma y espera el lápiz. Como no hay más lápices ni gomas, ninguna mano puede avanzar. Eso es un abrazo mortal.

> [!example] Ejemplo del profe — los filósofos
> Una mesa redonda, cada filósofo tiene un plato de fideos y entre cada plato hay un solo tenedor. La condición para comer es tener **dos** tenedores. Cada filósofo agarra el tenedor de su lado y espera el del costado, que el vecino ya agarró. Como nadie suelta el que tiene, todos mueren de hambre. Es un ejemplo clásico que aparece en cualquier libro de SO.

---

## Terminología: tipos y clases de recursos

El profe aclara que en la bibliografía se usan dos términos que hay que conocer:

- **Tipo de recurso**: la categoría del recurso (ej: impresora láser blanco y negro).
- **Clases**: la cantidad de instancias de ese tipo que existen en el sistema (ej: tengo 3 impresoras láser blanco y negro → 3 clases).

---

## Representación del sistema de cómputo

Para administrar el abrazo mortal, el sistema operativo necesita saber en cada momento qué procesos hay, qué recursos hay, cuáles están asignados y cuáles faltan. Hay dos formas de modelizarlo.

### Expresiones de configuración y estado

Se usan dos expresiones complementarias:

1. **Configuración del sistema**: lista entre llaves todos los procesos `{P1, P2, …}` y todos los recursos con su cantidad `{R1: 2, R2: 3, …}`. Es una "foto" del sistema en un momento dado.
2. **Estado de asignación de recursos**: a la izquierda, qué recursos están asignados a qué procesos; a la derecha, qué recursos le faltan a cada proceso para poder ejecutarse.

Con estas dos expresiones juntas se tiene información completa del sistema.

### Grafo de Asignación de Recursos (GAR)

> [!note] Definición
> El ==Grafo de Asignación de Recursos== (GAR) es una representación gráfica del estado del sistema. Es muy gráfica e intuitiva.

Convenciones del grafo:

| Elemento | Representación |
|---|---|
| Proceso | Círculo |
| Recurso | Cuadrado |
| Clase (instancia) del recurso | Punto negro dentro del cuadrado |
| Flecha **proceso → recurso** | El proceso **requiere** una clase de ese recurso |
| Flecha **recurso → proceso** | El recurso tiene **asignada** una clase a ese proceso |

Con estos elementos se puede dibujar el estado completo del sistema: todos los círculos (procesos), todos los cuadrados (recursos), los puntos dentro de cada cuadrado (cuántas clases tiene), y las flechas (qué está asignado y qué está requerido).

> [!important] El profe recalca
> La **espera circular** se detecta visualmente en el GAR: si se siguen las flechas y forman un ciclo (todas apuntan en el mismo sentido cerrando un circuito), hay espera circular. El grafo hace esto muy explícito.

#### Espera circular transitoria

El profe aclara que una espera circular en el grafo **no garantiza** que haya abrazo mortal definitivo. Puede darse una espera circular **transitoria**: si algún proceso del ciclo tiene asignado otro recurso que no forma parte del ciclo y eventualmente lo libera, ese recurso puede ser asignado a otro proceso del ciclo, que se ejecuta, libera lo suyo, y así se deshace el bloqueo solo.

### Modelo matricial

> [!note] Definición
> El ==modelo matricial== representa el sistema con **dos matrices y un vector**. Con esa información mínima se tiene todo lo necesario.

**Matriz de máximo requerido**: filas = procesos, columnas = recursos. Cada celda indica cuántas unidades de ese recurso necesita ese proceso para ejecutarse (el máximo que puede llegar a pedir).

**Matriz de asignado** (o de recursos asignados): igual estructura, pero indica cuántas unidades de cada recurso ya fueron asignadas a cada proceso.

**Vector de existencias o de disponibles** (uno de los dos es suficiente):
- *Vector de existencias totales*: para cada recurso, el total que existe en el sistema.
- *Vector de disponibles*: para cada recurso, cuántas unidades están libres en este momento.

> [!important] El profe recalca
> Con las **dos matrices + uno de los dos vectores** ya es suficiente para tener la representación completa. El otro vector se puede calcular: existencias totales − total asignado = disponibles (y viceversa).

**Matriz de necesidad** (auxiliar, recomendada por el profe para resolver ejercicios):

$$\text{Necesidad} = \text{Máximo requerido} - \text{Asignado}$$

Indica cuánto le falta a cada proceso para poder ejecutarse. No es obligatoria, pero el profe siempre la recomienda porque reduce errores al hacer el ejercicio.

---

## Las 4 condiciones para que ocurra el abrazo mortal

> [!important] El profe recalca
> Para que se produzca un abrazo mortal **deben cumplirse las 4 condiciones simultáneamente**. Basta con eliminar una para que no haya abrazo mortal.

### 1. Exclusión mutua

Si un proceso tiene asignado un recurso, **ningún otro proceso puede usar ese recurso** mientras el primero lo tenga. El recurso es de uso exclusivo.

### 2. Espera y retención

Un proceso que ya tiene asignado un recurso **no lo suelta** mientras espera que le asignen los recursos que le faltan. Retiene lo que tiene y espera lo que le falta.

### 3. Sin desalojo

El sistema operativo **no puede quitarle un recurso a un proceso** que lo tiene asignado. El proceso lo mantiene hasta que lo libera voluntariamente al terminar o al no necesitarlo más.

> [!example] Ejemplo del profe
> "Vos estás escribiendo el examen con la birome, yo te la saco y te faltó terminar. ¿Qué me decís?" — No se puede sacar un recurso a la fuerza así.

### 4. Espera circular

Existe una cadena cerrada de dos o más procesos donde cada uno espera un recurso que está retenido por el siguiente de la cadena. En el GAR se visualiza como un ciclo: las flechas dan siempre en el mismo sentido cerrando un circuito.

> [!example] Ejemplo del profe
> Con dos procesos y dos recursos: el proceso A tiene R1 asignado y necesita R2; el proceso B tiene R2 asignado y necesita R1. Eso es la espera circular mínima (la misma situación de las dos manos con el lápiz y la goma).

---

## Estrategias para tratar el abrazo mortal

La idea general es **eliminar alguna de las 4 condiciones**. El profe recorre cada una y explica qué tan viable es.

### Eliminar la exclusión mutua

Si se permite que varios procesos usen el mismo recurso a la vez (recurso compartido), no hay exclusión mutua y no puede haber deadlock por esa condición. Pero el problema es que si todos usan el mismo recurso indiscriminadamente, se generan todos los conflictos de sección crítica que ya vimos en U4. En la práctica equivale a hacer monoprogramación o genera conflictos peores. Conceptualmente es una opción, pero muy difícil de aplicar.

### Eliminar la espera y retención

No asignar recursos de a uno: solo asignar **todos los recursos que el proceso necesita cuando todos están disponibles al mismo tiempo**. Si no están todos disponibles, el proceso espera sin tener nada asignado.

- **Ventaja**: simple de administrar, no hay riesgo de abrazo mortal por esta condición.
- **Desventaja**: durante todo el tiempo que el proceso se ejecuta, retiene todos sus recursos aunque no los use todo el tiempo. Eso hace un **menor aprovechamiento de los recursos**, porque otros procesos no pueden usarlos mientras tanto.

> [!important] El profe recalca
> Es una solución "potable" y más viable que eliminar la exclusión mutua. El costo es el aprovechamiento subóptimo de los recursos, no un problema de corrección.

### Eliminar el sin desalojo (permitir desalojo de recursos)

Permitir que el sistema operativo le quite recursos a un proceso. Como se vio arriba, en la práctica es catastrófico: si el proceso está en medio de una operación y le sacás el recurso, la tarea queda a medias. No es una opción real.

### Actuar sobre la espera circular — Método de Estado Seguro

> [!important] El profe recalca
> Esta es la opción **más utilizada** en la práctica. Las tres primeras condiciones son difíciles de eliminar sin consecuencias graves; por eso se actúa sobre la espera circular.

> [!note] Definición
> El ==método de Estado Seguro== (también llamado método del orden seguro) consiste en encontrar un **orden de ejecución de los procesos tal que ninguno quede bloqueado**. El sistema simula la ejecución en ese orden y verifica que siempre haya recursos disponibles para el siguiente proceso. Si encuentra ese orden, el sistema está en **estado seguro**.

**Cómo funciona (algoritmo):**

1. Calcular la matriz de **necesidad** = máximo requerido − asignado.
2. Con el vector de **disponibles**, intentar ejecutar el primer proceso de la lista:
   - Si tiene suficientes recursos disponibles → se "ejecuta" (simulación): devuelve sus recursos asignados al pool de disponibles.
   - Si no tiene suficientes → se saltea, se sigue con el siguiente.
3. Al llegar al final de la lista, volver al principio (algoritmo cíclico) y repetir hasta ejecutar todos.
4. Si se ejecutan todos → el orden es seguro. Si hay alguno que nunca puede ejecutarse porque el sistema nunca tendrá los recursos que necesita, hay abrazo mortal.

> [!example] Ejemplo del profe (clase 5 — ejercicio mínimo)
> 3 procesos, 1 tipo de recurso, total = 12 unidades.
> - P0: máximo 10, asignado 5 → necesita 5
> - P1: máximo 4, asignado 2 → necesita 2
> - P2: máximo 9, asignado 2 → necesita 7
> - Disponibles: 3
>
> En el orden P0→P1→P2: P0 necesita 5, solo hay 3 → **no es orden seguro**.
>
> Orden seguro encontrado: **P1 → P0 → P2**
> - Ejecuto P1 (necesita 2, tengo 3): devuelve 2 → ahora tengo 5.
> - Ejecuto P0 (necesita 5, tengo 5): devuelve 5 → ahora tengo 10.
> - Ejecuto P2 (necesita 7, tengo 10): se ejecuta sin problema.

> [!example] Ejemplo del profe (clase 6 — ejercicio completo con 4 recursos)
> 6 procesos (P1 a P6), 4 tipos de recursos (A, B, C, D). Se tienen la matriz de máximo requerido, la de asignado y el vector de disponibles.
>
> **Parte a — ¿está en orden seguro el orden P1, P2, P3, P4, P5, P6?**
> - Calcular matriz de necesidad = máximo requerido − asignado.
> - P1 necesita 1 de B y 1 de C; disponibles 3B, 1C → se puede ejecutar. Devuelve su asignado → disponibles aumentan.
> - P2 necesita 3A y algo de C; no hay suficiente A ni C → **no se puede ejecutar → el orden NO es seguro**.
>
> **Parte b — encontrar el orden seguro:**
> Recorriendo cíclicamente:
> - P1 → se puede → ejecutar, devuelve recursos.
> - P2 → no puede.
> - P3 → no puede.
> - P4 → se puede → ejecutar, devuelve recursos.
> - P5 → se puede → ejecutar, devuelve recursos.
> - P6 → se puede → ejecutar, devuelve recursos.
> - Volver arriba: P3 → ahora se puede → ejecutar.
> - P2 → ahora se puede → ejecutar.
>
> **Orden seguro: P1 → P4 → P5 → P6 → P3 → P2**
>
> Verificación al final: disponibles + asignado total debe dar las existencias totales del sistema.

> [!warning] Ojo / suele tomar
> El algoritmo **siempre va hacia abajo** en la lista. Cuando llega al final, vuelve al principio (es cíclico). No se ejecuta el proceso siguiente hasta haber intentado todos los que están por debajo primero. Esa es la dinámica que hay que reproducir en el parcial.

### Bloqueo y recuperación

> [!note] Definición
> El ==método de bloqueo y recuperación== consiste en detectar que un proceso no puede ejecutarse porque no tiene los recursos necesarios, **sacarlo del sistema** y hacerlo reingresar.

- Es una solución más drástica (el proceso aborta y vuelve a empezar).
- Es "bastante económica" de implementar comparada con el estado seguro.
- No es lo ideal, pero resuelve el problema.

> [!example] Ejemplo del profe
> En el ejemplo de 3 procesos con 12 recursos, disponibles = 3, P0 necesita 5 → lo saco del sistema, recupero sus 5 asignados → ahora tengo 8 disponibles → ejecuto P1 y P2 → P0 reingresa al sistema.

### Ignorar el abrazo mortal (política del avestruz)

> [!note] Definición
> La opción de ==ignorar el abrazo mortal== consiste en no implementar ningún mecanismo de prevención o detección, y aceptar que eventualmente puede ocurrir un bloqueo.

- La implementación de los métodos anteriores es **muy costosa** en procesamiento.
- En equipos de menor porte (computadoras personales, celulares) **no se justifica** ese costo.
- Los sistemas operativos de equipos grandes (servidores, mainframes) sí implementan estos métodos.
- Los sistemas operativos de equipos personales tienen **algunos algoritmos para tratar de evitar** el abrazo mortal, pero no lo garantizan.

> [!important] El profe recalca
> Algunas de las veces que una computadora personal se traba y hay que hacer Ctrl+Alt+Supr o reiniciarla, la causa es justamente un abrazo mortal. La decisión de dejarlo pasar hace que el sistema sea **más ágil y rápido** en condiciones normales; el daño de un bloqueo ocasional es menor que el costo de un sistema operativo demasiado pesado que previene todos los bloqueos posibles.

---

## Resumen de estrategias

| Estrategia | Condición que elimina | Viabilidad práctica |
|---|---|---|
| Eliminar exclusión mutua | Exclusión mutua | Muy difícil; genera conflictos de sección crítica |
| Asignar todo junto | Espera y retención | Viable; menor aprovechamiento de recursos |
| Permitir desalojo | Sin desalojo | Catastrófico en la práctica |
| Método de estado seguro | Espera circular | El más usado en sistemas grandes; costoso |
| Bloqueo y recuperación | Detección post-hecho | Drástico pero económico |
| Ignorar (avestruz) | — | Equipos personales; el más común en la práctica cotidiana |

---

## Enlaces

- [[U4 - Recursos Compartidos, Sincronización y Comunicación]]
- [[U6 - Administración de Memoria Central]]
