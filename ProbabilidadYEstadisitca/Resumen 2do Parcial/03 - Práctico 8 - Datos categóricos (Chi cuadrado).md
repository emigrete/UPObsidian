---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - practico-8
  - chi-cuadrado
  - jamovi
tema: Datos categóricos - Chi cuadrado
---

# 📙 Práctico 8 — Datos categóricos (Chi-cuadrado χ²)

> [!abstract] Contenido — 3 pruebas con la MISMA distribución χ²
> 1. **Bondad de ajuste** → ¿una muestra sigue proporciones dadas/históricas/uniforme?
> 2. **Independencia** → ¿dos variables están relacionadas?
> 3. **Homogeneidad** → ¿una variable categórica se distribuye igual en k poblaciones?
>
> Volver a la [[00 - GUÍA 2do Parcial|guía principal]].

---

## 📐 La distribución Chi-cuadrado (χ²)

> [!info] Características
> - Solo toma valores **positivos** (dominio $\mathbb{R}^+$), es **asimétrica a la derecha**.
> - Tiene un solo parámetro: los **grados de libertad** $\nu$.
> - $E(\chi^2)=\nu$ , $V(\chi^2)=2\nu$.
> - Las pruebas χ² son **siempre unilaterales a derecha**: $\;p\text{-}v = P(\chi^2 > \chi^2_m)$.

**Estadístico de prueba (común a las 3):**
$$\chi^2_m = \sum \frac{(o_i-e_i)^2}{e_i}$$
donde $o_i$ = frecuencias **observadas**, $e_i$ = frecuencias **esperadas** (bajo $H_0$).

> [!warning] Condición de validez
> Cada frecuencia esperada debe ser **≥ 5** ($n\cdot p_i \ge 5$); si no, se agrupan categorías.

**Condición de Rechazo (igual que siempre):** $\boxed{p\text{-}v < \alpha \Rightarrow \text{Rechazo } H_0}$

---

## 1️⃣ PRUEBA DE BONDAD DE AJUSTE

Sirve para ver si los datos de **una muestra** se ajustan a una distribución teórica (proporciones históricas, uniforme, etc.).

- $H_0$: los datos **siguen** la distribución XX (se mantienen las proporciones).
- $H_1$: los datos **NO siguen** la distribución XX.
- Frecuencias esperadas: $\;e_i = n \cdot p_i\;$ · grados de libertad: $\;\nu = c-1$ (c = nº de categorías).

> [!tip] "Uniforme" significa…
> Igual probabilidad para cada categoría: $p_i = 1/c$ para todas.

### 🖥️ Con jamovi
1. Cargar **dos columnas**: una con las categorías y otra con las frecuencias observadas.
2. **Recuento → N Resultados (χ² de bondad de ajuste).**
3. Pasar la *Variable* (categorías) y las *Frecuencias*.
4. En **Proporciones Esperadas → Razón**, escribir las proporciones $p_i$ dadas (ej: 0,4 ; 0,32 ; 0,28).
5. (Opcional) tildar *Frecuencias esperadas* para verlas en la tabla.
6. jamovi devuelve $\chi^2_m$, gl y p. **CR: pv < α ⇒ Rechazo $H_0$**.

> [!example]- Ejemplo del teórico — servicios informáticos (40/32/28%, n=300, α=4%)
> Observados: 138 / 87 / 75. Esperados $e_i=n\cdot p_i$: 120 / 96 / 84.
> $H_0$: se mantienen las proporciones históricas. $\chi^2_m=4{,}51$, gl=2, **pv=0,105 > 0,04 → NO Rechazo**: no hay evidencia de que las proporciones cambiaran.

> [!example]- Ejercicio 5 — criptomonedas, ¿uniforme? (χ²=7,4917, α=8%)
> 5 categorías → $p_i=1/5=0{,}2$ cada una. $H_0$: la preferencia es uniforme. gl=4. **pv=0,112 > 0,08 → NO Rechazo**: la preferencia es uniforme (riesgo 8%).

> [!example]- Ejercicio 7 — lenguajes de programación, ¿proporciones históricas? (χ²=9,9854, α=6%)
> $H_0$: mantienen las proporciones históricas. **pv = P(χ²>9,9854)=0,0407 < 0,06 → Rechazo**: las preferencias se modificaron (riesgo 6%).

---

## 2️⃣ PRUEBA DE INDEPENDENCIA

Una **sola muestra** clasificada según **DOS variables** (tabla de contingencia). Pregunta: ¿están relacionadas?

- $H_0$: las variables son **independientes** (no están relacionadas).
- $H_1$: las variables **NO son independientes** (están relacionadas).

**Frecuencias esperadas** (basadas en $P(A\cap B)=P(A)\cdot P(B)$ si son independientes):
$$\boxed{e_{ij} = \frac{(\text{total fila}_i)\cdot(\text{total columna}_j)}{n}}$$

**Grados de libertad:** $\;\nu = (\text{filas}-1)\cdot(\text{columnas}-1)$.

### 🖥️ Con jamovi
1. Cargar **tres columnas**: variable 1 (filas), variable 2 (columnas) y frecuencias.
2. **Recuento → Prueba de independencia χ²** (Tablas de contingencia).
3. *Filas* = una variable ; *Columnas* = la otra ; *Frecuencias* = los conteos.
4. En **Celdas** podés tildar **Frecuencias esperadas** para que las muestre.
5. jamovi devuelve la tabla de contingencia, $\chi^2_m$, gl y p. **CR: pv < α ⇒ Rechazo $H_0$**.

> [!example]- Ejercicio 2 — voto vs años de experiencia (n=250, χ²=10,7957, α=5%)
> Tabla 3×3 (3 niveles de experiencia × 3 votos: a favor/contra/indeciso).
> - a) Esperada de "a favor y más de 5 años": $e_{31}=\dfrac{95\cdot100}{250}=38$.
> - b) $H_0$: el voto es independiente de los años de experiencia. gl=(3-1)(3-1)=4. **pv = P(χ²>10,7957)=0,029 < 0,05 → Rechazo $H_0$**: el voto y la experiencia **están relacionados** (riesgo 5%).

> [!example]- Ejercicio 1 — inscripción a carreras clásicas (α=4%)
> Es bondad de ajuste contra el % histórico (35/48/17%). Esperadas: 1155 / 1584 / 561. $H_0$: la inscripción no se está modificando. **pv=0,007 < 0,04 → Rechazo**: la inscripción se está modificando.
> *(Ojo: este se plantea como bondad de ajuste a las proporciones históricas, aunque esté en P8.)*

---

## 3️⃣ PRUEBA DE HOMOGENEIDAD

Se toman **k muestras de k poblaciones distintas** y se observa **una misma variable categórica**. Pregunta: ¿se distribuye igual en todas?

- $H_0$: hay **homogeneidad** (las proporciones de cada categoría son iguales en todas las poblaciones).
- $H_1$: **NO** hay homogeneidad (alguna difiere).

Mismas fórmulas que independencia ($e_{ij}$ y gl iguales) y **misma operación en jamovi** (*Recuento → Prueba de independencia χ²*).

> [!note] Independencia vs Homogeneidad — la diferencia conceptual
> - **Independencia:** UNA muestra clasificada en 2 variables → "¿están relacionadas?".
> - **Homogeneidad:** VARIAS muestras (poblaciones), UNA variable categórica → "¿se distribuye igual en todas las poblaciones?".
> - En la práctica de jamovi y los cálculos son **idénticas**; cambia solo cómo planteás $H_0/H_1$.

> [!example]- Ejercicio 4 — tipo de defecto según línea de producción (n=255, χ²=0,85, α=5%)
> a) Esperada grietas en línea 3: $e_{G3}=\dfrac{80\cdot85}{255}=26{,}667$.
> b) $H_0$: el tipo de defecto **no difiere** según la línea (es homogéneo) ; $H_1$: difiere. gl=(3-1)(3-1)=4. **pv = P(χ²>0,85)=0,932 > 0,05 → NO Rechazo**: no hay evidencia de diferencias en el tipo de defecto según la línea.

> [!example]- Ejercicio 3 — normas ambientales según rubro (χ²=14,91, α=0,05)
> $H_0$: el cumplimiento es independiente del rubro ; $H_1$: se relaciona con el rubro. **pv=0,021 < 0,05 → Rechazo**: el cumplimiento está relacionado con el rubro.

> [!example]- Ejercicio 6 — fiabilidad de componente según proveedor (χ²=7,6775, 4 proveedores, α=0,10)
> Homogeneidad: $H_0$: la fiabilidad es la misma para los 4 proveedores. **pv=0,053 < 0,10 → Rechazo**: existe diferencia en la fiabilidad según el proveedor (riesgo 10%).

---

> [!success] Lo mínimo que tenés que saber del P8
> - Todo usa $\chi^2_m=\sum\dfrac{(o_i-e_i)^2}{e_i}$ y es **unilateral derecha**: pv = P(χ² > χ²ₘ).
> - **Bondad de ajuste:** $e_i=n\cdot p_i$, gl = c−1. jamovi: *Recuento → N Resultados (χ² bondad de ajuste)* + Razón.
> - **Independencia / Homogeneidad:** $e_{ij}=\dfrac{\text{fila}\cdot\text{col}}{n}$, gl=(f−1)(c−1). jamovi: *Recuento → Prueba de independencia χ²*.
> - **pv < α ⇒ Rechazo H₀** (las variables se relacionan / cambiaron las proporciones / no hay homogeneidad).

Siguiente: [[04 - Práctico 9 - Regresión y correlación]]
