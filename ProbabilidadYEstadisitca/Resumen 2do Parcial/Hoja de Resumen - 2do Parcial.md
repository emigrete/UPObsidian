---
title: "Hoja de Resumen — 2do Parcial"
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - resumen
  - hoja-de-formulas
  - jamovi
materia: Probabilidad y Estadística
parcial: 2
alcance: Prácticos 6 a 9 (inferencia)
aliases:
  - "Hoja Resumen 2P"
  - "Chuleta 2do Parcial"
---

# 📝 Hoja de Resumen — 2do Parcial (Probabilidad y Estadística)

> [!abstract] Qué entra y cómo se aprueba
> Todo el parcial es **inferencia estadística**: Prácticos **6 (una población) → 9 (regresión)**. Lo que más se evalúa **NO es calcular**, es: **(1) elegir bien la prueba**, **(2) plantear $H_0/H_1$**, **(3) interpretar el p-valor** y **(4) concluir en palabras**. Seguí siempre la misma receta (sección 0).

> [!important] ⭐ Lo que te dan en la hoja de fórmulas permitida (¡y lo que NO!)
> La hoja oficial **solo** trae las fórmulas del **Práctico 6 (una población)**: IC y PH de la media (con $t$) y de la proporción (con $z$).
> **Para P7 (2 muestras), P8 (χ²) y P9 (regresión) NO hay fórmulas** → eso se resuelve **con jamovi**. Ahí no calculás a mano: **elegís la prueba, leés el p-valor de la salida y concluís**. ⇒ Para esos prácticos memorizá *cuándo* va cada prueba y *cómo se interpreta*, no fórmulas.

---

## 🧭 0. LA RECETA UNIVERSAL (seguila SIEMPRE, en TODOS los ejercicios)

1. **Identificar la prueba** → con el árbol de decisión (sección 1).
2. **Plantear las hipótesis** $H_0$ y $H_1$ con la **dirección correcta** (sección 3).
3. **Obtener el p-valor** → jamovi (casi todo) o a mano (proporciones, χ², regresión por $r$).
4. **Decidir** con la regla de oro 👇
5. **Concluir** en términos del problema, nombrando el nivel de significación.

$$\boxed{\;p\text{-valor} < \alpha \;\Rightarrow\; \text{Rechazo } H_0\;}$$

> [!tip] Qué significa rechazar y frase de cierre modelo
> **Rechazar $H_0$ = tomar la acción / confirmar lo que se quería demostrar** ($H_1$).
> Si $pv > \alpha$ → **NO se rechaza** $H_0$ (ojo: NO es "se acepta", es "no hay evidencia suficiente").
> 🗣️ *"Con un nivel de significación del **[α]**, los datos **(muestran / NO muestran)** evidencia suficiente para concluir que **[lo que dice $H_1$ en palabras]**."*

---

## 🌳 1. PASO 1 — ¿QUÉ PRUEBA USO?  *(tu punto flojo nº1)*

```
¿Qué quiero analizar?
│
├── UNA población → Práctico 6
│   ├── Media (cuantitativa, σ desconocido) ............... PRUEBA t DE UNA MUESTRA
│   └── Proporción / % de éxitos .......................... PRUEBA z PARA PROPORCIÓN
│
├── DOS poblaciones → Práctico 7
│   ├── Medias, DOS grupos DISTINTOS de individuos ........ t INDEPENDIENTES
│   │      (1º Levene; si varianzas ≠ → Welch)
│   ├── Medias, MISMO individuo medido 2 veces ........... t APAREADAS
│   │      (antes/después, pre-post, test-retest)
│   └── Proporciones de dos grupos ....................... z DIFERENCIA DE PROPORCIONES
│
├── DATOS CATEGÓRICOS / conteos en tablas → Práctico 8 (χ²)
│   ├── ¿Una muestra sigue proporciones dadas/uniforme? .. χ² BONDAD DE AJUSTE
│   ├── ¿Dos variables están relacionadas? (1 muestra) ... χ² INDEPENDENCIA
│   └── ¿Una variable igual en k poblaciones? ............ χ² HOMOGENEIDAD
│
└── RELACIÓN entre dos variables CUANTITATIVAS → Práctico 9
    └── Recta de regresión + r, r² + PH para β
```

> [!tip] Palabras clave que delatan la prueba
> - **"antes y después / pre-post / el mismo grupo"** → t **apareadas**
> - **"dos grupos / dos sucursales / hombres vs mujeres / dos algoritmos"** (distintos) → t **independientes**
> - **"porcentaje / proporción / % de…"** una sola → z proporción · dos → z dif. de proporciones
> - **"¿depende de? / ¿está relacionado con? / ¿es independiente de?"** + tabla de conteos → χ² **independencia**
> - **"¿se mantienen las proporciones históricas? / ¿es uniforme?"** → χ² **bondad de ajuste**
> - **"¿se distribuye igual según la línea/proveedor/rubro?"** + tabla → χ² **homogeneidad**
> - **"estimar y en función de x / recta / asociación lineal"** → regresión

---

## 📋 2. FÓRMULAS PERMITIDAS Y CÓMO USARLAS — Práctico 6  *(tu punto flojo nº3)*

> [!note] Recordá: estas 4 son las ÚNICAS que te dan. Para P7/P8/P9 → jamovi.

### a) Intervalos de Confianza (IC) — estructura: **parámetro estimado ± EM**

**IC de la MEDIA** (variable normal, $\sigma$ desconocido, usa **t** con $\nu=n-1$):
$$L_{i,s} = \bar{x} \mp EM \qquad EM = t_{\left(1-\frac{\alpha}{2};\,n-1\right)}\cdot\frac{s}{\sqrt{n}}$$

**IC de la PROPORCIÓN** (muestra grande $n>30$, usa **z**), con $\hat{p}=x/n$:
$$L_{i,s} = \hat{p} \mp EM \qquad EM = z_{\left(1-\frac{\alpha}{2}\right)}\cdot\sqrt{\frac{\hat{p}\,(1-\hat{p})}{n}}$$

**Cómo armarlo paso a paso:** ① calculo $\bar{x}$ y $s$ (o $\hat p = x/n$). ② busco el fractil ($t$ con jamovi/tabla según gl; $z$ de la tabla de abajo). ③ $EM = \text{fractil}\cdot\text{error estándar}$. ④ intervalo $=(\text{estim}-EM\,;\,\text{estim}+EM)$. ⑤ redacto: *"Con [conf]% de confianza, [parámetro] está entre [li] y [ls]"*.

> [!tip] Fractiles z más usados (memorizar)
> | Confianza | $z_{(1-\alpha/2)}$ |
> |---|---|
> | 90% | 1,645 |
> | 95% | 1,96 |
> | 96% | ≈ 2,05 |
> | 98% | 2,33 |
> | 99% | 2,576 |
>
> Para **t** y **χ²** los fractiles dependen de los grados de libertad → jamovi (**distrACTION**) o tabla.

> [!important] Relación EM ↔ confianza ↔ n (pregunta típica de completar)
> - ↑ **confianza** (con $n$ fijo) ⇒ ↑ EM ⇒ intervalo más ancho ⇒ **MENOS preciso**.
> - ↑ **tamaño de muestra $n$** ⇒ ↓ EM ⇒ **MÁS preciso** (EM se divide por $\sqrt{n}$).
> - $EM = \dfrac{\text{amplitud}}{2}$. Para ganar precisión con $n$ fijo → hay que **bajar la confianza (subir $\alpha$)**.

### b) Prueba de Hipótesis (PH)

**PH de la MEDIA** ($\sigma$ desconocido, $\nu=n-1$):
$$t_m = \frac{\bar{x}-\mu_0}{s/\sqrt{n}}$$

**PH de la PROPORCIÓN** (muestra grande, Normal — **a mano, no con la t de jamovi**):
$$z_m = \frac{\hat{p}-p_0}{\sqrt{\dfrac{p_0\,(1-p_0)}{n}}}$$

**p-valor según la cola** (la $H_1$ manda):

| Tipo | p-valor media | p-valor proporción |
|---|---|---|
| Unilateral derecha ($>$) | $P(t>t_m)$ | $P(z>z_m)$ |
| Unilateral izquierda ($<$) | $P(t<t_m)$ | $P(z<z_m)$ |
| **Bilateral** ($\ne$) | $2\cdot P(t>\lvert t_m\rvert)$ | $2\cdot P(z>\lvert z_m\rvert)$ |

### c) Plantear $H_0/H_1$ según la dirección  *(error que más resta)*

| El enunciado dice… | $H_0$ | $H_1$ | Cola |
|---|---|---|---|
| "es **mayor** / supera / excede / aumenta" | $\mu \le \mu_0$ | $\mu > \mu_0$ | derecha |
| "es **menor** / inferior / disminuye / reduce" | $\mu \ge \mu_0$ | $\mu < \mu_0$ | izquierda |
| "es **distinto** / difiere / cambió" | $\mu = \mu_0$ | $\mu \ne \mu_0$ | bilateral |

> [!warning] Regla de oro del planteo
> En $H_1$ va **lo que se quiere demostrar / la acción**. $H_0$ siempre lleva el `=`. Si es **bilateral, el p-valor se DUPLICA**.

---

## 🧠 3. INTERPRETAR Y CONCLUIR — lo que más cae  *(tu punto flojo nº2)*

### Conceptos que caen siempre en teoría

- **Parámetro** = característica de la **población** (constante): $\mu$, $\sigma^2$, $p$.
  **Estadístico/estimador** = característica de la **muestra** (variable aleatoria): $\bar{x}$, $S^2$, $\hat{p}$.
  Estima: $\bar{x}\to\mu$ · $S^2\to\sigma^2$ · $\hat{p}\to p$.
- **Nivel de confianza $(1-\alpha)$:** de cada 100 intervalos con muestras distintas, $(1-\alpha)\cdot100$ contienen al verdadero parámetro.
- **p-valor:** el **mínimo** nivel de significación con el que se rechazaría $H_0$.

### Errores y potencia (saber redactarlos)

| | $H_0$ Verdadera | $H_0$ Falsa |
|---|---|---|
| **Rechazo $H_0$** | ❌ Error tipo I ($\alpha$) | ✅ Correcto |
| **No Rechazo $H_0$** | ✅ Correcto | ❌ Error tipo II ($\beta$) |

- $\alpha = P(\text{Rechazar }H_0\mid H_0\text{ verdadera})$ → **error tipo I** (= nivel de significación).
- $\beta = P(\text{No rechazar }H_0\mid H_0\text{ falsa})$ → **error tipo II**.
- **Potencia** $=1-\beta=P(\text{Rechazar }H_0\mid H_0\text{ falsa})$ → detectar el efecto que sí existe.

> [!example] Cómo redactar los errores (modelo)
> Si rechazar $H_0$ = **"comprar la máquina"**:
> - **Error tipo I:** comprar la máquina cuando en realidad **NO** mejora la producción.
> - **Error tipo II:** **NO** comprar la máquina cuando en realidad **SÍ** mejora.
> - ¿Qué $\alpha$? Quien teme el error tipo I → $\alpha$ chico (0,01). Quien quiere que la acción se tome → $\alpha$ grande (0,10).

### Supuestos en jamovi (¡acá la lógica se invierte!)

| Prueba de supuesto | $H_0$ | $pv > \alpha$ significa… | $pv < \alpha$ significa… |
|---|---|---|---|
| **Normalidad** (Shapiro-Wilk) | datos normales | ✅ se cumple normalidad | se viola |
| **Homogeneidad** (Levene) | varianzas iguales | ✅ varianzas iguales → **t de Student** | varianzas distintas → **t de Welch** |

> [!info] La trampa mental
> En los **supuestos** lo que querés es **NO rechazar** (que el supuesto se cumpla) → buscás **pv ALTO ($>\alpha$)**. Es al revés que en la prueba principal.

---

## 🧪 4. PRÁCTICO 7 — Dos muestras (con jamovi)

> [!important] LO PRIMERO: ¿apareadas o independientes?
> - **APAREADAS:** la **misma** variable medida 2 veces sobre el **mismo individuo** (antes/después). UNA muestra, dos columnas. **Mismo n** en ambas.
> - **INDEPENDIENTES:** **dos grupos distintos** de individuos (A vs B, hombres vs mujeres).
> - 🪤 **Trampa clásica:** dice "antes y después" pero son **personas distintas** (n distintos, ej. 10 vs 12) → son **INDEPENDIENTES**, no apareadas.

**Independientes** (jamovi: *Pruebas t → Muestras Independientes*; tildar Levene y Shapiro):
1º **Levene** ($H_0:\sigma_1^2=\sigma_2^2$): si $pv>\alpha$ → **Student**; si $pv<\alpha$ → **Welch**. Luego leo el $pv$ de la t.

**Apareadas** (jamovi: *Pruebas t → Muestras Pareadas*; tildar Normalidad): defino **$D=$ medida1 − medida2**, planteo $H_0:\mu_D=0$. Si espero que "baje" (antes>después) → $H_1:\mu_D>0$.

**Diferencia de proporciones** (jamovi: *Recuento → Prueba de independencia χ²* → destildar χ² y **tildar "z para diferencia de 2 proporciones"**): $H_0:p_1-p_2=0$.

En los tres: **$pv<\alpha \Rightarrow$ Rechazo $H_0$**.

---

## 🎲 5. PRÁCTICO 8 — Chi-cuadrado χ² (con jamovi)

**Estadístico común a las 3** (y **SIEMPRE unilateral derecha**):
$$\chi^2_m=\sum\frac{(o_i-e_i)^2}{e_i} \qquad p\text{-}v = P(\chi^2 > \chi^2_m)$$
Condición de validez: cada esperada $e_i \ge 5$.

| Prueba | Cuándo | $H_0$ | Esperadas | gl | jamovi |
|---|---|---|---|---|---|
| **Bondad de ajuste** | 1 muestra, ¿sigue proporciones dadas/uniforme? | sigue esas proporciones | $e_i=n\cdot p_i$ | $c-1$ | *Recuento → N Resultados* + Razón |
| **Independencia** | 1 muestra, 2 variables, ¿relacionadas? | son independientes | $e_{ij}=\dfrac{\text{fila}_i\cdot\text{col}_j}{n}$ | $(f-1)(c-1)$ | *Recuento → Prueba de independencia χ²* |
| **Homogeneidad** | k poblaciones, 1 variable, ¿igual en todas? | hay homogeneidad | igual que independencia | $(f-1)(c-1)$ | igual que independencia |

> [!note] Independencia vs Homogeneidad
> Mismos cálculos y misma operación en jamovi. Cambia el **planteo**: independencia = *"¿están relacionadas?"* (1 muestra, 2 variables); homogeneidad = *"¿se distribuye igual?"* (varias poblaciones, 1 variable). "Uniforme" = $p_i=1/c$ para todas.

---

## 📉 6. PRÁCTICO 9 — Regresión y correlación (con jamovi)

Recta por mínimos cuadrados (jamovi: *Regresión → Regresión lineal*; y = Dependiente, x = Covariable):
$$\hat{y}=a+b\,x$$

- **$a$ (intercepto):** valor medio de $y$ cuando $x=0$. ⚠️ Aclarar **si no tiene sentido** (x=0 fuera de rango, costo negativo, etc.).
- **$b$ (pendiente):** por cada unidad que **aumenta x**, $y$ cambia en promedio **$b$** (sube si $b>0$, baja si $b<0$).
- **Residuo:** $e=y-\hat{y}$. $e<0$ → punto **debajo** de la recta (se sobreestimó); $e>0$ → **encima**.
- 🚫 **Extrapolar** (estimar fuera del rango de x) **no es válido**.

**$r^2$ (determinación):** % de la variación de $y$ explicada por $x$. $0\le r^2\le1$; regla: $r^2<0{,}5$ → modelo malo.
**$r$ (correlación):** intensidad y **signo** de la asociación lineal; $r=\pm\sqrt{r^2}$ (**signo de $b$**); $-1\le r\le1$; $|r|>0{,}7$ → bueno. ⚠️ **$r=0$ no implica independencia** (puede haber relación no lineal).

**PH de asociación lineal (test de $\beta$, siempre bilateral):**
$$H_0:\beta=0 \quad H_1:\beta\ne0 \qquad t_m=\frac{b}{S_b},\;\nu=n-2,\quad pv=2\cdot P(t>\lvert t_m\rvert)$$
**Rechazar $H_0$ ⇒ las variables están linealmente asociadas** (la recta sirve para estimar). En jamovi, el $p$ de la covariable en *Coeficientes del Modelo* ya es este test.

---

## 🖥️ 7. CHEAT-SHEET jamovi (rutas)

| Necesito… | Ruta |
|---|---|
| IC / PH media, 1 población | Pruebas t → **Prueba t de una Muestra** (Valor de prueba = $\mu_0$; Hipótesis = dirección) |
| Comparar medias, 2 grupos distintos | Pruebas t → **Muestras Independientes** (+ Levene + Shapiro) |
| Comparar medias, antes/después | Pruebas t → **Muestras Pareadas** (+ Normalidad) |
| 2 proporciones | Recuento → Independencia χ² → tildar **z para diferencia de 2 proporciones** |
| χ² bondad de ajuste | Recuento → **N Resultados (χ² bondad de ajuste)** + Razón |
| χ² independencia / homogeneidad | Recuento → **Prueba de independencia χ²** |
| Recta, r, r², test de β | Regresión → **Regresión lineal** |
| Fractiles/probabilidades de t, z, χ² | módulo **distrACTION** |

> [!note] PH de **proporción** (1 o 2) y **χ²** y el **p-valor de regresión por $r$** son los que se hacen **a mano**; las medias van con la prueba t de jamovi.

---

## ✍️ 8. EJERCICIOS INTEGRADORES RESUELTOS (paso a paso)

> [!tip] Estos 4 cubren TODO el parcial. Tapá la solución, resolvé, y comparate.

> [!example]- 1️⃣ Soporte técnico — t de UNA media + concepto de precisión (P6)
> *n=16, $\bar x=4{,}12$, s=1{,}31. Se optimiza si el tiempo medio **supera 4 h**.*
> - **Identifico:** 1 población, cuantitativa, σ desconocido, n chico → **t de una muestra** ($\nu=15$).
> - **Hipótesis** ("supera" → cola derecha): $H_0:\mu\le4 \;;\; H_1:\mu>4$. Rechazar = **implementar**.
> - **Estadístico:** $t_m=\dfrac{4{,}12-4}{1{,}31/\sqrt{16}}=\dfrac{0{,}12}{0{,}3275}\approx0{,}37 \Rightarrow pv=P(T_{15}>0{,}37)\approx0{,}36$ (muy alto).
> - **Decisión:** $0{,}36>\alpha$ (cualquiera) → **NO rechazo** → **no se implementa** (no hay evidencia de que supere 4 h).
> - **Bonus (precisión con n fijo):** para un IC más preciso con el mismo $n$ hay que **subir $\alpha$** (bajar la confianza) → menor valor crítico → menor EM.

> [!example]- 2️⃣ Supermercado A vs B + proporción de conformes (P7 indep. + P6 z)
> *Tiempos en caja, clientes **distintos** de A (nuevo) y B (tradicional).*
> - **a) Test:** **t independientes** (medias, 2 grupos distintos, σ desconocido). Antes corro **Levene** (varianzas) → Student o Welch.
> - **b) Hipótesis** (el nuevo "mejora" = reduce tiempo, $\mu_A<\mu_B$): $H_0:\mu_B-\mu_A=0 \;;\; H_1:\mu_B-\mu_A>0$.
> - **c)** Con $t_m=1{,}47$, $\nu=10$, $\alpha=0{,}10$: $pv=P(T_{10}>1{,}47)\approx0{,}087$. Como $0{,}087<0{,}10$ → **Rechazo** → conviene implementarlo. 🪤 **Ojo:** con $\alpha=0{,}05$ NO se rechazaría ($0{,}087>0{,}05$).
> - **d) ¿Aumentaron los conformes?** ($n=150$, 64%, histórico 62%, α=0,05) → **z de proporción** a mano. $H_0:p\le0{,}62\;;\;H_1:p>0{,}62$. $z_m=\dfrac{0{,}64-0{,}62}{\sqrt{0{,}62\cdot0{,}38/150}}\approx0{,}505 \Rightarrow pv\approx0{,}307>0{,}05$ → **NO rechazo** (la suba 62→64% es ruido muestral).

> [!example]- 3️⃣ Formación vs rendimiento — χ² independencia (V/F) (P8)
> *Una muestra, 2 variables categóricas: formación (4) × rendimiento (3) → tabla 4×3.*
> - **a)** *"Se hace PH de independencia"* → **VERDADERO**: 1 muestra, 2 variables categóricas, "¿se relacionan?" → χ² de independencia, $\nu=(4-1)(3-1)=6$.
> - **b)** *"Es bilateral"* → **FALSO**: χ² es **siempre unilateral derecha** ($pv=P(\chi^2>\chi^2_m)$), porque $\chi^2_m\ge0$ y solo los valores grandes alejan de $H_0$.
> - **c)** *"Si a α=8% no hay relación, a α=10% sí habría"* → **FALSO**: no rechazar a 0,08 solo dice $pv>0{,}08$. Subir a 0,10 solo cambia algo si $0{,}08<pv<0{,}10$, dato que no tengo. **Subir α no garantiza rechazar.**

> [!example]- 4️⃣ Chatbot — regresión: conocimiento (x) vs tiempo (y) (P9)
> *n=12, $\hat y=12{,}86-1{,}29x$, $r^2=0{,}789$, x∈[1,5].*
> - **a) Coeficientes:** $a=12{,}86$ = tiempo medio si $x=0$, pero $x=0$ está **fuera del rango (1–5) → sin sentido práctico**. $b=-1{,}29$ = por cada punto más de conocimiento, el tiempo **baja 1,29** en promedio (coherente: más sabe, menos tarda).
> - **b) $r^2=0{,}789$:** el **78,9%** de la variación del tiempo se explica por el conocimiento → buen ajuste.
> - **c) Test de β:** $H_0:\beta=0\;;\;H_1:\beta\ne0$. Como $b<0$ → $r=-\sqrt{0{,}789}\approx-0{,}888$; $t_m=\dfrac{r\sqrt{n-2}}{\sqrt{1-r^2}}=\dfrac{-0{,}888\sqrt{10}}{\sqrt{0{,}211}}\approx-6{,}11$ ($\nu=10$) → $pv\approx0{,}0001<\alpha$ → **Rechazo**: hay asociación lineal, el conocimiento **sirve** para estimar el tiempo.

---

## ⚠️ 9. CHECKLIST FINAL — no pierdas puntos tontos

> [!danger] Errores que más restan
> 1. Confundir **apareadas** (mismo individuo) vs **independientes** (grupos distintos). Pista: distinto $n$ = independientes.
> 2. Equivocar la **dirección de $H_1$** (mayor `>`, menor `<`, difiere `≠`). Bilateral → **pv ×2**.
> 3. Comparar mal: se rechaza **solo si $pv<\alpha$**. $pv>\alpha$ = NO rechazo (no "acepto").
> 4. No verificar **Levene + Shapiro** en t independientes (y recordar: ahí $pv>\alpha$ = bueno).
> 5. Olvidar que **χ² es unilateral derecha**.
> 6. **Extrapolar** en regresión / interpretar el intercepto $a$ cuando no tiene sentido.
> 7. No cerrar con **conclusión en palabras** y el nivel de significación.

> [!quote] Si te bloqueás, volvé a los 5 pasos
> Identifico (árbol) → planteo $H_0/H_1$ → p-valor (jamovi/mano) → **$pv<\alpha\Rightarrow$ Rechazo** → concluyo en contexto.

---

🔙 Volver a la guía: [[00 - GUÍA 2do Parcial]] · Teoría por práctico: [[01 - Práctico 6 - Inferencia para una población|P6]] · [[02 - Práctico 7 - Comparación de dos muestras|P7]] · [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|P8]] · [[04 - Práctico 9 - Regresión y correlación|P9]] · Práctica: [[07 - Ejercicios de repaso resueltos]] · [[Preguntas Integradoras - Probabilidad (2do Parcial)]]
