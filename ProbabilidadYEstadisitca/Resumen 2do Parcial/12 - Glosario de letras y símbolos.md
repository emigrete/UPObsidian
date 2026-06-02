---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - glosario
  - referencia
tema: Glosario de letras y símbolos — qué significa cada una y cuándo cambia
materia: Probabilidad y Estadística
parcial: 2
aliases:
  - Glosario de letras
  - Qué significa cada letra
  - Símbolos
---

# 🔠 Glosario de letras y símbolos — 2do Parcial

> [!abstract] Para qué sirve
> Diccionario rápido de **todas las letras** que aparecen en los prácticos 6 a 9. Si en un ejercicio ves una letra y no te acordás qué es, buscala acá. Ojo: **algunas letras significan cosas distintas según el contexto** → empezá por esa sección.

---

## ⚠️ 1. CUIDADO: mismas letras, distinto significado

> [!danger] La trampa que más confunde
> Estas letras **cambian de significado** según el ejercicio. Es lo que viste entre el P2·Ej2 y el P2·Ej3.

| Letra | Significado A | Significado B | Cómo darte cuenta |
|---|---|---|---|
| **β** (beta) | **Error tipo II** = $P(\text{no rechazar }H_0\mid H_0\text{ falsa})$ — en cualquier **prueba de hipótesis** | **Pendiente verdadera** de la recta (regresión), parámetro poblacional que se estima con $b$ | Si el tema es **errores/potencia** → error tipo II. Si el tema es **regresión** ($\hat y=a+bx$) → pendiente. |
| **a** | **Ordenada al origen** (intercepto) de la recta de regresión | (no confundir con $\alpha$, que es otra cosa) | "a" latina = intercepto. "α" griega = nivel de significación. |
| **x** | **Variable independiente** (eje horizontal) en regresión | **Cantidad de éxitos** en proporciones ($\hat p = x/n$) | En regresión va con $y$. En proporción va con $n$ ("x de cada n"). |
| **s** | **Desvío estándar muestral** (cuando va solo o $s$) | $S_b$ = **error estándar de la pendiente** (lleva subíndice $b$) | El subíndice manda: $s$ pelado = desvío de los datos; $S_b$ = error de $b$. |

> [!example] El caso concreto que te confundió
> - En **P2·Ej3** te piden "identificar $\alpha$ y $\beta$" → ahí **$\beta$ = error tipo II** (la $\alpha$ y la $\beta$ son las dos probabilidades de error). Oficial: $\alpha=0{,}08$, $\beta=0{,}10$.
> - En **P2·Ej2** la prueba es sobre "$\beta$" → ahí **$\beta$ = la pendiente real** de la recta ($H_0:\beta=0$).
> - **Misma letra, dos mundos:** uno es el de los **errores** de una PH; el otro, el de la **regresión**. El contexto del ejercicio te dice cuál.

---

## 2. Parámetros vs estimadores (lo más importante)

> [!info] La regla madre
> **Parámetro** = característica de la **POBLACIÓN** (valor fijo y desconocido, letra **griega** o sin sombrero). **Estimador/estadístico** = lo que calculás de la **MUESTRA** (letra latina, con barra o sombrero). *El estimador estima al parámetro.*

| Concepto | Parámetro (población) | Estimador (muestra) |
|---|---|---|
| Media | **$\mu$** (mu) | **$\bar x$** (equis barra) |
| Varianza | **$\sigma^2$** (sigma cuadrado) | **$s^2$** |
| Desvío estándar | **$\sigma$** (sigma) | **$s$** |
| Proporción | **$p$** | **$\hat p$** (pe sombrero) = $x/n$ |
| Pendiente de la recta | **$\beta$** (beta) | **$b$** |
| Ordenada de la recta | **$\alpha_{regresión}$** (a veces) | **$a$** |

> [!tip] Truco para no confundir
> ¿Tiene **sombrero (^), barra (‾) o es latina**? → es de la **muestra** (lo calculaste vos). ¿Es **griega o "pelada"** ($\mu$, $\sigma$, $p$, $\beta$)? → es de la **población** (lo verdadero, lo que no conocés).

---

## 3. Letras de la prueba de hipótesis

| Símbolo | Nombre | Qué es |
|---|---|---|
| **$H_0$** | Hipótesis nula | "NO hay efecto/diferencia". Lleva el `=`. Es la que se intenta rechazar. |
| **$H_1$** (o $H_a$) | Hipótesis alternativa | Lo que se quiere demostrar. Lleva `>`, `<` o `≠`. |
| **$\alpha$** (alfa) | Nivel de significación = **error tipo I** | $P(\text{rechazar }H_0\mid H_0\text{ verdadera})$. El riesgo que aceptás. Te lo dan (0,05; 0,01…). |
| **$\beta$** (beta) | **Error tipo II** | $P(\text{no rechazar }H_0\mid H_0\text{ falsa})$. *(En regresión es otra cosa, ver sección 1.)* |
| **$1-\beta$** | **Potencia** | $P(\text{rechazar }H_0\mid H_0\text{ falsa})$ = detectar el efecto que sí existe. |
| **$1-\alpha$** | Nivel de confianza | Para los IC. Ej: 95% ⇒ $\alpha=0{,}05$. |
| **$p\text{-valor}$ (pv)** | p-valor | Mínimo $\alpha$ con el que se rechaza $H_0$. **Regla: $pv<\alpha \Rightarrow$ Rechazo $H_0$**. |
| **$\mu_0$ , $p_0$** | Valor de prueba | El número con el que comparás bajo $H_0$ (ej. $H_0:\mu=10{,}3 \Rightarrow \mu_0=10{,}3$). |
| **$t_m$ , $z_m$ , $\chi^2_m$** | Estadístico calculado | El valor que te da la cuenta con TUS datos (la "m" es de muestral). Se compara contra la distribución. |
| **$\nu$** (nu) | Grados de libertad (gl) | t una muestra: $n-1$ · regresión: $n-2$ · χ² bondad: $c-1$ · χ² tabla: $(f-1)(c-1)$. |

---

## 4. Letras de los intervalos de confianza (IC)

| Símbolo | Nombre | Qué es |
|---|---|---|
| **EM** | Error muestral (margen de error) | La mitad de la amplitud del IC: $EM=\text{amplitud}/2$. Es el "± " del intervalo. |
| **$l_i$ , $l_s$** | Límite inferior / superior | Los extremos del IC: $l_i=\text{centro}-EM$, $l_s=\text{centro}+EM$. |
| **$t_{(n-1;\,1-\alpha/2)}$** | Fractil t | El multiplicador para el IC de la media (depende de gl y confianza). |
| **$z_{(1-\alpha/2)}$** | Fractil z | Multiplicador para el IC de proporción. (95%→1,96 · 90%→1,645 · 99%→2,576.) |

---

## 5. Letras de regresión y correlación (Práctico 9)

| Símbolo | Nombre | Qué es |
|---|---|---|
| **$x$** | Variable independiente | La que "explica" (eje horizontal). Ej: edad. |
| **$y$** | Variable dependiente | La que se predice (eje vertical). Ej: compras. |
| **$\hat y$** (ye sombrero) | Valor estimado | Lo que predice la recta para un $x$ dado: $\hat y=a+bx$. |
| **$a$** | Ordenada al origen (intercepto) | Valor de $\hat y$ cuando $x=0$. Ojo: a veces no tiene sentido práctico. |
| **$b$** | Pendiente (muestra) | Cuánto cambia $\hat y$ por cada +1 de $x$. Estima a **$\beta$**. |
| **$\beta$** | Pendiente verdadera (población) | Lo que se prueba: $H_0:\beta=0$ (sin relación). |
| **$S_b$** | Error estándar de la pendiente | Va en $t_m=b/S_b$ para el test de $\beta$. |
| **$e$** | Residuo | Diferencia entre lo real y lo predicho: $e=y-\hat y$. |
| **$r$** | Coef. de correlación | Entre $-1$ y $1$. **Signo** = dirección · **$\lvert r\rvert$** = fuerza ($>0{,}7$ fuerte). |
| **$r^2$** | Coef. de determinación | % de la variación de $y$ explicado por $x$ ($>0{,}5$ buen modelo). |

> [!warning] No confundir
> **$\beta$ me dice SI hay relación** (test) · **el signo de $b$/$r$ me dice HACIA DÓNDE** (directa/inversa) · **$\lvert r\rvert$ y $r^2$ me dicen CUÁN FUERTE**. La fuerza **NO** se mide por qué tan grande es $b$.

---

## 6. Letras de Chi-cuadrado (Práctico 8)

| Símbolo | Nombre | Qué es |
|---|---|---|
| **$\chi^2$** (ji/chi cuadrado) | Estadístico χ² | $\chi^2_m=\sum\dfrac{(o_i-e_i)^2}{e_i}$. Siempre **unilateral derecha**. |
| **$o_i$** | Frecuencia observada | Lo que **contaste** en la tabla. |
| **$e_i$** | Frecuencia esperada | Lo que se esperaría si $H_0$ fuera cierta. Bondad: $e_i=n\cdot p_i$ · Tabla: $e_{ij}=\dfrac{\text{fila}\cdot\text{col}}{n}$. |
| **$f$ , $c$** | Filas, columnas | Para los gl: bondad $\nu=c-1$ · tabla $\nu=(f-1)(c-1)$. |

---

## 7. Letras de dos muestras (Práctico 7)

| Símbolo | Nombre | Qué es |
|---|---|---|
| **$\mu_1,\mu_2$** | Medias de cada población | Se comparan en t independientes ($H_0:\mu_1=\mu_2$). |
| **$d$** | Diferencia (apareadas) | $d=\text{antes}-\text{después}$ (o al revés, aclaralo). $H_0:\mu_d=0$. |
| **$\mu_d$** | Media verdadera de las diferencias | El parámetro que se prueba en apareadas. |
| **$\hat p_1,\hat p_2$** | Proporciones muestrales de cada grupo | En diferencia de proporciones. |
| **$\bar p$** (pe barra) | Proporción combinada (pooled) | Junta los éxitos de ambos grupos: $\bar p=\dfrac{x_1+x_2}{n_1+n_2}$. Va en el denominador del $z_m$. |
| **Levene** | Test de varianzas iguales | $H_0$: varianzas iguales. Si $pv<\alpha$ → Welch. |
| **Shapiro-Wilk** | Test de normalidad | $H_0$: datos normales. $pv>\alpha$ = se cumple. |

---

> [!quote] Las 4 que más se confunden — memorizá esto
> - **$\alpha$** = nivel de significación / **error tipo I** (rechazar de más).
> - **$\beta$** = **error tipo II** en PH… **PERO** = **pendiente** en regresión. Mirá el contexto.
> - **$p$** (proporción poblacional) ≠ **$\hat p$** (muestral) ≠ **$\bar p$** (combinada) ≠ **$p_0$** (valor de prueba) ≠ **$pv$** (p-valor). ¡Son cinco "p" distintas!
> - **$b$** = pendiente de TU muestra · **$\beta$** = pendiente verdadera (lo que estimás).

🔙 Volver: [[00 - GUÍA 2do Parcial]] · [[05 - Formulario y errores típicos]] · [[10 - Árbol de decisión (chuleta rápida)]]
