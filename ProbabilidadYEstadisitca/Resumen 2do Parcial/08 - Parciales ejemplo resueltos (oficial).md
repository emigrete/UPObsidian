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

# 📄 PARCIAL 1

---

## 🟦 P1 · Ejercicio 1 — Dos soluciones químicas (semiconductores)

> [!info] Datos
> Rapidez de acción (ms), distribución **normal**:
> - **Solución 1:** $\bar{x}_1=10{,}45$ · $s_1=0{,}2309$ · $n_1=12$
> - **Solución 2:** $\bar{x}_2=9{,}95$ · $s_2=0{,}5169$ · $n_2=10$

### a) ¿La rapidez media de la Solución 1 supera 10,3 ms? ($\alpha=0{,}07$)

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

**Identifico:** estimación por intervalo de una **media**, $\sigma$ desconocido → IC con **t**, $\nu=n_2-1=9$. Confianza $1-\alpha=0{,}96 \Rightarrow \alpha/2=0{,}02 \Rightarrow t_{0{,}98;\,9}\approx 2{,}2616$.

$$EM=t_{0{,}98;\,9}\cdot\frac{s_2}{\sqrt{n_2}}=2{,}2616\cdot\frac{0{,}5169}{\sqrt{10}}=2{,}2616\cdot 0{,}16346\approx 0{,}3698$$
$$IC_{0{,}96}=\bar x_2\pm EM = 9{,}95 \pm 0{,}3698 = (9{,}5802\,;\ 10{,}3198)$$

> [!success] Interpretación
> Con una **confianza del 96%** se estima que la **rapidez media de reacción de la Solución 2** está entre **9,5802 ms y 10,3198 ms**.
> **✅ Oficial:** $IC_{0{,}96}=(9{,}5802;\ 10{,}3198)$. ✔️

### c) ¿Difieren ambas soluciones? ($|t_m|=2{,}8324$, $\nu=12$, $\alpha=0{,}03$)

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

### a) Interpretar los coeficientes

- **Ordenada $a=88{,}5$:** sería el nivel de innovación estimado para un equipo de $x=0$ empleados. Pero la variable va de **3 a 7**, así que $x=0$ está **fuera del rango** → **no tiene interpretación práctica** (no existe un equipo de 0 personas).
- **Pendiente $b=0{,}23$:** por **cada empleado más** en el equipo, el nivel de innovación **aumenta 0,23 puntos en promedio**.

### b) ¿Hay asociación lineal? ($S_b=0{,}108$, $\alpha=0{,}05$)

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

### a) Interpretar los coeficientes

- **Ordenada $a=73{,}5$:** correspondería a un comprador de $x=0$ años → **sin sentido práctico** (no hay compradores de 0 años; además está fuera del rango 20–60).
- **Pendiente $b=-1{,}17$:** por **cada año más** de edad del comprador, el número medio anual de compras internacionales **disminuye en 1,17** (el signo negativo indica relación inversa).

### b) Prueba de hipótesis para $\beta$

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

"Abrir solo si supera 18" ⇒ lo que hay que demostrar (abrir) va en $H_1$:
$$H_0:\ \mu\le 18 \qquad H_1:\ \mu>18 \qquad (\textbf{Rechazar }H_0 = \text{abrir la maestría})$$

> [!note] Identificar los errores con la definición
> - **$\alpha$ = error tipo I** = $P(\text{rechazar }H_0 \mid H_0\text{ verdadera}) = P(\text{ofrecer} \mid \mu=18) = \mathbf{0{,}08}$.
> - **$\beta$ = error tipo II** = $P(\text{no rechazar }H_0 \mid H_0\text{ falsa}) = P(\text{no ofrecer} \mid \mu=19{,}5) = \mathbf{0{,}10}$.
>
> **✅ Oficial:** $H_0:\mu\le 18$, $H_1:\mu>18$ · $\alpha=0{,}08$ · $\beta=0{,}10$. ✔️

### b) ¿Conviene abrir la inscripción? ($\bar x=19{,}444$, $s=3{,}779$, $n=9$ carreras)

**Prueba t de una media**, $\nu=n-1=8$, "error del enunciado" $=\alpha=0{,}08$.
$$t_m=\frac{\bar x-\mu_0}{s/\sqrt n}=\frac{19{,}444-18}{3{,}779/\sqrt 9}=\frac{1{,}444}{1{,}2597}\approx 1{,}146$$
$$pv=P(T_8>1{,}146)\approx 0{,}1423$$

> [!failure] Conclusión
> $0{,}1423>0{,}08 \Rightarrow$ **NO Rechazo $H_0$**. **No** hay evidencias suficientes para aconsejar abrir la inscripción de la maestría, con una probabilidad de error del 8%.
> **Error posible:** como **no se rechazó**, el riesgo es de **error tipo II ($\beta$)** → no abrir la maestría cuando en realidad sí habría suficientes interesados.
> **✅ Oficial:** $pv=0{,}1423$ → no se aconseja abrir. ✔️

### c) IC del 92% para el número medio de interesados

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
