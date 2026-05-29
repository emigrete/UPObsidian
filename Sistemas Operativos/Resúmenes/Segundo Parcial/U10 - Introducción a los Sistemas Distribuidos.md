---
title: "U10 — Introducción a los Sistemas Distribuidos"
aliases:
  - "Unidad 10"
  - "Sistemas Distribuidos"
tags:
  - sistemas-operativos
  - segundo-parcial
  - unidad-10
parcial: "Segundo Parcial"
unidad: 10
clases: "Clase 12"
fuente: "Transcripciones Prof. Arzubi"
---

# U10 — Introducción a los Sistemas Distribuidos

> [!info] De qué trata
> Esta unidad es **solo una introducción** (el profe lo repite varias veces): da una visión con un **nivel de abstracción elevado** de los sistemas distribuidos. Cubre qué es un sistema distribuido, el rol de la ISO y el modelo OSI con sus 7 capas, la taxonomía de Flynn, el concepto de middleware y el de cluster. El profe aclara que casi todos estos temas se ven en profundidad en **otras materias**, por eso acá se da solo un panorama general.

> [!important] ¿Entra en el parcial?
> **Sí.** Ante la consulta de un alumno, el profe responde: *"Todo lo que dije lo puedo preguntar"* y *"Lo que yo dije en clase hoy lo puedo preguntar"* (aclarando que **no es mucho** lo que dio). El segundo parcial entra **de la unidad 6 a la 10** (5 unidades). El modelo OSI puede preguntarse.

---

## Qué es un sistema distribuido

> [!note] Definición
> Un **sistema distribuido** es un sistema formado por **equipos completos ubicados geográficamente en distintos lugares** que trabajan en conjunto, porque tienen **objetivos comunes** y, como mínimo, **comparten una determinada cantidad de recursos**. No son equipos sueltos en el mundo: están **vinculados** de alguna manera, justamente para aprovechar recursos o tener un desarrollo común.

> [!example] Ejemplo del profe
> Una empresa con equipos en distintas provincias del país, o dentro de la misma ciudad en distintos barrios. Eso es un sistema distribuido.

**Comunicación:** se realiza a través de **llamadas a procesos remotos** (es el **paso de mensajes**).

---

## La ISO y los estándares

La **ISO (International Standard Organization)** es una organización internacional que **establece estándares y protocolos** para una gran cantidad de temas (desarrollar software, programar, comunicar, etc.).

> [!important] Los estándares NO son obligatorios
> El profe aclara este "desconcepto" frecuente: usar los estándares y protocolos internacionales **no es obligatorio para nadie**. Sirven para que quienes **quieran comunicarse** "hablen el mismo idioma". Una empresa puede usar sus propios protocolos (por ejemplo, para mantener reserva o secreto), sin ningún problema. El único inconveniente de no adscribirse a los estándares aceptados internacionalmente es que **no se va a poder interactuar con otros sistemas**.

---

## El modelo OSI (Open System Interconnection)

La ISO desarrolló el **OSI (Open System Interconnection)**, un conjunto de protocolos para la **interconexión de sistemas abiertos** (sistemas que están a disposición de cualquiera). Organiza la comunicación en **7 capas** (niveles).

> [!note] La idea conceptual de dividir en capas
> Sirve para **resolver un problema complejo**: se lo va **abriendo / desgranando** en partes hasta llegar a un nivel donde los problemas a resolver son simples y comprensibles. El profe lo compara con organizar un **congreso de informática**: planteado en bloque es inabordable; al separarlo (parte académica, logística, etc.) se llega a un montón de tareas simples.

> [!example] Ejemplo del profe: mandar una fotografía de A a B
> La fotografía completa es compleja de enviar. El sistema la va **dividiendo al bajar por las capas**, reduciendo la complejidad, hasta el **nivel físico**, donde lo único que se envía son **ceros y unos**. En el equipo B se hace el **camino inverso**: se sube por las capas, cada una aporta lo suyo, hasta reconstruir la misma fotografía.

### Las 7 capas

| # | Capa | De qué se encarga |
|---|---|---|
| 7 | **Aplicación** | Servicios al usuario: SMTP, FTP, TELNET (mails, mensajes, etc.) |
| 6 | **Presentación** | Codificación de los caracteres (ASCII o EBCDIC) |
| 5 | **Sesión** | Tipo de diálogo: **Full Duplex** o **Half Duplex** |
| 4 | **Transporte** | Tamaño de los paquetes, normalización/protocolización → **TCP** en Internet |
| 3 | **Red** | Identificación unívoca de cada usuario → **IP** en Internet |
| 2 | **Enlace** | Cómo se realiza el enlace entre los participantes (tipo de red) |
| 1 | **Física** | Intensidad de la señal; transmisión analógica o digital |

> [!note] Internet y las capas
> Para el caso de **Internet** (la que más conocen), se considera de la capa de red hacia arriba: **red** (IP), **transporte** (TCP), y agrupa **sesión + presentación + aplicación** todo bajo "**aplicación**".

### Capa física: analógica vs. digital

El nivel físico determina la **intensidad de la señal** a enviar (la comunicación puede ser por cable o no). La transmisión puede ser **analógica** o **digital**; ambas son válidas y se pueden usar.

- **Señal analógica:** onda continua con **infinitos puntos** a transmitir (el **teorema de Shannon** reduce ese infinito). Al recorrer, la señal **disminuye y recibe ruido** (interferencias exteriores). Existen **regeneradores** que levantan la forma de la señal, pero al reconstruir toman el punto medio de los que llegaron → cuando la señal llega al destino **puede no ser exactamente igual** a la original (será parecida, según distancia, ruido y calidad de los regeneradores).
- **Señal digital:** usa la **onda cuadrada**, con **2 estados** conocidos (0 y 1). Al regenerar, solo hay que determinar **si es un 1 o un 0** → la reconstrucción es **exactamente igual** a la original. Esa es la ventaja de la digital. Nuestro equipo de computación trabaja con **digital**.

> [!important] El profe recalca
> La ventaja de la transmisión **digital** es que se puede **reconstruir exactamente igual** en el destino, porque solo hay que decidir entre 1 y 0; la analógica, en cambio, acumula ruido y se deforma. Aun así, las dos son válidas y se utilizan.

### Capa de enlace: tipos de red

Determina **cómo va a ser el enlace** entre los participantes. El profe menciona **tres tipos**:

- **Conmutación de circuito:** ejemplo, la **telefonía fija**. A llama a B, la central deriva la llamada por donde haya **menos tráfico** y se **establece un circuito fijo** que se **mantiene durante toda la comunicación**. Al cortar y volver a llamar, el circuito puede ser otro.
- **Conmutación de paquetes:** lo que se manda se **divide en paquetes** (de menor tamaño). Cada paquete lleva en su cabecera **quién es el destinatario, quién lo manda**, etc., y los datos. Los paquetes **viajan por distintos caminos** según el tráfico, por lo que **llegan en cualquier orden** y al destino **hay que ordenarlos**.
- **ATM (Método de Transmisión Asincrónica):** **combina** los dos anteriores. Los paquetes ahora son **celdas**, mucho más chicas. **Establece un circuito** entre A y B y lo mantiene durante la comunicación; las celdas viajan por ahí y **llegan ordenadas** (no hay que ordenarlas). Al cortar y establecer otra comunicación, puede ir por otro camino.

### Resto de capas (panorama)

- **Capa de red:** identifica **unívocamente** a cada usuario de la red para que, en su recorrida hasta el destinatario, no se pierda. En Internet (que es **redes de redes**) es el **IP**, que identifica al usuario de forma única en el mundo. Siglas conocidas: `.com`, `.ar`, `.gov`, `.edu`.
- **Capa de transporte:** define de qué **tamaño son los paquetes** y lo **normaliza/protocoliza**. En Internet es el **TCP**.
- **Capa de sesión:** tiene que ver con si la comunicación es **Full Duplex** o **Half Duplex**.
- **Capa de presentación:** qué **codificación** se usa para representar los caracteres (la `a` minúscula, la `A` mayúscula, el signo `+`, etc.): **ASCII** (el más conocido, combinación de 8 bits) o **EBCDIC**.
- **Capa de aplicación:** donde están **SMTP, FTP, TELNET**, etc. (todo lo que son mails, mensajes y demás).

---

## Taxonomía de Flynn

Propuesta por **Michael Flynn**, establece las combinaciones posibles entre el **flujo de instrucciones** y el **flujo de datos** (cada uno puede ser único o múltiple → 4 combinaciones):

| | Un flujo de datos | Múltiples flujos de datos |
|---|---|---|
| **Una instrucción** | **SISD** | **SIMD** |
| **Múltiples instrucciones** | **MISD** | **MIMD** |

- **SISD (instrucción simple, dato simple):** una computadora con **un solo procesador** que ejecuta una instrucción sobre un dato. La más simple.
- **SIMD (instrucción simple, múltiples datos):** **procesadores vectoriales**. Se aplica **una misma instrucción a muchos datos**. Ejemplo del profe: a un vector de números, sumarle 1 a cada posición (la instrucción "sumar 1" es una sola, pero se aplica sobre muchos datos).
- **MISD (múltiples instrucciones, un dato):** **paralelismo redundante**. Se usa para aplicaciones **muy exigentes que no pueden fallar**.
- **MIMD (múltiples instrucciones, múltiples datos):** **multiprocesamiento**. Es la estructura de los sistemas distribuidos (y del multiprocesamiento en general).

> [!example] Paralelismo redundante (MISD) — ejemplo del profe
> Un sistema crítico en tiempo, como el de un **avión de combate** (donde las exigencias se miden en nanosegundos y "se va la vida del piloto"). Como el **software nunca se puede probar al 100%** (sería necesario cargar todos los datos posibles y recorrer todos los caminos, y los números son infinitos → conceptualmente imposible), se recurre a la redundancia: se le da el **mismo algoritmo a 3 programadores distintos**, cada uno lo programa y prueba por su lado. Después se ejecutan los **3 programas en 3 procesadores** y se comparan los resultados: si dos dan 5 y uno da 4, el sistema **elige el resultado de la mayoría** (los dos que coinciden). Así se "cura en salud" frente a errores del software que podrían generar una catástrofe.

> [!important] MIMD: fuertemente vs. débilmente acoplado
> El **MIMD** puede ser de dos tipos (visto ya en la Unidad 1):
> - **Fuertemente acoplado:** varios procesadores **dentro de un mismo equipo**, cada uno actuando sobre distintos datos. **NO es un sistema distribuido.**
> - **Débilmente acoplado:** los equipos están **distribuidos geográficamente** e interconectados → **este sí es un sistema distribuido**.

---

## Middleware

> [!note] Qué es
> El **middleware** es **software**: una **interfaz de programación y protocolos estándares** que se ubica **entre la plataforma y las aplicaciones**. (La **plataforma** = el **hardware** del equipo + el **sistema operativo**; sobre el SO corren las aplicaciones.)

**Por qué aparece:** cuando uno desarrolla una aplicación, quiere que la use la **mayor cantidad de plataformas posibles**. Antes había que hacer **una versión por plataforma** y darle al cliente la que correspondía. El middleware **compatibiliza distintas plataformas**: si se desarrolla la aplicación **una sola vez** sobre el middleware, y el middleware funciona sobre varias plataformas, esa aplicación puede usarse en todas (el que hace la compatibilidad es el middleware).

```
        Aplicaciones
   ───────────────────────
        Middleware          ← interfaz de programación / protocolos estándares
   ───────────────────────
   Plataforma (Hardware + SO)
```

---

## Cluster

> [!note] Qué es
> Un **cluster** (su traducción es "racimo") es un **conjunto de equipos reunidos en un mismo lugar**, conectados por una **red de comunicaciones extremadamente veloz**. **NO es un sistema distribuido** (los equipos están todos en el mismo lugar, no distribuidos geográficamente).

**Objetivo:** **potenciar determinadas capacidades** (de almacenamiento, de procesamiento, de impresión, lo que sea). Tiene además un **software que administra el cluster**.

> [!example] Analogía del profe
> Es como los sistemas **RAID** (un conjunto de discos + un software que los administra): en el cluster uno tiene equipos repetidos en un lugar, más un software que los administra, para potenciar lo que le interese.

> [!warning] No confundir
> **Cluster ≠ sistema distribuido.** El cluster reúne equipos en **un mismo lugar**; el sistema distribuido los tiene **distribuidos geográficamente** e interconectados. (Tampoco es lo mismo que el multiprocesamiento **fuertemente acoplado**, que está dentro de un único equipo.)

---

## Enlaces

- [[U9 - Administración de Archivos]]
- [[U1 - Introducción a los Sistemas Operativos]] (multiprocesamiento fuertemente / débilmente acoplado)
- [[Sistemas Operativos — Índice]]
