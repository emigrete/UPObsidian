---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - formulario
tema: Formulario y errores típicos
---

# 🧮 Formulario y errores típicos — 2do Parcial

Volver a la [[00 - GUÍA 2do Parcial|guía principal]].

---

## 📋 FÓRMULAS — ✅ en la hoja permitida vs ❌ a memorizar

> [!important] Leé esto antes
> La hoja oficial **solo trae lo de UNA población** (media y proporción, PH e IC). Todo lo de **dos grupos, χ² y regresión NO viene** → te lo tenés que saber de memoria (o resolverlo por jamovi). Marca de cada fórmula:
> - ✅ = **está en la hoja permitida**, no la memorices.
> - ❌ = **NO está**, memorizala (o salida por jamovi).

> [!note] Práctico 6 — Una población ✅ (TODO en la hoja)
> ✅ **IC media (t):** $\quad \bar{x}\pm \underbrace{t_{(n-1;\,1-\frac{\alpha}{2})}\cdot\dfrac{s}{\sqrt n}}_{EM}$
>
> ✅ **IC proporción (z):** $\quad \hat{p}\pm \underbrace{z_{(1-\frac{\alpha}{2})}\cdot\sqrt{\dfrac{\hat p(1-\hat p)}{n}}}_{EM}$ , con $\hat p=\dfrac{x}{n}$
>
> ✅ **PH media:** $\quad t_m=\dfrac{\bar{x}-\mu_0}{s/\sqrt n}$ , gl $=n-1$
>
> ✅ **PH proporción:** $\quad z_m=\dfrac{\hat p-p_0}{\sqrt{p_0(1-p_0)/n}}$
>
> 💡 De la amplitud: $EM=\dfrac{\text{amplitud}}{2}$ , y el IC va centrado ($l_i=\text{centro}-EM$, $l_s=\text{centro}+EM$).

> [!warning] Práctico 7 — Dos poblaciones ❌ (NADA en la hoja → memorizar)
> - ❌ **t independientes:** primero Levene (varianzas) y Shapiro (normalidad). Si varianzas ≠ → Welch. *(En general te dan los datos → jamovi.)*
> - ❌ **t apareadas:** definir $D=$ medida1 − medida2 ; $H_0:\mu_D=0$. *(También suele venir con datos → jamovi.)*
> - ❌ **z diferencia de proporciones:** $z_m=\dfrac{\hat p_1-\hat p_2}{\sqrt{\bar p(1-\bar p)\left(\frac1{n_1}+\frac1{n_2}\right)}}$ , con $\bar p=\dfrac{x_1+x_2}{n_1+n_2}$ ; $H_0:p_1=p_2$. *(Jamovi: Recuento → independencia → "comparing proportions".)*

> [!warning] Práctico 8 — Chi-cuadrado ❌ (NO está en la hoja → memorizar)
> ❌ **Estadístico:** $\quad \chi^2_m=\sum\dfrac{(o_i-e_i)^2}{e_i}$ , unilateral derecha: $p\text{-}v=P(\chi^2>\chi^2_m)$
> - **Bondad de ajuste:** $e_i=n\cdot p_i$ , gl $=c-1$.
> - **Independencia / Homogeneidad:** $e_{ij}=\dfrac{\text{fila}_i\cdot\text{col}_j}{n}$ , gl $=(f-1)(c-1)$.
> 💡 La más probable y la más corta: tenela fresca. Salida por jamovi: Recuento.

> [!warning] Práctico 9 — Regresión ❌ (NO está en la hoja → memorizar)
> ❌ **Recta:** $\hat y=a+bx$ , con $b=\dfrac{\sum xy-n\bar x\bar y}{\sum x^2-n\bar x^2}$ , $a=\bar y-b\bar x$
> ❌ **Residuo:** $e=y-\hat y$
> ❌ **Test de β:** $H_0:\beta=0$ vs $H_1:\beta\ne0$ ; $t_m=\dfrac{b}{S_b}$ , gl $=n-2$ , $pv=2\cdot P(t>|t_m|)$
> ❌ **Correlación:** $r=\pm\sqrt{r^2}$ (signo de b) ; $|r|>0{,}7$ y $r^2>0{,}5$ → buen modelo.
> 💡 Salida por jamovi: Regresión → Linear Regression (te da $b$, $r^2$ y el p de β directo).

---

## ⭐ La regla universal

$$\boxed{p\text{-valor} < \alpha \;\Rightarrow\; \text{Rechazo } H_0}$$

**Rechazar $H_0$ = tomar la acción / confirmar lo que se quería demostrar.**

---

## ⚠️ ERRORES TÍPICOS (no los cometas en el parcial)

> [!danger] Los que más restan puntos
> 1. **Confundir apareadas vs independientes.** Mismo individuo medido 2 veces = **apareadas**. Dos grupos distintos = **independientes**.
> 2. **Olvidar la dirección de $H_1$.** "mayor/supera" → `>` ; "menor/inferior" → `<` ; "difiere/distinto" → `≠`. Si es bilateral, el pv se **duplica**.
> 3. **Comparar mal pv y α.** Solo se rechaza si **pv < α**. Si pv > α → NO se rechaza (NO es "se acepta").
> 4. **No verificar supuestos en t independientes.** Siempre **Levene** (varianzas) y **Shapiro** (normalidad). Recordá: en estas pruebas $H_0$ es "se cumple el supuesto", así que **pv > α = bueno** (se cumple).
> 5. **χ²: olvidar que es unilateral derecha.** Siempre $pv=P(\chi^2>\chi^2_m)$.
> 6. **Regresión: extrapolar.** No estimar para x fuera del rango de los datos.
> 7. **Interpretar a (intercepto) sin sentido.** Aclarar cuando x=0 no aplica (costos negativos, fuera de rango, etc.).
> 8. **No escribir la conclusión en términos del problema.** Siempre cerrar con una frase contextualizada y el nivel de significación.

> [!tip] Frase de conclusión modelo
> "Con un nivel de significación del **[α]**, los datos **(muestran / NO muestran)** evidencia suficiente para concluir que **[lo que dice $H_1$ en palabras]**."

---

## 🧠 Supuestos en jamovi (cómo leerlos)

| Prueba | $H_0$ | pv > α significa… | Si pv < α… |
|---|---|---|---|
| Normalidad (Shapiro-Wilk) | datos normales | ✅ se cumple normalidad | se viola normalidad |
| Homogeneidad (Levene) | varianzas iguales | ✅ varianzas iguales → t de Student | varianzas distintas → t de **Welch** |

> [!info] Recordá
> Para las pruebas de **supuestos**, "lo que querés" es **NO rechazar** (que se cumpla el supuesto), es decir **pv alto (> α)**. Es al revés que la prueba principal.

---

## 🔢 Fractiles más usados (memorizar)

| Confianza | $z_{(1-\alpha/2)}$ |
|---|---|
| 90% | 1,645 |
| 95% | 1,96 |
| 98% | 2,33 |
| 99% | 2,576 |

Para la **t** y **χ²** los fractiles dependen de los grados de libertad → usar **jamovi (distrACTION)** o tabla.

---

> [!quote] En resumen, para aprobar
> 1. Identificá la prueba con el [[00 - GUÍA 2do Parcial#🌳 ÁRBOL DE DECISIÓN ¿QUÉ PRUEBA USO|árbol de decisión]].
> 2. Planteá $H_0$ y $H_1$ con la dirección correcta.
> 3. Corré la prueba en jamovi (sabés la ruta de cada una).
> 4. Aplicá **pv < α ⇒ Rechazo H₀**.
> 5. Concluí en términos del problema con el nivel de significación.
