---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - teoria
  - jamovi
  - desde-cero
tema: Explicación desde cero de la teoría del 2do parcial + jamovi clic por clic
materia: Probabilidad y Estadística
parcial: 2
aliases:
  - Explicado de cero
  - Jamovi paso a paso
  - Teoría desde cero
---

# 🐣 Explicado de CERO — teoría + jamovi paso a paso

> [!abstract] Para quién es esta página
> Para cuando sentís que **no entendés nada**. Acá no se asume nada: primero la **teoría en criollo** (qué es todo esto y por qué), y después **un ejercicio entero hecho clic por clic en jamovi**. Despacio, de a poco.
>
> Cuando ya entiendas el mecanismo, las otras páginas te van a cerrar solas: [[08 - Parciales ejemplo resueltos (oficial)|Parciales ejemplo]] · [[06 - TODOS los ejercicios resueltos|Todos resueltos]] · [[00 - GUÍA 2do Parcial|Guía]].

---
---

# 🧠 PARTE 0 — La teoría, en criollo

Antes de tocar jamovi tenés que entender **una sola idea**. Todo lo demás cuelga de ahí.

## 1. La situación es SIEMPRE la misma

Imaginá una fábrica de discos duros. Fabrica **millones**. Vos querés saber la **velocidad promedio real** de TODOS los discos. A ese promedio real lo llamamos **$\mu$** (se lee "mu").

El problema: **no podés medir millones**. Entonces agarrás **9 discos** (una *muestra*), los medís, y sacás el promedio de esos 9. A ese promedio lo llamamos **$\bar{x}$** (se lee "equis barra").

> [!info] Las dos palabras que tenés que distinguir SIEMPRE
> - **$\mu$ (mu)** = el promedio verdadero de **TODOS** (la *población*). **NO lo conocés.** Es lo que querés averiguar.
> - **$\bar{x}$ (equis barra)** = el promedio de los **pocos** que mediste (la *muestra*). **SÍ lo conocés**, pero es solo una pista.
>
> Toda la materia es esto: **usar la muestra ($\bar x$, que conocés) para decir algo sobre la población ($\mu$, que NO conocés).**

> [!note] Lo mismo pasa con porcentajes
> Si en vez de un promedio te interesa un **porcentaje** (ej. "% de clientes conformes"), cambian las letras pero la idea es idéntica:
> - **$p$** = el porcentaje real de TODOS (no lo conocés).
> - **$\hat p$** (se lee "pe sombrero") = el porcentaje de tu muestra (lo conocés).

## 2. El problema del azar (acá está toda la gracia)

La empresa dice: *"mis discos van a más de 500 MB/s"*. Vos medís 9 y te da $\bar x = 505$. ¿Listo, superan 500?

**No tan rápido.** Mediste solo 9. Capaz tuviste suerte y te tocaron 9 rápidos, pero el promedio real de los millones es 498. El 505 que viste **podría ser pura casualidad del muestreo.**

> [!question] La pregunta de verdad que responde TODA prueba de hipótesis
> *"El $505$ que medí, ¿es porque los discos REALMENTE superan 500? ¿O es casualidad de que me tocaron unos pocos rápidos?"*

## 3. Las dos hipótesis ($H_0$ y $H_1$)

Son **dos apuestas opuestas**. Siempre las mismas dos:

- **$H_0$ (hipótesis nula)** = "no pasa nada", "es casualidad", "no hay efecto". Es la **aburrida**. Siempre se queda con el **igual** ($=$, $\le$ o $\ge$).
- **$H_1$ (hipótesis alternativa)** = "sí pasa algo", "el efecto es real". Es **lo que querés demostrar**.

> [!tip] La regla para no equivocarte NUNCA al plantearlas
> **Lo que el enunciado quiere probar va en $H_1$.** $H_0$ es lo contrario.
> En el ejemplo: quiero probar que "supera 500" (eso es lo interesante) → va en $H_1$.
> $$H_0:\ \mu\le 500 \quad(\text{no supera, es lo aburrido}) \qquad H_1:\ \mu>500 \quad(\text{sí supera, lo que quiero probar})$$

> [!note] ¿Por qué el signo de $H_1$ importa? (la "cola")
> El verbo del enunciado te da el signo de $H_1$, y eso define cómo se mide después:
> - "supera / es mayor / aumenta" → $H_1$ con `>` → se mira **una cola, la derecha**.
> - "es menor / disminuye / reduce" → $H_1$ con `<` → **una cola, la izquierda**.
> - "difiere / cambió / es distinto" → $H_1$ con `≠` → **dos colas** (esto hace que el p-valor se multiplique por 2; ya lo vas a ver).

## 4. El p-valor (EL concepto, el que hay que sentir)

Acá está la palabra que asusta. Pero es simple si la pensás así:

> [!important] Qué es el p-valor, en una frase
> El **p-valor** es **la probabilidad de que lo que viste sea casualidad** (suponiendo que $H_0$ —la aburrida— fuera verdad).

Sigamos con los discos. Medí $\bar x = 505$ y el valor de referencia era 500.

- Si el p-valor da **0,40** → quiere decir *"hay 40% de chances de que ese 505 sea pura casualidad"*. **40% es muchísimo.** No me la creo. → me quedo con la aburrida ($H_0$): no hay evidencia de que supere 500.
- Si el p-valor da **0,01** → *"hay solo 1% de chances de que sea casualidad"*. **1% es muy poco.** Es muy difícil que sea suerte → algo real está pasando → me quedo con $H_1$: sí supera 500.

> [!tip] La intuición
> **p-valor chico** = "es casi imposible que sea casualidad" = **algo real pasa** = me quedo con $H_1$ (rechazo $H_0$).
> **p-valor grande** = "tranquilamente puede ser casualidad" = **no me la creo** = me quedo con $H_0$.

## 5. El $\alpha$ (la línea que decide "chico" vs "grande")

¿Pero cuánto es "chico"? Necesitamos una **línea de corte**. Esa línea es **$\alpha$** (alfa), y **te la da el enunciado** ("con un nivel de significación del 5%", "asumiendo un error del 0,01", etc.).

$\alpha$ es el riesgo que estás dispuesto a correr de equivocarte. Los típicos: 0,01 · 0,05 · 0,10.

> [!warning] LA REGLA UNIVERSAL (la que nunca cambia, jamás)
> $$\boxed{\ p\text{-valor} < \alpha \ \Rightarrow\ \text{Rechazo } H_0\ }$$
> $$p\text{-valor} > \alpha \ \Rightarrow\ \text{NO Rechazo } H_0$$
> Es comparar dos números. Si el p-valor (la chance de casualidad) es **más chico** que la línea de corte → es tan poco probable que sea casualidad que **rechazo la aburrida** y me quedo con que el efecto es real.

> [!example] Mini-ejemplo numérico
> p-valor = 0,01 ; $\alpha$ = 0,05. ¿$0,01 < 0,05$? **Sí** → **Rechazo $H_0$**.
> p-valor = 0,40 ; $\alpha$ = 0,05. ¿$0,40 < 0,05$? **No** → **NO Rechazo $H_0$**.

## 6. La conclusión (traducir al castellano del problema)

Rechazar o no rechazar es la mitad. Falta **decir qué significa** en el problema.

- **Rechazo $H_0$** → me quedo con $H_1$ → *"Sí, hay evidencia de que los discos superan 500 MB/s, con un riesgo del 5%."*
- **No rechazo $H_0$** → *"No hay evidencia suficiente para afirmar que superan 500 MB/s."* (¡Ojo! No digo "son ≤500", solo digo "no pude probar que superan".)

> [!quote] Resumen de la Parte 0 (releelo hasta que te salga solo)
> 1. Tengo una **muestra** ($\bar x$) y quiero saber algo de la **población** ($\mu$), que no conozco.
> 2. **$H_0$** = aburrida (no pasa nada) · **$H_1$** = lo que quiero probar.
> 3. **p-valor** = probabilidad de que lo que vi sea **casualidad**.
> 4. **$\alpha$** = línea de corte (la da el enunciado).
> 5. **$pv<\alpha$ ⇒ Rechazo $H_0$** (algo real pasa). Si no, no rechazo.
> 6. **Conclusión** en palabras del problema.

---
---

# 🖥️ PARTE 1 — jamovi: lo básico antes de empezar

> [!info] ¿Qué es jamovi?
> Un programa gratis que hace las cuentas por vos. Vos cargás los datos, tocás un par de botones, y él te escupe el **p-valor** ya calculado. Tu laburo es **elegir el botón correcto** (la prueba) e **interpretar** el resultado. La cuenta la hace él.

## La pantalla de jamovi

Cuando lo abrís hay **dos pestañas** arriba a la izquierda:

- **Datos (Data):** una planilla tipo Excel. Acá van tus números, **una variable por columna**.
- **Análisis (Analyses):** la barra con los botones de las pruebas (Pruebas t, Recuento, Regresión, etc.).

> [!tip] Cómo cargar los datos
> **Opción A — escribir a mano:** andá a la pestaña **Datos**, hacé doble clic en el encabezado de la primera columna, ponele nombre (ej. `velocidad`), y escribí los números hacia abajo, uno por fila.
> **Opción B — importar un Excel/CSV:** menú **☰** (arriba a la izquierda) → **Importar especial / Abrir** → elegís el archivo.

> [!note] Tipos de variable (un detalle que importa)
> Al lado del nombre de cada columna hay un iconito que dice si la variable es:
> - **Continua** (la regla 📏): números que se promedian → para pruebas t y regresión.
> - **Nominal** (tres círculos): categorías (ej. "sucursal A / sucursal B", "hombre / mujer") → para agrupar o para χ².
> Si jamovi no te deja poner una variable donde querés, casi siempre es porque tiene el **tipo equivocado**. Hacé clic en la columna → cambiá el tipo.

---
---

# ✅ PARTE 2 — Ejercicio COMPLETO clic por clic (t de una muestra)

Vamos con el de los discos, entero, como si nunca hubieras tocado jamovi.

> [!info] Enunciado
> Una empresa afirma que sus discos duros tienen una velocidad de lectura **superior a 500 MB/s**. Se toman **9 discos** y se mide su velocidad (datos ≈ normales). ¿Hay evidencia de que la velocidad media **supera los 500 MB/s**? Usar $\alpha=0{,}05$.
> Datos (MB/s): 498, 512, 505, 500, 515, 503, 508, 495, 513.

### Paso 1 — Pensar QUÉ prueba es (antes de tocar la compu)

Hago las 3 preguntas (ver [[08 - Parciales ejemplo resueltos (oficial)#🧠 ANTES DE EMPEZAR — Cómo pensar CUALQUIER ejercicio (de cero)|las 3 preguntas]]):
- **¿Cuántos grupos?** Uno solo (los discos) → **una población**.
- **¿Media o porcentaje?** Velocidad es un número que se promedia → **media**.
- **¿Me dan σ o s?** No me dan el desvío de la población → es **desconocido** → la prueba es **t** (no z).

→ **Prueba t de una muestra.** El valor de referencia que quiero superar es **500**.

### Paso 2 — Plantear las hipótesis

"Supera 500" es lo que quiero probar → va en $H_1$ con `>`:
$$H_0:\ \mu\le 500 \qquad H_1:\ \mu>500 \quad(\text{cola derecha})$$

### Paso 3 — Cargar los datos en jamovi

1. Abrí jamovi. Andá a la pestaña **Datos**.
2. Doble clic en el encabezado de la **columna A**. Ponele de nombre `velocidad`. Enter.
3. Escribí los 9 números hacia abajo, uno por fila: 498, 512, 505, 500, 515, 503, 508, 495, 513.
4. Fijate que el iconito de la columna sea **Continua** (la reglita 📏). Si no, hacé clic en la columna y cambialo.

### Paso 4 — Correr la prueba

1. Andá a la pestaña **Análisis (Analyses)**.
2. Clic en **Pruebas t (T-Tests)**.
3. En el menú que se abre, elegí **Prueba t de una Muestra (One Sample T-Test)**.
4. Se abre una ventana partida en dos. A la **izquierda** está tu variable `velocidad`. Seleccionala y, con la flechita ▶, pasala al casillero de la **derecha** (Variables / Dependent Variables).

### Paso 5 — Configurar las opciones (acá está el detalle que todos saltean)

En el panel de opciones (a la derecha o abajo) buscá:

1. **Test Value / Valor de prueba:** escribí **500**. (Es el número de $H_0$, el que quiero superar.)
2. **Hypothesis / Hipótesis:** elegí la opción que diga **"> Test value" / "> Valor de prueba"**. ➜ Esto le dice a jamovi que es **cola derecha** (porque mi $H_1$ tiene `>`).
3. **(Recomendado)** Tildá **Normality test / Prueba de normalidad (Shapiro-Wilk)** para chequear el supuesto.

> [!tip] La parte que más confunde: la "Hipótesis" en jamovi
> Ese menú de Hipótesis es EXACTAMENTE el signo de tu $H_1$:
> - $H_1$ con `>` → elegí **"> Valor de prueba"**.
> - $H_1$ con `<` → elegí **"< Valor de prueba"**.
> - $H_1$ con `≠` ("difiere") → elegí **"≠ Valor de prueba"** (la opción del medio, bilateral).
> Si elegís mal acá, el p-valor te sale distinto. **El signo de jamovi = el signo de tu $H_1$.**

### Paso 6 — Leer el resultado

A la derecha aparece una tabla. Buscá la fila de **Student's t** y la columna **p**. Ese número es tu **p-valor**. Con estos 9 datos jamovi da: **t = 2,32**, **df = 8**, y como pedimos la cola derecha (`> 500`), **p = 0,025**. (El promedio muestral, en Descriptivos, es $\bar x = 505{,}44$ MB/s.)

> [!note] El chequeo de normalidad (Shapiro-Wilk)
> Aparece otra tabla chiquita. Si su **p es ALTO (> α)** → los datos son normales → **bien, podés usar la t**. Acá es al revés que en la prueba principal: en los supuestos, **p alto es lo bueno**.

### Paso 7 — Decidir con la regla universal

Comparo el p-valor con $\alpha$:
$$p = 0{,}025 \quad\text{vs}\quad \alpha = 0{,}05 \qquad \Rightarrow\qquad 0{,}025 < 0{,}05 \ \Rightarrow\ \textbf{Rechazo } H_0$$

### Paso 8 — Conclusión en palabras

> [!success] Conclusión
> Como **rechazo $H_0$** me quedo con $H_1$: **sí hay evidencia** de que la velocidad media de los discos **supera los 500 MB/s**, asumiendo un riesgo del 5%. El promedio muestral fue $\bar x = 505{,}44$ MB/s, y el p-valor de 0,025 dice que es muy poco probable que ese exceso sea pura casualidad del muestreo. (La empresa pudo respaldar su afirmación con estos datos.)

> [!success] ¿Ves el patrón?
> Pensar la prueba (pasos 1-2) → cargar (3) → correr (4-5) → leer el p-valor (6) → comparar con α (7) → conclusión (8).
> **Todos** los ejercicios son ESTO. Lo único que cambia es qué botón tocás en el paso 4 y qué opciones en el 5.

---
---

# 🔀 PARTE 3 — Dos grupos distintos (t independientes) clic por clic

> [!info] Cuándo
> Comparás **medias de DOS grupos de individuos distintos**: sucursal A vs B, hombres vs mujeres, sistema nuevo vs viejo. Ejemplo: tiempos en caja de la sucursal A (sistema nuevo) y la B (tradicional). ¿Difieren? $\alpha=0{,}05$.

### Cómo se cargan los datos (¡distinto al caso anterior!)

Acá NO ponés una columna por grupo. Ponés **dos columnas**:
- Columna `tiempo` (Continua 📏): TODOS los tiempos juntos, de los dos grupos.
- Columna `sucursal` (Nominal): al lado de cada tiempo, una etiqueta que diga a qué grupo pertenece (`A` o `B`).

```
tiempo | sucursal
  3.2  |   A
  2.9  |   A
  4.1  |   B
  3.8  |   B
  ...  |  ...
```

### Clic por clic

1. Pestaña **Análisis → Pruebas t → Prueba t para Muestras Independientes**.
2. Pasá `tiempo` al casillero **Variables dependientes**.
3. Pasá `sucursal` al casillero **Variable de agrupación (Grouping Variable)**.
4. En opciones, tildá:
   - **Student's** (la t normal) y **Welch's** (la versión para varianzas distintas) — tildá las dos por ahora.
   - **Homogeneity test / Levene** (chequea si las varianzas son iguales).
   - **Normality / Shapiro-Wilk**.
5. Elegí la **Hipótesis** según el verbo ("difieren" → ≠ bilateral).

### Cómo leer (el orden importa)

> [!warning] Primero Levene, DESPUÉS la t
> 1. Mirá la tabla de **Levene**. Su $H_0$ es "las varianzas son iguales".
>    - Si **Levene p > α** → varianzas iguales → leé la fila de **Student**.
>    - Si **Levene p < α** → varianzas distintas → leé la fila de **Welch**.
> 2. Recién ahí leé el **p-valor** de la fila que corresponde (Student o Welch).
> 3. Comparás ese p con α → regla universal → conclusión.

> [!note] Por qué este paso extra
> Comparar dos grupos asume que dispersan parecido. Levene chequea eso. Si dispersan muy distinto, Student se equivoca y hay que usar Welch (que corrige). Es el único "trámite previo" que tienen las independientes.

## 📝 Ejercicio resuelto ENTERO (explicando cada paso)

> [!example] Enunciado
> Un supermercado prueba un **sistema de caja nuevo** en la sucursal **A** y mantiene el **tradicional** en la **B**. Mide el tiempo (en minutos) que tarda cada cliente en pagar. Quiere saber si el sistema nuevo **reduce** el tiempo. $\alpha=0{,}05$.
> - **Sucursal A (nuevo):** 3,2 · 2,9 · 3,1 · 2,8 · 3,0
> - **Sucursal B (tradicional):** 4,1 · 3,8 · 4,3 · 3,9 · 4,0

**Paso 1 — ¿Qué prueba es? (pienso antes de tocar la compu)**
- ¿Cuántos grupos? **Dos** (sucursal A y sucursal B). → rama "dos poblaciones".
- ¿Son los mismos clientes o distintos? Los clientes de A **no son** los de B → grupos **distintos** → **INDEPENDIENTES** (no apareadas).
- ¿Comparo medias o porcentajes? Tiempos que se promedian → **medias** → familia **t**.
- Conclusión: **prueba t para muestras independientes.**

**Paso 2 — Planteo las hipótesis.**
Quiero probar que el nuevo (A) **reduce** el tiempo, o sea que A tarda **menos** que B: $\mu_A < \mu_B$. Eso es lo que quiero demostrar → va en $H_1$.
$$H_0:\ \mu_A = \mu_B \quad(\text{tardan igual, lo aburrido}) \qquad H_1:\ \mu_A < \mu_B \quad(\text{A tarda menos})$$
Como $H_1$ tiene `<`, es **unilateral** (una cola).

**Paso 3 — Cargo los datos en jamovi (ojo, acá van DOS columnas especiales).**
En la pestaña **Datos** armo:
- Columna `tiempo` (tipo **Continua** 📏): pongo TODOS los tiempos, los 10, uno abajo del otro.
- Columna `sucursal` (tipo **Nominal**): al lado de cada tiempo, escribo a qué grupo pertenece (`A` o `B`).

```
tiempo | sucursal
  3.2  |   A
  2.9  |   A
  3.1  |   A
  2.8  |   A
  3.0  |   A
  4.1  |   B
  3.8  |   B
  4.3  |   B
  3.9  |   B
  4.0  |   B
```
> 💡 *¿Por qué así y no una columna por sucursal?* Porque jamovi necesita una columna que diga el **valor** (tiempo) y otra que diga el **grupo** (sucursal). Así sabe a quién comparar con quién.

**Paso 4 — Corro la prueba.**
Pestaña **Análisis → Pruebas t → Prueba t para Muestras Independientes**.
- Paso `tiempo` a **Variables dependientes** (el número que mido).
- Paso `sucursal` a **Variable de agrupación** (lo que separa los dos grupos).

**Paso 5 — Tildo las opciones.**
- **Student's** y **Welch's** (las dos, para tener ambas a mano).
- **Homogeneity test (Levene)** → para decidir cuál de las dos leer.
- **Hipótesis:** elijo la dirección. Como mi $H_1$ es $\mu_A<\mu_B$ y A va primero, elijo la opción **"Grupo 1 < Grupo 2"**.

**Paso 6 — Leo, PERO en orden: primero Levene.**
Con estos datos jamovi devuelve:
- **Levene → p = 0,700.** Comparo con α: $0{,}700 > 0{,}05$ → Levene **NO** es significativo → **las varianzas son iguales** → leo la fila de **Student** (no Welch).
- **Student's t = −9,16 (df = 8) → p < 0,001** (mi p-valor de verdad, en la cola que corresponde a $\mu_A<\mu_B$).

> 💡 *¿Por qué miro Levene primero?* Porque me dice cuál fila creer. Si Levene hubiera dado p < 0,05 (varianzas distintas), tendría que leer la fila de **Welch** en vez de Student.

**Paso 7 — Aplico la regla universal.**
$$p < 0{,}001 \quad\text{vs}\quad \alpha = 0{,}05 \qquad\Rightarrow\qquad p < 0{,}05 \Rightarrow \textbf{Rechazo } H_0$$

**Paso 8 — Conclusión en palabras.**

> [!success] Conclusión
> Como rechazo $H_0$ me quedo con $H_1$: **hay evidencia de que el sistema nuevo (A) reduce el tiempo** de pago respecto del tradicional (B), con un riesgo del 5%. → Convendría implementarlo.
>
> *(Tiene sentido: el promedio de A es 3,0 min y el de B es 4,02 min; la diferencia es grande y consistente.)*

---
---

# 👥 PARTE 4 — Mismo grupo medido dos veces (t apareadas) clic por clic

> [!info] Cuándo
> El **mismo individuo medido dos veces**: ausencias de las mismas plantas *antes* y *después* de un plan; tiempos de las mismas apps *antes* y *después* de optimizar. Pista: el $n$ es **igual** en los dos momentos.

### Cómo se cargan los datos

Acá sí van **dos columnas**, una por momento, y **cada fila es el mismo individuo**:

```
antes | despues
  12  |   9
  15  |  11
  10  |   8
  ... |  ...
```

### Clic por clic

1. Pestaña **Análisis → Pruebas t → Prueba t para Muestras Pareadas (Paired Samples)**.
2. Pasá las **dos columnas** (`antes` y `despues`) al casillero de pares. jamovi las empareja fila por fila.
3. Elegí la **Hipótesis** según el verbo. Ojo con el orden: jamovi calcula `Medida 1 − Medida 2`.
   - Si querés probar que "bajó" (antes > después, o sea `antes − después > 0`) → poné `antes` primero, `después` segundo, y Hipótesis **"Measure 1 > Measure 2"**.
4. (Recomendado) tildá **Normality**.
5. Leé el **p-valor**, comparalo con α, concluí.

> [!tip] El truco mental de apareadas
> jamovi por dentro hace una sola cosa: calcula la **diferencia** $d = \text{antes} - \text{después}$ de cada individuo, y testea si ese promedio de diferencias es $>0$, $<0$ o $\ne 0$. Por eso es casi una "t de una muestra" sobre las diferencias.

## 📝 Ejercicio resuelto ENTERO (explicando cada paso)

> [!example] Enunciado
> Una empresa aplica un **plan de seguridad** en **6 plantas** industriales. Mide los **días de ausencia** de cada planta **antes** y **después** del plan. Quiere saber si el plan **redujo** las ausencias. $\alpha=0{,}05$.
>
> | Planta | Antes | Después |
> |---|---|---|
> | 1 | 12 | 9 |
> | 2 | 15 | 11 |
> | 3 | 10 | 8 |
> | 4 | 14 | 10 |
> | 5 | 11 | 9 |
> | 6 | 13 | 10 |

**Paso 1 — ¿Qué prueba es?**
- ¿Cuántos grupos? Parecen dos (antes y después), pero ojo: son **las MISMAS 6 plantas** medidas dos veces. → no son grupos distintos → **APAREADAS**.
- La pista que lo confirma: el $n$ es **igual** en los dos momentos (6 y 6), y a cada planta la seguís en el tiempo.
- Conclusión: **prueba t para muestras apareadas (pareadas).**

**Paso 2 — Planteo las hipótesis.**
"Redujo las ausencias" significa que **antes había más que después**, o sea $\text{antes} - \text{después} > 0$. Llamo a esa resta $d$ (la diferencia de cada planta). Quiero probar que el promedio de las $d$ es positivo.
$$H_0:\ \mu_d = 0 \quad(\text{no cambió nada}) \qquad H_1:\ \mu_d > 0 \quad(\text{bajó: antes > después})$$
$H_1$ con `>` → **unilateral derecha**.

**Paso 3 — Cargo los datos (DOS columnas, cada fila = una planta).**
```
antes | despues
  12  |   9
  15  |  11
  10  |   8
  14  |  10
  11  |   9
  13  |  10
```
> 💡 *Acá sí va una columna por momento*, porque jamovi necesita emparejar "antes 12" con "después 9" de la MISMA planta (fila 1), etc.

**Paso 4 — Corro la prueba.**
Pestaña **Análisis → Pruebas t → Prueba t para Muestras Pareadas.**
Paso las dos columnas (`antes` y `despues`) al casillero de pares. jamovi calcula $d = \text{antes} - \text{después}$ solo.

**Paso 5 — Configuro.**
- **Hipótesis:** como quiero $\text{antes} - \text{después} > 0$ y puse `antes` primero, elijo **"Measure 1 > Measure 2"**.
- (Recomendado) tildo **Normality**.

> 💡 *Si lo quisiera ver a mano:* las diferencias son $d = 3, 4, 2, 4, 2, 3$. Todas **positivas** → la planta siempre bajó las ausencias → ya intuyo que va a dar significativo. El promedio de esas diferencias es $\bar d = 18/6 = 3$ días.

**Paso 6 — Leo el p-valor.**
Con estos datos jamovi devuelve **t = 8,22 (df = 5)** y, en la cola que corresponde (antes > después), **p ≈ 0,0002** (jamovi lo muestra como **p < 0,001**).

**Paso 7 — Regla universal.**
$$p \approx 0{,}0002 < \alpha = 0{,}05 \Rightarrow \textbf{Rechazo } H_0$$

**Paso 8 — Conclusión.**

> [!success] Conclusión
> Me quedo con $H_1$: **hay evidencia de que el plan de seguridad redujo los días de ausencia** (en promedio bajaron 3 días por planta), con un riesgo del 5%. → El plan fue exitoso.

---
---

# 📊 PARTE 5 — Datos categóricos (Chi-cuadrado χ²) clic por clic

Acá NO hay promedios. Hay **categorías y conteos**. Hay dos sabores.

## 5A — χ² de BONDAD DE AJUSTE (una variable vs porcentajes dados)

> [!info] Cuándo
> Una sola variable categórica, y la comparás contra **proporciones dadas** (históricas, o "¿es uniforme?"). Ejemplo: navegadores Chrome/Firefox/Edge/Otros, ¿siguen los porcentajes históricos 58/15/7/20?

### Clic por clic

1. Pestaña **Análisis → Recuento (Frequencies) → N Resultados (χ² de bondad de ajuste) / N Outcomes**.
2. Pasá tu variable de categorías al casillero **Variable**.
3. Abrí la sección **Razones esperadas / Expected Proportions** y cargá los porcentajes teóricos (0,58 ; 0,15 ; 0,07 ; 0,20) en el orden de las categorías.
4. Leé el **p-valor** de la tabla χ². Comparalo con α.

> [!note] Hipótesis y gl
> $H_0$: la distribución **sigue** las proporciones dadas. $H_1$: no las sigue.
> Grados de libertad $\nu = c-1$ (categorías − 1). Y recordá: **χ² siempre es cola derecha** (no se toca ninguna opción de "dirección").

### 📝 Ejercicio resuelto ENTERO (acá la cuenta la hacemos a mano, paso a paso)

> [!example] Enunciado
> Históricamente los navegadores se usaban así: **Chrome 58%, Firefox 15%, Edge 7%, Otros 20%**. Se encuesta a **200** usuarios y se obtiene: **Chrome 110, Firefox 19, Edge 14, Otros 57**. ¿Cambió la distribución de uso? $\alpha=0{,}05$.

**Paso 1 — ¿Qué prueba es?**
Hay **categorías** (navegadores) y **conteos** (cuántos usan cada uno) → es **Chi-cuadrado**. Y como comparo **una** variable contra **porcentajes históricos dados** → es **χ² de bondad de ajuste**.

**Paso 2 — Hipótesis.**
$$H_0:\ \text{la distribución sigue siendo 58/15/7/20 (no cambió)} \qquad H_1:\ \text{cambió}$$

**Paso 3 — Calculo las frecuencias ESPERADAS ($e_i$).**
La idea: *"si $H_0$ fuera verdad, ¿cuántos usuarios DEBERÍA haber en cada navegador?"* Se calcula multiplicando el total por cada porcentaje: $e_i = n \cdot p_i$.
$$e_{\text{Chrome}}=200\cdot0{,}58=116 \qquad e_{\text{Firefox}}=200\cdot0{,}15=30$$
$$e_{\text{Edge}}=200\cdot0{,}07=14 \qquad e_{\text{Otros}}=200\cdot0{,}20=40$$
> 💡 Comparemos lo **observado** (lo que realmente salió) con lo **esperado** (lo que debería salir si nada cambió):
> | | Chrome | Firefox | Edge | Otros |
> |---|---|---|---|---|
> | Observado $o_i$ | 110 | 19 | 14 | 57 |
> | Esperado $e_i$ | 116 | 30 | 14 | 40 |
>
> Ya a ojo: Firefox cayó (19 vs 30 esperados) y "Otros" creció mucho (57 vs 40). Eso es lo que el χ² va a "medir".

**Paso 4 — Calculo el estadístico $\chi^2$, término por término.**
La fórmula es $\chi^2_m=\sum\dfrac{(o_i-e_i)^2}{e_i}$. En palabras: para cada navegador, *(observado − esperado), al cuadrado, dividido por el esperado*. Después sumo todo.
$$\frac{(110-116)^2}{116}=\frac{(-6)^2}{116}=\frac{36}{116}=0{,}310$$
$$\frac{(19-30)^2}{30}=\frac{(-11)^2}{30}=\frac{121}{30}=4{,}033$$
$$\frac{(14-14)^2}{14}=\frac{0}{14}=0$$
$$\frac{(57-40)^2}{40}=\frac{(17)^2}{40}=\frac{289}{40}=7{,}225$$
Sumo los cuatro:
$$\chi^2_m = 0{,}310+4{,}033+0+7{,}225 = 11{,}57$$
> 💡 *¿Qué significa ese número?* Mide **cuánto se aleja** lo observado de lo esperado. Si todo coincidiera, daría 0. Cuanto más grande, más "raro" sería que nada haya cambiado.

**Paso 5 — Grados de libertad.**
En bondad de ajuste: $\nu = c-1$, donde $c$ = cantidad de categorías. Hay 4 navegadores → $\nu = 4-1 = 3$.

**Paso 6 — Saco el p-valor (con jamovi o tabla).**
En jamovi: módulo **distrACTION → χ²-Distribution**, pido $P(\chi^2_3 > 11{,}57)$. (O lo corro derecho con **Recuento → N Resultados**, cargando los porcentajes.) Da:
$$pv = P(\chi^2_3 > 11{,}57) \approx 0{,}009$$
> 💡 χ² es **siempre cola derecha**: solo los valores grandes (mucha diferencia) son "raros". Por eso nunca se multiplica por 2.

**Paso 7 — Regla universal.**
$$pv = 0{,}009 < \alpha = 0{,}05 \Rightarrow \textbf{Rechazo } H_0$$

**Paso 8 — Conclusión.**

> [!success] Conclusión
> Me quedo con $H_1$: **hay evidencia de que la distribución de uso de navegadores cambió** respecto de los porcentajes históricos, con un nivel de significación del 5%. (El mayor responsable es "Otros", que creció de 40 esperados a 57 observados.)

## 5B — χ² de INDEPENDENCIA / HOMOGENEIDAD (dos variables en tabla)

> [!info] Cuándo
> Dos variables categóricas cruzadas: "¿la preferencia depende de la edad?", "¿el cumplimiento depende del rubro?". Tenés una **tabla** (filas × columnas).

### Clic por clic

1. Pestaña **Análisis → Recuento (Frequencies) → Prueba de independencia χ² (Independent Samples / χ² test of association)**.
2. Pasá una variable a **Filas (Rows)** y la otra a **Columnas (Columns)**.
3. Si tus datos ya vienen como tabla de conteos (no como filas individuales), pasá la columna de cantidades a **Counts (frecuencias)**.
4. Leé el **p-valor** (`χ² · p`). Comparalo con α.

> [!note] Hipótesis y gl
> $H_0$: las dos variables son **independientes** (no se relacionan). $H_1$: están relacionadas.
> Grados de libertad $\nu = (\text{filas}-1)(\text{columnas}-1)$. Cola derecha siempre.

### 📝 Ejercicio resuelto ENTERO (con la cuenta de las esperadas en tabla)

> [!example] Enunciado
> Una tienda online quiere saber si **comprar o no** depende del **dispositivo** (móvil / computadora). Sobre 200 visitas registra esta tabla. ¿Están relacionadas las dos variables? $\alpha=0{,}05$.
>
> | | Compró | No compró | **Total fila** |
> |---|---|---|---|
> | **Móvil** | 40 | 60 | 100 |
> | **Computadora** | 50 | 50 | 100 |
> | **Total columna** | 90 | 110 | **200** |

**Paso 1 — ¿Qué prueba es?**
Categorías y conteos → χ². Hay **DOS variables** cruzadas (dispositivo × compra) en una tabla → es **χ² de independencia**.

**Paso 2 — Hipótesis.**
$$H_0:\ \text{comprar es INDEPENDIENTE del dispositivo} \qquad H_1:\ \text{están relacionados}$$

**Paso 3 — Calculo las frecuencias esperadas de cada celda.**
La fórmula para tablas es $e_{ij} = \dfrac{(\text{total de su fila}) \cdot (\text{total de su columna})}{n}$. En palabras: *"si fueran independientes, ¿cuántos caerían en esta celda?"*
$$e_{\text{Móvil, Compró}}=\frac{100\cdot 90}{200}=45 \qquad e_{\text{Móvil, No}}=\frac{100\cdot 110}{200}=55$$
$$e_{\text{Compu, Compró}}=\frac{100\cdot 90}{200}=45 \qquad e_{\text{Compu, No}}=\frac{100\cdot 110}{200}=55$$

**Paso 4 — Estadístico $\chi^2$, celda por celda.** $\chi^2_m=\sum\dfrac{(o-e)^2}{e}$ recorriendo las 4 celdas:
$$\frac{(40-45)^2}{45}=\frac{25}{45}=0{,}556 \qquad \frac{(60-55)^2}{55}=\frac{25}{55}=0{,}455$$
$$\frac{(50-45)^2}{45}=\frac{25}{45}=0{,}556 \qquad \frac{(50-55)^2}{55}=\frac{25}{55}=0{,}455$$
$$\chi^2_m = 0{,}556+0{,}455+0{,}556+0{,}455 = 2{,}02$$

**Paso 5 — Grados de libertad.** Tabla de 2 filas × 2 columnas: $\nu=(f-1)(c-1)=(2-1)(2-1)=1$.

**Paso 6 — p-valor.** $pv = P(\chi^2_1 > 2{,}02) \approx 0{,}155$.

**Paso 7 — Regla universal.**
$$pv = 0{,}155 > \alpha = 0{,}05 \Rightarrow \textbf{NO Rechazo } H_0$$

**Paso 8 — Conclusión.**

> [!failure] Conclusión
> Como **no rechazo $H_0$**: **no** hay evidencia de que comprar dependa del dispositivo. Es decir, la decisión de comprar parece ser **independiente** de si entran por móvil o computadora, con un riesgo del 5%.
>
> 💡 Fijate que acá el ejercicio dio **"no rechazo"** — y está perfecto. No siempre se rechaza; depende de los números.

## 5C — Cómo cargar el χ² en jamovi (las dos pruebas): el truco del casillero *Counts*

> [!warning] El detalle que casi nadie sabe
> El χ² casi siempre te lo dan como **tabla de conteos** (ej. "Chrome 110, Firefox 19…"), no como datos fila por fila. El truco para que jamovi lo entienda es una **columna extra de frecuencias** metida en el casillero **Counts / Recuentos**. Si te lo olvidás, jamovi cuenta "4 filas" en vez de "200 casos" y te da cualquier cosa.

### Bondad de ajuste (navegadores) — qué se carga

**Pestaña Datos → armás 2 columnas (una fila por categoría):**

| `navegador` (Nominal) | `frec` (Continua/entero) |
|---|---|
| Chrome | 110 |
| Firefox | 19 |
| Edge | 14 |
| Otros | 57 |

**Corrés el análisis:**
1. **Análisis → Recuento (Frequencies) → N Resultados / N Outcomes (χ² Goodness of fit).**
2. **Variable:** pasá `navegador`.
3. **Counts / Recuentos (optional):** pasá `frec`. ← *le dice a jamovi que cada fila es un conteo, no un solo caso.*
4. Abrí **Expected Proportions / Razones esperadas** y cargá los porcentajes históricos en el orden de las filas: **0,58 · 0,15 · 0,07 · 0,20**.

→ **Da:** χ² = 11,6 · gl = 3 · **p = 0,009** → $p<0{,}05$ → **rechazo H₀** (la distribución cambió). ✅ *(Coincide con la cuenta a mano de 5A.)*

### Independencia (compra × dispositivo) — qué se carga

**Pestaña Datos → armás 3 columnas (una fila por celda de la tabla):**

| `dispositivo` (Nominal) | `compra` (Nominal) | `frec` (entero) |
|---|---|---|
| Móvil | Compró | 40 |
| Móvil | No compró | 60 |
| Computadora | Compró | 50 |
| Computadora | No compró | 50 |

**Corrés el análisis:**
1. **Análisis → Recuento (Frequencies) → Prueba de independencia / Independent Samples (χ² test of association).**
2. **Filas (Rows):** `dispositivo`.
3. **Columnas (Columns):** `compra`.
4. **Counts / Recuentos (optional):** pasá `frec`.

→ **Da:** χ² = 2,02 · gl = 1 · **p = 0,155** → $p>0{,}05$ → **NO rechazo H₀** (comprar es independiente del dispositivo). ✅ *(Coincide con la cuenta a mano de 5B.)*

> [!tip] La clave de las dos
> Como te dan **totales ya contados** (110, 40, etc.) y no una fila por persona, esa columna de frecuencias **va en Counts / Recuentos**. Ese es el único secreto de cargar χ² en jamovi.

> [!note] ¿No te aparece "N Resultados" o "Prueba de independencia" en el menú Recuento?
> Puede que el módulo no esté activado. Andá al **➕ (Modules)** arriba a la derecha → **jamovi library** → instalá/activá lo que falte. Igual, si te falla, siempre podés hacer el χ² **a mano** (5A/5B) y sacar el p-valor con **distrACTION → χ²-Distribution**.

---
---

# 📈 PARTE 6 — Regresión y correlación clic por clic

> [!info] Cuándo
> Dos variables **numéricas** relacionadas: $x$ (la que explica) e $y$ (la que se explica). Querés la recta $\hat y = a + bx$, qué tan fuerte es la relación ($r$, $r^2$) y si la relación es real (test de $\beta$).

### Cómo se cargan los datos

Dos columnas continuas, una para `x` y otra para `y`, **emparejadas por fila** (cada fila es un caso).

### Clic por clic

1. Pestaña **Análisis → Regresión (Regression) → Regresión lineal (Linear Regression)**.
2. Pasá la variable `y` (la que querés estimar) al casillero **Variable dependiente (Dependent)**.
3. Pasá la variable `x` (la que explica) al casillero **Covariables (Covariates)**.
4. (Para el dispersograma) Pestaña **Exploración → Gráfico de dispersión (Scatterplot)**, poné `x` en el eje X e `y` en el eje Y.

### Cómo leer los resultados

> [!tip] Qué número es cada cosa
> - **Model Fit → R²:** el coeficiente de determinación. Multiplicalo por 100 → es el **% de la variación de $y$ que explica $x$**.
> - **Model Coefficients → fila (Intercept):** es la **ordenada $a$**.
> - **Model Coefficients → fila de tu variable $x$:** el **Estimate** es la **pendiente $b$**; la columna **p** de esa fila es el **p-valor del test de $\beta$**.

> [!note] El test de la pendiente ($\beta$)
> "¿Hay asociación lineal?" se responde con el **p de la fila de $x$**.
> $H_0:\beta=0$ (no hay relación) · $H_1:\beta\ne0$ (sí hay). Si **p < α** → rechazo → **sí hay asociación lineal**.
> El signo de $r$ es el **mismo signo que la pendiente $b$**. Si $b$ es negativa, $r=-\sqrt{r^2}$.

> [!warning] Dos trampas clásicas de regresión
> - **No interpretar la ordenada $a$** si $x=0$ está **fuera del rango** de los datos (ej. "0 empleados", "0 años": no existen).
> - **No extrapolar:** solo estimás $y$ para valores de $x$ **dentro** del rango observado.

## 📝 Ejercicio resuelto ENTERO (interpretando cada número)

> [!example] Enunciado
> Se estudia la relación entre **horas de estudio** ($x$) y la **nota** del examen ($y$) en 5 alumnos. ¿Hay asociación lineal? Interpretá los coeficientes y el $r^2$. $\alpha=0{,}05$.
>
> | Horas $x$ | 2 | 3 | 4 | 5 | 6 |
> |---|---|---|---|---|---|
> | Nota $y$ | 50 | 55 | 65 | 70 | 80 |

**Paso 1 — ¿Qué prueba es?**
Dos variables **numéricas** ($x$ explica, $y$ se explica) y me piden la relación → **regresión y correlación** (P9).

**Paso 2 — Cargo los datos y corro jamovi.**
- Dos columnas continuas: `horas` y `nota`, emparejadas por fila.
- **Análisis → Regresión → Regresión lineal.** Paso `nota` a **Dependiente** y `horas` a **Covariable**.

**Paso 3 — Leo la recta que devuelve jamovi.**
jamovi da los coeficientes: **ordenada $a = 34$** (fila *Intercept*) y **pendiente $b = 7{,}5$** (fila *horas*). La recta es:
$$\hat y = 34 + 7{,}5\,x$$

**Paso 4 — Interpreto los coeficientes (¡esto es lo que más se pregunta!).**
- **Pendiente $b = 7{,}5$:** por **cada hora más** de estudio, la nota **sube 7,5 puntos** en promedio. (El signo es positivo → relación directa: más estudio, más nota.)
- **Ordenada $a = 34$:** sería la nota de alguien que estudia $x=0$ horas. **PERO** los datos van de 2 a 6 horas, así que $x=0$ está **fuera del rango** → **no tiene interpretación práctica** confiable (no medimos a nadie con 0 horas).

**Paso 5 — Interpreto el $r^2$ (coeficiente de determinación).**
jamovi devuelve **$r^2 = 0{,}987$**. Lo paso a porcentaje: **98,7%**.
> 💡 Significa: el **98,7% de la variación de las notas** se explica por las horas de estudio. Es altísimo → la recta ajusta muy bien.

**Paso 6 — ¿Hay asociación lineal? → test de la pendiente $\beta$.**
"¿Hay asociación?" se responde probando si la pendiente real es distinta de 0 (si fuera 0, la recta sería plana y $x$ no influiría).
$$H_0:\ \beta = 0\ (\text{no hay asociación}) \qquad H_1:\ \beta \ne 0\ (\text{sí hay})$$
El p-valor de esto está en la **fila de `horas`, columna p** de la tabla de coeficientes. Con estos datos jamovi da **t = 15,0 (df = 3)** y **p < 0,001** (≈ 0,0006). *(A mano sería $t_m=b/S_b=7{,}5/0{,}5=15$; ver [[#➕ ADICIONAL B — Cuando te dan x̄, s, n (sin datos): el estadístico A MANO|Adicional B]].)*

**Paso 7 — Regla universal.**
$$p < 0{,}001 < \alpha = 0{,}05 \Rightarrow \textbf{Rechazo } H_0$$

**Paso 8 — Conclusión.**

> [!success] Conclusión
> Me quedo con $H_1$: **sí hay asociación lineal** entre horas de estudio y nota, con un riesgo del 5%. Como además $r^2 = 98{,}7\%$ (muy alto), **el modelo es muy bueno** para estimar la nota a partir de las horas.
>
> 💡 **El signo de $r$:** como la pendiente es positiva, la correlación es positiva: $r = +\sqrt{0{,}987} \approx 0{,}993$ (relación directa y muy fuerte).

---
---

# 🔢 PARTE 7 — Proporciones (esto va casi siempre A MANO)

> [!info] Importante
> Las pruebas de **una proporción** (ej. "¿el % supera 45%?") en general **no** se hacen con un botón de jamovi: se calculan **a mano** con la fórmula, y solo usás jamovi (módulo **distrACTION**, distribución **Normal**) para sacar el p-valor a partir del estadístico $z$.

### Una proporción — a mano

1. Calculás $\hat p = \dfrac{\text{éxitos}}{n}$ (ej. 50 de 100 → 0,50).
2. Armás las hipótesis con el **valor afirmado** del enunciado (¡no con $\hat p$!): ej. "¿supera 45%?" → $H_0: p\le 0{,}45$, $H_1: p>0{,}45$.
3. Calculás el estadístico:
$$z_m=\frac{\hat p - p_0}{\sqrt{\dfrac{p_0(1-p_0)}{n}}}$$
4. Sacás el p-valor con la Normal (en **distrACTION → Normal Distribution**, o tabla) según la cola.
5. Regla universal y conclusión.

### Dos proporciones — sí hay botón

> [!tip] Ruta en jamovi para diferencia de 2 proporciones
> **Recuento → Prueba de independencia χ²** → en opciones, **tildá "Prueba z para diferencia de 2 proporciones / Comparing proportions"**. Te da el p-valor directo. Hipótesis: $H_0: p_1=p_2$ vs $H_1: p_1\ne p_2$ (o la dirección que pida).

## 📝 Ejercicio resuelto ENTERO de una proporción (todo a mano, paso a paso)

> [!example] Enunciado
> Se sospecha que **más del 45%** de los comercios de una zona tuvo pérdidas. Se revisan **100** comercios y **50** tuvieron pérdidas. ¿Hay evidencia de que el porcentaje **supera el 45%**? $\alpha=0{,}05$.

**Paso 1 — ¿Qué prueba es?**
Un solo grupo, y el dato es un **porcentaje** (% de comercios con pérdidas) → **prueba z para una proporción** (se hace a mano).

**Paso 2 — Calculo la proporción muestral $\hat p$.**
$$\hat p = \frac{\text{éxitos}}{n} = \frac{50}{100} = 0{,}50$$
> 💡 "Éxito" acá es "tuvo pérdidas" (lo que estamos contando), no algo bueno. Es solo el nombre técnico.

**Paso 3 — Planteo las hipótesis con el valor AFIRMADO (no con $\hat p$).**
La trampa más típica: la hipótesis NO se arma con el 0,50 que medí, sino con el **0,45 del enunciado** (lo que se sospecha superar).
$$H_0:\ p \le 0{,}45 \qquad H_1:\ p > 0{,}45 \quad(\text{"supera" → cola derecha})$$

**Paso 4 — Calculo el estadístico $z$.**
La fórmula compara mi $\hat p$ con el valor de $H_0$ ($p_0 = 0{,}45$):
$$z_m = \frac{\hat p - p_0}{\sqrt{\dfrac{p_0(1-p_0)}{n}}}$$
Lo lleno por partes para no marearme:
- Arriba: $\hat p - p_0 = 0{,}50 - 0{,}45 = 0{,}05$.
- Abajo, adentro de la raíz: $\dfrac{0{,}45 \cdot 0{,}55}{100} = \dfrac{0{,}2475}{100} = 0{,}002475$.
- La raíz: $\sqrt{0{,}002475} = 0{,}04975$.
- Divido: $z_m = \dfrac{0{,}05}{0{,}04975} \approx 1{,}005$.

**Paso 5 — Saco el p-valor (con la Normal).**
Como $H_1$ es cola derecha, busco la probabilidad de que la Normal supere 1,005. En **distrACTION → Normal Distribution** (o tabla):
$$pv = P(Z > 1{,}005) \approx 0{,}157$$

**Paso 6 — Regla universal.**
$$pv = 0{,}157 > \alpha = 0{,}05 \Rightarrow \textbf{NO Rechazo } H_0$$

**Paso 7 — Conclusión.**

> [!failure] Conclusión
> Como **no rechazo $H_0$**: **no** hay evidencia suficiente para afirmar que el porcentaje de comercios con pérdidas **supere el 45%**, con un riesgo del 5%.
>
> 💡 Aunque mi muestra dio 50% (más que 45%), la diferencia es chica y con solo 100 comercios **puede ser casualidad** del muestreo. Por eso no alcanza para afirmarlo. ¡Justo lo que vimos en la teoría!

---
---

# 🎯 PARTE 8 — Intervalos de confianza (IC): NO son prueba de hipótesis

> [!info] Cómo se reconocen
> Cuando el enunciado dice **"estimar"**, **"dar un intervalo"** o **"con una confianza del X%"**, NO hay $H_0/H_1$ ni p-valor: solo construís un rango donde "vive" el parámetro real.

### Para una media — en jamovi

> [!tip] Truco: el IC sale de la misma prueba t de una muestra
> 1. **Pruebas t → Prueba t de una Muestra**, pasá tu variable.
> 2. En opciones tildá **Mean difference / Diferencia de medias → Confidence interval**, y poné la confianza (95%, 92%, etc.).
> 3. ⚠️ Dejá la **Hipótesis en "≠"** (bilateral) para que el intervalo salga centrado.
> 4. Leé los dos números: ese es tu $IC = (l_i\,;\ l_s)$.

### La cuenta a mano (para entender qué hace jamovi)

$$IC = \bar x \pm \underbrace{t_{(1-\alpha/2;\,\nu)}\cdot\frac{s}{\sqrt n}}_{\text{esto es el }EM\ (\text{error de muestreo})}$$

> [!note] Tres datos que siempre caen sobre IC
> - **Amplitud** = ancho total del intervalo = $2\cdot EM$. (Si te dan la amplitud, $EM=$ amplitud$/2$.)
> - El IC está **centrado** en el valor muestral ($\bar x$ o $\hat p$): $l_i = \text{centro}-EM$, $l_s=\text{centro}+EM$.
> - **Más confianza → intervalo más ancho** (menos preciso). **Más $n$ → más angosto** (más preciso).

## 📝 Ejercicio resuelto ENTERO (construir un IC a mano, paso a paso)

> [!example] Enunciado
> Se miden **10** baterías y duran en promedio **$\bar x = 50$ horas**, con desvío muestral **$s = 4$ horas**. Construí un **intervalo de confianza del 95%** para la duración media real. (Datos ≈ normales.)

**Paso 1 — ¿Qué pide?**
Dice "**intervalo de confianza**" → NO es prueba de hipótesis. No hay $H_0/H_1$ ni p-valor: solo armo un rango donde "vive" la media real $\mu$.

**Paso 2 — Elijo la fórmula.**
Es una media con $\sigma$ desconocido (me dan $s$, no $\sigma$) → IC con **t**:
$$IC = \bar x \pm \underbrace{t_{(1-\alpha/2;\,\nu)}\cdot\frac{s}{\sqrt n}}_{EM}$$

**Paso 3 — Busco el valor crítico $t$.**
- Confianza 95% → $1-\alpha = 0{,}95$ → $\alpha = 0{,}05$ → $\alpha/2 = 0{,}025$ (se reparte el error en las dos colas).
- Grados de libertad: $\nu = n-1 = 10-1 = 9$.
- Busco $t_{(0{,}975;\,9)}$ (en distrACTION o tabla) → **$\approx 2{,}262$**.

**Paso 4 — Calculo el error de muestreo (EM).**
$$EM = t \cdot \frac{s}{\sqrt n} = 2{,}262 \cdot \frac{4}{\sqrt{10}} = 2{,}262 \cdot \frac{4}{3{,}162} = 2{,}262 \cdot 1{,}265 \approx 2{,}86$$

**Paso 5 — Armo el intervalo (centro ± EM).**
$$IC_{95\%} = 50 \pm 2{,}86 = (50-2{,}86\ ;\ 50+2{,}86) = (47{,}14\ ;\ 52{,}86)$$

**Paso 6 — Interpreto en palabras.**

> [!success] Interpretación
> Con una **confianza del 95%**, se estima que la **duración media real** de las baterías está entre **47,14 y 52,86 horas**.
>
> 💡 La **amplitud** (ancho total) es $52{,}86 - 47{,}14 = 5{,}72$, que es exactamente $2\cdot EM = 2\cdot 2{,}86$. Si me pidieran un IC del 99% (más confianza), el $t$ sería más grande → el intervalo saldría **más ancho** (menos preciso).

---
---

# ➕ ADICIONAL A — Los dos errores y la potencia (esto lo toman SÍ o SÍ)

> [!warning] Por qué leés esto
> En el parcial **siempre** cae una pregunta de "¿qué error se puede cometer?" o un Verdadero/Falso sobre error tipo 1, error tipo 2 o potencia. El resto del 09 no lo tocó: acá está, en criollo.

## La idea: decidir con una muestra = poder equivocarse de DOS formas

Cuando rechazás o no rechazás $H_0$, **podés acertar o podés errar**. Y hay **dos maneras distintas de errar**, según cuál sea la verdad que vos no conocés:

> [!info] El cuadro que tenés que poder dibujar de memoria
> |  | **$H_0$ es VERDADERA** (de verdad no pasa nada) | **$H_0$ es FALSA** (de verdad sí pasa algo) |
> |---|---|---|
> | **Rechazo $H_0$** | ❌ **Error tipo 1** (prob. = $\alpha$) | ✅ Acierto = **Potencia** (prob. = $1-\beta$) |
> | **No rechazo $H_0$** | ✅ Acierto | ❌ **Error tipo 2** (prob. = $\beta$) |

- **Error tipo 1 ($\alpha$):** rechazo $H_0$ **cuando era verdadera**. "Grité que pasaba algo y no pasaba nada." Su probabilidad es **$\alpha$** (sí, el mismo α de la línea de corte: por eso α también se llama *nivel de significación* o *riesgo*).
- **Error tipo 2 ($\beta$):** **NO** rechazo $H_0$ **cuando era falsa**. "Pasaba algo de verdad y yo no me di cuenta."
- **Potencia ($1-\beta$):** acertar cuando sí pasaba algo = **rechazar $H_0$ siendo falsa**. Es lo que querés que sea grande.

> [!tip] El truco para no confundirlos NUNCA
> Mirá **qué hiciste** y **cuál era la verdad**:
> - Erré **rechazando** (dije "pasa algo" y era mentira) → **tipo 1** = $\alpha$.
> - Erré **NO rechazando** (dije "no pasa nada" y sí pasaba) → **tipo 2** = $\beta$.
> - **Rechazar siendo falsa** $H_0$ = **acierto** = **potencia** (NO es un error). ⚠️ Esta es la trampa más típica del V/F.

> [!example] La trampa textual del parcial (caso real)
> *"En una prueba de hipótesis, la probabilidad de rechazar la hipótesis nula siendo esta falsa es el error tipo 1."* → **FALSO.** Rechazar $H_0$ siendo falsa es una **decisión correcta**: es la **potencia** ($1-\beta$), no un error.

## Cómo identificar α y β cuando el enunciado te da dos probabilidades

A veces el enunciado te da dos números sueltos y te pide decir cuál es $\alpha$ y cuál es $\beta$. La regla:

> [!important] Asociá cada probabilidad con qué hipótesis era cierta
> - La probabilidad ligada a **"$H_0$ es cierta pero igual actúo / decido el efecto"** → es **$\alpha$** (tipo 1).
> - La probabilidad ligada a **"$H_1$ es cierta pero NO actúo / no detecto el efecto"** → es **$\beta$** (tipo 2).

> [!example] Caso real (maestría)
> "La probabilidad de **NO ofrecer** la maestría cuando la media real es **19,5** (o sea, cuando SÍ convenía, $H_1$ cierta) es **0,10**" → ese es **$\beta = 0{,}10$** (no detecté algo que pasaba).
> "La probabilidad de **ofrecer** la maestría cuando la media es **18** (o sea, cuando NO convenía, $H_0$ cierta) es **0,08**" → ese es **$\alpha = 0{,}08$** (actué de más).

> [!note] Dos detalles que cierran el tema
> - **Conclusión cuando NO rechazás:** el error que podrías estar cometiendo es el **tipo 2 ($\beta$)** (te quedaste con la aburrida, capaz pasaba algo).
> - **Conclusión cuando SÍ rechazás:** el error posible es el **tipo 1 ($\alpha$)**.
> - Bajar $\alpha$ (ser más exigente) **sube $\beta$**, y al revés. No se pueden achicar los dos a la vez con el mismo $n$. La forma de bajar ambos es **agrandar la muestra**.

---
---

# ➕ ADICIONAL B — Cuando te dan x̄, s, n (sin datos): el estadístico A MANO

> [!warning] El caso más común del parcial (y el que el 09 no mostró)
> En la Parte 2 cargamos **9 números crudos** en jamovi. Pero en el parcial real casi siempre te dan el **resumen ya hecho**: `x̄ = 10,45  s = 0,2309  n = 12`. **No hay datos para cargar** → no podés "tocar el botón". Tenés que calcular el **estadístico a mano** con la fórmula del formulario y sacar el p-valor con **distrACTION** (o tabla). Es más fácil de lo que parece: es reemplazar en una fórmula.

## Media (σ desconocido) — paso a paso a mano

La fórmula (está en las **Fórmulas Permitidas**):
$$t_m=\frac{\bar x-\mu_0}{\dfrac{s}{\sqrt n}}\qquad \nu=n-1$$
- $\mu_0$ = el valor del enunciado (el de $H_0$), **no** tu $\bar x$.
- El p-valor depende de la cola de $H_1$: derecha $P(t>t_m)$, izquierda $P(t<t_m)$, bilateral $2\cdot P(t>|t_m|)$.

> [!example] Resuelto con números reales (P1-1a del parcial ejemplo)
> *Solución 1: $\bar x=10{,}45$, $s=0{,}2309$, $n=12$. ¿La rapidez media supera 10,3? $\alpha=0{,}07$.*
> **1.** Hipótesis: "supera 10,3" → $H_0:\mu\le10{,}3$ , $H_1:\mu>10{,}3$ (cola derecha).
> **2.** Calculo $t_m$ por partes:
> - Arriba: $\bar x-\mu_0 = 10{,}45-10{,}3 = 0{,}15$.
> - Abajo: $\dfrac{s}{\sqrt n}=\dfrac{0{,}2309}{\sqrt{12}}=\dfrac{0{,}2309}{3{,}464}=0{,}0667$.
> - Divido: $t_m=\dfrac{0{,}15}{0{,}0667}\approx 2{,}25$.
> **3.** Grados de libertad: $\nu=n-1=11$.
> **4.** p-valor (cola derecha): en **distrACTION → t-Distribution**, pido $P(t_{11}>2{,}25)\approx \mathbf{0{,}023}$.
> **5.** Regla: $0{,}023 < 0{,}07 \Rightarrow$ **Rechazo $H_0$**. Hay evidencia de que la rapidez media supera 10,3 ms (riesgo 7%). *(La respuesta oficial da p-v ≈ 0,0229. ✅)*

## Regresión — el test de β a mano con $S_b$

En la Parte 6 el p-valor del test de β lo sacaste con jamovi (te daba $t=15$, $p<0{,}001$). Pero en el parcial te dan **$S_b$** (el desvío de la pendiente) y lo calculás vos:
$$t_m=\frac{b}{S_b}\qquad \nu=n-2\qquad pv=2\cdot P(t>|t_m|)$$
($H_0:\beta=0$ vs $H_1:\beta\ne0$, **siempre bilateral**.)

> [!example] Resuelto con números reales (P1-3b del parcial ejemplo)
> *$\hat y=88{,}5+0{,}23x$, $n=10$ equipos, $S_b=0{,}108$. ¿Hay asociación lineal? $\alpha=0{,}05$.*
> - $t_m=\dfrac{b}{S_b}=\dfrac{0{,}23}{0{,}108}\approx 2{,}13$.
> - $\nu=n-2=8$.
> - $pv=2\cdot P(t_8>2{,}13)\approx \mathbf{0{,}066}$.
> - Regla: $0{,}066 > 0{,}05 \Rightarrow$ **NO rechazo $H_0$** → **no** hay evidencia de asociación lineal (riesgo 5%). *(Respuesta oficial: p-v=0,066, no se rechaza. ✅)*

## Cómo sacar el p-valor en jamovi cuando ya tenés el estadístico

> [!tip] distrACTION = tu "tabla" digital
> Cuando ya calculaste $t_m$, $z_m$ o $\chi^2_m$ a mano, el p-valor sale así:
> 1. **Análisis → distrACTION → "X-Distribution"** (Normal para z, t-Distribution para t, χ²-Distribution para χ²).
> 2. Cargá los grados de libertad ($\nu$) si los pide (t y χ² sí; la Normal no).
> 3. Tildá **"Compute probability"**, poné tu estadístico como límite y elegí la cola:
>    - cola **derecha** → $P(X > \text{estad.})$
>    - cola **izquierda** → $P(X < \text{estad.})$
>    - **bilateral** → calculás $P(X>|\text{estad}|)$ y **lo multiplicás por 2**.
> 4. Ese número es tu **p-valor** → regla universal → conclusión.

> [!quote] Regla de oro para el día del parcial
> **¿Te dan los datos crudos?** → jamovi (Partes 2–6 de arriba).
> **¿Te dan solo $\bar x$, $s$, $n$ (o $b$, $S_b$, o un $\chi^2$/$t_m$ ya calculado)?** → fórmula a mano + distrACTION para el p-valor. **Las dos rutas llegan al mismo p-valor.**

---
---

> [!quote] El mapa mental de todo el parcial (versión final)
> 1. **Leé el enunciado:** ¿cuántos grupos? ¿media/proporción/categoría? ¿probar o estimar? ¿me dan datos crudos o resumen?
> 2. Eso te da **la prueba** (y si va por jamovi o a mano).
> 3. **$H_1$** = lo que querés probar; el **verbo** te da la cola.
> 4. Conseguí el **p-valor** (jamovi, o estadístico a mano + distrACTION).
> 5. **$pv<\alpha$ ⇒ Rechazo $H_0$.** Siempre.
> 6. **Conclusión** en palabras del problema, y si te preguntan, **qué error podrías cometer** (rechacé → tipo 1; no rechacé → tipo 2).

---
---

# 🖐️ BANCO FINAL — Ejercicios A MANO resueltos (los que NO salen solos por jamovi)

> [!abstract] Para qué es esto
> Acá están los casos que en el parcial **te dan resumidos** (`x̄, s, n`, conteos, coeficientes) — **no hay datos para cargar en jamovi**, los hacés con fórmula. Son **ejercicios REALES de los dos parciales ejemplo oficiales**, resueltos enteros. Los números coinciden con las respuestas de la cátedra. Si estos te salen, estás a nivel parcial.

## 🅰️ t de una muestra con datos RESUMIDOS (no te dan la lista)

> [!example] Enunciado (real — P2-3)
> Una universidad abre la maestría solo si los interesados **superan 18**. Encuesta a las **9** carreras y obtiene $\bar x = 19{,}444$ interesados, $s = 3{,}779$. ¿Conviene abrir la inscripción, con un error del **8%** ($\alpha=0{,}08$)?

**1. Qué prueba:** un grupo, media, me dan $s$ (no σ) → **t de una muestra**. *(No hay datos crudos → a mano.)*
**2. Hipótesis:** "superan 18" → $H_0:\mu\le18$ , $H_1:\mu>18$ (cola derecha).
**3. Estadístico** $t_m=\dfrac{\bar x-\mu_0}{s/\sqrt n}$:
- Arriba: $19{,}444-18 = 1{,}444$.
- Abajo: $\dfrac{3{,}779}{\sqrt 9}=\dfrac{3{,}779}{3}=1{,}260$.
- $t_m=\dfrac{1{,}444}{1{,}260}\approx \mathbf{1{,}146}$ , con $\nu=n-1=8$.
**4. p-valor** (cola der., en distrACTION → t-Distribution): $P(t_8>1{,}146)\approx \mathbf{0{,}142}$.
**5. Regla:** $0{,}142 > 0{,}08 \Rightarrow$ **NO rechazo $H_0$**.

> [!failure] Conclusión
> No hay evidencia suficiente para aconsejar abrir la maestría, con un error del 8% (p-v ≈ 0,142). *(El error que podrías cometer al no rechazar es el **tipo 2 / β**.)*

## 🅱️ Diferencia de DOS proporciones

> [!example] Enunciado (real — P2-3d)
> De **120** egresados de Ingeniería, **54** siguen un posgrado; de **200** de Cs. Económicas, el **54%** (108) lo hace. ¿Hay diferencia entre las dos facultades? $\alpha=0{,}08$.

**1. Qué prueba:** dos grupos, **porcentajes** → **z de dos proporciones**. *(Bilateral: "¿hay diferencia?")*
**2. Hipótesis:** $H_0:p_1=p_2$ , $H_1:p_1\ne p_2$.
**3. Proporciones:** $\hat p_1=\dfrac{54}{120}=0{,}45$ ; $\hat p_2=\dfrac{108}{200}=0{,}54$ ; combinada $\bar p=\dfrac{54+108}{120+200}=\dfrac{162}{320}=0{,}506$.
**4. Estadístico** $z_m=\dfrac{\hat p_1-\hat p_2}{\sqrt{\bar p(1-\bar p)\left(\frac1{n_1}+\frac1{n_2}\right)}}$:
- Abajo: $\sqrt{0{,}506\cdot0{,}494\cdot\left(\frac1{120}+\frac1{200}\right)}=\sqrt{0{,}250\cdot0{,}0133}=\sqrt{0{,}00333}=0{,}0577$.
- $z_m=\dfrac{0{,}45-0{,}54}{0{,}0577}=\dfrac{-0{,}09}{0{,}0577}\approx \mathbf{-1{,}56}$.
**5. p-valor** (bilateral, Normal): $2\cdot P(Z>1{,}56)\approx \mathbf{0{,}117}$.
**6. Regla:** $0{,}117 > 0{,}08 \Rightarrow$ **NO rechazo $H_0$**.

> [!failure] Conclusión
> No hay evidencia de diferencia en el % de egresados con posgrado entre las dos facultades (p-v ≈ 0,117, $\alpha=0{,}08$).
>
> ⚠️ **Ojo:** la fórmula de 2 proporciones **NO está en la hoja permitida**. O te la sabés de memoria, o lo hacés por jamovi: **Recuento → independencia χ² → tildá "Comparing proportions / z de 2 proporciones"**.

## 🅲️ Test de la pendiente β en regresión (te dan los coeficientes)

> [!example] Enunciado (real — P1-3)
> $\hat y = 88{,}5 + 0{,}23x$, con **$n=10$** equipos y **$S_b = 0{,}108$**. ¿Hay asociación lineal? $\alpha=0{,}05$.

**1. Qué prueba:** test de la pendiente → **t bilateral** ($H_0:\beta=0$ vs $H_1:\beta\ne0$).
**2. Estadístico** $t_m=\dfrac{b}{S_b}=\dfrac{0{,}23}{0{,}108}\approx \mathbf{2{,}13}$ , con $\nu=n-2=8$.
**3. p-valor** (bilateral): $2\cdot P(t_8>2{,}13)\approx \mathbf{0{,}066}$.
**4. Regla:** $0{,}066 > 0{,}05 \Rightarrow$ **NO rechazo $H_0$** → **no** hay asociación lineal (riesgo 5%). *(Lo mismo que [[#➕ ADICIONAL B — Cuando te dan x̄, s, n (sin datos): el estadístico A MANO|Adicional B]].)*

## 🅳️ Completar / IC de proporción a mano

> [!example] Enunciado (real — P2-5b)
> Un IC para proporciones tiene **amplitud 0,06** y el valor muestral es **$\hat p = 0{,}45$**. Hallá $l_i$ y $l_s$.

- La amplitud es $2\cdot EM$ → $EM = \dfrac{0{,}06}{2}=0{,}03$.
- El IC está **centrado** en $\hat p$: $l_i=0{,}45-0{,}03=\mathbf{0{,}42}$ , $l_s=0{,}45+0{,}03=\mathbf{0{,}48}$.

> [!example] Otro de completar (real — P2-5a)
> *"Si en un IC aumento el nivel de significación y mantengo el $n$, la amplitud del intervalo…"* → **se reduce** (más significación = menos confianza = intervalo más angosto).

## 🅴️ Conceptuales: errores, α, β, potencia (cero jamovi, puro entender)

> [!example] V/F real — P1-4c
> *"La probabilidad de rechazar $H_0$ siendo falsa es el error tipo 1."* → **FALSO.** Rechazar $H_0$ siendo falsa es una **decisión correcta**: es la **potencia** ($1-\beta$), no un error.

> [!example] Identificar α y β — P2-3a
> "Prob. de **NO ofrecer** la maestría cuando SÍ convenía ($\mu=19{,}5$) = 0,10" → es **$\beta=0{,}10$** (error tipo 2). "Prob. de **ofrecer** cuando NO convenía ($\mu=18$) = 0,08" → es **$\alpha=0{,}08$** (error tipo 1).

> [!example] ¿Qué error se comete? — P2-1b
> Si **no rechazás** $H_0$ → el error posible es **tipo 2 ($\beta$)**. Si **rechazás** → es **tipo 1 ($\alpha$)**. *(Ver [[#➕ ADICIONAL A — Los dos errores y la potencia (esto lo toman SÍ o SÍ)|Adicional A]].)*

> [!quote] Si estos 6 te salen solos, aprobás
> 🅰️ media resumida · 🅱️ dos proporciones · 🅲️ β con $S_b$ · 🅳️ IC/amplitud · 🅴️ errores. Son **exactamente** los que jamovi NO te resuelve de un click — y los que más alumnos dejan en blanco. El resto (t con datos, χ², regresión con tabla) sale por jamovi.

🔙 Volver: [[00 - GUÍA 2do Parcial]] · [[Hoja de Resumen - 2do Parcial]] · Practicar con esto: [[08 - Parciales ejemplo resueltos (oficial)]] · [[06 - TODOS los ejercicios resueltos]] · [[07 - Ejercicios de repaso resueltos]]
