---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - ejercicios-resueltos
  - repaso
  - jamovi
tema: Ejercitación de repaso del 2do parcial resuelta y explicada (4 ejercicios)
materia: Probabilidad y Estadística
parcial: 2
---

# 📝 Ejercitación de repaso 2do parcial — resuelta y explicada

> [!abstract] Qué es esta página
> Resolución **paso a paso** de los 4 ejercicios de la *Ejercitación de repaso para el 2do parcial* (archivo [[Ejercicio de repaso 2do parcial '25-2.pdf|Ejercicio de repaso 2do parcial]]). Cada inciso sigue la misma receta de siempre:
> 1. **Identifico la prueba** con el [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|árbol de decisión]] (¿una o dos poblaciones? ¿media o proporción? ¿categórico? ¿relación?).
> 2. **Planteo $H_0$ y $H_1$** según la dirección del enunciado.
> 3. **jamovi / cálculo** → la ruta o la cuenta a mano.
> 4. **Decisión** con la regla universal $\boxed{pv<\alpha \Rightarrow \text{Rechazo }H_0}$.
> 5. **Conclusión** redactada en términos del problema.
>
> Teoría de respaldo: [[01 - Práctico 6 - Inferencia para una población|P6]] · [[02 - Práctico 7 - Comparación de dos muestras|P7]] · [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]] · [[04 - Práctico 9 - Regresión y correlación|P9]] · [[05 - Formulario y errores típicos|Formulario]] · [[06 - TODOS los ejercicios resueltos|Todos los ejercicios]].

> [!warning] Sobre los resultados numéricos
> Esta ejercitación **no trae respuestas oficiales**, así que los p-valores y estadísticos de abajo están **calculados por mí** con las fórmulas del curso. Las **interpretaciones y el planteo** (que es lo que más se evalúa) son lo importante; los números, verificalos en jamovi cuando tengas los datos crudos.

---

## 🟦 Ejercicio 1 — Tiempo de soporte técnico
> Soporte técnico: $n=16$, media $=4{,}12$, $s=1{,}31$. Se implementa la optimización **si el tiempo medio supera las 4 horas**. Tiempos ≈ normales.

> [!question]- Cómo leí el enunciado (las 3 preguntas → árbol)
> - **P1 ¿cuántos grupos?** Habla de **un solo** grupo (el tiempo de soporte) → rama *UNA población* (P6).
> - **P2 ¿qué dato?** "Tiempo (horas)" es un número que se promedia → es una **media**.
> - **¿σ o s?** Me dan $s=1{,}31$ (desvío muestral) ⇒ σ poblacional desconocido ⇒ la prueba es **t** (no z).
> - **P3 ¿qué piden?** "se implementa **si el tiempo medio supera** las 4 horas" → decidir → prueba de hipótesis, y "supera" → $H_1$ con `>` → **unilateral derecha**.
>
> → **t de una muestra**, $\nu=n-1=15$.

**Identifico:** una sola población, variable **cuantitativa** (tiempo), $\sigma$ **desconocido** y $n$ chico → **prueba t de una muestra** ($\nu=n-1=15$). Ver [[01 - Práctico 6 - Inferencia para una población|P6]].

### a) Hipótesis y decisión si no se rechaza $H_0$
"superior a 4 horas" ⇒ lo que se quiere demostrar va con `>` en $H_1$ (cola derecha):
$$H_0:\ \mu\le 4 \qquad H_1:\ \mu>4$$

> [!success] Qué se decide
> **Rechazar $H_0$ = implementar** el sistema de optimización. Por lo tanto, **si NO se rechaza $H_0$ → NO se implementa**: no hay evidencia de que el tiempo medio supere las 4 horas, así que se mantiene el flujo de trabajo actual.

> [!note]- Mirada extra (no la pide, pero ayuda a entender)
> Con los datos se puede ver el estadístico: $t_m=\dfrac{\bar x-\mu_0}{s/\sqrt n}=\dfrac{4{,}12-4}{1{,}31/\sqrt{16}}=\dfrac{0{,}12}{0{,}3275}\approx 0{,}37$. Eso da $pv=P(T_{15}>0{,}37)\approx0{,}36$, **muy alto**: para cualquier $\alpha$ razonable (0,05 / 0,10) **no se rechazaría** → no se implementaría. (El enunciado no da $\alpha$, por eso el inciso a) es conceptual.)

### b) Más precisión con el mismo $n$: ¿el nivel de significación sube o baja?
**Debe AUMENTAR.** Razonamiento con las reglas mentales del [[05 - Formulario y errores típicos|formulario]]:

- "Mayor precisión" = **menor error de muestreo (EM)** = intervalo **más angosto**.
- El EM es $EM = t_{(1-\alpha/2;\,\nu)}\cdot\dfrac{s}{\sqrt n}$. Con $n$ fijo, lo único que puedo mover es el valor crítico $t$.
- Para achicar el EM tengo que **bajar el nivel de confianza** $(1-\alpha)$, es decir **subir $\alpha$** (el nivel de significación).

> [!tip] En una línea
> $\uparrow\alpha \Rightarrow \downarrow$ confianza $\Rightarrow \downarrow$ valor crítico $\Rightarrow \downarrow EM \Rightarrow$ **más preciso**. Se gana precisión a costa de confianza.

---

## 🟩 Ejercicio 2 — Supermercado: sistema nuevo (A) vs tradicional (B)
> Tiempos en caja de clientes de la sucursal **A** (sistema nuevo) y **B** (tradicional), dos muestras de clientes **distintos**. Tiempos ≈ normales.

> [!question]- Cómo leí el enunciado
> - **P1 ¿cuántos grupos?** Dos sucursales, A y B → rama *DOS poblaciones* (P7).
> - **Sub-decisión clave:** los clientes de A son **distintos** de los de B (no es el mismo cliente medido en los dos sistemas) → **independientes**, NO apareadas.
> - **P2 ¿qué dato?** Comparo **medias** de una variable cuantitativa (tiempo en caja) → familia **t**.
> - **Paso previo:** al ser independientes, antes de la t va **Levene** para decidir Student vs Welch.
> - **Cola:** "se instala si el nuevo **mejora/reduce** el tiempo" → unilateral.
> - El inciso d) es **aparte**: ahí cambia a un **porcentaje** → otra rama (z proporción).

### a) ¿Qué test corresponde? (justificar)
**Prueba t para muestras INDEPENDIENTES.** Justificación:
- Comparo **medias** de una variable **cuantitativa** (tiempo en caja).
- Son **dos grupos de individuos distintos** (clientes de A ≠ clientes de B) → independientes, **no** apareadas.
- $\sigma$ desconocido en ambas → familia t. Ver [[02 - Práctico 7 - Comparación de dos muestras|P7]].

> [!note] Antes de la t: test de Levene
> En independientes primero corro **Levene** ($H_0:\sigma^2_A=\sigma^2_B$). Si $pv>\alpha$ → varianzas iguales → **t de Student**; si $pv<\alpha$ → uso **Welch**.

### b) Hipótesis estadísticas
Se instala en todas las sucursales si el sistema nuevo **mejora**, o sea **reduce** el tiempo en caja ($\mu_A<\mu_B$). Lo escribo con la diferencia $\mu_B-\mu_A$ para que el estadístico positivo del inciso c) apoye a $H_1$:
$$H_0:\ \mu_B-\mu_A = 0 \qquad H_1:\ \mu_B-\mu_A > 0$$
(equivale a $H_1:\mu_A<\mu_B$: el tradicional **tarda más** que el nuevo).

### c) Decisión con $t_m=1{,}47$, $\nu=10$, $\alpha=0{,}10$
Test **unilateral derecha**. Calculo el p-valor con la t (módulo **distrACTION → T-Distribution**, o a mano):
$$pv = P(T_{10}>1{,}47)\approx 0{,}087$$

> [!success] Conclusión
> $0{,}087 < 0{,}10 \Rightarrow$ **Rechazo $H_0$**. Con un riesgo del 10% hay evidencia de que el sistema nuevo reduce el tiempo en caja → **sí aconsejaría implementarlo** en todas las sucursales.

> [!warning] Ojo con el $\alpha$ (trampa típica)
> El p-valor (≈0,087) **cae entre 0,05 y 0,10**. Con $\alpha=0{,}10$ se rechaza, pero **con $\alpha=0{,}05$ NO se rechazaría** ($0{,}087>0{,}05$). La conclusión depende del riesgo que se fije.

### d) ¿Aumentó el % de clientes conformes? ($n=150$, $64\%$, histórico $62\%$, $\alpha=0{,}05$)

> [!question]- Cómo leí el enunciado (cambió la rama respecto de a/b/c)
> - **P2 ¿qué dato?** Acá ya no es un tiempo: es un **porcentaje de clientes conformes (64%)** → es una **proporción** → rama *UNA población → z para proporción* (P6).
> - Es **un solo** grupo comparado contra un valor histórico (62%), no dos grupos → no es diferencia de proporciones.
> - **Cola:** "¿**aumentó** respecto del 62%?" → $H_1$ con `>` → unilateral derecha. La hipótesis se arma con el valor afirmado **0,62** (no con el muestral 0,64).

**Identifico:** una **proporción** → **prueba z para una proporción** (a mano, no jamovi-t).

"¿aumentó respecto del 62%?" → cola derecha:
$$H_0:\ p\le 0{,}62 \qquad H_1:\ p>0{,}62$$
Con $\hat p=0{,}64$:
$$z_m=\frac{\hat p-p_0}{\sqrt{\dfrac{p_0(1-p_0)}{n}}}=\frac{0{,}64-0{,}62}{\sqrt{\dfrac{0{,}62\cdot0{,}38}{150}}}=\frac{0{,}02}{0{,}0396}\approx 0{,}505$$
$$pv = P(Z>0{,}505)\approx 0{,}307$$

> [!failure] Conclusión
> $0{,}307 > 0{,}05 \Rightarrow$ **NO Rechazo $H_0$**. **No** se puede afirmar que el porcentaje de clientes conformes haya aumentado: la suba de 62% a 64% es chica y entra dentro de la variabilidad del muestreo.

---

## 🟧 Ejercicio 3 — Formación académica vs rendimiento laboral (V/F)
> Una muestra de trabajadores, dos variables categóricas: **formación** (Media / Terciario / Universitario / Posgrado, 4 categorías) y **rendimiento** (Muy bueno / Bueno / Regular, 3 categorías). Pregunta: ¿están relacionadas? → tabla **4×3**. Ver [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]].

> [!question]- Cómo leí el enunciado
> - **P2 ¿qué dato?** Dos variables **categóricas** (formación y rendimiento) con **conteos** en una tabla → rama χ² (P8).
> - **¿Cuál χ²?** Es **una sola muestra** medida en **dos variables** que quiero ver si se relacionan → **χ² de independencia** (no bondad de ajuste, que sería una variable contra porcentajes fijos).
> - **gl:** tabla 4×3 → $\nu=(4-1)(3-1)=6$.
> - **Cola:** χ² siempre **unilateral derecha** (esto es justo lo que prueba el inciso b).

### a) "Se debe realizar un PH de independencia para datos categóricos" → **VERDADERO** ✅
Es **una sola muestra** medida en **dos variables categóricas** y se pregunta si **una depende de la otra** → ése es exactamente el escenario de la **prueba χ² de independencia**. Estadístico $\chi^2_m=\sum\dfrac{(o_i-e_i)^2}{e_i}$, con $\nu=(4-1)(3-1)=6$ grados de libertad.

### b) "Es una PH bilateral" → **FALSO** ❌
La prueba **χ² es SIEMPRE unilateral derecha**. Como $\chi^2_m=\sum\frac{(o-e)^2}{e}\ge 0$ (suma de cuadrados), sólo los valores **grandes** indican que lo observado se aleja de lo esperado bajo independencia. La región de rechazo está **únicamente en la cola derecha**:
$$pv=P(\chi^2>\chi^2_m)$$

### c) "Si a $\alpha=8\%$ se concluye que NO hay relación, entonces a $\alpha=10\%$ se concluiría que SÍ están relacionados" → **FALSO** ❌
- No rechazar a $\alpha=0{,}08$ sólo me dice que $pv>0{,}08$.
- Subir el riesgo a $\alpha=0{,}10$ haría **más fácil** rechazar, pero **sólo se rechazaría si $pv<0{,}10$**.
- Como únicamente sé que $pv>0{,}08$, el p-valor podría ser $0{,}09$ (ahí sí cambiaría: se rechazaría) **o** podría ser $0{,}40$ (ahí NO cambia nada).

> [!danger] Por qué es falso
> La afirmación da por seguro un cambio que **no está garantizado**. Sólo sería cierto si supiéramos que $0{,}08<pv<0{,}10$, dato que el enunciado **no** da. Subir $\alpha$ no *fuerza* a rechazar.

---

## 🟥 Ejercicio 4 — Chatbot: conocimiento técnico vs tiempo de resolución
> Regresión con $n=12$: $\ \hat y = 12{,}86 - 1{,}29\,x\ $, con $r^2=0{,}789$. Variables: **x = nivel de conocimiento técnico (1 a 5)**, **y = tiempo de resolución**. Ver [[04 - Práctico 9 - Regresión y correlación|P9]].

> [!question]- Cómo leí el enunciado
> - **P1/P2:** dos variables cuantitativas ($x=$ conocimiento, $y=$ tiempo) con una **recta** dada → rama *regresión* (P9).
> - **Pendiente negativa** ($b=-1{,}29$) → la correlación será **negativa**: $r=-\sqrt{r^2}$.
> - **Qué pide cada inciso:** a) interpretar $a$ y $b$; b) leer $r^2$ como % explicado; c) PH para $\beta$ ($H_0:\beta=0$). Como no dan $S_b$, el estadístico se saca desde $r$ con $t_m=\frac{r\sqrt{n-2}}{\sqrt{1-r^2}}$.

### a) Interpretar los coeficientes
- **Ordenada $a=12{,}86$:** tiempo medio estimado para un usuario con conocimiento $x=0$. Pero la escala va de **1 a 5**, así que $x=0$ está **fuera del rango** y **no tiene interpretación práctica** (no existe un nivel de conocimiento 0).
- **Pendiente $b=-1{,}29$:** por cada **punto que aumenta el nivel de conocimiento técnico**, el **tiempo de resolución baja 1,29** (unidades de tiempo) **en promedio**. El signo negativo es coherente: a más conocimiento, menos tiempo.

### b) Coeficiente de determinación $r^2=0{,}789$
El **78,9% de la variabilidad del tiempo de resolución** queda **explicada por el nivel de conocimiento técnico** a través de la recta. El 21,1% restante se debe a otros factores no incluidos en el modelo. Un $r^2$ alto indica **buen ajuste lineal**.

### c) Prueba de hipótesis para el coeficiente de regresión ($\beta$)
$$H_0:\ \beta=0 \quad(\text{no hay asociación lineal}) \qquad H_1:\ \beta\ne 0 \quad(\text{sí la hay})$$
No dan $S_b$, pero el estadístico se puede obtener desde $r$. Como $b<0$, la correlación es **negativa**: $r=-\sqrt{0{,}789}\approx-0{,}888$.
$$t_m=\frac{r\sqrt{n-2}}{\sqrt{1-r^2}}=\frac{-0{,}888\cdot\sqrt{10}}{\sqrt{0{,}211}}=\frac{-2{,}81}{0{,}459}\approx -6{,}11 \qquad (\nu=n-2=10)$$
$$pv = 2\cdot P(T_{10}>6{,}11)\approx 0{,}0001 \;(\text{muy chico})$$

> [!success] Conclusión
> $pv\approx 0 < \alpha$ (cualquier $\alpha$ usual) ⇒ **Rechazo $H_0$**. **Por qué:** el $r^2$ es alto (78,9%) y el estadístico $t_m\approx-6{,}11$ deja un p-valor ínfimo → **las variables están asociadas linealmente** → el nivel de conocimiento técnico **sí sirve** para estimar el tiempo de resolución del chatbot.

---

> [!quote] Receta de examen (los 5 pasos)
> 1) Clasificá con el [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|árbol de decisión]] · 2) Planteá $H_0$/$H_1$ con la dirección correcta · 3) p-valor (jamovi o cuenta a mano en proporciones/χ²/regresión) · 4) **$pv<\alpha \Rightarrow$ Rechazo $H_0$** · 5) Conclusión en términos del problema con el $\alpha$.
> **Trampas que aparecieron acá:** rechazar = tomar la acción (Ej 1a) · más precisión con $n$ fijo ⇒ **subir $\alpha$** (Ej 1b) · "antes/después" con grupos distintos = **independientes** (Ej 2) · χ² **siempre unilateral derecha** (Ej 3b) · subir $\alpha$ **no garantiza** rechazar (Ej 3c) · no interpretar la ordenada fuera del rango (Ej 4a).

Ver también: [[06 - TODOS los ejercicios resueltos]] · [[05 - Formulario y errores típicos]] · [[00 - GUÍA 2do Parcial]].
