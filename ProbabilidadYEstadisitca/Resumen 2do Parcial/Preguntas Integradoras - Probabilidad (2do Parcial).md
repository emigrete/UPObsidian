---
title: "Preguntas Integradoras — Probabilidad (2do Parcial)"
tags:
  - probabilidad-estadistica
  - preguntas
  - 2do-parcial
aliases:
  - "Preguntas Probabilidad 2do Parcial"
parcial: 2
---

# 🧩 Preguntas Integradoras — Probabilidad y Estadística (2do Parcial)

> [!tip] Cómo usar esta página
> Las respuestas están **plegadas**. Leé la pregunta, resolvéla vos, y recién después abrí el callout (click en el ▸ a la izquierda de "Ver respuesta"). Entra Práctico 6 a 9 (inferencia).

## 📈 Práctico 6 — Inferencia para una población

**1.** Definí con tus palabras qué es un **parámetro** y qué es un **estadístico/estimador**. Dado el listado $\mu$, $\bar{x}$, $\sigma^2$, $S^2$, $\hat{p}$, $p$, clasificá cuáles son parámetros y cuáles estimadores puntuales, e indicá qué estima cada estimador.
> [!success]- Ver respuesta
> - **Parámetro:** característica de la **población**, es una **constante**. Ejemplos: $\mu$, $\sigma^2$, $p$.
> - **Estadístico / estimador:** característica de la **muestra**, es una **variable aleatoria**. Ejemplos: $\bar{x}$, $S^2$, $\hat{p}$.
> - Clasificación del listado:
> 	- Parámetros: $\mu$, $\sigma^2$, $p$.
> 	- Estimadores puntuales: $\bar{x}$, $S^2$, $\hat{p}$.
> - Qué estima cada uno: $\bar{x}$ estima $\mu$ · $S^2$ estima $\sigma^2$ · $S$ estima $\sigma$ · $\hat{p}$ estima $p$.
> 
> Ver [[01 - Práctico 6 - Inferencia para una población]].

**2.** ¿Qué distribución se usa para el **IC de la media** y bajo qué condiciones? ¿Y para el **IC de una proporción**? Escribí la estructura de cada intervalo.
> [!success]- Ver respuesta
> - **IC de la media:** se usa la distribución **t de Student** con $\nu = n-1$ grados de libertad, cuando $\sigma$ es **desconocido** y la variable proviene de una **población normal** (importante con muestra chica).
> $$IC_{1-\alpha}(\mu) = \bar{x} \pm \underbrace{t_{(n-1;\,1-\frac{\alpha}{2})}\cdot\frac{s}{\sqrt{n}}}_{EM}$$
> - **IC de una proporción:** se usa la **Normal (z)**, válida con muestras **grandes** ($n>30$) por el TCL. Con $\hat{p}=x/n$:
> $$IC_{1-\alpha}(p) = \hat{p} \pm \underbrace{z_{(1-\frac{\alpha}{2})}\cdot\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}}_{EM}$$
> - En ambos casos la estructura es **parámetro estimado ± EM** (Error Muestral).

**3.** Explicá correctamente qué significa el **nivel de confianza** $(1-\alpha)$ de un intervalo (por ejemplo, 95%).
> [!success]- Ver respuesta
> El nivel de confianza $(1-\alpha)$ significa que, de cada 100 intervalos construidos con **muestras distintas**, aproximadamente $(1-\alpha)\cdot100$ contienen al **verdadero parámetro**.
> - Para 95%: de cada 100 intervalos construidos con muestras distintas, **95** contienen al parámetro verdadero.
> - Interpretación válida también: "la probabilidad de que $\mu$ pertenezca al intervalo es $(1-\alpha)$".

**4.** ¿Qué le pasa al **error muestral (EM)** y a la **precisión** del intervalo si, manteniendo $n$ fijo, **aumento el nivel de confianza**? ¿Y si, con confianza fija, **aumento el tamaño de muestra $n$**?
> [!success]- Ver respuesta
> - **↑ confianza (n fijo):** ↑ EM ⇒ intervalo más **ancho** ⇒ **menos preciso**. (Sube también el valor crítico; baja $\alpha$.)
> - **↑ tamaño de muestra $n$ (confianza fija):** ↓ EM ⇒ intervalo más **angosto** ⇒ **más preciso**, porque el EM se divide por $\sqrt{n}$.
> - **↓ tamaño de muestra $n$:** ↑ EM ⇒ menos preciso.
> - Regla mental: más amplitud de intervalo = menos precisión, y $EM = \text{amplitud}/2$.

**5.** Un intervalo de confianza para una proporción tiene una **amplitud de 0,134**. ¿Cuánto vale el **error muestral**? Si para la misma muestra el EM hubiera sido 0,072, ¿el nivel de confianza sería mayor o menor?
> [!success]- Ver respuesta
> - El EM es la mitad de la amplitud:
> $$EM = \frac{\text{amplitud}}{2} = \frac{0{,}134}{2} = 0{,}067$$
> - Si el EM hubiera sido **0,072** (mayor), el intervalo sería más **ancho** ⇒ menos preciso ⇒ corresponde a un nivel de confianza **mayor**.
> 
> (Es el razonamiento del Ej 19 del [[01 - Práctico 6 - Inferencia para una población|P6]].)

**6.** Definí **error tipo I**, **error tipo II** y **potencia** de una prueba, indicando con qué letra se simboliza cada uno y cuál se considera el más grave.
> [!success]- Ver respuesta
> - **Error tipo I** $\;\alpha = P(\text{Rechazar } H_0 \mid H_0 \text{ verdadera})$. Es el **más grave** y coincide con el **nivel de significación**.
> - **Error tipo II** $\;\beta = P(\text{No rechazar } H_0 \mid H_0 \text{ falsa})$.
> - **Potencia** $\;1-\beta = P(\text{Rechazar } H_0 \mid H_0 \text{ falsa})$: probabilidad de tomar la decisión correcta, es decir, **detectar el efecto que sí existe**.

**7.** Una empresa decide **comprar una máquina** si esta mejora la producción. Si **rechazar $H_0$ = comprar la máquina**, redactá el **error tipo I** y el **error tipo II** en términos del problema. ¿Qué $\alpha$ le conviene a quien teme comprar de más?
> [!success]- Ver respuesta
> - **Error tipo I:** comprar la máquina cuando en realidad **NO** mejora la producción.
> - **Error tipo II:** **NO** comprar la máquina cuando en realidad **SÍ** mejora la producción.
> - A quien teme el error tipo I (comprar de más) le conviene un **$\alpha$ chico (por ej. 0,01)**. A quien le interesa que la acción se tome (por ej. el vendedor) le conviene un **$\alpha$ grande (por ej. 0,10)**.

**8.** ¿Qué es el **p-valor**? Enunciá la **condición de rechazo** universal de una prueba de hipótesis.
> [!success]- Ver respuesta
> - El **p-valor** es el **mínimo nivel de significación** para el que se rechazaría $H_0$.
> - Condición de rechazo (la regla de oro, vale para casi todo):
> $$p\text{-valor} < \alpha \;\Rightarrow\; \text{Rechazo } H_0$$
> $$p\text{-valor} > \alpha \;\Rightarrow\; \text{NO Rechazo } H_0$$
> - Cuidado: si $p\text{-valor} > \alpha$ se dice "**NO se rechaza** $H_0$", **nunca** "se acepta $H_0$".

**9.** Un directivo afirma que el tiempo medio de uso **supera las 50 horas**. Con una muestra ($n=20$) jamovi devuelve **p-valor = 0,1421** y el riesgo es del 7%. ¿Se rechaza $H_0$? Planteá las hipótesis, concluí y nombrá el error que se podría estar cometiendo.
> [!success]- Ver respuesta
> - Variable cuantitativa, una población, $\sigma$ desconocido → **prueba t de una muestra**. "Supera 50" → cola derecha:
> $$H_0: \mu \le 50 \qquad H_1: \mu > 50$$
> - Decisión: $0{,}1421 > 0{,}07 \Rightarrow$ **NO se rechaza $H_0$**.
> - Conclusión: con $\alpha = 0{,}07$ **no hay evidencia** de que el directivo tenga razón (el uso medio no supera las 50 hs).
> - Como **no se rechazó** $H_0$, el error posible es el **tipo II**: concluir que no supera 50 cuando en realidad sí, perdiendo la oportunidad.
> 
> (Ejercicio 17 del [[01 - Práctico 6 - Inferencia para una población|P6]].)

**10.** Para una campaña de ahorro de agua se prueba si el consumo medio **bajó de 136**. El p-valor obtenido es **0,044**. Decidí con $\alpha = 0{,}06$ y luego con $\alpha = 0{,}02$, e indicá qué error está en juego en cada caso.
> [!success]- Ver respuesta
> - "¿Disminuyó de 136?" → cola izquierda:
> $$H_0: \mu \ge 136 \qquad H_1: \mu < 136$$
> - Con **$\alpha = 0{,}06$:** $0{,}044 < 0{,}06 \Rightarrow$ **Rechazo $H_0$** → la campaña tuvo efecto positivo. Riesgo en juego: **error tipo I** (afirmar que fue efectiva sin serlo).
> - Con **$\alpha = 0{,}02$:** $0{,}044 > 0{,}02 \Rightarrow$ **NO Rechazo $H_0$** → **cambia la conclusión**, no se puede afirmar que tuvo efecto. Riesgo en juego: **error tipo II** (decir que no fue efectiva cuando sí lo fue).
> 
> (Ejercicio 18 del [[01 - Práctico 6 - Inferencia para una población|P6]].)

**11.** Un antivirus afirma detectar el 80% de las amenazas. En una muestra de 400, detectó el 75%. Con $\alpha = 0{,}01$, ¿la detección real es menor que la afirmada? Indicá qué prueba usás, planteá las hipótesis y cómo se obtiene el p-valor (¿con jamovi-t?).
> [!success]- Ver respuesta
> - Es un **porcentaje / proporción** de una sola población → **prueba z para proporción**. Las hipótesis ("¿es menor que el 80%?") van en cola izquierda:
> $$H_0: p \ge 0{,}80 \qquad H_1: p < 0{,}80$$
> - El p-valor **NO** se calcula con la prueba t de jamovi (esa es para medias); se resuelve **a mano** con la Normal o con **distrACTION (Normal)**:
> $$z_m = \frac{\hat{p}-p_0}{\sqrt{\dfrac{p_0(1-p_0)}{n}}}$$
> - Con $\hat{p}=0{,}75$ el resultado es **p-valor = 0,0062**. Decisión: $0{,}0062 < 0{,}01 \Rightarrow$ **Rechazo $H_0$**: la proporción real es **menor** que la estimada (riesgo 1%).
> 
> (Ejercicio 16 del [[01 - Práctico 6 - Inferencia para una población|P6]].)

**12.** Indicá cómo se plantean las hipótesis ($H_0$ y $H_1$) y de qué cola es cada prueba según diga el enunciado: "es **mayor** / supera", "es **menor** / inferior a", "es **distinto** / difiere de".
> [!success]- Ver respuesta
> | El enunciado dice… | $H_0$ | $H_1$ | Cola |
> |---|---|---|---|
> | "es mayor / supera / excede" | $\mu \le \mu_0$ | $\mu > \mu_0$ | derecha |
> | "es menor / inferior a" | $\mu \ge \mu_0$ | $\mu < \mu_0$ | izquierda |
> | "es distinto / difiere de" | $\mu = \mu_0$ | $\mu \ne \mu_0$ | bilateral |
> 
> - $H_0$ (nula) siempre lleva el **=** y afirma que NO hay efecto/cambio.
> - $H_1$ (alternativa) es lo que se quiere probar; **rechazar $H_0$ = tomar la acción**.
> - Si es **bilateral**, el p-valor se **duplica** respecto al de una cola.

**13.** En jamovi, ¿qué ruta de menú usás para una **PH de la media de una población**? ¿Dónde se pone $\mu_0$ y dónde la dirección de $H_1$? ¿Y para construir el IC de la media?
> [!success]- Ver respuesta
> - Ruta: **Pruebas t → Prueba t de una Muestra**.
> - Se pasa la variable a *Variables Dependientes*; en **Valor de prueba** se pone $\mu_0$; en **Hipótesis** se marca la dirección ($>$, $<$ o $\ne$ del Valor de prueba).
> - jamovi devuelve el estadístico (t), los grados de libertad (gl) y el **p** (ya viene de una cola si se eligió $>$ o $<$). Luego se aplica $pv < \alpha \Rightarrow$ Rechazo $H_0$.
> - Para el **IC de la media**: misma ruta, en **Estadísticas Adicionales → Diferencia de medias → tildar Intervalo de confianza** y poner el nivel. ⚠️ En *Hipótesis* dejar **≠ Valor de prueba** para que el IC salga **bilateral**.

## 📊 Práctico 7 — Comparación de dos muestras

**1.** ¿Cuál es la **primera pregunta** que hay que hacerse al comparar dos muestras de medias? Diferenciá **muestras apareadas** de **muestras independientes**.
> [!success]- Ver respuesta
> La primera pregunta es: **¿apareadas o independientes?**
> - **Apareadas (pareadas):** se mide la **misma variable dos veces sobre el mismo individuo** (antes/después, pre-post, test-retest) o pares naturales. Hay UNA muestra medida 2 veces.
> - **Independientes:** dos **grupos distintos** de individuos (uno por cada nivel del factor). Ej: hombres vs mujeres, sucursal A vs B, algoritmo A vs B.
> 
> Ver [[02 - Práctico 7 - Comparación de dos muestras]].

**2.** Antes de aplicar la prueba **t para muestras independientes** hay que validar dos supuestos. ¿Cuáles son, qué $H_0$ tiene cada uno y cómo se interpreta el resultado?
> [!success]- Ver respuesta
> - **Normalidad (Shapiro-Wilk):** $H_0$: los datos son normales. **pv > α ⇒ se cumple normalidad** (NO se rechaza, que es lo deseado).
> - **Homogeneidad de varianzas (Levene):** $H_0: \sigma_1^2 = \sigma_2^2$.
> 	- **pv > α ⇒ varianzas iguales** → uso **t de Student**.
> 	- **pv < α ⇒ varianzas distintas** → uso **t de Welch** (destildar Student, tildar Welch).
> - Ojo: en las pruebas de **supuestos**, lo que querés es **NO rechazar** (pv alto, > α), al revés que en la prueba principal.

**3.** En una prueba t de muestras independientes, **Levene** da un p-valor de **0,009** (con $\alpha = 0{,}06$). ¿Qué decidís y qué prueba usás finalmente?
> [!success]- Ver respuesta
> - $H_0$ de Levene: varianzas iguales. Como $0{,}009 < 0{,}06 \Rightarrow$ **se rechaza** → **varianzas distintas**.
> - Por lo tanto se destilda la t de Student y se usa la **t de Welch**.
> 
> (Es el caso del Ej 11, ventas centro vs shopping, del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

**4.** En muestras **apareadas** se trabaja con la diferencia $D$. Explicá cómo se define $D$, qué es $H_0$ y cómo se decide la dirección de $H_1$ si se espera que "el valor baje".
> [!success]- Ver respuesta
> - Se define **$D = \text{medida 1} - \text{medida 2}$** (por ejemplo: antes − después). Hay que **aclarar siempre qué es D**.
> - La nula es $H_0: \mu_D = 0$ (no hay cambio).
> - Si se espera que el valor **baje** (antes > después), entonces $D > 0$, por lo que $H_1: \mu_D > 0$.
> - En jamovi: **Pruebas t → Prueba t para Muestras Pareadas**, respetando el orden de $D$ en *Variables Apareadas*; se tilda la **Prueba de Normalidad**.

**5.** ⚠️ Un enunciado dice "se midió la satisfacción **antes y después** de una actualización": se tomó una muestra de **10 usuarios antes** y **otra de 12 usuarios después**. ¿Apareadas o independientes? Justificá.
> [!success]- Ver respuesta
> Son **muestras INDEPENDIENTES**, no apareadas. Aunque diga "antes y después", se miden **grupos distintos** de individuos (10 usuarios antes ≠ 12 después).
> - La pista clave es que los $n$ son **distintos** (10 ≠ 12); en apareadas siempre hay el **mismo número de pares**.
> - Al ser independientes, antes de la t corresponde correr **Levene** (varianzas) y **Shapiro** (normalidad).
> 
> (Es la trampa del Ejercicio 1 del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

**6.** Se comparan dos algoritmos distintos A y B para ver si **difieren** en eficacia. jamovi da **p-valor = 0,0431** con $\alpha = 0{,}05$. ¿Qué prueba se usa, cómo se plantean las hipótesis y qué se concluye? ¿Hace falta el test de homogeneidad?
> [!success]- Ver respuesta
> - Dos grupos distintos, medias cuantitativas → **t para muestras independientes** (por eso **sí** corresponde hacer el test de homogeneidad de varianzas / Levene).
> - "¿Difieren?" → bilateral:
> $$H_0: \mu_A = \mu_B \qquad H_1: \mu_A \ne \mu_B$$
> - Decisión: $0{,}0431 < 0{,}05 \Rightarrow$ **Rechazo $H_0$**: los algoritmos **difieren** en eficacia (riesgo 5%).
> 
> (Ejercicio 5 del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

**7.** En un estudio de fatiga se mide a los **mismos individuos** con y sin fatiga. Se da $|t_m| = 2{,}5259$ con $\nu = 14$ y $\alpha = 0{,}04$. Planteá $D$, las hipótesis y concluí (el p-valor de una cola es 0,012).
> [!success]- Ver respuesta
> - Mismo individuo medido dos veces → **t para muestras apareadas**. Defino **$D = \text{con fatiga} - \text{sin fatiga}$**.
> - "¿La fatiga altera (aumenta el tiempo)?" → cola derecha:
> $$H_0: \mu_D = 0 \qquad H_1: \mu_D > 0$$
> - p-valor: $P(t > 2{,}5259) = 0{,}012$. Decisión: $0{,}012 < 0{,}04 \Rightarrow$ **Rechazo $H_0$**: la fatiga **altera** el desempeño.
> 
> (Ejercicio 2 del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

**8.** ¿Qué prueba se usa para comparar el **porcentaje de éxitos de dos grupos**? Indicá la ruta en jamovi y cómo se plantea $H_0$.
> [!success]- Ver respuesta
> - Se usa la **prueba z para diferencia de dos proporciones** (muestras grandes → distribución normal).
> - Planteo de la nula: $H_0: p_1 - p_2 = 0$ (o equivalentemente $p_1 = p_2$).
> - Ruta en jamovi: **Recuento → Muestras independientes (Prueba de independencia χ²)** → en *Pruebas* **destildar χ²** y **tildar "Prueba z para diferencia de 2 proporciones"**; en *Medidas Comparativas* (solo 2×2) tildar "Diferencia de proporciones". Cargar 3 columnas (grupo, categoría, frecuencia) y verificar que **Comparar = filas**.

**9.** Se compara la inseguridad percibida en CABA vs Provincia (¿hay **más** inseguridad en provincia?). jamovi da **p-valor = 0,019** con $\alpha = 0{,}04$. Planteá las hipótesis y concluí.
> [!success]- Ver respuesta
> - Comparación de proporciones de dos grupos → **z diferencia de proporciones**. "¿Mayor en provincia?" → cola derecha:
> $$H_0: p_{CABA} = p_{BsAs} \qquad H_1: p_{BsAs} > p_{CABA}$$
> - Decisión: $0{,}019 < 0{,}04 \Rightarrow$ **Rechazo $H_0$**: hay **mayor inseguridad en provincia** (riesgo 4%).
> 
> (Ejercicio 7 del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

**10.** Se mide el tiempo de carga de las **mismas 7 aplicaciones** antes y después de una optimización (Normalidad pv = 0,218; $\alpha = 0{,}06$). El test da **p-valor = 0,026**. ¿Apareadas o independientes? Planteá y concluí.
> [!success]- Ver respuesta
> - Las **mismas 7 apps** medidas dos veces → **t para muestras apareadas** (acá el $n$ coincide). El supuesto de normalidad se cumple ($0{,}218 > 0{,}06$).
> - $D = \text{antes} - \text{después}$. "¿Se redujo el tiempo?" → cola derecha:
> $$H_0: \mu_D = 0 \qquad H_1: \mu_D > 0$$
> - Decisión: $0{,}026 < 0{,}06 \Rightarrow$ **Rechazo $H_0$**: la optimización **tuvo el efecto deseado** (los tiempos bajaron).
> 
> (Ejercicio 9 del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

**11.** En una prueba t para navegadores C vs F sobre las **mismas 16 computadoras** se sospecha que C es **más rápido** ($|t| = 1{,}988$, $\nu = 15$). El p-valor de una cola es 0,033. ¿Qué pasaría con el p-valor si solo se quisiera saber si **difieren**?
> [!success]- Ver respuesta
> - Mismas computadoras → **apareadas**, $D = C - F$. "¿C más rápido?" (menor tiempo) → cola izquierda:
> $$H_0: \mu_D = 0 \qquad H_1: \mu_D < 0$$
> - p-valor de una cola: $P(t > 1{,}988) = 0{,}033 < 0{,}05 \Rightarrow$ **Rechazo**: C es más rápido.
> - Si solo se quisiera ver si **difieren** (bilateral), el p-valor se **duplica**:
> $$pv = 2 \cdot 0{,}033 = 0{,}066$$
> 
> (Ejercicio 13 del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

**12.** En un estudio de satisfacción según el estilo de CEO (dos grupos distintos), Levene da **pv = 0,029** con $\alpha = 0{,}04$. ¿Qué versión de la t leés y por qué? Si el test principal da **pv = 0,002**, ¿qué concluís?
> [!success]- Ver respuesta
> - Dos grupos distintos → **t independientes**. Levene: $0{,}029 < 0{,}04 \Rightarrow$ varianzas **distintas** → leo el p de la **t de Welch**.
> - Hipótesis: $H_0: \mu_A - \mu_B \ge 0 \;;\; H_1: \mu_A - \mu_B < 0$.
> - Decisión: $0{,}002 < 0{,}04 \Rightarrow$ **Rechazo $H_0$**: las sospechas del investigador **son ciertas** (mayor satisfacción con el CEO que prioriza el bienestar).
> 
> (Ejercicio 10 del [[02 - Práctico 7 - Comparación de dos muestras|P7]].)

## 🎲 Práctico 8 — Datos categóricos (Chi-cuadrado)

**1.** Enumerá las **tres pruebas** que usan la distribución χ² y para qué sirve cada una.
> [!success]- Ver respuesta
> Las tres usan la **misma distribución $\chi^2$** y el mismo estadístico:
> - **Bondad de ajuste:** ¿una muestra sigue proporciones dadas / históricas / uniforme?
> - **Independencia:** una muestra clasificada en dos variables, ¿están relacionadas?
> - **Homogeneidad:** una variable categórica medida en $k$ poblaciones, ¿se distribuye igual en todas?
> 
> Ver [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)]].

**2.** Mencioná las características de la **distribución χ²** y por qué las pruebas χ² son **siempre unilaterales a derecha**.
> [!success]- Ver respuesta
> - Solo toma valores **positivos** (dominio $\mathbb{R}^+$) y es **asimétrica a la derecha**.
> - Tiene un único parámetro: los **grados de libertad** $\nu$, con $E(\chi^2) = \nu$ y $V(\chi^2) = 2\nu$.
> - Es **siempre unilateral a derecha** porque el estadístico es una suma de cuadrados ($\chi^2_m \ge 0$): solo los valores **grandes** indican que lo observado se aleja de lo esperado. Por eso:
> $$p\text{-valor} = P(\chi^2 > \chi^2_m)$$

**3.** Escribí el **estadístico de prueba** común a las tres pruebas χ² y enunciá la **condición de validez** sobre las frecuencias esperadas.
> [!success]- Ver respuesta
> - Estadístico (común a las 3):
> $$\chi^2_m = \sum \frac{(o_i - e_i)^2}{e_i}$$
> donde $o_i$ son las frecuencias **observadas** y $e_i$ las **esperadas** (bajo $H_0$).
> - Condición de validez: cada frecuencia esperada debe ser **≥ 5** ($n \cdot p_i \ge 5$); si no, se **agrupan categorías**.

**4.** En la **prueba de bondad de ajuste**, ¿cómo se calculan las frecuencias esperadas y los grados de libertad? ¿Qué significa que una distribución sea "uniforme"?
> [!success]- Ver respuesta
> - Frecuencias esperadas: $e_i = n \cdot p_i$.
> - Grados de libertad: $\nu = c - 1$ (c = número de categorías).
> - **Uniforme** significa igual probabilidad para cada categoría: $p_i = 1/c$ para todas.
> - $H_0$: los datos **siguen** la distribución (se mantienen las proporciones); $H_1$: **NO** la siguen.
> - jamovi: **Recuento → N Resultados (χ² de bondad de ajuste)**, escribiendo las proporciones en *Proporciones Esperadas → Razón*.

**5.** En la **prueba de independencia**, escribí la fórmula de la frecuencia esperada de cada celda y los grados de libertad. Planteá $H_0$ y $H_1$.
> [!success]- Ver respuesta
> - Frecuencia esperada de la celda $(i,j)$:
> $$e_{ij} = \frac{(\text{total fila}_i)\cdot(\text{total columna}_j)}{n}$$
> - Grados de libertad: $\nu = (\text{filas}-1)\cdot(\text{columnas}-1)$.
> - $H_0$: las variables son **independientes** (no están relacionadas).
> - $H_1$: las variables **NO son independientes** (están relacionadas).

**6.** ¿Cuál es la **diferencia conceptual** entre la prueba de **independencia** y la de **homogeneidad**? ¿En qué se parecen?
> [!success]- Ver respuesta
> - **Independencia:** UNA muestra clasificada en **2 variables** → "¿están relacionadas?".
> - **Homogeneidad:** VARIAS muestras (poblaciones), UNA variable categórica → "¿se distribuye igual en todas las poblaciones?".
> - En qué se parecen: usan las **mismas fórmulas** ($e_{ij}$ y gl iguales) y la **misma operación en jamovi** (*Recuento → Prueba de independencia χ²*). Cambia solo **cómo se plantean** $H_0$ y $H_1$.

**7.** En una tabla 3×3 de voto vs años de experiencia ($n = 250$), la fila "más de 5 años" suma 95 y la columna "a favor" suma 100. Calculá la frecuencia esperada de la celda "a favor y más de 5 años".
> [!success]- Ver respuesta
> Se aplica la fórmula de la frecuencia esperada:
> $$e_{31} = \frac{(\text{total fila})\cdot(\text{total columna})}{n} = \frac{95 \cdot 100}{250} = 38$$
> 
> (Ejercicio 2 del [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].)

**8.** Se analiza si el **voto depende de los años de experiencia** (tabla 3×3, $n=250$). jamovi da $\chi^2_m = 10{,}7957$ y **p-valor = 0,029** con $\alpha = 0{,}05$. ¿Qué prueba es, cuántos gl tiene y qué se concluye?
> [!success]- Ver respuesta
> - Una muestra, dos variables categóricas → **prueba χ² de independencia**.
> - Grados de libertad: $\nu = (3-1)(3-1) = 4$.
> - Hipótesis: $H_0$: el voto es independiente de los años de experiencia; $H_1$: están relacionados.
> - Decisión: $0{,}029 < 0{,}05 \Rightarrow$ **Rechazo $H_0$**: voto y experiencia **están relacionados** (riesgo 5%).
> 
> (Ejercicio 2 del [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].)

**9.** Se estudia si el tipo de defecto **difiere según la línea** de producción (3 líneas, n=255). El test da $\chi^2_m = 0{,}85$ y **p-valor = 0,932** con $\alpha = 0{,}05$. ¿Qué prueba es y qué se concluye?
> [!success]- Ver respuesta
> - Varias poblaciones (líneas) y una variable categórica (tipo de defecto) → **prueba χ² de homogeneidad** (misma operación que independencia, gl = $(3-1)(3-1) = 4$).
> - Hipótesis: $H_0$: el tipo de defecto **no difiere** según la línea (es homogéneo); $H_1$: difiere.
> - Decisión: $0{,}932 > 0{,}05 \Rightarrow$ **NO Rechazo $H_0$**: **no hay evidencia** de diferencias en el tipo de defecto según la línea.
> 
> (Ejercicio 4 del [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].)

**10.** Se quiere saber si la preferencia entre 5 criptomonedas es **uniforme** ($\chi^2_m = 7{,}4917$, $\alpha = 0{,}08$, p-valor = 0,112). Indicá qué prueba es, las proporciones esperadas, los gl y la conclusión.
> [!success]- Ver respuesta
> - "¿Es uniforme?" → **bondad de ajuste**. Con 5 categorías: $p_i = 1/5 = 0{,}2$ cada una.
> - Grados de libertad: $\nu = c - 1 = 4$.
> - Hipótesis: $H_0$: la preferencia es **uniforme**; $H_1$: no lo es.
> - Decisión: $0{,}112 > 0{,}08 \Rightarrow$ **NO Rechazo $H_0$**: la preferencia **es uniforme** (riesgo 8%).
> 
> (Ejercicio 5 del [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].)

**11.** Se compara la preferencia de lenguajes de programación contra **proporciones históricas** ($\chi^2_m = 9{,}9854$, $\alpha = 0{,}06$). El p-valor (cola derecha) es 0,0407. ¿Qué prueba es y qué se concluye?
> [!success]- Ver respuesta
> - Comparar una muestra contra proporciones históricas → **bondad de ajuste**.
> - Hipótesis: $H_0$: mantienen las proporciones históricas; $H_1$: cambiaron.
> - p-valor (siempre unilateral derecha): $P(\chi^2 > 9{,}9854) = 0{,}0407$.
> - Decisión: $0{,}0407 < 0{,}06 \Rightarrow$ **Rechazo $H_0$**: las preferencias **se modificaron** (riesgo 6%).
> 
> (Ejercicio 7 del [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].)

**12.** **Verdadero o Falso (justificá):** "Como la prueba de independencia mira si dos variables difieren en ambos sentidos, es una prueba **bilateral**".
> [!success]- Ver respuesta
> **FALSO.** La prueba **χ² es SIEMPRE unilateral derecha**.
> - Como $\chi^2_m = \sum \frac{(o-e)^2}{e} \ge 0$ es una suma de cuadrados, solo los valores **grandes** indican que lo observado se aleja de lo esperado bajo $H_0$.
> - Por eso la región de rechazo está **únicamente en la cola derecha**:
> $$pv = P(\chi^2 > \chi^2_m)$$
> 
> (Es la afirmación b) del Ejercicio 3 de [[07 - Ejercicios de repaso resueltos]].)

## 📉 Práctico 9 — Regresión y correlación

**1.** En la recta de regresión $\hat{y} = a + bx$, ¿cuál es la variable **independiente** y cuál la **dependiente**? ¿Qué representa $a$ y qué representa $b$, y qué parámetros estiman?
> [!success]- Ver respuesta
> - **x** = variable **independiente** / explicativa / regresora.
> - **y** = variable **dependiente** / explicada.
> - **a** = ordenada al origen (intercepto) → estima $\alpha$.
> - **b** = pendiente (coeficiente de regresión) → estima $\beta$.
> - La recta se obtiene por **mínimos cuadrados** (minimiza $\sum e_i^2$).
> 
> Ver [[04 - Práctico 9 - Regresión y correlación]].

**2.** ¿Cómo se interpretan los coeficientes $a$ y $b$ en palabras? ¿Cuándo hay que aclarar que el intercepto **no tiene sentido**?
> [!success]- Ver respuesta
> - **a (intercepto):** "Cuando $x = 0$, el valor medio de $y$ se estima en $a$".
> - **b (pendiente):** "Por cada unidad que **aumenta x**, $y$ **aumenta/disminuye** (según el signo de $b$) en promedio $|b|$ unidades".
> - Hay que aclarar que el intercepto **no tiene sentido** cuando $x = 0$ está **fuera del rango** de los datos, o cuando produce un valor sin sentido práctico (por ejemplo, un costo negativo, o "programar con 0 horas").

**3.** ¿Qué mide el **coeficiente de determinación $r^2$**? ¿Entre qué valores varía y qué regla práctica indica si el modelo sirve para pronósticos?
> [!success]- Ver respuesta
> - $r^2$ mide la **proporción (%) de la variación total de $y$ explicada por el modelo** (por las variaciones de $x$).
> - Varía entre $0 \le r^2 \le 1$: $r^2 = 1$ es ajuste perfecto (puntos sobre la recta), $r^2 = 0$ es sin relación lineal.
> - Regla práctica: si **$r^2 < 0{,}50$** el modelo **no es bueno** para pronósticos.
> - Redacción: "El $(r^2 \cdot 100)\%$ de las variaciones en $y$ se explican por las variaciones en $x$".

**4.** ¿Qué mide el **coeficiente de correlación $r$**? Enunciá sus propiedades, incluyendo su relación con el signo de $b$ y la regla práctica de buen modelo.
> [!success]- Ver respuesta
> - $r$ mide el **grado/intensidad** y el **sentido** de la asociación lineal.
> - Propiedades:
> 	- $-1 \le r \le 1$.
> 	- El **signo de $r$ = el signo de $b$** (de la pendiente).
> 	- $r = 1$ → asociación lineal positiva perfecta; $r = -1$ → negativa perfecta.
> 	- Si $x$ e $y$ son independientes → $r = 0$. (Pero $r = 0$ **no implica** independencia: puede haber relación no lineal.)
> 	- Relación con $r^2$: $r = \pm\sqrt{r^2}$ (el signo lo da $b$).
> - Regla práctica: si **$|r| > 0{,}7$** el modelo es **bueno** para pronósticos.

**5.** Definí **predicción**, **residuo** y **extrapolación**. Si el residuo es **negativo**, ¿el punto está por encima o por debajo de la recta?
> [!success]- Ver respuesta
> - **Predicción / estimación:** reemplazar un valor de $x$ en la recta → $\hat{y}$.
> - **Residuo:** $e = y - \hat{y}$ (observado − estimado).
> 	- $e < 0$ → el punto está **por debajo** de la recta (se **sobreestimó**).
> 	- $e > 0$ → el punto está **por encima** (se **subestimó**).
> - **Extrapolación:** estimar para un $x$ **fuera del rango** de los datos → **no es válido / no tiene sentido**.

**6.** Para la recta $\hat{y} = 8{,}397 - 0{,}345\,x$ (errores vs horas), donde $x$ va de 12 a 22: estimá $\hat{y}$ para $x = 18$ y calculá el residuo si el observado fue $y = 2$. ¿Es válido estimar para $x = 30$?
> [!success]- Ver respuesta
> - Estimación en $x = 18$:
> $$\hat{y}(18) = 8{,}397 - 0{,}345 \cdot 18 = 2{,}187$$
> - Residuo con observado $y = 2$:
> $$e = y - \hat{y} = 2 - 2{,}187 = -0{,}187$$
> Como $e < 0$, el punto está **por debajo** de la recta (se sobreestimó en 0,187).
> - Para **$x = 30$ NO tiene sentido** estimar: $x$ solo va de 12 a 22, así que sería **extrapolar**.
> 
> (Ejercicio 1 del [[04 - Práctico 9 - Regresión y correlación|P9]].)

**7.** ¿Para qué sirve la **prueba de hipótesis para $\beta$**? Escribí las hipótesis, el estadístico, los grados de libertad y por qué siempre es bilateral. ¿Qué significa rechazar $H_0$?
> [!success]- Ver respuesta
> - Sirve para ver si **hay asociación lineal**: si $\beta = 0$ la recta es horizontal → $x$ no sirve para predecir $y$. Las hipótesis son **siempre bilaterales**:
> $$H_0: \beta = 0 \qquad H_1: \beta \ne 0$$
> - Estadístico (análogo a la media con $\sigma$ desconocido), con $\nu = n - 2$:
> $$t_m = \frac{b}{S_b} \qquad\Rightarrow\qquad p\text{-valor} = 2\cdot P(t > |t_m|)$$
> - **Rechazar $H_0$** ⇒ las variables están **linealmente asociadas** (la recta es útil para estimar).
> - En jamovi, el **p de la covariable (x)** en *Coeficientes del Modelo* es el de la prueba de $\beta$.

**8.** En un modelo costos vs capacitación ($n = 6$), $b = 0{,}158$ y $S_b = 0{,}0296$, con $\alpha = 0{,}01$. Calculá $t_m$, indicá los gl y decidí (p-valor = 0,0059).
> [!success]- Ver respuesta
> - Estadístico de la prueba de $\beta$:
> $$t_m = \frac{b}{S_b} = \frac{0{,}158}{0{,}0296} = 5{,}3378$$
> - Grados de libertad: $\nu = n - 2 = 4$.
> - Decisión: $0{,}0059 < 0{,}01 \Rightarrow$ **Rechazo $H_0$**: costos y horas de capacitación están **linealmente asociados**.
> - (Con $r = 0{,}9782$, el $r^2 = 0{,}957$ → el 95,7% de la variación de costos se explica por las horas.)
> 
> (Ejercicio 9 del [[04 - Práctico 9 - Regresión y correlación|P9]].)

**9.** En un modelo daño vs km ($n = 6$), el test de $\beta$ da $t_m = 0{,}797$ ($\nu = 4$), **p-valor = 0,47** con $\alpha = 0{,}04$, y $r^2 = 0{,}1369$. ¿Hay asociación lineal? ¿El $r^2$ ratifica la conclusión?
> [!success]- Ver respuesta
> - Decisión: $0{,}47 > 0{,}04 \Rightarrow$ **NO Rechazo $H_0$**: **NO hay asociación lineal**.
> - El coeficiente de determinación $r^2 = 0{,}1369$ indica que solo el **13,69%** de la variación se explica con el modelo. Como es $< 0{,}50$, **ratifica** que el modelo **no sirve** para pronósticos.
> 
> (Ejercicio 11 del [[04 - Práctico 9 - Regresión y correlación|P9]].)

**10.** En un modelo con $\hat{y} = 12 - 0{,}17x$ y $r^2 = 0{,}81$, el test de $\beta$ rechaza $H_0$. Interpretá $a$, $b$ y $r^2$, y deducí el valor y signo de $r$. Si la correlación fuera perfecta, ¿cuánto valdría $r$?
> [!success]- Ver respuesta
> - **a = 12:** sin inversión, hay 12 (cientos de) incidentes en promedio.
> - **b = −0,17:** por cada 100 dólares más de inversión, los incidentes bajan 17 en promedio.
> - **$r^2 = 0{,}81$:** el **81%** de la variación de los incidentes se explica por la inversión.
> - Como rechaza $H_0$, están linealmente asociados; $r = \pm\sqrt{0{,}81} = \pm 0{,}9$, y como $b < 0$ → **$r = -0{,}9$** (intensa e inversa).
> - Si la correlación fuera **perfecta** → **$r = -1$** (mismo signo que $b$).
> 
> (Ejercicio 8 del [[04 - Práctico 9 - Regresión y correlación|P9]].)

**11.** Tenés tres gráficos de dispersión y los valores $r^2 = 0{,}88$, $r = 0{,}30$ y $b = -5$. ¿Cómo asignás cada valor a su gráfico? ¿Qué dos claves usás?
> [!success]- Ver respuesta
> - **Pendiente positiva** y nube **bien ajustada** → $r^2 = 0{,}88$ (alta correlación positiva).
> - **Pendiente positiva** pero nube **dispersa** → $r = 0{,}30$ (baja correlación).
> - **Pendiente negativa** (la recta baja) → $b = -5$.
> - Las dos claves: (1) el **signo de $b$/$r$** dice si la recta sube (+) o baja (−); (2) cuanto **más pegados a la recta** estén los puntos, **mayor $|r|$ y $r^2$**.
> 
> (Ejercicio 6 del [[04 - Práctico 9 - Regresión y correlación|P9]].)

**12.** **Verdadero o Falso:** si $r = 0$, entonces $x$ e $y$ son necesariamente **independientes**.
> [!success]- Ver respuesta
> **FALSO.** Si $x$ e $y$ son independientes entonces $r = 0$, pero **el recíproco no vale**: $r = 0$ **no implica** independencia, porque $r$ mide solo la asociación **lineal** y puede existir una **relación no lineal** con $r = 0$.

## 🌳 Integradoras — ¿qué prueba uso?

**1.** "Se mide la satisfacción de **un mismo grupo** de empleados **antes y después** de aplicar un nuevo algoritmo de recomendación. ¿Mejoró la satisfacción?" ¿Qué prueba usás?
> [!success]- Ver respuesta
> **Prueba t para muestras apareadas (pareadas).** Razón: se mide la **misma variable cuantitativa dos veces sobre el mismo grupo** (antes/después).
> - Palabras clave que la delatan: "antes y después", "el mismo grupo".
> - Se define $D = \text{antes} - \text{después}$ y se plantea $H_0: \mu_D = 0$.
> 
> Ver el [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|árbol de decisión]].

**2.** "Se comparan los tiempos de atención en **dos sucursales distintas** (A y B). ¿La sucursal A atiende más rápido?" ¿Qué prueba usás y qué hay que verificar primero?
> [!success]- Ver respuesta
> **Prueba t para muestras independientes.** Razón: se comparan **medias** de una variable cuantitativa en **dos grupos de individuos distintos** (sucursales distintas).
> - Antes de la t hay que verificar los supuestos con jamovi: **Shapiro-Wilk** (normalidad) y **Levene** (homogeneidad de varianzas).
> - Si Levene da varianzas distintas (pv < α) → usar **Welch**.
> - Palabras clave: "dos sucursales", individuos distintos.

**3.** "Una encuesta a una sola muestra clasifica a las personas según su **nivel de formación** (4 categorías) y su **rendimiento laboral** (3 categorías). ¿Están relacionados?" ¿Qué prueba? ¿Cuántos gl y de qué cola?
> [!success]- Ver respuesta
> **Prueba χ² de independencia** (tabla 4×3). Razón: es **una sola muestra** clasificada según **dos variables categóricas** y se pregunta si **una depende de la otra**.
> - Grados de libertad: $\nu = (4-1)(3-1) = 6$.
> - Es **unilateral derecha**: $pv = P(\chi^2 > \chi^2_m)$.
> - Palabras clave: "¿está relacionado con? / ¿es independiente de?" con tabla de conteos.
> 
> (Es el escenario del Ejercicio 3 de [[07 - Ejercicios de repaso resueltos]].)

**4.** "Se quiere saber si el **porcentaje** de usuarios satisfechos **difiere entre hombres y mujeres**." ¿Qué prueba usás?
> [!success]- Ver respuesta
> **Prueba z para diferencia de dos proporciones.** Razón: se compara el **porcentaje de éxitos de dos grupos** distintos.
> - Hipótesis (bilateral, "difiere"): $H_0: p_M = p_H \;;\; H_1: p_M \ne p_H$.
> - Palabras clave: "porcentaje / proporción / %" de **dos** grupos.
> - jamovi: **Recuento → Prueba de independencia χ²** → destildar χ², tildar "Prueba z para diferencia de 2 proporciones".

**5.** "Se mide el **tiempo de carga** de una página en función del **número de imágenes**. ¿Sirve el número de imágenes para predecir el tiempo?" ¿Qué prueba/herramienta usás?
> [!success]- Ver respuesta
> **Regresión lineal + prueba de hipótesis para $\beta$** (y correlación $r$, $r^2$). Razón: se busca la **relación entre dos variables cuantitativas** (estimar $y$ en función de $x$).
> - Se plantea $H_0: \beta = 0$ vs $H_1: \beta \ne 0$; rechazar $H_0$ significa que están **linealmente asociadas**.
> - Palabras clave: "estimar y en función de x / recta / asociación lineal".
> - jamovi: **Regresión → Regresión lineal**.

**6.** "Se quiere verificar si las preferencias de los usuarios entre 5 productos se **mantienen iguales a las proporciones históricas**." ¿Qué prueba?
> [!success]- Ver respuesta
> **Prueba χ² de bondad de ajuste.** Razón: se compara **una muestra** contra una distribución teórica conocida (proporciones históricas).
> - $H_0$: se mantienen las proporciones históricas; $H_1$: cambiaron.
> - $e_i = n \cdot p_i$, gl = $c - 1$, unilateral derecha.
> - Palabras clave: "¿se mantienen las proporciones históricas? / ¿es uniforme? / ¿sigue tal distribución?".

**7.** "Una sola muestra de 200 clientes; se quiere estimar **con 95% de confianza** la **proporción** que recomendaría el servicio." ¿Qué herramienta usás y con qué distribución?
> [!success]- Ver respuesta
> **Intervalo de confianza para una proporción**, usando la distribución **Normal (z)** (muestra grande, $n > 30$).
> $$IC_{1-\alpha}(p) = \hat{p} \pm z_{(1-\frac{\alpha}{2})}\cdot\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$
> - $\hat{p} = x/n$. Generalmente se calcula **a mano** (no con jamovi-t, que es para medias).
> - Palabras clave: "proporción / porcentaje" de **una sola** muestra + "estimar / confianza".

**8.** "Se mide la **fiabilidad** (fiable / no fiable) de un componente provisto por **4 proveedores distintos**. ¿La fiabilidad es la misma para todos?" ¿Qué prueba? ¿En qué se diferencia conceptualmente de la de independencia?
> [!success]- Ver respuesta
> **Prueba χ² de homogeneidad.** Razón: se toman **varias poblaciones distintas** (los 4 proveedores) y se observa **una misma variable categórica** (fiable/no), preguntando si se distribuye igual.
> - $H_0$: la fiabilidad es la misma para los 4 proveedores; $H_1$: difiere.
> - Diferencia con independencia: en **homogeneidad** hay **varias muestras (poblaciones)** y **una** variable; en **independencia** hay **una** muestra clasificada en **dos** variables. En la práctica de jamovi y los cálculos son idénticas.
> - Palabras clave: "¿diferencias según la línea/proveedor/rubro?" con tabla.

**9.** "Una afirmación dice que el tiempo medio de soporte técnico **supera las 4 horas**, sobre una sola muestra de tiempos." ¿Qué prueba usás y por qué? ¿La proporción o la media?
> [!success]- Ver respuesta
> **Prueba t de una muestra (para la media).** Razón: es **una** población, variable **cuantitativa** (tiempo), $\sigma$ **desconocido** y la variable es aproximadamente normal.
> - "Supera 4 horas" → cola derecha: $H_0: \mu \le 4 \;;\; H_1: \mu > 4$.
> - No es proporción porque no se trata de un porcentaje de éxitos, sino del **valor medio** de una variable cuantitativa.
> - jamovi: **Pruebas t → Prueba t de una Muestra**, Valor de prueba = 4.
> 
> (Es el Ejercicio 1 de [[07 - Ejercicios de repaso resueltos]].)

**10.** ⚠️ "Un enunciado dice 'antes y después de la actualización', pero se midieron **10 usuarios antes** y **12 usuarios después**." ¿Apareadas o independientes? ¿Cuál es la pista?
> [!success]- Ver respuesta
> **Muestras independientes** (no apareadas), a pesar del "antes/después".
> - La pista clave: los tamaños son **distintos** (10 ≠ 12). En **apareadas** siempre hay el **mismo número de pares** (mismos individuos medidos dos veces).
> - Al ser independientes, antes de la t corresponde correr **Levene** y **Shapiro**.
> - Es una **trampa típica**: no alcanza con leer "antes/después", hay que verificar si son los mismos individuos.

**11.** "Se quiere estimar, con cierta confianza, la **temperatura media** de un proceso a partir de una muestra chica ($n = 14$), sabiendo que la variable es normal y $\sigma$ es desconocido." ¿Qué herramienta y qué distribución usás?
> [!success]- Ver respuesta
> **Intervalo de confianza para la media**, usando la distribución **t de Student** con $\nu = n - 1 = 13$ grados de libertad.
> - Se usa la $t$ (y no la Normal) porque $\sigma$ es **desconocido** y la muestra es chica; por eso es **condición** que la variable provenga de una población normal.
> $$IC_{1-\alpha}(\mu) = \bar{x} \pm t_{(n-1;\,1-\frac{\alpha}{2})}\cdot\frac{s}{\sqrt{n}}$$
> - jamovi: **Pruebas t → Prueba t de una Muestra** → Estadísticas Adicionales → tildar Intervalo de confianza (Hipótesis en ≠).

**12.** "Resumí en una frase la **receta de 5 pasos** para resolver cualquier ejercicio de inferencia del parcial."
> [!success]- Ver respuesta
> 1. **Identificá la prueba** con el [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|árbol de decisión]] (¿una o dos poblaciones? ¿media o proporción? ¿categórico? ¿relación?).
> 2. **Planteá $H_0$ y $H_1$** con la dirección correcta (mayor → $>$, menor → $<$, difiere → $\ne$).
> 3. **Calculá el p-valor** (con jamovi o a mano en proporciones / χ² / regresión con estadístico dado).
> 4. Aplicá la **regla universal**:
> $$p\text{-valor} < \alpha \;\Rightarrow\; \text{Rechazo } H_0$$
> 5. **Concluí** en términos del problema y con el nivel de significación.

---
🔙 Volver a la guía: [[00 - GUÍA 2do Parcial]]
