---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - practico-7
  - inferencia
  - jamovi
tema: Comparación de dos muestras
---

# 📗 Práctico 7 — Comparación de dos muestras

> [!abstract] Contenido
> 1. **t para muestras independientes** (dos grupos distintos) + supuestos (Levene y Shapiro-Wilk).
> 2. **t para muestras apareadas/pareadas** (mismo individuo, antes/después).
> 3. **z para diferencia de proporciones.**
>
> Volver a la [[00 - GUÍA 2do Parcial|guía principal]].

---

## 🔀 LO PRIMERO: ¿apareadas o independientes?

> [!important] La pregunta que define todo
> - **APAREADAS (pareadas):** se mide la **misma variable dos veces sobre el mismo individuo** (antes/después, pre-post, test-retest), o pares naturales. Hay UNA muestra medida 2 veces.
> - **INDEPENDIENTES:** dos **grupos distintos** de individuos (uno por cada nivel del factor). Ej: hombres vs mujeres, sucursal A vs B, algoritmo A vs B.

Frase modelo para justificar:
> "Aplica prueba t para muestras **apareadas** porque se estudia la variable cuantitativa *[X]* medida en un **mismo grupo** antes y después de *[tratamiento]*."
> "Aplica prueba t para muestras **independientes** porque se estudia la variable cuantitativa *[X]*, dependiente de *[factor]* medido en **dos grupos distintos**."

---

## 1️⃣ t PARA MUESTRAS INDEPENDIENTES

Variable respuesta **cuantitativa** y normal, en función de un factor con **2 niveles** = dos grupos distintos.

> [!warning] Antes de la prueba t hay que validar 2 supuestos
> 1. **Normalidad (Shapiro-Wilk):** $H_0$: los datos son normales. **pv > α ⇒ se cumple normalidad.**
> 2. **Homogeneidad de varianzas (Levene):** $H_0:\sigma_1^2=\sigma_2^2$.
>    - **pv > α ⇒ varianzas iguales** → uso **t de Student**.
>    - **pv < α ⇒ varianzas distintas** → uso **t de Welch** (destildar Student, tildar Welch).

### 🖥️ Con jamovi
1. Cargar los datos en **dos columnas**: una con la variable dependiente (todos los valores juntos) y otra con el grupo/nivel (A, B…).
2. **Pruebas t → Prueba t para Muestras Independientes.**
3. *Variable dependiente* = la cuantitativa ; *Variable de agrupación* = el factor.
4. En **Hipótesis** elegir la dirección. ⚠️ jamovi toma como **Grupo 1 al primero ingresado**.
5. **Comprobaciones de supuestos:** tildar **Prueba de homogeneidad** (Levene) y **Prueba de Normalidad** (Shapiro-Wilk).
6. Leer: el p de la prueba t → **CR: pv < α ⇒ Rechazo $H_0$**.

> [!example]- Ejemplo del teórico — versión A (audio) vs B (texto)
> Diferencias A: 3 · 3,2 · 2,1 · 3,7 · 2,2 — B: 1,9 · 2,5 · 3,1 · 2,2 · 2,9.
> - $H_0:\mu_A=\mu_B \quad H_1:\mu_A>\mu_B$.
> - Normalidad pv=0,506 > 0,05 ✅ ; Levene pv=0,344 > 0,05 ✅ (varianzas iguales → Student).
> - Prueba t: $t_m=0{,}852$, gl=8, **pv=0,209 > 0,05 → NO Rechazo $H_0$**: la versión A no reduce más la vulnerabilidad que la B.

> [!example]- Ejercicio 5 — algoritmos A vs B (pv=0,0431; α=0,05)
> $H_0:\mu_A=\mu_B \quad H_1:\mu_A\ne\mu_B$. pv=0,0431 < 0,05 → **Rechazo $H_0$**: los algoritmos difieren en eficacia (riesgo 5%). **Sí** debe hacerse el test de homogeneidad de varianzas (son muestras independientes).

> [!example]- Ejercicio 4 — desempeño matemática según situación laboral (t=-1,0697; v=32)
> a) t independientes (calificación según trabaja/no trabaja, 2 grupos distintos). b) **Sí**, primero homogeneidad de varianzas: $H_0:\sigma^2_{nt}=\sigma^2_{t} \;;\; H_1:\sigma^2_{nt}\ne\sigma^2_{t}$. c) $H_0:\mu_{nt}=\mu_t \;;\; H_1:\mu_{nt}>\mu_t$ (pv=0,146) → **NO hay evidencia** de que los que trabajan rindan peor.

> [!example]- Ejercicio 11 — ventas centro vs shopping (α=6%)
> a) Se verifica normalidad. b) t independientes; Levene pv=0,009 → **varianzas distintas (Welch)**. $H_0:\mu_{centro}=\mu_{shopping} \;;\; H_1:\mu_{centro}>\mu_{shopping}$. c) pv=0,094 > 0,06 → **NO hay evidencia** de que el centro venda más → no conviene la campaña.

> [!example]- Ejercicio 10 — satisfacción según estilo de CEO (α=4%)
> a) t independientes; Levene pv=0,029 → **varianzas distintas**. $H_0:\mu_A-\mu_B\ge0 \;;\; H_1:\mu_A-\mu_B<0$. b) **pv=0,002 < 0,04 → Rechazo $H_0$**: las sospechas del investigador son ciertas (los empleados están más satisfechos con el CEO que prioriza el bienestar).

---

## 2️⃣ t PARA MUESTRAS APAREADAS (pareadas)

Variable cuantitativa medida **dos veces sobre los mismos individuos**. Se analiza la **diferencia** $D$.

> [!info] Planteo con la diferencia D
> Definir **D = medida 1 − medida 2** (por ejemplo: antes − después).
> - "¿bajó / es efectivo / mejoró (redujo)?" → si esperamos que baje, antes > después → $H_1:\mu_D>0$.
> - Siempre aclarar qué es D. $H_0:\mu_D=0$.

### 🖥️ Con jamovi
1. Cargar **una columna por cada medición** (ej: "Antes" y "Después").
2. **Pruebas t → Prueba t para Muestras Pareadas.**
3. Pasar las dos variables a *Variables Apareadas* (**respetar el orden de D**).
4. En **Hipótesis** marcar la dirección (Medida 1 ≠ / > / < Medida 2).
5. **Comprobación de supuestos:** tildar **Prueba de Normalidad**.
6. **CR: pv < α ⇒ Rechazo $H_0$**.

> [!example]- Ejemplo del teórico — nuevo algoritmo de recomendación (α=0,04)
> Satisfacción antes vs después (mismo grupo). D = antes − después. Si mejoró, debería subir → $H_0:\mu_D=0 \;;\; H_1:\mu_D<0$.
> - Normalidad pv=0,595 > 0,04 ✅. Prueba t: $t_m=-2{,}29$, gl=8, **pv=0,026 < 0,04 → Rechazo $H_0$**: el algoritmo cumplió el objetivo (la satisfacción aumentó).

> [!warning]- Ejercicio 1 — ¡TRAMPA! "antes/después" pero INDEPENDIENTES (α=0,01)
> Aunque dice "antes y después de la actualización", se mide **una muestra de 10 usuarios antes** y **otra de 12 usuarios después** → son **grupos distintos** ⇒ **muestras INDEPENDIENTES** (no apareadas). La pista es que los $n$ son distintos (10 ≠ 12); en apareadas siempre hay el mismo nº de pares.
> a) t **independientes**. b) Normalidad pv=0,898 ✅ ; homogeneidad de varianzas pv=0,323 ✅ → t de Student. c) $H_0:\mu_{antes}-\mu_{desp}=0 \;;\; H_1:\mu_{antes}-\mu_{desp}>0$. **pv=0,003 < 0,01 → Rechazo**: la actualización fue efectiva (riesgo 1%).

> [!example]- Ejercicio 2 — fatiga (datos apareados, |tm|=2,5259, v=14, α=0,04)
> D = con fatiga − sin fatiga. $H_0:\mu_D=0 \;;\; H_1:\mu_D>0$. **pv = P(t>2,5259)=0,012 < 0,04 → Rechazo**: la fatiga altera el desempeño.

> [!example]- Ejercicio 3 — zinc agua superficial vs fondo (α=1%)
> a) Apareadas (mismo punto de muestreo, dos ubicaciones dependientes). b) D = superficial − fondo. $H_0:\mu_D=0 \;;\; H_1:\mu_D<0$. **pv=0,008 < 0,01 → Rechazo**: el zinc del fondo supera al de la superficie.

> [!example]- Ejercicio 9 — tiempos de carga antes/después (α=0,06)
> a) Normalidad pv=0,218 ✅. b) t apareadas. c) D = antes − después; $H_1:\mu_D>0$. **pv=0,026 < 0,06 → Rechazo**: la optimización tuvo el efecto deseado.

> [!example]- Ejercicio 13 — navegadores C vs F (|t|=1,988, v=15, α=5%)
> Apareadas (16 computadoras, mismos equipos con ambos navegadores). Se sospecha C más rápido → $H_0:\mu_D=0 \;;\; H_1:\mu_D<0$ con D = C − F. **pv = P(t>1,988)=0,033 < 0,05 → Rechazo**: C es más rápido.
> ⚠️ Si solo se quisiera ver si **difieren** (bilateral): $pv = 2\cdot0{,}033 = 0{,}066$.

---

## 3️⃣ z PARA DIFERENCIA DE PROPORCIONES

Comparar el **porcentaje de éxitos de dos grupos**. Muestras grandes → distribución normal.

### 🖥️ Con jamovi
1. Cargar **3 columnas**: (1) el grupo (a/b), (2) la categoría (sí/no), (3) la frecuencia.
2. **Recuento → Muestras independientes (Prueba de independencia χ²)** / *Tablas de contingencia*.
3. *Filas* = lo que querés comparar (los grupos) ; *Columnas* = la otra variable ; *Frecuencias* = la columna de conteos.
4. En **Pruebas: destildar χ²** y **tildar "Prueba z para diferencia de 2 proporciones"**.
5. En **Medidas Comparativas (solo 2×2): tildar "Diferencia de proporciones"**.
6. En **Hipótesis** elegir la dirección y verificar que **Comparar = filas**.
7. **CR: pv < α ⇒ Rechazo $H_0$**.

> [!example]- Ejemplo del teórico — ciberseguridad A vs B (α=2,5%)
> A: 77 de 110 sin ataque ; B: 180 de 300 sin ataque. $H_0:P_A-P_B=0 \;;\; H_1:P_A-P_B>0$.
> jamovi: z=1,85, **pv = P(z>1,85)=0,032 > 0,025 → NO Rechazo**: el nuevo sistema no es mejor que el tradicional.

> [!example]- Ejercicio 6 — satisfacción hombres vs mujeres (α=0,10)
> $H_0:p_M=p_H \;;\; H_1:p_M\ne p_H$. **pv=0,145 > 0,10 → NO Rechazo**: no hay evidencia de diferencia entre hombres y mujeres.

> [!example]- Ejercicio 7 — inseguridad CABA vs Provincia (α=4%)
> $H_0:p_{CABA}=p_{BsAs} \;;\; H_1:p_{BsAs}>p_{CABA}$. **pv=0,019 < 0,04 → Rechazo**: hay mayor inseguridad en provincia (riesgo 4%).

> [!example]- Ejercicio 8 — sucursales A vs B (α=4%)
> $H_0:p_B-p_A\le0 \;;\; H_1:p_B-p_A>0$. **pv=0,06 > 0,04 → NO Rechazo**: no hay evidencia de que B sea más efectiva.

> [!example]- Ejercicio 12 — fidelización particulares vs empresas (α=0,05)
> $H_0:p_{part}=p_{emp} \;;\; H_1:p_{part}>p_{emp}$. **pv=0,037 < 0,05 → Rechazo**: conviene intensificar acciones de fidelización para empresas.

---

> [!success] Lo mínimo que tenés que saber del P7
> - **Apareadas** = mismo individuo 2 veces (antes/después). **Independientes** = 2 grupos distintos.
> - Independientes: primero **Levene** (varianzas) y **Shapiro** (normalidad). Si varianzas ≠ → **Welch**.
> - Apareadas: definir **D = medida1 − medida2** y plantear $H_0:\mu_D=0$.
> - Proporciones: **Recuento → tildar "Prueba z para diferencia de 2 proporciones"**.
> - Siempre: **pv < α ⇒ Rechazo H₀.**

Siguiente: [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)]]
