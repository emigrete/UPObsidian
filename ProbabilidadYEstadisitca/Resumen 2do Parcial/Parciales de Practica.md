---
title: "Parciales de Práctica — 2do Parcial"
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - parciales-practica
  - simulacro
  - jamovi
materia: Probabilidad y Estadística
parcial: 2
aliases:
  - "Simulacros 2P"
  - "Parciales de práctica Probabilidad"
---

# 🎯 Parciales de Práctica — 2do Parcial (Probabilidad y Estadística)

> [!tip] Cómo usar esta página
> Son **3 parciales completos** estilo cátedra. Hacé cada uno **entero** (con la [[Hoja de Resumen - 2do Parcial]] al lado y jamovi si tenés los datos crudos) y **recién después** abrí el callout *"Resolución"* para verificarte. Cada parcial cubre: **P6** (1 población), **P7** (2 muestras), **P8** (χ²), **P9** (regresión) y un **ítem conceptual**.

> [!note] De dónde salen
> Los datos, estadísticos y conclusiones están tomados de los **ejercicios resueltos** de [[01 - Práctico 6 - Inferencia para una población|P6]], [[02 - Práctico 7 - Comparación de dos muestras|P7]], [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]] y [[04 - Práctico 9 - Regresión y correlación|P9]], reorganizados y mezclados. Lo más importante (planteo + interpretación) es exacto; los p-valores verificalos en jamovi con los datos crudos.

---

# 📄 PARCIAL 1

### Ejercicio 1 — (P6, media)
Se mide la velocidad de escritura (MB/s) de **9 discos**: 540, 523, 503, 512, 498, 490, 502, 501, 480. El fabricante afirma que la velocidad media **supera los 500 MB/s** y la empresa decide **comprar** solo si eso es cierto. Trabajar con **α = 0,05** (datos ≈ normales).
**a)** Planteá las hipótesis e indicá qué significa rechazar $H_0$. **b)** Resolvé y concluí (jamovi: $\bar{x}=505{,}44$; $s=17{,}763$; $t_m=0{,}919$). **c)** ¿Qué error se podría estar cometiendo según la conclusión?

### Ejercicio 2 — (P7, dos muestras)
Para reducir la vulnerabilidad de un sistema se prueban dos versiones de capacitación: **A (en audio)** y **B (en texto)**. Reducción lograda por grupo (individuos distintos):
**A:** 3 · 3,2 · 2,1 · 3,7 · 2,2 — **B:** 1,9 · 2,5 · 3,1 · 2,2 · 2,9. Se sospecha que **A reduce más** que B. **α = 0,05**.
**a)** ¿Qué prueba corresponde y por qué? ¿Hay que verificar algún supuesto? **b)** Planteá las hipótesis. **c)** Concluí.

### Ejercicio 3 — (P8, datos categóricos)
Históricamente, los pedidos de servicios informáticos se reparten **40% / 32% / 28%** entre tres tipos. En una muestra de **n = 300** se observan **138 / 87 / 75**. ¿Cambió la distribución? **α = 0,04**.
**a)** ¿Qué prueba χ² es? Planteá $H_0/H_1$. **b)** Calculá las frecuencias esperadas. **c)** Concluí (jamovi: $\chi^2_m=4{,}51$, gl = 2).

### Ejercicio 4 — (P9, regresión)
Se relaciona **x = horas de codificación** con **y = cantidad de errores** (n = 7):
**x:** 12, 12, 15, 16, 17,5, 18, 22 — **y:** 5, 4, 3, 2, 3, 2, 1. jamovi: $\hat{y}=8{,}397-0{,}345\,x$, $R^2=0{,}823$.
**a)** Interpretá $a$ y $b$. **b)** Estimá los errores para 18 horas y calculá el residuo (observado = 2). **c)** ¿Tiene sentido estimar para x = 30? Justificá.

### Ejercicio 5 — (Conceptual)
Para el **Ejercicio 1** (compra de discos): **a)** redactá en palabras el **error tipo I** y el **error tipo II**. **b)** ¿Qué es la **potencia** acá? **c)** A quien **no quiere gastar de más**, ¿le conviene un α grande o chico?

> [!success]- ✅ Resolución — Parcial 1
> **Ej1 (t de una muestra, $\nu=8$).**
> a) "Supera 500" → cola derecha: $H_0:\mu\le500$ · $H_1:\mu>500$. **Rechazar $H_0$ = comprar.**
> b) $t_m=0{,}919 \Rightarrow pv=P(T_8>0{,}919)\approx0{,}192$. Como $0{,}192>0{,}05$ → **NO se rechaza $H_0$** → **no se compran** (no hay evidencia de que supere 500 MB/s).
> c) Como no se rechazó, el riesgo es de **error tipo II**: no comprar cuando en realidad la velocidad media sí supera 500.
>
> **Ej2 (t independientes — 2 grupos distintos).**
> a) **t para muestras independientes** (medias, variable cuantitativa, dos grupos de individuos distintos, σ desconocido). Sí: **Levene** (varianzas) y **Shapiro** (normalidad).
> b) $H_0:\mu_A=\mu_B$ · $H_1:\mu_A>\mu_B$.
> c) Normalidad $pv=0{,}506>0{,}05$ ✅ ; Levene $pv=0{,}344>0{,}05$ ✅ → **t de Student**. Prueba: $t_m=0{,}852$, gl = 8, $pv=0{,}209>0{,}05$ → **NO rechazo**: la versión A **no** reduce más la vulnerabilidad que la B.
>
> **Ej3 (χ² bondad de ajuste, $\nu=c-1=2$).**
> a) **Bondad de ajuste**: $H_0$ = se mantienen las proporciones históricas (40/32/28) · $H_1$ = cambiaron.
> b) $e_i=n\cdot p_i$ → $300\cdot0{,}40=120$ ; $300\cdot0{,}32=96$ ; $300\cdot0{,}28=84$. (Todas ≥ 5 ✅.)
> c) $\chi^2_m=4{,}51$, $pv=P(\chi^2_2>4{,}51)=0{,}105>0{,}04$ → **NO rechazo**: no hay evidencia de que las proporciones cambiaran.
>
> **Ej4 (regresión).**
> a) $a=8{,}397$: con 0 horas se estiman 8,397 errores → **sin sentido práctico** (no se programa con 0 horas). $b=-0{,}345$: por cada hora más de codificación, los errores **bajan** 0,345 en promedio.
> b) $\hat{y}(18)=8{,}397-0{,}345\cdot18=2{,}187$. Residuo $e=y-\hat{y}=2-2{,}187=-0{,}187$ (la observación quedó **por debajo** de la recta: se sobreestimó).
> c) **No**: x va de 12 a 22; estimar en x = 30 es **extrapolar** → no es válido.
>
> **Ej5 (conceptual).** Rechazar = comprar.
> a) **Error tipo I:** comprar los discos cuando en realidad **NO** superan los 500 MB/s. **Error tipo II:** **NO** comprarlos cuando en realidad **SÍ** superan los 500.
> b) **Potencia** $(1-\beta)$: concluir que superan los 500 (y comprar) cuando realmente los superan.
> c) Quien teme gastar de más teme el **error tipo I** → le conviene un **α chico (ej. 0,01)**.

---

# 📄 PARCIAL 2

### Ejercicio 1 — (P6, proporción)
Un antivirus afirma una **efectividad del 80%**. En una muestra de **n = 400** equipos resulta efectivo en el **75%**. ¿Hay evidencia de que la efectividad real es **menor** a la afirmada? **α = 0,01**.
**a)** Planteá $H_0/H_1$. **b)** Calculá el estadístico y el p-valor. **c)** Concluí y construí el **IC del 95%** para la proporción (dato: $z_{0,975}=1{,}96$).

### Ejercicio 2 — (P7, dos muestras — elegir la prueba)
Se quiere ver si una actualización **redujo** los errores. Se mide a **10 usuarios antes** de la actualización y a **otros 12 usuarios distintos después**. **α = 0,01**. jamovi: Shapiro $pv=0{,}898$ ; Levene $pv=0{,}323$ ; prueba $pv=0{,}003$.
**a)** ⚠️ ¿Apareadas o independientes? Justificá. **b)** ¿Student o Welch? **c)** Planteá y concluí.

### Ejercicio 3 — (P8, datos categóricos)
Sobre **una muestra de n = 250 empleados** se cruzan **sentido del voto** (a favor / en contra / indeciso) con **años de experiencia** (3 niveles) → tabla **3×3**. ¿El voto **depende** de la experiencia? **α = 0,05**. (El total de la fila *"a favor"* es 100 y el de la columna *"más de 5 años"* es 95.)
**a)** ¿Qué prueba χ² es? Planteá $H_0/H_1$ y dá los grados de libertad. **b)** Calculá la frecuencia **esperada** de la celda "a favor y más de 5 años". **c)** Concluí (jamovi: $\chi^2_m=10{,}7957$).

### Ejercicio 4 — (P9, regresión)
Se modela **y = daño ($) ** según **x = kilómetros recorridos** (n = 6): $\hat{y}=8{,}9831+0{,}102\,x$, con $r=0{,}37$ y $S_b=0{,}128$. **α = 0,04**.
**a)** Interpretá la pendiente; ¿es confiable la ordenada al origen? **b)** Probá si hay **asociación lineal** ($t_m=b/S_b$, gl = n−2). **c)** Calculá $r^2$ e indicá si el modelo sirve para pronosticar.

### Ejercicio 5 — (Conceptual, Verdadero/Falso — justificá)
**a)** "Si el p-valor es mayor que α, se **acepta** $H_0$."
**b)** "La prueba Chi-cuadrado puede ser **bilateral**."
**c)** "Si el coeficiente de correlación es **r = 0**, entonces las variables son **independientes**."

> [!success]- ✅ Resolución — Parcial 2
> **Ej1 (z de proporción).**
> a) "Menor a lo afirmado" → cola izquierda: $H_0:p\ge0{,}80$ · $H_1:p<0{,}80$. ($\hat{p}=0{,}75$.)
> b) $z_m=\dfrac{0{,}75-0{,}80}{\sqrt{0{,}80\cdot0{,}20/400}}=\dfrac{-0{,}05}{0{,}02}=-2{,}5 \Rightarrow pv=P(Z<-2{,}5)\approx0{,}0062$.
> c) $0{,}0062<0{,}01$ → **Rechazo $H_0$**: la efectividad real es **menor** al 80% (riesgo 1%). **IC 95%:** $0{,}75\pm1{,}96\sqrt{\tfrac{0{,}75\cdot0{,}25}{400}}=0{,}75\pm0{,}0424 \Rightarrow (0{,}7076\,;\,0{,}7924)$.
>
> **Ej2 (¡la trampa!).**
> a) **INDEPENDIENTES**: son **grupos distintos** (10 usuarios antes ≠ 12 usuarios después). Aunque diga "antes/después", los $n$ son distintos (10 ≠ 12) → no pueden ser pares → no apareadas.
> b) Shapiro $pv=0{,}898>0{,}01$ ✅ normalidad ; Levene $pv=0{,}323>0{,}01$ ✅ varianzas iguales → **t de Student**.
> c) $H_0:\mu_{antes}-\mu_{desp}=0$ · $H_1:\mu_{antes}-\mu_{desp}>0$ (si redujo, antes > después). $pv=0{,}003<0{,}01$ → **Rechazo**: la actualización **fue efectiva** (redujo los errores).
>
> **Ej3 (χ² independencia, $\nu=(3-1)(3-1)=4$).**
> a) **Independencia** (1 muestra, 2 variables): $H_0$ = el voto es independiente de la experiencia · $H_1$ = están relacionados. gl = 4.
> b) $e_{ij}=\dfrac{\text{fila}\cdot\text{col}}{n}=\dfrac{100\cdot95}{250}=38$.
> c) $\chi^2_m=10{,}7957 \Rightarrow pv=P(\chi^2_4>10{,}7957)\approx0{,}029<0{,}05$ → **Rechazo**: el voto y los años de experiencia **están relacionados** (riesgo 5%).
>
> **Ej4 (regresión, $\nu=n-2=4$).**
> a) $b=0{,}102$: por cada km más, el daño sube 0,102 (\$) en promedio. $a=8{,}9831$: corresponde a x = 0 km, **fuera del rango** de los datos → no tiene interpretación práctica.
> b) $H_0:\beta=0$ · $H_1:\beta\ne0$. $t_m=\dfrac{0{,}102}{0{,}128}=0{,}797 \Rightarrow pv=2\cdot P(T_4>0{,}797)\approx0{,}47>0{,}04$ → **NO rechazo**: **no** hay asociación lineal.
> c) $r^2=0{,}37^2\approx0{,}1369$ → solo el **13,69%** de la variación del daño se explica por los km → **modelo malo** para pronosticar (ratifica b).
>
> **Ej5 (V/F).**
> a) **FALSO.** Si $pv>\alpha$ se **NO rechaza** $H_0$ (no se "acepta": solo no hay evidencia suficiente para rechazarla).
> b) **FALSO.** χ² es **siempre unilateral derecha** ($pv=P(\chi^2>\chi^2_m)$), porque el estadístico es una suma de cuadrados ($\ge0$) y solo los valores grandes alejan de $H_0$.
> c) **FALSO.** $r=0$ indica que no hay asociación **lineal**, pero **puede haber relación no lineal** → $r=0$ **no** implica independencia.

---

# 📄 PARCIAL 3

### Ejercicio 1 — (P6, media — el α cambia la conclusión)
Una campaña busca **reducir** el consumo medio de agua por debajo de **136 litros**. Tomada una muestra, jamovi devuelve **p-valor = 0,044**.
**a)** Planteá $H_0/H_1$. **b)** Concluí con **α = 0,06**. **c)** Concluí con **α = 0,02**. ¿Qué se observa?

### Ejercicio 2 — (P7, diferencia de proporciones)
Se compara el **% de clientes fidelizados** entre **particulares** y **empresas** (muestras grandes). Se sospecha que el de **particulares es mayor**. **α = 0,05**. jamovi (z para diferencia de proporciones): **p-valor = 0,037**.
**a)** ¿Qué prueba es y qué ruta de jamovi? **b)** Planteá $H_0/H_1$. **c)** Concluí y decidí si conviene **intensificar la fidelización de empresas**.

### Ejercicio 3 — (P8, datos categóricos)
En **n = 255** piezas se registra el **tipo de defecto** (3 tipos) según la **línea de producción** (3 líneas) → tabla **3×3**. ¿El tipo de defecto **se distribuye igual** en las 3 líneas? **α = 0,05**. (El total de la fila *"grietas"* es 80 y el de la columna *"línea 3"* es 85.)
**a)** ¿Qué prueba χ² es? Planteá $H_0/H_1$ y gl. **b)** Calculá la esperada de "grietas en línea 3". **c)** Concluí (jamovi: $\chi^2_m=0{,}85$).

### Ejercicio 4 — (P9, regresión)
Se relaciona **x = consultas resueltas** con **y = satisfacción** (n = 12): $\hat{y}=50{,}14+4{,}87\,x$, con $r^2=0{,}735$ y $S_b=1{,}661$. **α = 0,04**.
**a)** Interpretá $a$ y $b$. **b)** ¿Qué % de la variación de la satisfacción explica el modelo? **c)** Probá si hay asociación lineal ($t_m=b/S_b$, gl = n−2) y concluí.

### Ejercicio 5 — (Conceptual — elegir la prueba)
Indicá **qué prueba** usarías y **por qué** en cada caso:
**a)** Se mide la velocidad de **16 computadoras** con el navegador C y **las mismas 16** con el navegador F.
**b)** Se mide la velocidad de **10 PC** con C y de **otras 12 PC distintas** con F.
**c)** En una prueba t para muestras independientes, **Levene da p-valor = 0,009** (α = 0,05): ¿usás Student o Welch?

> [!success]- ✅ Resolución — Parcial 3
> **Ej1 (t de una media).**
> a) "Reducir por debajo de 136" → cola izquierda: $H_0:\mu\ge136$ · $H_1:\mu<136$. Rechazar = la campaña **funcionó**.
> b) $\alpha=0{,}06$: $0{,}044<0{,}06$ → **Rechazo $H_0$** → la campaña **redujo** el consumo (riesgo 6%).
> c) $\alpha=0{,}02$: $0{,}044>0{,}02$ → **NO rechazo** → no se puede afirmar que haya bajado. **Observación:** con el mismo dato, **la conclusión cambia según el α**; cuando el p-valor cae cerca del α (acá entre 0,02 y 0,06), el riesgo elegido define el resultado.
>
> **Ej2 (z diferencia de proporciones).**
> a) **z para diferencia de 2 proporciones** (% de éxitos en 2 grupos). jamovi: *Recuento → Prueba de independencia χ² → tildar "Prueba z para diferencia de 2 proporciones"*.
> b) $H_0:p_{part}-p_{emp}=0$ · $H_1:p_{part}-p_{emp}>0$ (particulares mayor).
> c) $pv=0{,}037<0{,}05$ → **Rechazo**: los particulares están **más fidelizados** que las empresas → **sí conviene intensificar** las acciones de fidelización para empresas (riesgo 5%).
>
> **Ej3 (χ² homogeneidad, $\nu=(3-1)(3-1)=4$).**
> a) **Homogeneidad** (varias poblaciones/líneas, una variable categórica): $H_0$ = el tipo de defecto se distribuye **igual** en las 3 líneas · $H_1$ = difiere en alguna. gl = 4.
> b) $e=\dfrac{\text{fila}\cdot\text{col}}{n}=\dfrac{80\cdot85}{255}=26{,}667$.
> c) $\chi^2_m=0{,}85 \Rightarrow pv=P(\chi^2_4>0{,}85)\approx0{,}932>0{,}05$ → **NO rechazo**: no hay evidencia de diferencias en el tipo de defecto según la línea (es homogéneo).
>
> **Ej4 (regresión, $\nu=n-2=10$).**
> a) $a=50{,}14$: sin consultas resueltas (x = 0), la satisfacción media estimada es 50,14. $b=4{,}87$: por cada consulta resuelta más, la satisfacción sube 4,87 puntos en promedio.
> b) $r^2=0{,}735$ → el **73,5%** de la variación de la satisfacción se explica por las consultas resueltas.
> c) $H_0:\beta=0$ · $H_1:\beta\ne0$. $t_m=\dfrac{4{,}87}{1{,}661}=2{,}932 \Rightarrow pv=2\cdot P(T_{10}>2{,}932)\approx0{,}015<0{,}04$ → **Rechazo**: hay **asociación lineal** → las consultas resueltas sirven para estimar la satisfacción.
>
> **Ej5 (elegir la prueba).**
> a) **t apareadas**: son las **mismas 16 computadoras** medidas con dos navegadores (mismo individuo, dos medidas).
> b) **t independientes**: son **grupos distintos** (10 PC ≠ 12 PC) → además, distinto $n$ confirma que no son pares.
> c) Levene $pv=0{,}009<0{,}05$ → se rechaza la igualdad de varianzas → **varianzas distintas** → uso **t de Welch**.

---

> [!quote] Después de cada parcial
> Anotá en qué te equivocaste y volvé a esa sección de la [[Hoja de Resumen - 2do Parcial]]. Lo que más suma: **elegir bien la prueba** y **redactar la conclusión con el α**. ¡Éxitos! 🍀

🔙 [[00 - GUÍA 2do Parcial]] · [[Hoja de Resumen - 2do Parcial]] · [[Preguntas Integradoras - Probabilidad (2do Parcial)]]
