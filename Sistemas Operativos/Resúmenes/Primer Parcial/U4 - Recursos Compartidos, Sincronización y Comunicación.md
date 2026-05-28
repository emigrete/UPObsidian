---
title: "U4 — Administración de Recursos Compartidos, Sincronización y Comunicación entre Procesos"
aliases:
  - "Unidad 4"
  - "Sincronización"
tags:
  - sistemas-operativos
  - primer-parcial
  - unidad-4
parcial: "Primer Parcial"
unidad: 4
clases: "Clases 4-5"
fuente: "Transcripciones Prof. Arzubi"
---

# U4 — Administración de Recursos Compartidos, Sincronización y Comunicación entre Procesos

> [!info] De qué trata
> Esta unidad estudia los problemas que surgen cuando múltiples procesos acceden a datos compartidos: la condición de carrera, la sección crítica y cómo protegerla. El profe explica que los dispositivos los sincroniza el sistema operativo, pero los **datos** son responsabilidad del programador. La herramienta práctica central son los **semáforos binarios** con sus operaciones `wait` y `signal`, y se ven ejercicios concretos con los operadores OR y AND. Cierra con comunicación entre procesos (paso de mensajes directa e indirecta).

---

## 1. Concurrencia

> [!note] Definición
> ==Concurrencia== es la propiedad del sistema que permite que múltiples procesos se ejecuten al mismo tiempo o de manera que interactúen entre sí.

El profe distingue dos tipos:

- **Concurrencia real**: ocurre en sistemas multiprocesador. Hay varios procesadores y los procesos se ejecutan verdaderamente en paralelo, uno por procesador.
- **Concurrencia simulada**: otro nombre para la multiprogramación con un solo procesador. Los procesos se van alternando en el uso de la CPU tan rápido que el usuario tiene la sensación de ejecución simultánea, pero en realidad se ejecutan de forma alternada.

> [!important] El profe recalca
> En la práctica, los sistemas reales combinan las dos: varios procesadores, y sobre cada procesador se realiza multiprogramación. Es importante tener en claro cada concepto por separado aunque se den juntos.

---

## 2. Clasificación de procesos según su interacción

Los procesos que se ejecutan en un sistema pueden o no requerir colaborar entre sí:

- **Procesos independientes**: se ejecutan sin interactuar con otros procesos ni necesitar de ellos. Para el sistema operativo, esta es la situación más cómoda de administrar: lo ejecuta cuando quiere y no tiene que considerar nada más.
- **Procesos cooperativos**: diseñados conjuntamente para realizar una determinada actividad. Se subdividen en:
  - **Concurrentes**: pueden ejecutarse en paralelo (ej.: en 2 procesadores distintos).
  - **Complementarios**: requieren un orden determinado de ejecución. Por ejemplo, el proceso B no puede ejecutarse sin que antes se haya ejecutado el proceso A.

La sincronización de los procesos cooperativos es responsabilidad del sistema operativo.

---

## 3. El problema: acceso concurrente a datos compartidos

El sistema operativo sincroniza el acceso a los dispositivos (impresora, disco, etc.) de manera transparente al usuario. Sin embargo, cuando el recurso compartido son **datos** (desde una variable hasta un archivo completo), la responsabilidad de sincronizar el acceso es **del programador**.

> [!important] El profe recalca
> No porque el sistema operativo maneje la impresora quiere decir que no haya que sincronizar los datos. Los datos los tiene que sincronizar el programador. Si ustedes no se lo dicen al sistema operativo, él no sabe que tiene que hacer algo.

### 3.1 Condición de carrera

> [!note] Definición
> Una ==condición de carrera== ocurre cuando múltiples procesos leen y escriben datos compartidos de forma que el resultado final depende del orden de ejecución de las instrucciones. El resultado es impredecible y a veces incorrecto.

> [!example] Ejemplo del profe
> Proceso Productor: toma la variable `contador`, la pone en el Registro 1, le suma 1 y lo guarda de vuelta en `contador`.
> Proceso Consumidor: toma `contador`, lo pone en el Registro 2, le resta 1 y lo guarda.
>
> **Ejecución correcta** (uno después del otro): `contador = 5` → Productor lo lleva a 6 → Consumidor lo lleva a 5. Todo bien.
>
> **Ejecución con interrupción a mitad**: el Productor ejecuta las 2 primeras instrucciones (pone 5 en Reg1, Reg1 = 6) pero es interrumpido antes de guardar. El sistema ejecuta el Consumidor completo: toma `contador` = 5 (porque el Productor no guardó), le resta 1, deja `contador` = 4. Luego el Productor retoma y guarda el Registro 1 (que tenía 6) en `contador`. Resultado final: `contador` = 4 en lugar de 5.
>
> El profe también lo ilustra con la cuenta bancaria: si yo y mi señora accedemos a la misma cuenta con 10.000 pesos y los dos sacamos los 10.000 al mismo tiempo sin sincronización, al banco le van a faltar 10.000 pesos.

---

## 4. Exclusión mutua

> [!note] Definición
> ==Exclusión mutua==: si un proceso tiene tomado un recurso (el "lápiz"), ningún otro proceso puede acceder a ese mismo recurso al mismo tiempo.

La solución más directa al problema de la condición de carrera es aplicar exclusión mutua sobre todos los datos durante todo el tiempo que el proceso los use. Pero esto es demasiado costoso e ineficiente, porque bloquea el acceso por mucho tiempo.

Conceptos relacionados:

- **Operación atómica**: operación que no puede ser interrumpida (ya vista en unidades anteriores).
- **Espera activa**: cuando un proceso que espera un recurso no abandona la CPU y la usa para chequear periódicamente si el recurso está disponible. Es un comportamiento **no deseado** porque la CPU no ejecuta código útil ni del proceso que espera ni de otro.

---

## 5. Sección crítica

> [!note] Definición
> La ==sección crítica== es la parte del código de un proceso que accede al dato compartido.

En lugar de bloquear el acceso a los datos durante toda la ejecución del proceso, se define la sección crítica como la porción específica del código que accede al dato compartido. Solo se sincroniza esa sección, no todo el proceso.

> [!warning] Ojo / suele tomar
> La definición correcta es: "parte del código que accede a un **dato** compartido". NO es "parte del código que accede a un recurso compartido", porque un recurso puede ser también la impresora, y esa la maneja el SO. Si se dice "recurso" sin especificar, la definición está incompleta.

### 5.1 Requisitos para proteger la sección crítica

> [!important] El profe recalca
> Estos 3 requisitos los toma siempre. Son necesarios y suficientes: si se cumplen los 3, la sección crítica está protegida.

1. **Exclusión mutua**: si un proceso está ejecutando su sección crítica, ningún otro proceso puede estar ejecutando su sección crítica (con respecto al mismo dato compartido).

2. **Progreso**: cuando el sistema operativo tiene que determinar qué proceso va a ejecutar la sección crítica, elige **solamente** entre aquellos que están **justo por empezar** la sección crítica (llegaron a la primera instrucción de la sección). No entre los que están "cerca" o "por llegar" — eso sería hacer una reserva anticipada, lo cual no corresponde.

3. **Espera limitada**: si un proceso está esperando entrar a ejecutar su sección crítica, después de un tiempo determinado el sistema le va a dar prioridad para que la ejecute. Evita la inanición.

---

## 6. Mecanismos para proteger la sección crítica

El profe menciona tres formas. Los algoritmos históricos (Dekker, Peterson, Lamport) ya no se usan; se pueden leer como ilustración en la bibliografía.

### 6.1 Semáforos

Los ==semáforos== son variables de software que el programador implementa manualmente para sincronizar los procesos y proteger la sección crítica.

**Tipos:**

| Tipo | Valores | Características |
|---|---|---|
| Binario | 0 (cerrado) o 1 (abierto) | Más simple; tiene espera activa |
| Contador | Enteros positivos/negativos | Más completo; tiene 2 partes: el semáforo y el contador |

**En los ejercicios de la materia se usan semáforos binarios.**

**Instrucciones:**

- `wait(s)` (también llamado P o W): el proceso espera hasta que el semáforo `s` esté abierto (= 1). Al pasar, cierra el semáforo (pone `s = 0`).
- `signal(s)` (también llamado V): el proceso abre el semáforo `s` (pone `s = 1`), habilitando al siguiente proceso.
- `init`: define el estado inicial de cada semáforo (1 = abierto, 0 = cerrado).

> [!important] El profe recalca
> Cada proceso se define una sola vez. Los semáforos se ponen una sola vez. No se repiten los bloques de proceso en el ejercicio.

### 6.2 Regiones críticas

Son facilidades que ofrecen algunos lenguajes de programación más evolucionados. El lenguaje permite definir una "región" y asociarle una variable booleana. El compilador y el sistema operativo se encargan del acceso controlado. No requiere que el programador implemente la sincronización manualmente.

### 6.3 Monitores

Son lo más avanzado. Son constructores de programación concurrente que funcionan como una "caja negra": el programador solo define qué datos quiere preservar como monitor, y el compilador junto con el sistema operativo se encargan automáticamente de evitar los conflictos en la sección crítica, sin que el programador implemente nada a mano.

> [!note] Definición
> Los ==monitores== son constructores de programación concurrente. Con solo declarar los datos que se desea proteger como monitor, el sistema evita automáticamente los conflictos en la sección crítica.

---

## 7. Ejercicios de semáforos

### 7.1 Metodología general

El profe propone seguir siempre este método:

1. **Hacer el grafo**: dibujar el orden de ejecución deseado entre procesos. Usar línea continua para las relaciones en secuencia (AND), línea punteada para el OR (invento del profe para diferenciarlos visualmente).
2. **Asignar semáforos**: partiendo del grafo, determinar qué semáforos hacen falta.
3. **Escribir los procesos con `wait` y `signal`**.
4. **Definir el `init`**: estado inicial de cada semáforo.
5. **Verificar**: armar una tabla con los tiempos (T0, T1, T2…) y el valor de cada semáforo en cada momento. Si al terminar el ciclo los semáforos quedan en el mismo estado que el `init`, el planteo es consistente. Si quedan distintos, algo está mal.

> [!warning] Ojo / suele tomar
> Si el resultado de la tabla al cerrar el loop da distinto al init, el ejercicio está mal. Si da igual, es señal positiva pero no garantía absoluta en casos simples.

---

### 7.2 Ejercicio 1 — Secuencia simple (A → B → C → loop)

**Enunciado**: ejecutar los procesos en el orden A, B, C, y volver a empezar.

**Planteo:**

```
Proceso A          Proceso B          Proceso C
wait(a)            wait(b)            wait(c)
[código A]         [código B]         [código C]
signal(b)          signal(c)          signal(a)
```

**Init**: `a = 1`, `b = 0`, `c = 0`

**Verificación (tabla de tiempos):**

| Tiempo | Acción                  | a | b | c |
|--------|-------------------------|---|---|---|
| T0     | Init                    | 1 | 0 | 0 |
| T1     | Ejecuta A → cierra a   | 0 | 0 | 0 |
| T2     | A hace signal(b)        | 0 | 1 | 0 |
| T3     | Ejecuta B → cierra b   | 0 | 0 | 0 |
| T4     | B hace signal(c)        | 0 | 0 | 1 |
| T5     | Ejecuta C → cierra c   | 0 | 0 | 0 |
| T6     | C hace signal(a)        | 1 | 0 | 0 |

Al cerrar el loop, los semáforos quedan igual que el init → correcto.

---

### 7.3 Ejercicio 2 — Con operador OR (A → B|C → D → B|C → loop)

**Enunciado**: ejecutar A; luego B o C (uno solo, cualquiera de los dos); luego D; luego de nuevo B o C; y volver.

> [!note] Definición
> **OR en semáforos**: se quiere ejecutar exactamente **uno** de los procesos involucrados (no los dos). Se usa **un único semáforo** para todos los procesos del OR, porque con que uno de ellos sea habilitado alcanza.

**Planteo:**

```
Proceso A          Proceso B          Proceso C          Proceso D
wait(a)            wait(bc)           wait(bc)           wait(d)
[código A]         [código B]         [código C]         [código D]
signal(bc)         signal(d)          signal(d)          signal(bc)
signal(d)*                                               signal(a)*
```
*El profe agrega semáforos auxiliares para resolver el problema de que D necesita abrirle tanto el `bc` como el `a`. Ver nota abajo.

**Problema que surge**: después de que D ejecuta, necesita habilitar tanto al B/C del segundo OR como al A del siguiente ciclo. Pero si D hace `signal(bc)`, corre el riesgo de que el semáforo `a` esté abierto y en vez de ejecutar B o C se ejecute A antes de tiempo.

**Solución con semáforo auxiliar X:**

```
Proceso A               Proceso B            Proceso C            Proceso D
wait(a)                 wait(bc)             wait(bc)             wait(d)
wait(x)                 [código B]           [código C]           [código D]
[código A]              signal(d)            signal(d)            signal(bc)
signal(bc)                                                        signal(a)
```

`Init`: `a = 1`, `x = 1`, `bc = 0`, `d = 0`

El A tiene `wait(a)` y `wait(x)` — operación AND (necesita los dos semáforos abiertos). El E y el F (en el ejercicio de clase5) abren el `a` y el `x` respectivamente, de modo que A no puede ejecutarse hasta que los dos estén abiertos.

> [!important] El profe recalca
> Con el OR se usa **un solo semáforo** para todos los procesos del grupo. Con el AND se necesita **un semáforo distinto para cada uno** de los procesos del grupo, porque tiene que ejecutarse todos y cada uno debe habilitarse por separado.

---

### 7.4 Ejercicio 3 — Con operador AND (clase 5)

**Enunciado**: ejecutar A; luego B o C (OR, uno solo); luego D; luego E y F (AND, los dos); y volver a A.

**Grafo**:
- A → (B | C) OR → D → (E & F) AND → A (loop)

**Planteo:**

```
Proceso A          Proceso B        Proceso C        Proceso D        Proceso E        Proceso F
wait(a)            wait(bc)         wait(bc)         wait(d)          wait(e)          wait(f)
wait(x)            [código B]       [código C]       [código D]       [código E]       [código F]
[código A]         signal(d)        signal(d)        signal(e)        signal(a)        signal(x)
signal(bc)                                           signal(f)
```

`Init`: `a = 1`, `x = 1`, `bc = 0`, `d = 0`, `e = 0`, `f = 0`

**Lógica del AND**: para que A vuelva a ejecutarse, necesita que tanto `a` como `x` estén abiertos. El proceso E abre el semáforo `a` y el proceso F abre el semáforo `x`. Mientras no terminen los dos, A no puede ejecutarse. Así se garantiza que E y F se ejecuten ambos antes de que comience el próximo ciclo.

**Verificación resumida (tabla de tiempos):**

| Tiempo | Proceso ejecutado | Acción                     | a | x | bc | d | e | f |
|--------|-------------------|----------------------------|---|---|----|---|---|---|
| T1     | A                 | wait(a), wait(x) → cierra ambos; signal(bc) | 0 | 0 | 1 | 0 | 0 | 0 |
| T2     | B o C             | wait(bc) → cierra; signal(d) | 0 | 0 | 0 | 1 | 0 | 0 |
| T3     | D                 | wait(d) → cierra; signal(e), signal(f) | 0 | 0 | 0 | 0 | 1 | 1 |
| T4     | E                 | wait(e) → cierra; signal(a) | 1 | 0 | 0 | 0 | 0 | 1 |
| T5     | F                 | wait(f) → cierra; signal(x) | 1 | 1 | 0 | 0 | 0 | 0 |
| —      | Loop              | Estado = init → correcto    | 1 | 1 | 0 | 0 | 0 | 0 |

> [!warning] Ojo / suele tomar
> Con el AND, si E abre `a` y `x` sigue cerrado, el proceso A todavía no puede ejecutarse porque le falta el `x`. Solo cuando F abra el `x` se cumplen las dos condiciones y A puede arrancar de nuevo.

---

## 8. Comunicación entre procesos (IPC)

### 8.1 Paso de mensajes

La comunicación entre procesos puede realizarse mediante el modelo de **paso de mensajes**, que usa dos operaciones:
- `send(destino, mensaje)`: enviar un mensaje.
- `receive(origen, mensaje)`: recibir un mensaje.

### 8.2 Comunicación directa

> [!note] Definición
> En la ==comunicación directa==, el proceso A le envía un mensaje directamente al proceso B sin intermediario, y B le contesta. No hay ningún paso intermedio.

Puede ser:
- **Sincrónica**: el proceso A envía el mensaje y **se queda esperando sin ejecutar** hasta que recibe el `receive` (el acuse de recibo) del proceso B. No sigue ejecutando hasta entonces.
- **Asíncrona**: el proceso A envía el mensaje y **sigue ejecutando** sus líneas de código sin esperar. En algún momento posterior llega el `receive`.

> [!warning] Ojo / suele tomar
> La comunicación asíncrona parece más eficiente porque A no pierde tiempo esperando. Pero tiene un riesgo: si B necesita reenviar el mensaje porque no llegó bien (control de errores), A ya puede haber avanzado. Si A puede reconstruir el dato o lo tiene guardado, no hay problema. Si no puede recuperarlo, tiene un problema. El profe recalca: hay que saber que esto puede ocurrir y tomar los recaudos necesarios.

### 8.3 Comunicación indirecta (buzones)

> [!note] Definición
> En la ==comunicación indirecta==, los procesos se comunican a través de un **buzón** (también llamado puerto o buffer): A guarda el mensaje en el buzón y B lo saca de ahí.

Al definir un buzón hay que tener en cuenta:
- **Tamaño**: no puede ser tan chico que no quepa lo que se necesita guardar, ni tan grande que desperdicie memoria.
- **Quién vacía el buzón**: ¿cuando lo leyó el primero? ¿cuando lo leyeron todos los procesos que debían?
- **Quién puede leer o escribir**: permisos.
- **Qué pasa si está lleno**: no se puede guardar.
- **Qué pasa si está vacío**: no se puede sacar nada.

> [!example] Ejemplo del profe
> Cuando se manda a imprimir, las impresiones van a un buzón (buffer) y de ahí a la impresora que es mucho más lenta. No se puede guardar en el buzón si está lleno, y la impresora no puede imprimir si el buzón está vacío.

---

## 9. Resumen de mecanismos

| Mecanismo | Quién lo implementa | Nivel |
|---|---|---|
| Semáforos | El programador manualmente | Bajo nivel |
| Regiones críticas | El lenguaje / compilador | Nivel medio |
| Monitores | El lenguaje + compilador + SO (caja negra) | Alto nivel |

---

## Enlaces

- [[U3 - Planificación de Procesos]]
- [[U5 - Abrazo Mortal (Deadlock)]]
