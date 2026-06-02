---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - ejercicios-resueltos
  - jamovi
tema: Ejercicios resueltos completos P6 a P9 (paso a paso)
---

# ✅ TODOS los ejercicios resueltos — Prácticos 6 a 9

> [!abstract] Cómo usar esta página
> Acá están **todos** los ejercicios del 2do parcial, resueltos **uno por uno y paso a paso**, contando *cómo se fue pensando cada uno* tal como se hizo en clase con **jamovi**. Cada ejercicio sigue siempre la misma secuencia:
> 1. **Identifico la prueba** (con el [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|árbol de decisión]]: ¿una o dos poblaciones? ¿media o proporción? ¿categórico? ¿relación?).
> 2. **Planteo $H_0$ y $H_1$** mirando la dirección que pide el enunciado.
> 3. **jamovi / cálculo** → la ruta de menú exacta o la cuenta a mano, y qué número leer.
> 4. **Decisión** con la regla universal: $\boxed{pv<\alpha \Rightarrow \text{Rechazo }H_0}$.
> 5. **Conclusión** redactada en términos del problema y con el $\alpha$.
>
> Los valores numéricos son los de las **respuestas oficiales** (Patricia Arnal). Teoría detallada en: [[01 - Práctico 6 - Inferencia para una población|P6]] · [[02 - Práctico 7 - Comparación de dos muestras|P7]] · [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]] · [[04 - Práctico 9 - Regresión y correlación|P9]] · [[00 - GUÍA 2do Parcial|Guía]].

> [!note]- ⚙️ Recordatorio de supuestos (para las pruebas t)
> En las pruebas de **supuestos** lo bueno es **NO rechazar** (pv alto): Shapiro-Wilk ($H_0$: datos normales) y Levene ($H_0$: varianzas iguales). Si **Levene da pv < α → varianzas distintas → uso Welch**. Es al revés que en la prueba principal.

---

# 📘 PRÁCTICO 6 — Inferencia para una población

> [!question]- Cómo leo un enunciado de este práctico (para decidir la prueba)
> Acá **siempre hay UN solo grupo**. Las preguntas que me hago:
> - **¿El dato es una media o una proporción?**
>   - número que se promedia (tiempo, temperatura, velocidad…) → **media** → familia **t** (σ desconocido, $\nu=n-1$).
>   - porcentaje / "X de cada N" / % de éxitos → **proporción** → familia **z** (cuenta a mano).
> - **¿Me piden estimar o decidir?**
>   - "estimar / IC / con confianza del X%" → **intervalo de confianza** (sin $H_0/H_1$).
>   - "probar / supera / es menor / difiere de…" → **prueba de hipótesis**.
> - **La cola sale del verbo:** "supera/aumenta" → `>` (derecha) · "es menor/baja" → `<` (izquierda) · "difiere de" → `≠` (bilateral, pv ×2).
> - **Regla anti-trampa:** la hipótesis se arma con el **valor afirmado** del enunciado (ej. 0,62), nunca con el **valor muestral** ($\hat p$).

### Ej 1 — Estimadores puntuales (teoría)
1. **Idea clave:** un **estimador** se calcula con la **muestra** (es variable aleatoria); un **parámetro** describe la **población** (es constante).
2. **a)** Clasifico cada símbolo → son **estimadores puntuales**: $S^2$, $\bar{x}$, $\hat{p}$. ✅ ($\mu$ y $\sigma^2$ son **parámetros**, no estimadores).
3. **b)** Recuerdo las propiedades de la distribución de $\bar{X}$: **i)** $\mu(\bar{X})=\mu$ ✅ y **iii)** $\sigma(\bar{X})=\dfrac{\sigma}{\sqrt n}$ ✅. La **ii)** es **falsa** (la dispersión de $\bar X$ se achica con $\sqrt n$, no es $\sigma$).

### Ej 2 — IC media, temperatura (n=14) · *t de una muestra*
1. **Identifico:** quieren **estimar la media** de una variable cuantitativa con $\sigma$ desconocido y $n$ chico → **IC con t**, $\nu=n-1=13$.
2. **jamovi:** cargo la columna de 14 temperaturas → **Pruebas t → Prueba t de una Muestra** → paso la variable → **Estadísticas Adicionales → Diferencia de medias → tildar Intervalo de confianza** al 95%. ⚠️ Dejo **Hipótesis en "≠ Valor de prueba"** para que el IC salga **bilateral**.
3. **a)** Leo el intervalo → **IC(95%) = (89,3 ; 93)** → "con 95% de confianza, la temperatura media está entre 89,3 °C y 93 °C". **EM = amplitud/2 = (93−89,3)/2 = 1,85 °C**.
4. **b)** Cambio el nivel a 90% → **IC(90%) = (89,6 ; 92,7)** ; **EM = 1,55**.
5. **c)** El del 90% es **más angosto → más preciso** (menos confianza ⇒ menos EM).
6. **d)** Con más mediciones (↑$n$, igual 95%) → **IC(95%) = (90,3 ; 92,9)**, más preciso que el de a) porque **↑n ⇒ ↓EM**.

### Ej 3 — IC media, criptomoneda (n=10) · *t de una muestra*
1. **Identifico:** media, $\sigma$ desconocido, $n$ chico → **IC con t**.
2. **jamovi:** misma ruta que el Ej 2 (Una Muestra → IC al 95%).
3. **a)** **IC(95%) = (42,1 ; 52,2)** → "el precio medio está entre 42.100 y 52.200 dólares con 95% de confianza".
4. **b)** Si ↑$n$ con la misma confianza → intervalo **menos amplio** (el EM se divide por $\sqrt n$).
5. **c)** Se aclara que la variable es normal porque, **con muestra chica, es condición para poder usar la t**.

### Ej 4 — IC proporción, clientes (n=160, x=64) · *z*
1. **Identifico:** es un **porcentaje/proporción** → **IC con z** (a mano), no t.
2. **Cálculo:** $\hat p = 64/160 = 0{,}40$. Busco $z_{(1-\alpha/2)}$ y armo $\hat p \pm z\sqrt{\hat p(1-\hat p)/n}$.
3. **a)** **IC(96%) = (0,32 ; 0,48)** → "con 96% de confianza, el % que cambiaría de plan está entre 32% y 48%". **EM = 0,063**.
4. **b)** Con 90% (menos confianza) el **EM es menor → más preciso**.

### Ej 5 — IC proporción, app (n=150, x=117) · *z*
1. **Identifico:** proporción → **IC con z** a mano.
2. **Cálculo:** $\hat p = 117/150 = 0{,}78$ ; $z_{0,975}=1{,}96$ ; armo el intervalo.
3. **Resultado:** **IC(95%) = (0,714 ; 0,846)** → "el % de usuarios interesados está entre 71,4% y 84,6%".

### Ej 6 — Relación EM/α/n (teoría)
1. **Razono con las reglas mentales:** ↑$n$ ⇒ ↓EM (más preciso) ; ↑confianza ⇒ ↑EM (menos preciso) ; más amplitud = menos precisión.
2. **Resultado:** **solo la c) es correcta.** (a) Falsa: ↑n **disminuye** el EM. (b) Falsa: más amplitud ⇒ **menos** precisión.

### Ej 7 — Doble estimación (n=30) · *t y z*
1. **Identifico:** piden **dos IC** sobre la misma muestra → uno de **media (t)** y uno de **proporción (z)**.
2. **Media (jamovi, Una Muestra, IC 94%):** **IC(94%) = (31,4 ; 38,6)** minutos.
3. **Proporción (a mano, z):** **IC(94%) = (0,54 ; 0,86)** → entre 54% y 86% elegiría la red.

### Ej 8 — Hipótesis y errores (teoría)
1. **A.a)** El planteo correcto, para "difiere de 1,25", es **$H_1:\mu\ne1{,}25$** (bilateral).
2. **A.b)** Interpretaciones correctas del IC: "la probabilidad de que μ pertenezca al intervalo es 0,90" y "90 de cada 100 intervalos contienen a μ".
3. **B) Identifico cada error según la acción "detener":** $\alpha=P(\text{detener cuando media} \ge \mu_0)$ ; $\beta=P(\text{no detener cuando media} < \mu_0)$ ; potencia $1-\beta=P(\text{detener cuando media} < \mu_0)$.

### Ej 9 — Diseño web (plantear) · *PH media*
1. **Hipótesis de investigación:** "mejorar" = subir respecto a 75.
2. **Planteo:** $H_0:\mu\le75 \;;\; H_1:\mu>75$ (cola derecha, porque "mejora" = aumenta).
3. **Riesgos:** concluir mejora sin haberla = **α=0,03** ; no detectar la mejora real = **β=0,10**.
4. **Potencia:** concluir que el diseño mejoró cuando realmente mejoró.

### Ej 10 — Black Friday · *PH media*
1. **a) Planteo:** "superan 200" → $H_0:\mu\le200 \;;\; H_1:\mu>200$ ; α=0,02 ; β=0,05.
2. **b) Cálculo del p-valor a partir del estadístico dado:** con $t_m=2{,}014$ y $n=28$ (gl=27), uso **distrACTION (T-Distribution)** → **pv = P(t>2,014)=0,027**.
3. **Interpretación:** ese 0,027 es el **mínimo nivel de significación** con el que se rechazaría $H_0$ (= definición de p-valor).

### Ej 11 — Discos duros ("excede 500 MB/s", n=9, α=0,05) · *t de una muestra*
1. **Identifico:** una media, σ desconocido → **t de una muestra**.
2. **Planteo:** "excede 500" → $H_0:\mu\le500 \;;\; H_1:\mu>500$ (Rechazar $H_0$ = comprar).
3. **jamovi:** cargo los 9 datos → Una Muestra → **Valor de prueba = 500** → Hipótesis **"> Valor de prueba"**. Devuelve $\bar x=505{,}44$, $t_m=0{,}919$, **pv=0,192**.
4. **Decisión:** 0,192 **>** 0,05 → **NO Rechazo $H_0$**.
5. **Conclusión:** ❌ **no comprar** — no hay evidencia de que la velocidad supere 500 MB/s (riesgo 5%).

### Ej 12 — Aeropuerto Río (n=12, "media > 7", α=10%) · *t de una muestra*
1. **a) Planteo:** "servicio superior" = media > 7 → $H_0:\mu\le7 \;;\; H_1:\mu>7$.
2. **jamovi:** Una Muestra, Valor de prueba = 7, Hipótesis ">", tildando además **Prueba de Normalidad** (se verifica) → **pv=0,042**.
3. **Decisión:** 0,042 **<** 0,10 → **Rechazo $H_0$** → **es de servicio superior**.
4. **b)** Pido también el **IC(90%) = (7,03 ; 7,97)** → como **todo el intervalo es > 7**, **ratifica** la decisión.

### Ej 13 — Barra de progreso (n=25, "tiempo < 8,5", α=8%) · *t de una muestra*
1. **a) Planteo:** "es menor a 8,5" → $H_0:\mu\ge8{,}5 \;;\; H_1:\mu<8{,}5$ (cola izquierda).
2. **jamovi:** Una Muestra, Valor de prueba = 8,5, Hipótesis "<". Devuelve $t_m=-1{,}263$, **pv=0,109**.
3. **Decisión:** 0,109 **>** 0,08 → **NO Rechazo**: la barra **no** tuvo el efecto deseado.
4. **b)** Como no rechacé, el error posible es el **tipo II**: concluir que no tuvo efecto cuando el tiempo percibido sí es menor a 8,5.

### Ej 14 — Sistema en la nube (plantear) · *PH proporción*
1. **Hipótesis de investigación:** rentable si el interés **supera el 40%**.
2. **Planteo:** $H_0:p\le0{,}4 \;;\; H_1:p>0{,}4$.
3. **Errores:** **Tipo I** = concluir rentable cuando el interés ≤ 40% ; **Tipo II** = concluir no rentable cuando el interés > 40%.

### Ej 15 — Compras online ("¿bajó del 23%?", α=6%) · *PH proporción*
1. **Identifico:** una proporción → **z de proporción a mano** (no jamovi-t).
2. **Planteo:** "¿disminuyó del 23%?" → $H_0:p\ge0{,}23 \;;\; H_1:p<0{,}23$.
3. **Cálculo:** $z_m=\dfrac{\hat p-0{,}23}{\sqrt{0{,}23\cdot0{,}77/n}}$ → p-valor con la Normal (cola izquierda) → **pv=0,1254**.
4. **Decisión:** 0,1254 **>** 0,06 → **NO Rechazo**: no se puede concluir que el porcentaje haya disminuido.

### Ej 16 — Antivirus (afirma 80%, muestra 75% de 400, α=1%) · *PH proporción*
1. **a/b) Planteo:** "¿es menor que el 80% afirmado?" → $H_0:p\ge0{,}80 \;;\; H_1:p<0{,}80$.
2. **Cálculo:** $\hat p=0{,}75$ ; $z_m=\dfrac{0{,}75-0{,}80}{\sqrt{0{,}80\cdot0{,}20/400}}$ → **pv=0,0062** (cola izquierda).
3. **Decisión:** 0,0062 **<** 0,01 → **Rechazo**: la proporción real es **menor** que la estimada.
4. **c)** Construyo el IC de la proporción → **IC(95%) = (0,7076 ; 0,7924)**.

### Ej 17 — Operador internet (n=20, "más de 50 hs", α=7%) · *t de una muestra*
1. **a) IC primero (jamovi, Una Muestra, IC 95%):** **IC(95%) = (46,94 ; 59,86)**.
2. **b) Planteo:** "más de 50 hs" → $H_0:\mu\le50 \;;\; H_1:\mu>50$.
3. **jamovi:** Valor de prueba = 50, Hipótesis ">" → **pv=0,1421**.
4. **Decisión:** 0,1421 **>** 0,07 → **NO Rechazo**: no hay evidencia (posible **error tipo II** → se pierde la oportunidad).

### Ej 18 — Consumo de agua ("¿bajó de 136?", pv=0,044) · *PH media*
1. **a) Planteo:** "¿disminuyó de 136?" → $H_0:\mu\ge136 \;;\; H_1:\mu<136$. Me dan directamente **pv=0,044** y comparo con dos α distintos:
   - Con **α=0,06**: 0,044 **<** 0,06 → **Rechazo** → la campaña fue efectiva.
   - Con **α=0,02**: 0,044 **>** 0,02 → **NO Rechazo** → **cambia la conclusión** (no se puede afirmar que fue efectiva).
2. **b) Errores:** con α=0,06 el riesgo es **Tipo I** (afirmar efectiva sin serlo) ; con α=0,02 el riesgo es **Tipo II** (decir que no fue efectiva cuando sí lo fue).

### Ej 19 — Interfaz ("¿difiere del 65%?") · *PH proporción*
1. **a) Planteo:** "difiere de 65%" → **bilateral**: $H_0:p=0{,}65 \;;\; H_1:p\ne0{,}65$. **Se rechazó $H_0$**. Potencia: concluir correctamente que difiere cuando realmente difiere.
2. **b) EM desde la amplitud:** amplitud 0,134 → **EM = 0,134/2 = 0,067**.
3. **c)** Si el EM fuera 0,072 (mayor) → intervalo más ancho → menos preciso → corresponde a un nivel de confianza **mayor**.

### Ej 20 — Detección de intrusiones (p=0,80) · *PH proporción*
1. **a) Planteo:** "¿mejora (supera) el 80%?" → $H_0:p=0{,}80 \;;\; H_1:p>0{,}80$ ; **β=0,07**.
2. **b) p-valor desde el estadístico:** $z_m=2{,}725$ → con la Normal (cola derecha) → **pv=0,003**.
3. **Decisión:** 0,003 **<** 0,025 → **Rechazo**: la herramienta **mejora** la detección.

---

# 📗 PRÁCTICO 7 — Comparación de dos muestras

> [!tip] El primer paso SIEMPRE: ¿apareadas o independientes?
> Mismo individuo medido 2 veces = **apareadas** · dos grupos distintos = **independientes** (en independientes, antes de la t corro **Levene** y **Shapiro** en jamovi).

> [!question]- Cómo leo un enunciado de este práctico (para decidir la prueba)
> Acá **siempre hay DOS grupos**. El orden de preguntas:
> - **¿Medias o proporciones?**
>   - comparo **promedios** de una variable cuantitativa → familia **t** de dos muestras.
>   - comparo **dos porcentajes** → **z de diferencia de proporciones**.
> - **Si es t → ¿apareadas o independientes? (LA decisión clave)**
>   - **mismo** individuo medido dos veces (antes/después, pre-post, dos métodos sobre la misma unidad) → **APAREADAS**. Pista extra: el $n$ es **igual** en los dos momentos. Se trabaja con $d=\text{antes}-\text{después}$.
>   - individuos **distintos** en cada grupo (sucursal A vs B, hombres vs mujeres, trabaja vs no trabaja) → **INDEPENDIENTES**.
>   - ⚠️ **Trampa repetida:** "antes/después" pero con **$n$ distintos** (ej. 10 vs 12) → son grupos distintos → **independientes**, NO apareadas.
> - **Solo en independientes** corro **Levene** primero: $pv>\alpha$ → Student · $pv<\alpha$ → **Welch**.
> - **La cola sale del verbo:** "difieren" → `≠` bilateral · "A mejora/reduce/supera a B" → unilateral en la dirección que diga.

### Ej 1 — Autenticación (10 antes / 12 después, α=1%) · *t INDEPENDIENTES* ⚠️
1. **Identifico (¡trampa!):** dice "antes/después", pero son **10 usuarios** antes y **12** después → **grupos distintos** (10 ≠ 12) → **muestras INDEPENDIENTES**, no apareadas.
2. **jamovi:** dos columnas (valor + grupo) → **Pruebas t → Muestras Independientes** → tildo **Levene** y **Shapiro**. Normalidad pv=0,898 ✅ ; homogeneidad pv=0,323 ✅ → uso **Student**.
3. **Planteo:** "¿fue efectiva (bajó el tiempo)?" → $H_0:\mu_{antes}=\mu_{desp} \;;\; H_1:\mu_{antes}-\mu_{desp}>0$.
4. **Decisión:** **pv=0,003 < 0,01 → Rechazo**: la actualización **fue efectiva** (riesgo 1%).

### Ej 2 — Fatiga (|tm|=2,5259, v=14, α=4%) · *t APAREADAS*
1. **Identifico:** el **mismo individuo** medido con y sin fatiga → **apareadas**. Defino **D = con fatiga − sin fatiga**.
2. **Planteo:** "¿la fatiga altera (aumenta el tiempo)?" → $H_0:\mu_D=0 \;;\; H_1:\mu_D>0$.
3. **p-valor desde el estadístico (distrACTION, gl=14):** **pv = P(t>2,5259)=0,012**.
4. **Decisión:** 0,012 **<** 0,04 → **Rechazo**: la fatiga **altera** el desempeño.

### Ej 3 — Zinc río (6 puntos, α=1%) · *t APAREADAS*
1. **Identifico:** el **mismo punto** medido en superficie y fondo → dependientes → **apareadas**. **D = superficial − fondo**.
2. **Planteo:** "¿el fondo supera a la superficie?" → si fondo > superficie, D < 0 → $H_0:\mu_D=0 \;;\; H_1:\mu_D<0$.
3. **jamovi:** Muestras Pareadas (columnas Superficie y Fondo, en ese orden) → **pv=0,008**.
4. **Decisión:** 0,008 **<** 0,01 → **Rechazo**: el zinc del fondo **supera** al de superficie.

### Ej 4 — Matemática vs trabajo (t=−1,0697, v=32, α=8%) · *t INDEPENDIENTES*
1. **a) Identifico:** calificación según **trabaja / no trabaja** = 2 grupos distintos → **independientes**.
2. **b)** **Sí** corresponde test de homogeneidad: $H_0:\sigma^2_{nt}=\sigma^2_t$ (Levene).
3. **c) Planteo:** "¿los que trabajan rinden peor?" → $H_0:\mu_{nt}=\mu_t \;;\; H_1:\mu_{nt}>\mu_t$. Con t=−1,0697 → **pv=0,146**.
4. **Decisión:** 0,146 **>** 0,08 → **NO Rechazo**: no hay evidencia de peor rendimiento de los que trabajan.

### Ej 5 — Algoritmos A vs B (pv=0,0431, α=5%) · *t INDEPENDIENTES*
1. **Identifico:** dos algoritmos distintos → **independientes** (y por eso **sí** se hace Levene).
2. **Planteo:** "¿difieren en eficacia?" → bilateral: $H_0:\mu_A=\mu_B \;;\; H_1:\mu_A\ne\mu_B$.
3. **Decisión:** **pv=0,0431 < 0,05 → Rechazo**: los algoritmos **difieren** en eficacia.

### Ej 6 — Satisfacción H vs M (α=10%) · *z DIFERENCIA DE PROPORCIONES*
1. **Identifico:** comparo el **% de satisfechos** de dos grupos → **z diferencia de proporciones**.
2. **Planteo:** "¿difieren?" → bilateral: $H_0:p_M=p_H \;;\; H_1:p_M\ne p_H$.
3. **jamovi:** **Recuento → Prueba de independencia χ²** → destildo χ², **tildo "Prueba z para diferencia de 2 proporciones"** → **pv=0,145**.
4. **Decisión:** 0,145 **>** 0,10 → **NO Rechazo**: no hay diferencia entre hombres y mujeres.

### Ej 7 — Inseguridad CABA vs Provincia (α=4%) · *z DIFERENCIA DE PROPORCIONES*
1. **Planteo:** "¿hay más inseguridad en provincia?" → $H_0:p_{CABA}=p_{BsAs} \;;\; H_1:p_{BsAs}>p_{CABA}$ (cola derecha).
2. **jamovi:** misma ruta de diferencia de proporciones, eligiendo la dirección correcta → **pv=0,019**.
3. **Decisión:** 0,019 **<** 0,04 → **Rechazo**: **mayor inseguridad en provincia**.

### Ej 8 — Sucursales A vs B (α=4%) · *z DIFERENCIA DE PROPORCIONES*
1. **Planteo:** "¿B es más efectiva que A?" → $H_0:p_B-p_A\le0 \;;\; H_1:p_B-p_A>0$.
2. **jamovi:** diferencia de proporciones, cola derecha → **pv=0,06**.
3. **Decisión:** 0,06 **>** 0,04 → **NO Rechazo**: B **no** es más efectiva.

### Ej 9 — Tiempos de carga antes/después (7 apps, α=6%) · *t APAREADAS*
1. **a) Supuesto:** Normalidad **pv=0,218 ✅**.
2. **b) Identifico:** **las mismas 7 apps** antes y después → **apareadas** (acá sí coincide el n).
3. **c) Planteo:** **D = antes − después** ; "¿se redujo el tiempo?" → $H_1:\mu_D>0$, con $H_0:\mu_D=0$.
4. **Decisión:** **pv=0,026 < 0,06 → Rechazo**: la optimización **tuvo el efecto deseado**.

### Ej 10 — Satisfacción según CEO (α=4%) · *t INDEPENDIENTES*
1. **a) Identifico:** dos estilos de CEO = grupos distintos → **independientes**. Levene **pv=0,029 < α → varianzas distintas → Welch**.
2. **Planteo:** $H_0:\mu_A-\mu_B\ge0 \;;\; H_1:\mu_A-\mu_B<0$.
3. **b) Decisión:** leyendo el p de **Welch** → **pv=0,002 < 0,04 → Rechazo**: las sospechas del investigador **son ciertas**.

### Ej 11 — Ventas centro vs shopping (α=6%) · *t INDEPENDIENTES*
1. **a) Supuesto:** Normalidad ✅.
2. **b) Identifico:** dos locales distintos → **independientes**. Levene **pv=0,009 < α → varianzas distintas → Welch**. Planteo: $H_0:\mu_c-\mu_s=0 \;;\; H_1:\mu_c-\mu_s>0$.
3. **c) Decisión:** **pv=0,094 > 0,06 → NO Rechazo**: no hay evidencia de que el centro venda más → **no conviene la campaña**.

### Ej 12 — Fidelización particulares vs empresas (α=5%) · *z DIFERENCIA DE PROPORCIONES*
1. **Planteo:** $H_0:p_{part}=p_{emp} \;;\; H_1:p_{part}>p_{emp}$.
2. **jamovi:** diferencia de proporciones → **pv=0,037**.
3. **Decisión:** 0,037 **<** 0,05 → **Rechazo**: conviene **intensificar la fidelización en empresas** (los particulares fidelizan más).

### Ej 13 — Navegadores C vs F (|t|=1,988, v=15, α=5%) · *t APAREADAS*
1. **Identifico:** las **mismas 16 computadoras** con ambos navegadores → **apareadas**. **D = C − F**.
2. **Planteo:** "¿C es más rápido?" → menor tiempo → $H_0:\mu_D=0 \;;\; H_1:\mu_D<0$.
3. **p-valor (distrACTION, gl=15):** **pv = P(t>1,988)=0,033 < 0,05 → Rechazo**: **C es más rápido**.
4. ⚠️ **Variante bilateral:** si solo se quisiera ver si **difieren**, $pv = 2\cdot0{,}033 = 0{,}066$.

---

# 📙 PRÁCTICO 8 — Datos categóricos (Chi-cuadrado)

> [!tip] Recordá antes de cada uno
> Todo usa $\chi^2_m=\sum\dfrac{(o_i-e_i)^2}{e_i}$ y es **unilateral derecha** → $pv=P(\chi^2>\chi^2_m)$. **Bondad de ajuste:** $e_i=n\,p_i$, gl=c−1, jamovi *N Resultados*. **Indep./Homog.:** $e_{ij}=\dfrac{\text{fila}\cdot\text{col}}{n}$, gl=(f−1)(c−1), jamovi *Prueba de independencia χ²*.

> [!question]- Cómo leo un enunciado de este práctico (cuál de los χ² es)
> El disparador: **hay categorías y conteos** (no números que se promedian) → es χ². Después distingo **cuál**:
> - **¿Comparo UNA variable contra proporciones dadas?** (históricas, uniforme, "¿sigue tal distribución?") → **BONDAD DE AJUSTE**. $e_i=n\cdot p_i$, $\nu=c-1$ (categorías − 1).
> - **¿Cruzo DOS variables en una tabla y pregunto si se relacionan/dependen?** → **INDEPENDENCIA**. $\nu=(f-1)(c-1)$.
> - **¿Una variable comparada entre VARIAS poblaciones/grupos** ("¿difiere según la línea/proveedor/rubro?")? → **HOMOGENEIDAD**. Misma operación y mismos gl que independencia.
> - **Atajo mental:** una variable → bondad de ajuste ; dos variables o varios grupos en tabla → independencia/homogeneidad.
> - **La cola NUNCA cambia:** χ² es **siempre unilateral derecha** (nunca ×2). $H_0$ siempre dice "no hay diferencia / son independientes / sigue la distribución".

### Ej 1 — Carreras clásicas (α=4%) · *BONDAD DE AJUSTE*
1. **Identifico:** comparo una muestra contra **% históricos (35/48/17)** → **bondad de ajuste**.
2. **a) Esperadas $e_i=n\cdot p_i$:** **1155 ; 1584 ; 561**.
3. **Planteo:** $H_0$: la inscripción **no se modifica** ; $H_1$: se modifica.
4. **jamovi:** **Recuento → N Resultados (χ² bondad de ajuste)** → categorías + frecuencias + **Razón** con 0,35/0,48/0,17 → **pv=0,007**.
5. **Decisión:** 0,007 **<** 0,04 → **Rechazo**: la inscripción **se está modificando**.

### Ej 2 — Voto vs experiencia (n=250, χ²=10,7957, α=5%) · *INDEPENDENCIA*
1. **Identifico:** **una muestra**, dos variables (voto × experiencia) → **independencia** (tabla 3×3).
2. **a) Esperada de "a favor y +5 años":** $e_{31}=\dfrac{95\cdot100}{250}=\mathbf{38}$.
3. **b) Planteo:** $H_0$: el voto es **independiente** de la experiencia ; $H_1$: están relacionados. **gl=(3−1)(3−1)=4**.
4. **jamovi:** **Recuento → Prueba de independencia χ²** (filas, columnas, frecuencias) → **pv=0,029**.
5. **Decisión:** 0,029 **<** 0,05 → **Rechazo**: voto y experiencia **están relacionados**.

### Ej 3 — Normas ambientales vs rubro (χ²=14,91, α=5%) · *INDEPENDENCIA/HOMOG.*
1. **Identifico:** cumplimiento × rubro en tabla → **independencia/homogeneidad** (misma operación).
2. **Planteo:** $H_0$: cumplimiento **independiente** del rubro ; $H_1$: relacionado.
3. **Decisión:** **pv=0,021 < 0,05 → Rechazo**: el cumplimiento **se relaciona** con el rubro.

### Ej 4 — Defectos según línea (n=255, χ²=0,85, α=5%) · *HOMOGENEIDAD*
1. **Identifico:** **varias poblaciones** (líneas) y **una** variable categórica (tipo de defecto) → **homogeneidad**.
2. **a) Esperada grietas línea 3:** $e_{G3}=\dfrac{80\cdot85}{255}=\mathbf{26{,}667}$.
3. **b) Planteo:** $H_0$: el defecto **no difiere** según la línea (homogéneo) ; $H_1$: difiere. **gl=4**.
4. **Decisión:** **pv=0,932 > 0,05 → NO Rechazo**: **no hay diferencias** entre líneas.

### Ej 5 — Criptomonedas, ¿uniforme? (χ²=7,4917, α=8%) · *BONDAD DE AJUSTE*
1. **Identifico:** "¿es uniforme?" → bondad de ajuste con **$p_i=1/5=0{,}2$** cada una (5 categorías).
2. **Planteo:** $H_0$: la preferencia es **uniforme** ; $H_1$: no. **gl=4**.
3. **Decisión:** **pv=0,112 > 0,08 → NO Rechazo**: la preferencia **es uniforme**.

### Ej 6 — Fiabilidad vs proveedor (χ²=7,6775, 4 prov., α=10%) · *HOMOGENEIDAD*
1. **Identifico:** 4 proveedores (poblaciones), una variable (fiable/no) → **homogeneidad**.
2. **Planteo:** $H_0$: misma fiabilidad para los 4 ; $H_1$: difiere.
3. **Decisión:** **pv=0,053 < 0,10 → Rechazo**: **hay diferencia** según el proveedor.

### Ej 7 — Lenguajes, ¿proporciones históricas? (χ²=9,9854, α=6%) · *BONDAD DE AJUSTE*
1. **Identifico:** comparar contra **proporciones históricas** → bondad de ajuste.
2. **Planteo:** $H_0$: mantienen las proporciones históricas ; $H_1$: cambiaron.
3. **p-valor (distrACTION, cola derecha):** **pv = P(χ²>9,9854)=0,0407**.
4. **Decisión:** 0,0407 **<** 0,06 → **Rechazo**: las preferencias **se modificaron**.

---

# 📕 PRÁCTICO 9 — Regresión y correlación

> [!tip] jamovi para todo el P9
> Recta + r + r² + test de β: **Regresión → Regresión lineal** (y = Dependiente, x = Covariable). Dispersograma: **Exploración → Gráfico de dispersión**. El **p de la covariable** es el del test de β (bilateral): $t_m=b/S_b$, gl=n−2, $pv=2P(t>|t_m|)$.

> [!question]- Cómo leo un enunciado de este práctico
> El disparador: **dos variables CUANTITATIVAS relacionadas** ($x$ explica, $y$ se explica) y casi siempre una **recta** $\hat y=a+bx$. Lo que pueden pedir:
> - **Interpretar coeficientes:** $a$ (ordenada) = valor de $y$ cuando $x=0$ → **ojo:** si $x=0$ está **fuera del rango** de los datos, NO tiene sentido práctico. $b$ (pendiente) = cuánto cambia $y$ por cada **+1** en $x$ (el signo dice si sube o baja).
> - **¿Hay asociación lineal?** → se traduce a la **PH para la pendiente $\beta$**: $H_0:\beta=0$ (no hay) vs $H_1:\beta\ne0$ (sí, **bilateral**). Estadístico $t_m=b/S_b$, $\nu=n-2$.
> - **% explicado** → es directamente $r^2$ (coeficiente de determinación).
> - **Signo de la correlación:** $r$ tiene el **mismo signo que $b$**. Si la pendiente es negativa, $r=-\sqrt{r^2}$.
> - **No extrapolar:** estimar $y$ solo es válido para valores de $x$ **dentro del rango** observado.

### Ej 1 — Errores vs horas (n=7)
1. **a) Identifico variables:** **x = horas de codificación** (explica) ; **y = cantidad de errores** (se explica).
2. **b) Dispersograma:** muestra **tendencia decreciente** → una recta ajusta bien.
3. **c) Recta (jamovi, Regresión lineal):** **$\hat{y}=8{,}397-0{,}345\,x$** (R=0,907 ; R²=0,823).
   - **a=8,397:** con 0 horas habría 8,397 errores medios → **sin sentido** (no se programa con 0 horas).
   - **b=−0,345:** por cada hora más, los errores **bajan** 0,345 en promedio.
4. **d) Estimación + residuo:** $\hat{y}(18)=2{,}187$ ; observado=2 → **residuo $e=y-\hat y=2-2{,}187=-0{,}187$** → punto **por debajo** de la recta (se **sobreestimó**).
5. **e) Estimación válida vs extrapolación:** $\hat{y}(20)=1{,}497$ ✅. Para **x=30 NO tiene sentido** (x va de 12 a 22 → sería **extrapolar**).

### Ej 2 — Analistas vs incidentes (n=9)
1. **a) Variables:** **x = incidentes** ; **y = analistas**.
2. **b) Recta:** **$\hat{y}=0{,}006\,x-4{,}325$**. a (negativo) **sin sentido** ; **b:** por cada incidente más, **+0,006 analistas** en promedio.
3. **c) Estimación:** $\hat{y}(1500)=0{,}006\cdot1500-4{,}325=\mathbf{4{,}675}$ ≈ **5 analistas**.

### Ej 3 — Emociones, $y=3{,}4+0{,}6x$
1. **a)** Sesión 0 (inicio) → $\hat y = a = \mathbf{3{,}40}$.
2. **b)** Pendiente → por cada sesión, **+0,6** en promedio.
3. **c)** Sesión 3 → $3{,}4+0{,}6\cdot3=\mathbf{5{,}2}$.

### Ej 4 — Dispersograma
1. **a) Elegir la recta:** la correcta es **$\hat{y}=2{,}03+0{,}37x$** (es de **promedios** y respeta la **pendiente positiva** del gráfico).
2. **b) Correlación:** pendiente positiva + buen ajuste → **r=0,82**.
3. **c) Estimación:** $\hat{y}(3{,}5)=2{,}03+0{,}37\cdot3{,}5=\mathbf{3{,}325}$.

### Ej 5 — Rangos de r
1. **Razono signo e intensidad de cada par:**
   - **a)** ingreso vs gasto recreativo → relación directa fuerte → **entre 0,7 y 1**.
   - **b)** horas de simulador vs nº de errores → inversa fuerte → **entre −0,7 y −1**.
   - **c)** estatura vs CI → sin relación → **alrededor de 0**.

### Ej 6 — Asignar coeficientes a gráficos ($r^2=0{,}88$, $r=0{,}30$, $b=-5$)
1. **Uso dos claves:** el **signo de b/r** dice si la recta sube/baja ; cuanto **más pegados** los puntos, **mayor |r| y r²**.
2. **Asignación:**
   - Gráfico 1 (pendiente +, disperso) → **r=0,30**.
   - Gráfico 2 (recta que baja) → **b=−5**.
   - Gráfico 3 (pendiente +, bien ajustado) → **r²=0,88**.

### Ej 7 — Razonamiento (puntaje vs tiempo)
1. **a)** **ii)** a es positiva.
2. **b)** **ii)** por cada punto que aumenta el puntaje, el tiempo disminuye en "b" minutos.
3. **c)** **ii)** r=−0,83 (negativa, porque b<0).
4. **d)** **ii)** CR: si **α > pvalue → Rechazo $H_0$**, las variables están linealmente asociadas.

### Ej 8 — Ciberseguridad ($\hat{y}=12-0{,}17x$, r²=0,81)
1. **a) Interpretación:** **a=12** → sin inversión, 12 (cientos de) incidentes ; **b=−0,17** → por cada 100 USD más, **−17 incidentes** ; **r²=0,81** → el **81%** de la variación de incidentes se explica por la inversión.
2. **b) Test de β:** **Rechazar $H_0$** → están asociados linealmente ; **r=−0,9** (signo de b, intensa e inversa).
3. **c)** Si fuera correlación perfecta → **r=−1** (mismo signo que b).

### Ej 9 — Costos vs capacitación (n=6, r=0,9782, Sb=0,0296, α=1%)
1. **a) Recta:** **$\hat{y}=-0{,}064+0{,}158x$**. a (negativo) **sin sentido** ; **b:** por cada hora más, **+$158.000**.
2. **b) Test de β:** $H_0:\beta=0$. $t_m=\dfrac{0{,}158}{0{,}0296}=5{,}3378$ ; gl=4 → **pv=0,0059 < 0,01 → Rechazo**: están **asociados linealmente**.
3. **c) Determinación:** $r^2=0{,}9782^2=0{,}957$ → **95,7%** de la variación de costos explicada por las horas.

### Ej 10 — Consultas vs satisfacción (n=12, Sb=1,661, α=4%)
1. **a) Recta:** **$\hat{y}=50{,}14+4{,}87x$**. **a:** sin consultas, satisfacción 50,14 ; **b:** por cada consulta resuelta, **+4,87 puntos**.
2. **b) Determinación:** r²=0,735 → **73,5%** explicado.
3. **c) Test de β:** $t_m=\dfrac{4{,}87}{1{,}661}=2{,}932$ ; gl=10 → **pv=0,015 < 0,04 → Rechazo**: están **asociados**.

### Ej 11 — Daño vs km (n=6, r=0,37, Sb=0,128, α=4%)
1. **a) Recta:** **$\hat{y}=8{,}9831+0{,}102x$**. Con 0 km el modelo da 8.983,10 porque 0 está fuera de rango (no se interpreta literal).
2. **b) Test de β:** $H_0:\beta=0$. $t_m=\dfrac{0{,}102}{0{,}128}=0{,}797$ ; gl=4 → **pv=0,47 > 0,04 → NO Rechazo**: **NO hay asociación lineal**.
3. **c) Determinación:** r²=0,1369 → solo **13,69%** explicado → **ratifica** que el modelo no sirve.

### Ej 12 — Descargas vs actualizaciones (n=7, α=9%)
1. **a) Recta:** **$\hat{y}=27446{,}612+972{,}862x$**. **b:** por cada actualización más, **+972,862 descargas** ; a no es confiable (x va de 2 a 7).
2. **b) Test de β:** $H_0:\beta=0$ → **pv = 2·P(t>1,134 ; v=5)=0,308 > 0,09 → NO Rechazo**: no están relacionadas linealmente.
3. **c) Correlación:** r=0,452 (leve) → **ratifica** lo concluido en b.

---

> [!success] Tip final de examen (la receta de 5 pasos)
> 1) Clasificá la prueba con el [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|árbol de decisión]] · 2) Planteá $H_0$/$H_1$ con la dirección correcta · 3) Corré la prueba en jamovi (o la cuenta a mano en proporciones/χ² con estadístico dado) · 4) **pv < α ⇒ Rechazo H₀** · 5) Conclusión en términos del problema con el α.
> **Trampas frecuentes:** "antes/después" con $n$ distintos = **independientes** (no apareadas) ; χ² es **siempre unilateral derecha** ; **no extrapolar** en regresión ; en supuestos (Levene/Shapiro) **pv alto = bueno**.

Ver también: [[05 - Formulario y errores típicos]]
