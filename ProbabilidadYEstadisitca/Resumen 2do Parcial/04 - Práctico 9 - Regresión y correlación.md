---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - practico-9
  - regresion
  - correlacion
  - jamovi
tema: Regresión y correlación lineal
---

# 📕 Práctico 9 — Regresión y correlación lineal

> [!abstract] Contenido
> 1. **Recta de regresión** (cuadrados mínimos): coeficientes $a$ y $b$ e interpretación.
> 2. **Estimación / predicción / residuo** y extrapolación.
> 3. **Coeficiente de determinación $r^2$** y **coeficiente de correlación $r$**.
> 4. **Prueba de hipótesis para $\beta$** (¿hay asociación lineal?).
>
> Volver a la [[00 - GUÍA 2do Parcial|guía principal]].

---

## 📈 La recta de regresión

Buscamos una función lineal que vincule las variables. Por **mínimos cuadrados** (minimiza $\sum e_i^2$):

$$\hat{y} = a + b\,x$$

- **x** = variable **independiente** / explicativa / regresora.
- **y** = variable **dependiente** / explicada.
- **a** = ordenada al origen (intercepto) → estima $\alpha$.
- **b** = pendiente (coeficiente de regresión) → estima $\beta$.

$$b = \frac{\sum x\,y - n\,\bar{x}\,\bar{y}}{\sum x^2 - n\,\bar{x}^2} \qquad a = \bar{y}-b\,\bar{x}$$

### 🔑 Interpretación de los coeficientes (cae siempre)

> [!important] Cómo redactar
> - **a (intercepto):** "Cuando x = 0 (no hay *[x]*), el valor medio de y se estima en *a*." ⚠️ Aclarar **si no tiene sentido** (ej: x=0 fuera del rango, o un costo negativo).
> - **b (pendiente):** "Por cada unidad que **aumenta x**, y **aumenta/disminuye** (según el signo de b) en promedio **|b|** unidades."

### 🖥️ Con jamovi
- **Gráfico de dispersión:** cargar x e y en columnas → **Exploración → Gráfico de dispersión**.
- **Recta + r + r² + test de β:** **Regresión → Regresión lineal.** Pasar y a *Dependiente* y x a *Covariables*. Devuelve:
  - **Medidas de Ajuste del Modelo** → R (= r) y R² (= r²).
  - **Coeficientes del Modelo** → *Constante* = a, la covariable = b, con su **t** y **p**.

> [!example]- Ejercicio 1 resuelto — errores vs horas (n=7)
> Horas (x): 12, 12, 15, 16, 17.5, 18, 22 — Errores (y): 5, 4, 3, 2, 3, 2, 1.
> - a) x = horas dedicadas a la codificación ; y = cantidad de errores.
> - b) El dispersograma muestra que una recta ajusta bien (tendencia decreciente).
> - c) **$\hat{y}=8{,}397-0{,}345\,x$**. (jamovi: R=0,907 ; R²=0,823)
>   - a=8,397: cuando no se dedican horas, los errores medios se estiman en 8,397 → **no tiene sentido** (no se programa con 0 horas).
>   - b=−0,345: por cada hora más dedicada, los errores **disminuyen** en promedio 0,345.
> - d) $\hat{y}(18)=8{,}397-0{,}345\cdot18=2{,}187$. Observado=2 → **residuo $e=y-\hat{y}=2-2{,}187=-0{,}187$**: la observación está por debajo de la recta (se **sobreestimó** en 0,187).
> - e) $\hat{y}(20)=8{,}397-0{,}345\cdot20=1{,}497$. Para **x=30 NO tiene sentido** estimar (x solo va de 12 a 22 → sería **extrapolar**).

---

## 📏 Predicción, residuo y extrapolación

> [!info] Conceptos
> - **Predicción/estimación:** reemplazar un valor de x en la recta → $\hat{y}$.
> - **Residuo:** $e = y - \hat{y}$ (observado − estimado).
>   - $e<0$ → el punto está **por debajo** de la recta (se sobreestimó).
>   - $e>0$ → el punto está **por encima** (se subestimó).
> - **Extrapolación:** estimar para un x **fuera del rango** de los datos → **no es válido / no tiene sentido**.

---

## 🎯 Coeficiente de determinación $r^2$

Mide la **proporción (%) de la variación total de y explicada por el modelo** (por las variaciones de x).

> [!info] Propiedades de $r^2$
> - $0 \le r^2 \le 1$.
> - $r^2=1$ → ajuste perfecto (los puntos están sobre la recta).
> - $r^2=0$ → no hay relación lineal (nube totalmente dispersa).
> - Regla práctica: si **$r^2 < 0{,}50$** el modelo **no es bueno** para pronósticos.

**Redacción:** "El **(r²·100)%** de las variaciones en *[y]* se explican por las variaciones en *[x]*."

---

## 🔗 Coeficiente de correlación lineal $r$

Mide el **grado/intensidad y el sentido** de la asociación lineal.

> [!info] Propiedades de $r$
> - $-1 \le r \le 1$.
> - **El signo de r = el signo de b** (de la pendiente).
> - $r=1$ → asociación lineal positiva perfecta ; $r=-1$ → negativa perfecta.
> - Si x e y son independientes → $r=0$. (Pero $r=0$ **no implica** independencia: puede haber relación no lineal.)
> - Relación: $r = \pm\sqrt{r^2}$ (el signo lo da b).
> - Regla práctica: si **$|r|>0{,}7$** el modelo es **bueno** para pronósticos.

> [!example]- Ejercicio 8 — ciberseguridad ($\hat{y}=12-0{,}17x$, r²=0,81)
> - a) a=12: sin inversión, 12 (cientos de) incidentes en promedio. b=−0,17: por cada 100 dólares más de inversión, los incidentes bajan 17. r²=0,81: el **81%** de la variación de los incidentes se explica por la inversión.
> - b) Test de β → **Rechazar $H_0$**: están linealmente asociados; r=−0,9 (signo de b, intensa e inversa).
> - c) Si la correlación fuera perfecta, **r=−1** (b es negativa, r tiene el mismo signo).

---

## 🧪 PRUEBA DE HIPÓTESIS PARA β (¿hay asociación lineal?)

Si $\beta=0$ la recta es horizontal → x **no sirve** para predecir y. Por eso se prueba:

$$H_0:\beta=0 \qquad H_1:\beta\ne0 \quad(\text{siempre bilateral})$$

**Rechazar $H_0$ ⇒ las variables están linealmente asociadas** (la recta es útil para estimar).

Estadístico (análogo a la media con σ desconocido), con $\nu=n-2$:
$$t_m = \frac{b}{S_b} \qquad\Rightarrow\qquad p\text{-}v = 2\cdot P(t > |t_m|)$$

**CR: pv < α ⇒ Rechazo $H_0$.**

### 🖥️ Con jamovi
En **Regresión → Regresión lineal**, la tabla *Coeficientes del Modelo* ya trae el **t** y el **p** de cada coeficiente. El p de la covariable (x) es el de la prueba de β.

> [!example]- Ejercicio del teórico — gasto vs ingreso (n=7, Sb=0,001415, α=1%)
> $\hat{y}=16{,}516+0{,}008x$. $t_m=\dfrac{0{,}008}{0{,}001415}=5{,}6537$ ; gl=n−2=5.
> **pv = 2·P(t>5,6537)=0,0024 < 0,01 → Rechazo $H_0$**: ingreso y gasto están linealmente asociados.
> R²=0,865 → el 86,5% de la variación del gasto se explica por el ingreso ; r=0,93 (asociación intensa positiva) → **ratifica** la conclusión.

> [!example]- Ejercicio 9 — costos vs capacitación (n=6, r=0,9782, Sb=0,0296, α=0,01)
> $\hat{y}=-0{,}064+0{,}158x$. a) a negativo → no tiene sentido (no hay costos negativos). b=0,158: por cada hora más de capacitación, los costos suben $158.000.
> b) $t_m=\dfrac{0{,}158}{0{,}0296}=5{,}3378$ ; gl=4 ; **pv=0,0059 < 0,01 → Rechazo**: están linealmente asociados. c) r²=0,957 → el 95,7% de la variación de los costos se explica por las horas.

> [!example]- Ejercicio 11 — daño vs km (n=6, r=0,37, Sb=0,128, α=4%)
> $\hat{y}=8{,}9831+0{,}102x$. a) El modelo no da 0 con 0 km porque ese valor (8983,10) está fuera de rango. b) $t_m=\dfrac{0{,}102}{0{,}128}=0{,}797$ ; gl=4 ; **pv=0,47 > 0,04 (o 0,10) → NO Rechazo**: NO hay asociación lineal. c) r²=0,1369 → solo el 13,69% se explica → **ratifica** que el modelo no sirve.

> [!example]- Ejercicio 12 — descargas vs actualizaciones (n=7, α=0,09)
> $\hat{y}=27446{,}612+972{,}862x$. a) b: por cada actualización más, las descargas suben 972,862. a no es confiable (actualizaciones van de 2 a 7). b) $H_0:\beta=0$; **pv = 2·P(t>1,134; v=5)=0,308 > 0,09 → NO Rechazo**: no están linealmente relacionadas. c) r=0,452 (leve asociación) → ratifica lo concluido en b.

> [!example]- Ejercicio 10 — consultas vs satisfacción (n=12, Sb=1,661, α=4%)
> $\hat{y}=50{,}14+4{,}87x$. a) a: sin consultas, satisfacción media 50,14. b: por cada consulta resuelta, +4,87 puntos. b) r²=0,735 → 73,5% explicado. c) $t_m=\dfrac{4{,}87}{1{,}661}=2{,}932$; gl=10; **pv=0,015 < 0,04 → Rechazo**: están linealmente asociados.

---

## 🖼️ Leer gráficos (sin calcular)

> [!tip] Ejercicio 6 — asignar coeficientes a gráficos
> Dados $r^2=0{,}88$, $r=0{,}30$, $b=-5$:
> - **Pendiente positiva** y nube **bien ajustada** → $r^2=0{,}88$ (gráfico 3, alta correlación positiva).
> - **Pendiente positiva** pero nube **dispersa** → $r=0{,}30$ (gráfico 1, baja correlación).
> - **Pendiente negativa** (la recta baja) → $b=-5$ (gráfico 2).
>
> Claves: el **signo de b/r** = si la recta sube (+) o baja (−). Cuanto **más pegados a la recta** los puntos, **mayor |r| y r²**.

---

> [!success] Lo mínimo que tenés que saber del P9
> - Recta $\hat{y}=a+bx$ con jamovi: **Regresión → Regresión lineal** (y=Dependiente, x=Covariable).
> - **b** = cuánto cambia y por cada unidad de x ; **a** = y cuando x=0 (a veces sin sentido).
> - **r²** = % de variación de y explicada por x ; **r** = intensidad y signo (signo de b). $|r|>0{,}7$ → buen modelo.
> - Test de asociación lineal: $H_0:\beta=0$ vs $H_1:\beta\ne0$ ; $t_m=b/S_b$, gl=n−2, **pv bilateral**.
> - **No extrapolar** fuera del rango de x.

Ver también: [[05 - Formulario y errores típicos]]
