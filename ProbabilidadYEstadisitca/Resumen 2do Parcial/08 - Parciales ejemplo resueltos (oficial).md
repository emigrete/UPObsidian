---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - ejercicios-resueltos
  - parcial-ejemplo
  - oficial
  - jamovi
tema: Parciales ejemplo de la cátedra (2do parcial '26) resueltos y explicados paso a paso
materia: Probabilidad y Estadística
parcial: 2
aliases:
  - "Parciales ejemplo resueltos"
  - "Ejemplo 2do parcial 26 resuelto"
---

# 🧪 Parciales ejemplo '26 — resueltos y explicados al 100%

> [!abstract] Qué es esta página
> Resolución **paso a paso, sin saltear nada** de los **dos parciales ejemplo** que dio la cátedra:
> - 📄 Enunciados: [[Ejemplo 2do parcial '26 Ma.pdf|Ejemplo 2do parcial '26]]
> - ✅ Respuestas oficiales: [[Respuestas ejemplos 2do parcial '26 Ma.pdf|Respuestas oficiales]]
>
> Para **cada inciso** vas a tener: cómo **identificar la prueba**, el **planteo de $H_0/H_1$**, la **cuenta hecha a mano** (estadístico + p-valor), la **decisión** con la regla universal $\boxed{pv<\alpha \Rightarrow \text{Rechazo }H_0}$ y la **conclusión redactada**. Al final de cada inciso marco el **resultado oficial** para que te verifiques.
>
> Teoría de respaldo: [[01 - Práctico 6 - Inferencia para una población|P6]] · [[02 - Práctico 7 - Comparación de dos muestras|P7]] · [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]] · [[04 - Práctico 9 - Regresión y correlación|P9]] · [[05 - Formulario y errores típicos|Formulario]] · [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|Árbol de decisión]].

> [!tip] Cómo estudiar con esto
> Tapá las soluciones, hacé cada parcial **entero** (con la [[Hoja de Resumen - 2do Parcial]] y jamovi al lado) y recién después comparate. Lo que más se evalúa es **elegir bien la prueba** y **redactar la conclusión con el $\alpha$** — no solo el número.

> [!check] Verificación de los números
> Todos los p-valores y estadísticos de abajo **los recalculé a mano** y **coinciden con las respuestas oficiales** (te aviso inciso por inciso). Donde la respuesta oficial tiene una errata de tipeo, lo señalo en un callout.

---
---

# 🧠 ANTES DE EMPEZAR — Cómo pensar CUALQUIER ejercicio (de cero)

> [!abstract] Leé esto primero si "no entendés nada"
> Acá no van las respuestas, va **cómo se piensa**. La clave que casi nadie te dice:
> **todos los ejercicios de este parcial son EL MISMO ejercicio.** Cambia el disfraz (química, perfumes, maestrías), pero abajo siempre está la misma maquinaria de 6 pasos. Lo único que de verdad tenés que *decidir* es **un solo paso**: qué prueba usar. Todo lo demás es mecánico.

## 1. El esqueleto que se repite SIEMPRE

Cuando agarrás cualquier inciso, tené en la cabeza este molde vacío esperando que lo llenes:

```
1. ¿Qué me preguntan?          → leer el enunciado
2. ¿Qué prueba es?             → EL ÁRBOL (lo único que se decide)
3. H0 y H1                     → mecánico una vez sabés la prueba
4. Cuenta: estadístico + pv    → fórmula fija de esa prueba
5. Regla universal: pv < α ⇒ Rechazo H0
6. Conclusión en palabras del problema
```

> [!warning] El paso 5 NUNCA cambia
> $$pv<\alpha \Rightarrow \text{Rechazo }H_0 \qquad\qquad pv>\alpha \Rightarrow \text{NO Rechazo }H_0$$
> Literalmente jamás cambia. Memorizá esto y ya tenés un tercio del parcial resuelto. Todo el "arte" se reduce al **paso 2**.

## 2. Cómo leo el enunciado: las 3 preguntas

Antes de calcular **nada**, hacele al enunciado **tres preguntas en orden**. Las respuestas te llevan solas por el árbol.

> [!question] Pregunta 1 — ¿cuántos grupos / poblaciones hay?
> - **Un solo grupo** de mediciones → rama *UNA población* ([[01 - Práctico 6 - Inferencia para una población|P6]])
> - **Dos grupos** que comparo → rama *DOS poblaciones* ([[02 - Práctico 7 - Comparación de dos muestras|P7]])
> - **Una tabla de conteos / categorías** → rama *categóricos / Chi-cuadrado* ([[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]])
> - **Dos variables numéricas** que se relacionan ($x$ e $y$) → rama *regresión* ([[04 - Práctico 9 - Regresión y correlación|P9]])

> [!question] Pregunta 2 — ¿qué tipo de dato es?
> - ¿Un número que se promedia (ms, días, edad, puntaje)? → es una **media**
> - ¿Un porcentaje / "X de cada N" / proporción? → es una **proporción**
> - ¿Categorías que se cuentan (Chrome/Firefox, dorado/relieve)? → **categórico (χ²)**

> [!question] Pregunta 3 — ¿qué me piden hacer?
> - ¿"Probar / demostrar / verificar si…"? → **prueba de hipótesis**
> - ¿"Estimar / dar un intervalo / con confianza del X%"? → **intervalo de confianza (IC)**
> - ¿"Interpretar el coeficiente / qué % se explica"? → **interpretación** (regresión)

Con esas 3 respuestas caés en una sola hoja del árbol.

## 3. El árbol de decisión, narrado

```
¿Qué quiero analizar?
│
├── UNA población  → P6
│   ├── Media (σ desconocido, que es SIEMPRE el caso) ...... PRUEBA t DE UNA MUESTRA  (ν = n−1)
│   └── Proporción / porcentaje (% de éxitos) .............. PRUEBA z PARA PROPORCIÓN
│
├── DOS poblaciones → P7   ← acá está la trampa más común
│   ├── Medias, DOS grupos DISTINTOS de individuos ......... t MUESTRAS INDEPENDIENTES
│   │      (primero test de Levene; si varianzas ≠ → Welch)
│   ├── Medias, MISMO individuo medido 2 veces ............. t MUESTRAS APAREADAS
│   │      (antes/después, pre-post, test-retest)
│   └── Proporciones de dos grupos ........................ z DIFERENCIA DE PROPORCIONES
│
├── DATOS CATEGÓRICOS / conteos en tablas → P8 (Chi-cuadrado χ²)
│   ├── ¿Una muestra sigue proporciones dadas/históricas? .. χ² BONDAD DE AJUSTE   (ν = c−1)
│   └── ¿Dos variables relacionadas? (tabla cruzada) ....... χ² INDEPENDENCIA/HOMOGENEIDAD
│          (ν = (filas−1)(columnas−1))
│
└── DOS variables CUANTITATIVAS relacionadas → P9
    └── Recta ŷ = a + bx · correlación r, r² · PH para β
```

> [!important] Son SOLO 6 pruebas posibles
> El parcial entero es elegir bien entre estas seis y después ejecutar la receta. Nada más.

## 4. Cómo armo H0 y H1 (esto confunde a todos)

> [!tip] La regla de oro de las hipótesis
> **Lo que el enunciado quiere demostrar va SIEMPRE en $H_1$.** $H_0$ es la negación, y $H_0$ *siempre* se queda con el igual ($=$, $\le$ o $\ge$).

Y el **signo de $H_1$ te dice la cola** (cómo se calcula el p-valor):

| Palabra en el enunciado | Signo de $H_1$ | Cola |
|---|---|---|
| "supera / es mayor / aumenta" | $>$ | unilateral **derecha** |
| "es menor / disminuye / reduce" | $<$ | unilateral **izquierda** |
| "difiere / cambió / es distinto" | $\ne$ | **bilateral** → el p-valor se multiplica **×2** |

## 5. Ejemplo narrado palabra por palabra (P1·1a)

> *"Rapidez de acción (ms), distribución normal. Solución 1: $\bar x=10{,}45$, $s=0{,}2309$, $n=12$. ¿La rapidez media de la Solución 1 supera 10,3 ms? ($\alpha=0{,}07$)"*

1. **Leo "rapidez media de la Solución 1"** → me hablan de **una sola** solución → *Pregunta 1: un solo grupo* → rama P6.
2. **Leo "rapidez (ms)"** → número que se promedia → *Pregunta 2: es una media*.
3. **Me dan `s`, no `σ`** → `s` es el desvío de la *muestra* ⇒ el de la población es **desconocido** ⇒ la prueba es **t** (la `z` solo se usaría con `σ` conocido, que casi nunca pasa).
4. **Leo "¿supera 10,3?"** → me piden **probar algo** → *Pregunta 3: prueba de hipótesis*.

→ **Prueba t de una muestra**, $\nu=n-1=11$. *Eso era lo difícil; el resto es receta.*

**Hipótesis:** quiere demostrar que "supera" (`>`) ⇒ va en $H_1$:
$$H_0:\ \mu\le 10{,}3 \qquad H_1:\ \mu>10{,}3 \quad(\text{cola derecha})$$

**Cuenta** (fórmula fija de la t de una muestra — "¿a cuántos desvíos está mi promedio del valor testeado?"):
$$t_m=\frac{\bar x-\mu_0}{s/\sqrt n}=\frac{10{,}45-10{,}3}{0{,}2309/\sqrt{12}}\approx 2{,}25 \;\Rightarrow\; pv\approx0{,}0229$$

**Decisión:** $0{,}0229<0{,}07 \Rightarrow$ Rechazo $H_0$ → me quedo con $H_1$ → *"sí, la rapidez media supera 10,3 ms, con 7% de error"*.

> [!note] Lo importante de este ejemplo
> De los 6 pasos, **5 fueron mecánicos** y solo el paso 2 (elegir "t de una muestra") requirió pensar. Así es TODO el parcial.

## 6. La sub-decisión fina: independientes vs apareadas vs Welch

Cuando hay **dos grupos** (P7), antes de calcular preguntate:

- ¿Son **individuos distintos** en cada grupo? (Solución 1 vs Solución 2, Ingeniería vs Económicas) → **t independientes**
- ¿Es el **mismo individuo medido dos veces**? (la misma planta *antes* y *después*) → **t apareadas**

Y en **independientes** hay un paso previo obligatorio: el **test de Levene** decide la dispersión.
- Levene $pv>\alpha$ → varianzas iguales → **t de Student**
- Levene $pv<\alpha$ → varianzas distintas → **t de Welch**

> [!tip] Detectar Welch sin que te lo digan (como en P1·1c)
> Dos pistas: (1) los desvíos son **muy distintos** (uno dispersa más del doble que el otro), y (2) los grados de libertad **no dan** $n_1+n_2-2$. Si el enunciado da un $\nu$ que no cierra con esa cuenta → se usó **Welch**.

## 7. Las palabras que delatan la prueba

| Lo que dice el enunciado | Prueba |
|---|---|
| "antes y después", "pre/post", "el mismo grupo medido 2 veces" | t **apareadas** |
| "dos sucursales", "hombres vs mujeres", "solución 1 vs 2" (grupos distintos) | t **independientes** |
| "porcentaje / proporción / % de…", **uno solo** | z **una proporción** |
| "% de un grupo vs % de otro" | z **diferencia de proporciones** |
| tabla cruzando dos variables: "¿depende de? ¿se relaciona? ¿difiere según?" | χ² **independencia/homogeneidad** |
| "¿sigue las proporciones históricas / es uniforme / sigue tal distribución?" | χ² **bondad de ajuste** |
| "estimar y en función de x", "recta", "asociación lineal" | **regresión** |
| "estimar… con confianza del X%" | **intervalo de confianza** (no es PH) |

> [!success] El resumen que tenés que poder recitar dormido
> 1. **Todo es el mismo esqueleto de 6 pasos.**
> 2. Lo único que se *decide* es **qué prueba** (paso 2), y sale de **3 preguntas**: ¿cuántos grupos?, ¿media/proporción/categoría?, ¿probar o estimar?
> 3. **$H_1$ = lo que querés demostrar.** $H_0$ se queda con el $=$.
> 4. El signo de $H_1$ ($>$, $<$, $\ne$) te da la cola ($\ne$ = bilateral = ×2 el pv).
> 5. La cuenta es una **fórmula fija por prueba** (no hay creatividad).
> 6. **$pv<\alpha \Rightarrow$ Rechazo $H_0$.** Siempre, sin excepciones.
> 7. Conclusión **en palabras del problema**.

> [!tip] Ahora sí: aplicá esto a cada parcial de abajo
> En **cada inciso** vas a ver un bloque plegable **"Cómo leí el enunciado"** (tocá la flecha para abrirlo): ahí está el monólogo interno completo — las 3 preguntas aplicadas a *ese* enunciado y por qué cae en *esa* prueba. Es el paso 2 de este método, ejercicio por ejercicio. Después viene la cuenta y la conclusión.

---
---

# 📄 PARCIAL 1

---

## 🟦 P1 · Ejercicio 1 — Dos soluciones químicas (semiconductores)

> [!info] Datos
> Rapidez de acción (ms), distribución **normal**:
> - **Solución 1:** $\bar{x}_1=10{,}45$ · $s_1=0{,}2309$ · $n_1=12$
> - **Solución 2:** $\bar{x}_2=9{,}95$ · $s_2=0{,}5169$ · $n_2=10$

### a) ¿La rapidez media de la Solución 1 supera 10,3 ms? ($\alpha=0{,}07$)

> [!question]- Cómo leí el enunciado (las 3 preguntas → árbol)
> - **P1 ¿cuántos grupos?** Dice *"la rapidez media de **la Solución 1**"* → me habla de **un solo** grupo → rama *UNA población* (P6).
> - **P2 ¿qué dato?** "Rapidez (ms)" es un número que se promedia → es una **media** (no un porcentaje).
> - **¿σ o s?** Me dan $s_1=0{,}2309$ (desvío de la **muestra**) ⇒ el de la población es **desconocido** ⇒ media + σ desconocido → la prueba es **t** (la *z* solo iría con σ conocido).
> - **P3 ¿qué piden?** *"¿supera 10,3?"* → me piden **probar algo** → prueba de hipótesis.
> - **Cola:** "supera" = "es mayor" → $H_1$ con `>` → **unilateral derecha**.
>
> → cae en **t de una muestra**, $\nu=n_1-1=11$.

**Identifico:** **una sola población** (solución 1), variable **cuantitativa** (rapidez), $\sigma$ **desconocido**, $n$ chico → **prueba t de una muestra**, $\nu=n_1-1=11$. Ver [[01 - Práctico 6 - Inferencia para una población|P6]].

**Hipótesis** — "supera los 10,3" es lo que se quiere probar ⇒ va en $H_1$ con `>` (cola derecha):
$$H_0:\ \mu_1\le 10{,}3 \qquad H_1:\ \mu_1>10{,}3$$

**Estadístico:**
$$t_m=\frac{\bar x_1-\mu_0}{s_1/\sqrt{n_1}}=\frac{10{,}45-10{,}3}{0{,}2309/\sqrt{12}}=\frac{0{,}15}{0{,}06666}\approx 2{,}25$$

**p-valor** (cola derecha, módulo *distrACTION → T-Distribution* o tabla):
$$pv=P(T_{11}>2{,}25)\approx 0{,}0229$$

> [!success] Conclusión
> $0{,}0229<0{,}07 \Rightarrow$ **Rechazo $H_0$**. Hay evidencias suficientes para concluir que la rapidez media de acción de la **Solución 1 supera los 10,3 ms**, asumiendo un error del 7%.
> **✅ Oficial:** $H_1:\mu>10{,}3$ · $pv=0{,}0229$ → se rechaza. ✔️

### b) IC del 96% para la rapidez media de la Solución 2

> [!question]- Cómo leí el enunciado
> - **P3 es la que manda acá:** dice *"**IC del 96%**"* / "estimar la media" → **NO es prueba de hipótesis, es un intervalo de confianza**. No hay $H_0/H_1$ ni p-valor, solo construir el intervalo.
> - **P1/P2:** sigue siendo **una** media (Solución 2), σ desconocido → el IC se arma con **t**, $\nu=n_2-1=9$.
> - **El dato que importa de la consigna:** "96%" es la **confianza** $1-\alpha$ ⇒ $\alpha=0{,}04$ ⇒ como el IC reparte el error en las dos colas, busco $t$ con $\alpha/2=0{,}02$ ⇒ $t_{0{,}98;\,9}$.

**Identifico:** estimación por intervalo de una **media**, $\sigma$ desconocido → IC con **t**, $\nu=n_2-1=9$. Confianza $1-\alpha=0{,}96 \Rightarrow \alpha/2=0{,}02 \Rightarrow t_{0{,}98;\,9}\approx \mathbf{2{,}398}$ (lo que da jamovi: distrACTION → t → quantile con $p=0{,}98$, $df=9$).

$$EM=t_{0{,}98;\,9}\cdot\frac{s_2}{\sqrt{n_2}}=2{,}398\cdot\frac{0{,}5169}{\sqrt{10}}=2{,}398\cdot 0{,}16346\approx 0{,}392$$
$$IC_{0{,}96}=\bar x_2\pm EM = 9{,}95 \pm 0{,}392 = (9{,}558\,;\ 10{,}342)$$

> [!success] Interpretación
> Con una **confianza del 96%** se estima que la **rapidez media de reacción de la Solución 2** está entre **9,558 ms y 10,342 ms**.

> [!warning] ⚠️ Ojo con el solucionario oficial acá
> El solucionario de la cátedra da $IC_{0{,}96}=(9{,}5802;\ 10{,}3198)$, que sale de usar $t\approx 2{,}262$. **Pero 2,262 es $t_{0{,}975;\,9}$, el multiplicador del IC del 95%, NO del 96%.** Parece un desliz del solucionario (usó el t del 95% para una confianza del 96%).
> **El valor correcto para 96% es $t_{0{,}98;\,9}=2{,}398$** (el que te da jamovi), y el intervalo correcto es $(9{,}558;\ 10{,}342)$ — un poco más ancho, como tiene que ser al pedir más confianza.
> 👉 En el parcial, usá **2,398**. Si te corrigen con la planilla oficial y marcan el suyo, mostrá la cuenta: 96% ⇒ α/2=0,02 ⇒ $t_{0,98;9}$.

### c) ¿Difieren ambas soluciones? ($|t_m|=2{,}8324$, $\nu=12$, $\alpha=0{,}03$)

> [!abstract]- 🐣 Explicado de cero (en criollo, sin fórmulas)
> Este inciso es **distinto** a los demás: no te dan datos para cargar en jamovi. Te dan ya hecho el estadístico ($t_m=2{,}8324$) y los grados de libertad ($\nu=12$), y te piden dos cosas: **decir qué prueba es** (justificando) y **concluir**.
>
> **Qué prueba es:**
> - Hay dos grupos: Solución 1 y Solución 2 → dos poblaciones.
> - Son obleas distintas en cada solución (no la misma medida dos veces) → muestras **independientes**.
> - Comparás rapidez, que es un promedio → es una prueba **t**.
> - → **t para diferencia de medias, muestras independientes.**
>
> **La cola:**
> - El enunciado dice "difiere" = distinto en cualquier sentido → es **bilateral**.
> - $H_0$: las medias son iguales. $H_1$: las medias son distintas.
> - Que sea bilateral importa porque el **p-valor se multiplica por 2**.
>
> **El detalle del $\nu=12$ (lo más importante del ejercicio):**
> - En t independientes, antes va el test de **Levene**, que mira si las varianzas (la dispersión) de los dos grupos son iguales o distintas.
> - Si fueran **iguales** → t de **Student**, y los grados de libertad serían $n_1+n_2-2 = 12+10-2 = 20$.
> - Pero el enunciado dice $\nu=12$, **no 20** → entonces NO se usó Student, se usó la t de **Welch** (la versión para varianzas distintas).
> - Tiene sentido: $s_1=0{,}23$ y $s_2=0{,}52$, la Solución 2 dispersa más del doble → varianzas distintas → Welch.
> - **Resumen: el $\nu=12$ es la pista de que las varianzas eran distintas y se usó Welch.**
>
> **La conclusión:**
> - Con $t_m=2{,}8324$ y $\nu=12$ sacás el p-valor (jamovi distrACTION o tabla).
> - Como es bilateral, ×2: $p\text{-valor}=0{,}0152$.
> - Regla: $0{,}0152 < 0{,}03 \Rightarrow$ se rechaza $H_0$.
> - En palabras: sí, hay evidencia de que la rapidez de las dos soluciones es distinta, con 3% de error.
>
> **Lo que te tenés que llevar:**
> 1. Es del tipo "te doy el estadístico, vos concluí": solo sacás el p-valor y comparás con α.
> 2. "Difiere" = bilateral = p-valor ×2. Si te olvidás el ×2, te da 0,0076 y podés concluir mal.
> 3. El $\nu=12$ no es decorativo: comparado con 20 (que sería Student) te avisa que se usó **Welch** porque las varianzas eran distintas.

> [!question]- Cómo leí el enunciado (acá cambia la rama)
> - **P1 ¿cuántos grupos?** Ahora dice *"¿difieren **ambas** soluciones?"* → son **DOS** grupos (Solución 1 *y* Solución 2) → ya no es P6, salto a la rama *DOS poblaciones* (P7).
> - **Sub-decisión clave de P7:** ¿son los **mismos** individuos o **distintos**? La Solución 1 se probó en unas obleas y la 2 en otras → **individuos distintos** → **t independientes** (si fuera la misma oblea medida con las dos soluciones, sería apareada).
> - **P2:** comparo **medias** → t (no proporciones).
> - **Cola:** "difieren" = "son distintas en cualquier dirección" → $H_1$ con `≠` → **bilateral** (p-valor ×2).
> - **Paso extra de independientes:** antes de la t va **Levene** para decidir Student vs Welch (ver abajo).

**Identifico:** se comparan las **medias de dos grupos distintos** (Solución 1 vs Solución 2, obleas diferentes) → **prueba t para diferencia de medias con muestras INDEPENDIENTES** (variable cuantitativa, $\sigma$ desconocidos). Ver [[02 - Práctico 7 - Comparación de dos muestras|P7]].

**"difiere significativamente"** ⇒ prueba **bilateral**:
$$H_0:\ \mu_{1}=\mu_{2} \qquad H_1:\ \mu_{1}\ne\mu_{2}$$

> [!note] Paso previo obligatorio en independientes: homogeneidad de varianzas (Levene)
> Antes de la t hay que decidir Student vs Welch con el test de **Levene**:
> $$H_0:\ \sigma^2_1=\sigma^2_2 \qquad H_1:\ \sigma^2_1\ne\sigma^2_2$$
> Si Levene da $pv>\alpha$ → varianzas iguales → **t de Student**. Si $pv<\alpha$ → varianzas distintas → **t de Welch**.

> [!tip] Detalle fino: el $\nu=12$ delata que se usó **Welch**
> Acá $s_1=0{,}2309$ y $s_2=0{,}5169$ son **muy distintas** (la 2 dispersa más del doble). Si fuera Student, los grados de libertad serían $n_1+n_2-2=20$. Como el enunciado dice $\nu=12$, se aplicó **Welch** (varianzas desiguales). Lo comprobé con el estadístico de Welch:
> $$t_m=\frac{\bar x_1-\bar x_2}{\sqrt{\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2}}}=\frac{10{,}45-9{,}95}{\sqrt{\frac{0{,}2309^2}{12}+\frac{0{,}5169^2}{10}}}=\frac{0{,}5}{\sqrt{0{,}004443+0{,}026719}}=\frac{0{,}5}{0{,}1765}\approx 2{,}832$$
> ✔️ coincide con el $|t_m|=2{,}8324$ del enunciado, y los grados de libertad de Welch dan $\approx 12$. Es decir: **Levene dio significativo (varianzas distintas) → Welch**.

**p-valor** (bilateral):
$$pv=2\cdot P(T_{12}>2{,}8324)\approx 2\cdot 0{,}0076\approx 0{,}0152$$

> [!success] Conclusión
> $0{,}0152<0{,}03 \Rightarrow$ **Rechazo $H_0$**. Hay evidencias suficientes para concluir que la **rapidez media de acción de ambas soluciones difiere**, asumiendo un 3% de error.
> **✅ Oficial:** prueba de diferencia de medias independientes · $H_0:\mu_{s1}=\mu_{s2}$, $H_1:\mu_{s1}\ne\mu_{s2}$ · $pv=0{,}0152$ → se rechaza. ✔️

> [!warning] Errata en la respuesta oficial
> En la hoja oficial figura *"H0: $\sigma^2_{s1} \ne \sigma^2_{s2}$ ; H1: $\sigma^2_{s1} \ne \sigma^2_{s2}$"* (las dos con $\ne$): es un **error de tipeo**. La $H_0$ de Levene siempre es de **igualdad**: $H_0:\sigma^2_{s1}=\sigma^2_{s2}$ vs $H_1:\sigma^2_{s1}\ne\sigma^2_{s2}$.

---

## 🟧 P1 · Ejercicio 2 — Preferencia de envase de perfume según edad (χ²)

> [!info] Datos
> Dos variables categóricas: **diseño** (línea dorada / línea relieve / clásica → **3 categorías**) y **edad** (20-30 / 30-45 / 45-60 / más de 60 → **4 categorías**) → tabla **3×4**. Estadístico muestral $\chi^2_m=18{,}34$. $\alpha=0{,}01$.

> [!question]- Cómo leí el enunciado
> - **P2 ¿qué dato?** No hay números que se promedien: hay **categorías** ("diseño" y "edad") y lo que tengo son **conteos** en una tabla → rama *categóricos / χ²* (P8). Ese es el disparador: tabla de conteos = Chi-cuadrado, casi seguro.
> - **¿Cuál de los χ²?** Hay **DOS variables** cruzadas (diseño × edad) en una tabla → es de **independencia/homogeneidad** (no de bondad de ajuste, que sería una sola variable contra porcentajes fijos).
> - **Pista del verbo:** *"¿difiere la preferencia **según** la edad?"* → comparo la misma variable en varios grupos de edad → la cátedra lo llama **homogeneidad** (operativamente, idéntico a independencia).
> - **gl:** tabla 3×4 → $\nu=(f-1)(c-1)=(3-1)(4-1)=6$.
> - **Cola:** χ² es **SIEMPRE unilateral derecha** (nunca se multiplica ×2).

**Identifico:** datos **categóricos**, se pregunta si la preferencia **difiere según la edad** → **prueba χ²**. Ver [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].

> [!note] ¿Independencia u homogeneidad?
> La respuesta oficial la llama **prueba de homogeneidad** (se mira si la distribución de preferencias es la **misma** en los distintos grupos de edad, tomados como poblaciones). Operativamente da **lo mismo** que independencia: misma fórmula, mismos grados de libertad y misma cola. Lo que **no** podés equivocar son los **gl** y que es **unilateral derecha**.

**Hipótesis:**
$$H_0:\ \text{no hay diferencias en la preferencia del envase según la edad}$$
$$H_1:\ \text{hay diferencias en la preferencia del envase según la edad}$$

**Grados de libertad:** $\nu=(\text{filas}-1)(\text{columnas}-1)=(3-1)(4-1)=2\cdot 3=\mathbf{6}$.

**p-valor** (χ² **siempre** cola derecha):
$$pv=P(\chi^2_{6}>18{,}34)\approx 0{,}00544$$

> [!success] Conclusión
> $0{,}00544<0{,}01 \Rightarrow$ **Rechazo $H_0$**. Los datos muestran evidencias para concluir que **existe diferencia en la preferencia del envase según la edad** de la mujer, con un nivel de significación del 1%.
> **✅ Oficial:** prueba de homogeneidad · $pv=P(\chi^2>18{,}34)=0{,}00544$ → se rechaza. ✔️

---

## 🟥 P1 · Ejercicio 3 — Tamaño del equipo vs innovación (regresión)

> [!info] Datos
> $x=$ tamaño del equipo (entre **3 y 7** empleados), $y=$ nivel de innovación (escala **20 a 100**), $n=10$.
> Recta: $\ \hat y = 88{,}5 + 0{,}23\,x\ $, con $r=0{,}667$.

> [!question]- Cómo leí el enunciado
> - **P1/P2 juntas:** hay **dos variables cuantitativas** ($x=$ tamaño, $y=$ innovación) y me dan una **recta** $\hat y=a+bx$ con su $r$ → eso es la firma inequívoca de la rama *regresión* (P9).
> - **Palabra clave:** "estimar $y$ en función de $x$", "recta", "asociación lineal" → regresión y correlación.
> - **Qué se pide en cada inciso:** a) *interpretar* la ordenada $a$ y la pendiente $b$ (no se calcula nada, se explica en palabras); b) *probar* si hay asociación → PH para la pendiente $\beta$; c) calcular $r^2$.

### a) Interpretar los coeficientes

- **Ordenada $a=88{,}5$:** sería el nivel de innovación estimado para un equipo de $x=0$ empleados. Pero la variable va de **3 a 7**, así que $x=0$ está **fuera del rango** → **no tiene interpretación práctica** (no existe un equipo de 0 personas).
- **Pendiente $b=0{,}23$:** por **cada empleado más** en el equipo, el nivel de innovación **aumenta 0,23 puntos en promedio**.

### b) ¿Hay asociación lineal? ($S_b=0{,}108$, $\alpha=0{,}05$)

> [!question]- Cómo lo razoné
> - *"¿Hay asociación lineal?"* en regresión se traduce **siempre** a una pregunta sobre la pendiente: si la recta no tiene pendiente ($\beta=0$) entonces $y$ no depende de $x$. Por eso la prueba es sobre $\beta$.
> - **$H_0$ = "no hay efecto"** (la regla general) → $\beta=0$. **$H_1$** = lo que quiero ver = sí hay asociación → $\beta\ne 0$ (**bilateral**, porque "asociación" no dice si sube o baja).
> - **gl de regresión:** siempre $\nu=n-2$ (acá $10-2=8$). El estadístico es $t_m=b/S_b$ (pendiente sobre su error estándar).

**Prueba t para el coeficiente de regresión** $\beta$, $\nu=n-2=8$. Ver [[04 - Práctico 9 - Regresión y correlación|P9]].
$$H_0:\ \beta=0\ (\text{no hay asociación lineal}) \qquad H_1:\ \beta\ne 0\ (\text{sí hay})$$
$$t_m=\frac{b}{S_b}=\frac{0{,}23}{0{,}108}\approx 2{,}1296 \qquad pv=2\cdot P(T_8>2{,}1296)\approx 0{,}066$$

> [!failure] Conclusión
> $0{,}066>0{,}05 \Rightarrow$ **NO Rechazo $H_0$**. **No** hay evidencias suficientes para concluir que exista asociación lineal entre el tamaño del equipo y la innovación, con un error del 5%.
> **✅ Oficial:** $t_m=2{,}1296$ · $pv=0{,}066$ → no se rechaza. ✔️

### c) Coeficiente de determinación

$$r^2=0{,}667^2\approx 0{,}449$$

> [!note] Interpretación
> El **44,9%** de la variación del nivel de innovación se explica por la variación en el tamaño del equipo. Es un $r^2$ **bajo** y además la prueba de $\beta$ no fue significativa → **no es un buen modelo** para hacer pronósticos.
> **✅ Oficial:** $r^2=44{,}9\%$, no es buen modelo. ✔️

---

## 🟪 P1 · Ejercicio 4 — Verdadero/Falso (justificar)

> [!question]- Cómo se encaran los V/F
> Estos no tienen árbol ni cuenta: son **conceptuales**. La estrategia es detectar **qué concepto te están tocando** y contrastarlo con la definición exacta:
> - (a) toca la relación **confianza ↔ amplitud del IC**: más confianza → valor crítico más grande → EM más grande → intervalo más ancho.
> - (b) toca **cómo se arma la hipótesis**: la trampa clásica es meter el **valor muestral** ($\hat p$) en $H_1$ en vez del **valor afirmado**.
> - (c) toca la **tabla de errores**: hay que distinguir error tipo I, tipo II y potencia leyendo *qué decisión* con *qué estado real de $H_0$*.

### a) "Con un IC del 80% el error muestral (EM) es menor que con uno del 90%." → **VERDADERO** ✅
A **menor** nivel de confianza, **menor** valor crítico $\Rightarrow$ **menor EM** $\Rightarrow$ intervalo más angosto (más preciso). Como $80\%<90\%$, el EM del 80% es **menor**. (Se gana precisión a costa de confianza.)
**✅ Oficial:** Verdadero. ✔️

### b) "50 de 100 comercios con pérdidas; se sospecha que el % supera 45% ⇒ $H_1: p>0{,}50$." → **FALSO** ❌
El **0,50** es el **valor muestral** $\hat p=50/100$, **no** el valor de la hipótesis. La hipótesis se arma con el valor **afirmado/sospechado (0,45)**:
$$H_0:\ p\le 0{,}45 \qquad H_1:\ p>0{,}45$$
**✅ Oficial:** Falso, 0,50 es el valor muestral, $H_1:p>0{,}45$. ✔️

### c) "La probabilidad de rechazar $H_0$ siendo $H_0$ falsa es el error tipo I." → **FALSO** ❌
Rechazar $H_0$ **cuando es falsa** es una **decisión correcta** → eso es la **potencia** $(1-\beta)$. El **error tipo I** es rechazar $H_0$ **siendo verdadera**.

> [!info] Mini-tabla de decisiones
> | | $H_0$ verdadera | $H_0$ falsa |
> |---|---|---|
> | **No rechazo $H_0$** | ✅ correcta | ❌ Error tipo II ($\beta$) |
> | **Rechazo $H_0$** | ❌ Error tipo I ($\alpha$) | ✅ correcta = **potencia** $(1-\beta)$ |
>
> **✅ Oficial:** Falso, es la potencia de la prueba. ✔️

---
---

# 📄 PARCIAL 2

---

## 🟦 P2 · Ejercicio 1 — Plan de seguridad: ausencias antes/después (apareadas)

> [!info] Datos
> **8 plantas** industriales: se mide los días de ausencia **antes** y **después** del plan de seguridad → se busca ver si el plan **redujo** las ausencias.

### a) Hipótesis y prueba

> [!question]- Cómo leí el enunciado (la trampa apareadas vs independientes)
> - **P1 ¿cuántos grupos?** Hay dos conjuntos de números (antes y después) → parece P7, dos muestras.
> - **Sub-decisión clave:** ¿distintos individuos o el mismo dos veces? Leo *"las **mismas 8 plantas**, antes y después"* → es el **mismo individuo medido dos veces** → **APAREADAS** (¡NO independientes!). El detector: aparecen las palabras *antes/después, pre-post,* y el $n$ es **igual** en los dos momentos.
> - **P2:** comparo **medias** de una diferencia → t.
> - **El truco de apareadas:** se trabaja con **una sola** variable nueva, $d=\text{antes}-\text{después}$, y la prueba se vuelve una t "de una muestra" sobre esa $d$.
> - **Cola:** "el plan **redujo** las ausencias" ⇒ antes > después ⇒ $\mu_d>0$ → $H_1$ con `>` → **unilateral derecha**.

> [!warning] La clave: son las **MISMAS 8 plantas** medidas dos veces → APAREADAS
> No son grupos distintos: a cada planta se le mide *antes* y *después* (dos medidas del mismo individuo) → **prueba t para muestras apareadas**. Definimos la diferencia $d=\text{antes}-\text{después}$. Ver [[02 - Práctico 7 - Comparación de dos muestras|P7]].

**Hipótesis de investigación:** si el plan fue exitoso, las ausencias **disminuyeron** ⇒ en promedio *antes > después* ⇒ $\mu_d>0$.
$$H_0:\ \mu_d\le 0 \qquad H_1:\ \mu_d>0 \quad (d=\text{antes}-\text{después})$$

### b) Conclusión ($\alpha=0{,}05$) y error posible

> [!note] Leyendo el enunciado
> "El mínimo nivel de significación es 0,01" significa que el **p-valor ronda 0,01** (el menor $\alpha$ con el que se rechazaría). Se nos pide concluir asumiendo un **error del 5%** → comparamos contra $\alpha=0{,}05$.

$$pv\approx 0{,}01<0{,}05 \Rightarrow \textbf{Rechazo }H_0$$

> [!success] Conclusión
> Hay evidencias para concluir que el **plan de seguridad fue exitoso** (redujo los días de ausencia), con una probabilidad de error del 5%.
> **Error posible:** como se **rechazó** $H_0$, el riesgo es de **error tipo I** → concluir que el plan fue exitoso cuando en realidad **no** disminuyó las ausencias.
> **✅ Oficial:** prueba t apareada · se rechaza al 5% · error tipo I. ✔️

---

## 🟧 P2 · Ejercicio 2 — Compras internacionales vs edad (regresión)

> [!info] Datos
> $x=$ edad del comprador (20 a 60 años), $y=$ compras anuales internacionales, $n=18$.
> Recta: $\ \hat y = 73{,}5 - 1{,}17\,x\ $, con $r^2=0{,}713$.

> [!question]- Cómo leí el enunciado
> - Otra vez **dos variables cuantitativas** ($x=$ edad, $y=$ compras) con **recta** dada → rama *regresión* (P9), idéntico al P1·3.
> - **Detalle nuevo:** la pendiente es **negativa** ($b=-1{,}17$). Eso cambia el signo de la correlación: $r=-\sqrt{r^2}$ (negativa). En el P1·3 era positiva; acá es **inversa**.
> - **Qué pide cada inciso:** a) interpretar $a$ y $b$; b) PH para $\beta$ (igual que antes: $H_0:\beta=0$); c) el % explicado, que es directamente $r^2$.

### a) Interpretar los coeficientes

- **Ordenada $a=73{,}5$:** correspondería a un comprador de $x=0$ años → **sin sentido práctico** (no hay compradores de 0 años; además está fuera del rango 20–60).
- **Pendiente $b=-1{,}17$:** por **cada año más** de edad del comprador, el número medio anual de compras internacionales **disminuye en 1,17** (el signo negativo indica relación inversa).

### b) Prueba de hipótesis para $\beta$

> [!info]- ¿Qué es $\beta$ y por qué se prueba si vale 0?
> - **$b=-1{,}17$** es la pendiente que sacaste de **tu muestra** (los 18 compradores). **$\beta$ (beta)** es la pendiente **verdadera de toda la población**, que nunca conocés y solo estimás con $b$. Mismo par parámetro/estimador de siempre: **$\beta$ es a la población lo que $b$ es a la muestra** (como $\mu$/$\bar x$ o $p$/$\hat p$).
> - **Por qué se prueba $\beta=0$:** si la pendiente real fuera **cero**, la recta sería **plana (horizontal)** → cambiar $x$ **no cambia** $y$ → **NO hay relación lineal**. Por eso *"¿hay asociación lineal?"* se traduce **siempre** a una pregunta sobre la pendiente:
>   - $H_0:\beta=0$ → recta plana → **no** hay relación.
>   - $H_1:\beta\ne0$ → recta inclinada → **sí** hay relación. (Bilateral: la inclinación puede ser positiva o negativa.)
> - **Acá:** $b=-1{,}17$ (lejos de 0) → se **rechaza $H_0$** → la pendiente real no es cero → **hay asociación lineal** (inversa, porque $b<0$).
> - **A mano:** $t_m=\dfrac{b}{S_b}$, $\nu=n-2$, $pv=2\,P(t>|t_m|)$. **En jamovi:** Regresión → Lineal; el **p-valor de la covariable ($x$) ES el p de $\beta$**.
> - 💡 En una frase: probar $\beta$ es preguntarle a la recta *"¿estás inclinada de verdad, o esa inclinación de la muestra es casualidad?"* Rechazar $H_0$ = la inclinación es real = hay relación entre $x$ e $y$.

$$H_0:\ \beta=0 \qquad H_1:\ \beta\ne 0 \qquad (\nu=n-2=16)$$
Como $b<0$, la correlación es **negativa**: $r=-\sqrt{r^2}=-\sqrt{0{,}713}\approx -0{,}844$ → **asociación lineal inversa fuerte**.

> [!success] Conclusión
> Se **Rechaza $H_0$** → **sí hay asociación lineal** (inversa) entre la edad y las compras internacionales. La fuerza de $r\approx-0{,}844$ y el $r^2=0{,}713$ respaldan esto.
> **✅ Oficial:** se rechaza, hay asociación lineal, $r=-0{,}844$ (fuerte e inversa). ✔️

### c) % de variación explicada

$$r^2=0{,}713 \Rightarrow \textbf{71{,}3\%}$$
El **71,3%** de la variación en la cantidad de compras internacionales anuales se explica por la **edad** del consumidor. ✔️ (Oficial: 71,3%.)

---

## 🟥 P2 · Ejercicio 3 — Maestría: decisión, errores, IC y diferencia de proporciones

> [!info] Contexto
> La Universidad abre la maestría **solo si los interesados superan 18**. Define:
> - $P(\text{no ofrecer} \mid \mu=19{,}5)=0{,}10$
> - $P(\text{ofrecer} \mid \mu=18)=0{,}08$

### a) Hipótesis e identificación de $\alpha$ y $\beta$

> [!question]- Cómo leí el enunciado (identificar α y β con la definición)
> - **Las hipótesis primero:** la acción que se quiere tomar ("abrir la maestría") va en $H_1$. "Abrir **solo si supera 18**" → $H_1:\mu>18$, $H_0:\mu\le18$. Entonces **rechazar $H_0$ = abrir**.
> - **El truco para α y β:** no los adivino, los **traduzco** desde las probabilidades que me dan, usando la definición:
>   - $\alpha=P(\text{rechazar }H_0\mid H_0\text{ verdadera})$. Rechazar = ofrecer; $H_0$ verdadera = $\mu=18$. El enunciado da $P(\text{ofrecer}\mid\mu=18)=0{,}08$ → **ese es $\alpha$**.
>   - $\beta=P(\text{no rechazar }H_0\mid H_0\text{ falsa})$. No rechazar = no ofrecer; $H_0$ falsa = $\mu=19{,}5$. El enunciado da $P(\text{no ofrecer}\mid\mu=19{,}5)=0{,}10$ → **ese es $\beta$**.
> - La gimnasia es: leé *qué decisión* (ofrecer/no ofrecer) y *con qué valor real de μ*, y matcheá contra la tabla de errores.

"Abrir solo si supera 18" ⇒ lo que hay que demostrar (abrir) va en $H_1$:
$$H_0:\ \mu\le 18 \qquad H_1:\ \mu>18 \qquad (\textbf{Rechazar }H_0 = \text{abrir la maestría})$$

> [!note] Identificar los errores con la definición
> - **$\alpha$ = error tipo I** = $P(\text{rechazar }H_0 \mid H_0\text{ verdadera}) = P(\text{ofrecer} \mid \mu=18) = \mathbf{0{,}08}$.
> - **$\beta$ = error tipo II** = $P(\text{no rechazar }H_0 \mid H_0\text{ falsa}) = P(\text{no ofrecer} \mid \mu=19{,}5) = \mathbf{0{,}10}$.
>
> **✅ Oficial:** $H_0:\mu\le 18$, $H_1:\mu>18$ · $\alpha=0{,}08$ · $\beta=0{,}10$. ✔️

### b) ¿Conviene abrir la inscripción? ($\bar x=19{,}444$, $s=3{,}779$, $n=9$ carreras)

> [!question]- Cómo lo razoné
> - Ahora me dan $\bar x$, $s$ y $n$ de **un solo** grupo y me piden **decidir** (abrir o no) → es la **t de una media** (idéntica al P1·1a), $\nu=n-1=8$.
> - Las hipótesis ya las armé en a): $H_0:\mu\le18$, $H_1:\mu>18$ → **unilateral derecha**.
> - El "error del 8%" del enunciado es el $\alpha$ contra el que comparo el p-valor (es el mismo $\alpha$ que identifiqué en a).

**Prueba t de una media**, $\nu=n-1=8$, "error del enunciado" $=\alpha=0{,}08$.
$$t_m=\frac{\bar x-\mu_0}{s/\sqrt n}=\frac{19{,}444-18}{3{,}779/\sqrt 9}=\frac{1{,}444}{1{,}2597}\approx 1{,}146$$
$$pv=P(T_8>1{,}146)\approx 0{,}1423$$

> [!failure] Conclusión
> $0{,}1423>0{,}08 \Rightarrow$ **NO Rechazo $H_0$**. **No** hay evidencias suficientes para aconsejar abrir la inscripción de la maestría, con una probabilidad de error del 8%.
> **Error posible:** como **no se rechazó**, el riesgo es de **error tipo II ($\beta$)** → no abrir la maestría cuando en realidad sí habría suficientes interesados.
> **✅ Oficial:** $pv=0{,}1423$ → no se aconseja abrir. ✔️

### c) IC del 92% para el número medio de interesados

> [!question]- Cómo lo razoné
> - *"IC del 92%"* → es **estimación por intervalo**, no PH (igual que el P1·1b): no hay $H_0/H_1$, solo construyo el intervalo.
> - Misma media, σ desconocido → IC con **t**, $\nu=n-1=8$.
> - "92%" es la confianza ⇒ $\alpha=0{,}08$ ⇒ reparto en dos colas ⇒ busco $t$ con $\alpha/2=0{,}04$ ⇒ $t_{0{,}96;\,8}$.

$\nu=8$, confianza $0{,}92 \Rightarrow \alpha/2=0{,}04 \Rightarrow t_{0{,}96;\,8}\approx 2{,}00$ (jamovi).
$$EM=t_{0{,}96;\,8}\cdot\frac{s}{\sqrt n}\approx 2{,}00\cdot\frac{3{,}779}{3}\approx 2{,}52$$
$$IC_{0{,}92}=19{,}444\pm 2{,}52 \approx (16{,}919\,;\ 21{,}969)$$

> [!success] Interpretación
> Con una **probabilidad del 92%** se estima que el número medio real de alumnos que seguiría la maestría está entre **16,919 y 21,969**.
> **✅ Oficial:** $IC_{0{,}92}=(16{,}919;\ 21{,}969)$. ✔️
>
> 💡 Notá que el IC **incluye valores por debajo de 18** → coherente con que en b) **no** se pudo afirmar que se supere 18.

### d) ¿Difieren los % de posgrado entre Ingeniería y Económicas? ($\alpha=0{,}08$)

> [!info] Datos
> - **Ingeniería:** $54$ de $120$ → $\hat p_I=0{,}45$
> - **Económicas:** $54\%$ de $200$ → $\hat p_E=0{,}54$ (es decir $108$ de $200$)

> [!question]- Cómo leí el enunciado
> - **P1 ¿cuántos grupos?** Dos: Ingeniería y Económicas → rama *DOS poblaciones* (P7).
> - **P2 ¿qué dato?** No son medias: son **"54 de 120"** y **"54% de 200"** → eso es **proporciones / porcentajes** → la prueba es **z para diferencia de dos proporciones** (no t).
> - **Ojo con el dato disfrazado:** Económicas viene como **54%** de 200; hay que pasarlo a conteo → $0{,}54\cdot200=108$. Y Ingeniería ya viene como conteo (54 de 120 → $\hat p_I=0{,}45$).
> - **Cola:** *"¿existen **diferencias**?"* → $H_1$ con `≠` → **bilateral** (p-valor ×2).
> - **Detalle de la fórmula:** en diferencia de proporciones se usa la **proporción combinada** $\hat p$ (juntando los éxitos de ambos grupos) para el denominador.

**Prueba z para diferencia de dos proporciones**, bilateral ("¿existen diferencias?"):
$$H_0:\ p_I=p_E \qquad H_1:\ p_I\ne p_E$$
Proporción combinada: $\hat p=\dfrac{54+108}{120+200}=\dfrac{162}{320}=0{,}50625$.
$$z_m=\frac{\hat p_I-\hat p_E}{\sqrt{\hat p(1-\hat p)\left(\frac1{n_I}+\frac1{n_E}\right)}}=\frac{0{,}45-0{,}54}{\sqrt{0{,}50625\cdot 0{,}49375\left(\frac1{120}+\frac1{200}\right)}}=\frac{-0{,}09}{0{,}0577}\approx -1{,}56$$
$$pv=2\cdot P(Z<-1{,}56)\approx 0{,}117$$

> [!failure] Conclusión
> $0{,}117>0{,}08 \Rightarrow$ **NO Rechazo $H_0$**. **No** hay evidencias de que existan diferencias en el porcentaje de egresados con estudios de posgrado entre Ingeniería y Económicas, asumiendo un 8% de error.
> **✅ Oficial:** $H_1:p_I\ne p_E$ · $pv=0{,}117$ → no se rechaza. ✔️
>
> **Ruta jamovi:** *Frecuencias → Muestras independientes (χ²)* o *Recuento → tildar "Prueba z para diferencia de 2 proporciones"*.

---

## 🟩 P2 · Ejercicio 4 — Distribución de navegadores (χ² bondad de ajuste)

> [!info] Datos · $n=200$, $\alpha=0{,}05$
> | Navegador | Chrome | Firefox | Edge | Otros |
> |---|---|---|---|---|
> | **Histórico** $p_i$ | 58% | 15% | 7% | **20%** (el resto) |
> | **Observado** $o_i$ | 110 | 19 | 14 | 57 |

> [!question]- Cómo leí el enunciado (cuál de los dos χ² es)
> - **P2 ¿qué dato?** Categorías (navegadores) con **conteos** → rama χ² (P8), igual que el P1·2.
> - **PERO acá es el OTRO χ²:** hay **una sola** variable (el navegador) y la comparo contra **porcentajes fijos dados** (los históricos 58/15/7/20) → **χ² de bondad de ajuste**. No hay tabla cruzada de dos variables, así que no es independencia/homogeneidad.
> - **Cómo distinguirlos rápido:** ¿comparo *una* variable contra proporciones teóricas? → **bondad de ajuste** ($\nu=c-1$). ¿Cruzo *dos* variables en una tabla? → **independencia/homogeneidad** ($\nu=(f-1)(c-1)$).
> - **gl acá:** 4 categorías → $\nu=c-1=3$.
> - **Mecánica propia:** primero calculo las **frecuencias esperadas** $e_i=n\cdot p_i$ (lo que "debería" haber si $H_0$ fuera cierta) y las comparo con las observadas $o_i$.
> - **Cola:** χ² siempre **unilateral derecha**.

**Identifico:** **una** variable categórica comparada contra **proporciones teóricas** → **χ² de bondad de ajuste**, $\nu=c-1=4-1=\mathbf{3}$. Ver [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].

$$H_0:\ \text{la distribución sigue las proporciones históricas (58/15/7/20)}$$
$$H_1:\ \text{la distribución NO sigue las proporciones históricas}$$

**Frecuencias esperadas** $e_i=n\cdot p_i$:
$$e_{\text{Chrome}}=200\cdot0{,}58=116 \quad e_{\text{Firefox}}=200\cdot0{,}15=30 \quad e_{\text{Edge}}=200\cdot0{,}07=14 \quad e_{\text{Otros}}=200\cdot0{,}20=40$$
(todas $\ge 5$ ✅).

**Estadístico:**
$$\chi^2_m=\sum\frac{(o_i-e_i)^2}{e_i}=\frac{(110-116)^2}{116}+\frac{(19-30)^2}{30}+\frac{(14-14)^2}{14}+\frac{(57-40)^2}{40}$$
$$=0{,}310+4{,}033+0+7{,}225\approx 11{,}57$$
$$pv=P(\chi^2_3>11{,}57)\approx 0{,}009$$

> [!success] Conclusión
> $0{,}009<0{,}05 \Rightarrow$ **Rechazo $H_0$**. Hay evidencias para concluir que la **distribución de uso de navegadores cambió** respecto de las proporciones históricas, con un nivel de significación del 5%.
> **✅ Oficial:** $pv=0{,}009$ → se rechaza. ✔️ (El mayor aporte al $\chi^2$ viene de "Otros": $57$ observados vs $40$ esperados.)

---

## 🟪 P2 · Ejercicio 5 — Completar para que sea verdadero

> [!question]- Cómo se encaran estos "completar"
> Son conceptuales (sin árbol). Ambos giran alrededor del **intervalo de confianza** y su amplitud:
> - (a) cadena de causa-efecto: más $\alpha$ → menos confianza → valor crítico más chico → EM más chico → intervalo **más angosto**. Es la misma lógica que P1·4a, vista al revés.
> - (b) usa la relación geométrica del IC: **amplitud $=2\cdot EM$**, y el IC está **centrado** en el valor muestral. De ahí salen $l_i$ y $l_s$.

### a) "Si aumentamos el nivel de significación ($\alpha$) y mantenemos $n$, la amplitud del intervalo… **se reduce**."
Más $\alpha$ ⇒ menos confianza ⇒ menor valor crítico ⇒ menor $EM$ ⇒ intervalo **más angosto**. ✔️ (Oficial: *se reduce*.)

### b) IC para proporciones de **amplitud 0,06** con valor muestral **0,45** → $l_i=?$, $l_s=?$
La **amplitud** es el ancho total $=2\cdot EM$, así que $EM=\dfrac{0{,}06}{2}=0{,}03$. El IC se centra en $\hat p=0{,}45$:
$$l_i=0{,}45-0{,}03=\mathbf{0{,}42} \qquad l_s=0{,}45+0{,}03=\mathbf{0{,}48}$$
✔️ (Oficial: $l_i=0{,}42$ · $l_s=0{,}48$.)

---
---

> [!quote] Repaso de las trampas que aparecieron en estos parciales
> - **Welch vs Student**: cuando los $s$ son muy distintos (y los $gl$ no dan $n_1+n_2-2$), es **Welch** — P1·1c.
> - **Homogeneidad de varianzas (Levene)** $H_0$ es de **igualdad** ($=$), no de desigualdad — errata oficial en P1·1c.
> - **χ² siempre unilateral derecha**, $\nu=(f-1)(c-1)$ en tablas y $\nu=c-1$ en bondad de ajuste — P1·2 y P2·4.
> - **No interpretar la ordenada** si $x=0$ está fuera del rango — P1·3a, P2·2a.
> - $r=-\sqrt{r^2}$ cuando la **pendiente es negativa** — P2·2b.
> - **Apareadas**: mismas unidades medidas dos veces (antes/después con **igual** $n$) — P2·1.
> - $\alpha$ y $\beta$ se identifican leyendo $P(\text{decisión} \mid \text{estado real})$ — P2·3a.
> - La hipótesis se arma con el **valor afirmado**, no con el muestral — P1·4b.
> - **Amplitud $=2\cdot EM$** — P2·5b.

🔙 [[00 - GUÍA 2do Parcial]] · [[Hoja de Resumen - 2do Parcial]] · [[06 - TODOS los ejercicios resueltos]] · [[07 - Ejercicios de repaso resueltos]] · [[Preguntas Integradoras - Probabilidad (2do Parcial)]]
