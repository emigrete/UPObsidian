---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - formulario
  - calculo-a-mano
materia: Probabilidad y Estadística
parcial: 2
alcance: Prácticos 6 a 9
---

# 🧮 Cómo calcular el estadístico (tm y zm) a mano

> [!abstract] La idea de esta nota
> Desarmar la duda de "¿cómo calculo el tm o el zm cuando es bilateral o de comparación?". Spoiler: **la cola NO cambia el cálculo del estadístico**. Lo único que cambia la fórmula es **una muestra vs comparación (dos muestras)**. Ver también [[05 - Formulario y errores típicos]] y [[00 - GUÍA 2do Parcial]].

---

## ⚠️ Lo primero (y lo que más confunde)

> [!warning] Bilateral / unilateral NO cambia el tm
> El estadístico (tm o zm) se calcula **EXACTAMENTE IGUAL** sea cola derecha, izquierda o bilateral. El tm es siempre el mismo número.
> Lo único que cambia con la cola es **el p-valor**:
> - **Cola derecha:** $p = P(T > t_m)$
> - **Cola izquierda:** $p = P(T < t_m)$
> - **Bilateral:** $p = 2\cdot P(T > |t_m|)$ → tomo valor absoluto y multiplico por 2

Lo que **sí** cambia la fórmula del estadístico es: **¿una muestra o comparación de dos muestras?**

---

## 🧩 El patrón que une a TODAS las fórmulas

$$\text{estadístico} = \frac{\text{lo que observo} - \text{lo que dice } H_0}{\text{error estándar}}$$

- **Arriba:** la diferencia entre mi muestra y el valor de $H_0$. En comparación, $H_0$ dice "son iguales" → la diferencia poblacional vale 0, por eso la resta de las dos muestras ya se compara contra 0 (no le resto nada extra).
- **Abajo:** el error estándar. **Es lo único que cambia de fórmula en fórmula.**

---

## 📐 Las fórmulas, caso por caso

### 1) t de UNA muestra (una media contra un valor fijo μ₀)
$$t_m = \frac{\bar{x} - \mu_0}{s/\sqrt{n}} \qquad gl = n-1$$

### 2) t APAREADAS (antes/después, mismo individuo)
> [!tip] Truco: apareadas = una muestra disfrazada
> Calculo la diferencia de cada par ($d = \text{después} - \text{antes}$) y hago una **t de una muestra sobre las diferencias**, comparando contra 0.

$$t_m = \frac{\bar{d} - 0}{s_d/\sqrt{n}} \qquad gl = n-1$$
- $\bar{d}$ = media de las diferencias · $s_d$ = desvío de las diferencias · $n$ = cantidad de pares.

### 3) t INDEPENDIENTES (dos grupos distintos, varianzas iguales / Student)
$$t_m = \frac{\bar{x}_1 - \bar{x}_2}{s_p\cdot\sqrt{\tfrac{1}{n_1}+\tfrac{1}{n_2}}}$$
- $s_p$ = desvío combinado (*pooled*). **Es la fea → normalmente la calcula jamovi.**

### 4) z para UNA proporción (un %)
$$z_m = \frac{\hat{p} - p_0}{\sqrt{\dfrac{p_0(1-p_0)}{n}}}$$

### 5) z para DIFERENCIA de proporciones (dos %)
$$z_m = \frac{\hat{p}_1 - \hat{p}_2}{\sqrt{\hat{p}(1-\hat{p})\left(\tfrac{1}{n_1}+\tfrac{1}{n_2}\right)}}$$
- $\hat{p}$ = proporción combinada de los dos grupos.

> [!note] Por qué "las otras no aparecen" en las fórmulas permitidas
> Las de **comparación con pooled** (3 y 5) en la cátedra se resuelven con **jamovi**, por eso no te las dan para hacer a mano. A mano suelen pedir las simples (1, 2 y 4). En las de dos grupos: confiá en el p-valor de jamovi.

---

## ✏️ EJEMPLO resuelto a mano — t APAREADAS

> Se mide el puntaje de **6 alumnos** antes y después de un método de estudio. ¿Mejoró el puntaje? ($\alpha = 0,05$)

| Alumno | Antes | Después | $d$ = Después − Antes |
|---|---|---|---|
| 1 | 60 | 65 | +5 |
| 2 | 55 | 60 | +5 |
| 3 | 70 | 68 | −2 |
| 4 | 65 | 72 | +7 |
| 5 | 50 | 58 | +8 |
| 6 | 58 | 63 | +5 |

**Paso 1 — Identificar:** una población, variable cuantitativa, mismo individuo medido 2 veces → **t apareada**. "¿Mejoró?" → después > antes → $d>0$ → **cola derecha**.

**Paso 2 — Hipótesis** (sobre la diferencia $\mu_d$):
- $H_0: \mu_d \le 0$ (no mejoró)
- $H_1: \mu_d > 0$ (mejoró)

**Paso 3 — Media de las diferencias:**
$$\bar{d} = \frac{5+5-2+7+8+5}{6} = \frac{28}{6} = 4,67$$

**Paso 4 — Desvío de las diferencias** ($s_d$):
$$\sum (d-\bar{d})^2 = 0,11+0,11+44,44+5,44+11,11+0,11 = 61,33$$
$$s_d = \sqrt{\frac{61,33}{6-1}} = \sqrt{12,27} = 3,50$$

**Paso 5 — Estadístico:**
$$t_m = \frac{\bar{d}-0}{s_d/\sqrt{n}} = \frac{4,67}{3,50/\sqrt{6}} = \frac{4,67}{1,43} = 3,27 \qquad gl = 5$$

**Paso 6 — p-valor** (cola derecha, 5 gl): $p = P(t > 3,27) \approx 0,011$.

**Paso 7 — Conclusión:** $p = 0,011 < 0,05$ → **rechazo $H_0$**. Con un riesgo del 5%, hay evidencia de que el método **mejoró** el puntaje de los alumnos.

---

## ✏️ EJEMPLO rápido — z UNA proporción

> Un partido afirma tener más del 40% de intención de voto. En una muestra de $n=200$, $80$ lo votarían ($\hat{p}=0,40$... ojo, justo el 40%). Supongamos $\hat{p}=0,46$ (92 de 200). ($\alpha=0,05$, "más del 40%" → cola derecha)

- $H_0: p \le 0,40$ · $H_1: p > 0,40$
$$z_m = \frac{0,46-0,40}{\sqrt{\dfrac{0,40\cdot0,60}{200}}} = \frac{0,06}{\sqrt{0,0012}} = \frac{0,06}{0,0346} = 1,73$$
- $p = P(z>1,73) \approx 0,042 < 0,05$ → **rechazo $H_0$** → hay evidencia de que supera el 40%.

> [!warning] OJO en proporción: el desvío del denominador usa $p_0$ (el de $H_0$), NO $\hat{p}$. En la **diferencia** de proporciones, en cambio, se usa la $\hat{p}$ combinada.

---

## 🎯 Resumen para el parcial

1. **La cola NO afecta el tm/zm.** Solo afecta el p-valor (en bilateral: $2\cdot P(T>|t_m|)$).
2. Lo que cambia la fórmula es **una muestra vs dos muestras**.
3. **Apareadas = una muestra sobre las diferencias** (lo más fácil de tomar a mano).
4. Las de **dos grupos con pooled** (t independientes y diferencia de proporciones) → **jamovi**.
5. Patrón universal: $\dfrac{\text{observado} - \text{lo de } H_0}{\text{error estándar}}$.
