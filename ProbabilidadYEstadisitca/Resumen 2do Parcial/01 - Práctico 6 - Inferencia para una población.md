---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - practico-6
  - inferencia
  - jamovi
tema: Inferencia para una población
---

# 📘 Práctico 6 — Inferencia para una población

> [!abstract] Contenido
> 1. **Estimación por Intervalos de Confianza** → para la media (con $t$) y para la proporción (con $z$).
> 2. **Prueba de Hipótesis** → para la media (con $t$) y para la proporción (con $z$).
>
> Volver a la [[00 - GUÍA 2do Parcial|guía principal]].

---

## 1️⃣ INTERVALO DE CONFIANZA PARA LA MEDIA ($\mu$)

Se usa cuando $\sigma$ es **desconocido** y la variable proviene de una **población normal**. Usa la distribución **t de Student** con $\nu = n-1$ grados de libertad.

$$IC_{1-\alpha}(\mu) = \left(\;\bar{x} - t_{(n-1;\,1-\frac{\alpha}{2})}\cdot\frac{s}{\sqrt{n}}\;;\;\; \bar{x} + t_{(n-1;\,1-\frac{\alpha}{2})}\cdot\frac{s}{\sqrt{n}}\;\right)$$

$$\boxed{EM = t_{(n-1;\,1-\frac{\alpha}{2})}\cdot\frac{s}{\sqrt{n}}} \qquad li = \bar{x}-EM \qquad ls = \bar{x}+EM$$

donde **EM** = Error Muestral o de estimación (el "± " del intervalo).

> [!info] Distribución t de Student
> - Es simétrica respecto a 0, pero **más achatada** que la normal (más dispersión).
> - Depende de los **grados de libertad** $\nu = n-1$. A medida que $n$ crece, la $t$ se aproxima a la normal.
> - Es el estadístico apropiado para la media **cuando la población es normal y $\sigma$ es desconocido**.

### 🖥️ Con jamovi
1. Cargar la columna de datos.
2. **Pruebas t → Prueba t de una Muestra.**
3. Pasar la variable a *Variables Dependientes*.
4. **Estadísticas Adicionales → Diferencia de medias → tildar Intervalo de confianza** y poner el nivel (95, 90, etc.).
5. ⚠️ En *Hipótesis* dejar siempre **≠ Valor de prueba** (para que el IC salga bilateral).

> [!example]- Ejercicio 2 resuelto — temperatura (n=14)
> Datos: 91,2 · 88,8 · 92,6 · 95,2 · 95,0 · 96,3 · 86,5 · 91,4 · 94,7 · 89,3 · 90,3 · 87,8 · 87,4 · 89,6
> - a) **IC(95%) = (89,3 ; 93)** → "Se estima con 95% de confianza que la temperatura media del ciclo está entre 89,3 °C y 93 °C". **EM = 1,85 °C**.
> - b) **IC(90%) = (89,6 ; 92,7)** ; EM = 1,55.
> - c) El intervalo del 90% es **más preciso** (más angosto) que el del 95%.
> - d) Agregando más mediciones (mayor n): **IC(95%) = (90,3 ; 92,9)** → más preciso que el del ítem a), porque al aumentar $n$ disminuye EM.

> [!example]- Ejercicio 3 — criptomoneda
> IC(0,95) = **(42,1 ; 52,2)** → "Es 95% probable que el precio medio esté entre 42.100 y 52.200 dólares".
> - b) Si aumenta $n$ (con la misma confianza) el intervalo es **menos amplio / más preciso** (EM se divide por $\sqrt{n}$).
> - c) Se aclara que sigue distribución normal porque, con muestra chica, **es condición para poder usar la $t$**.

---

## 2️⃣ INTERVALO DE CONFIANZA PARA LA PROPORCIÓN ($p$)

Se usa con muestras **grandes** ($n>30$); por TCL la distribución del estimador es **normal**. El estimador es $\hat{p}=\dfrac{x}{n}$ (x = nº de éxitos).

$$IC_{1-\alpha}(p) = \hat{p} \pm \underbrace{z_{(1-\frac{\alpha}{2})}\cdot\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}}_{EM}$$

### 🖥️ Cómo
Generalmente se calcula **a mano**: se obtiene $\hat p = x/n$, se busca el fractil $z_{(1-\alpha/2)}$ (en la Normal o con distrACTION) y se arma el intervalo.

> [!tip] Fractiles z más usados
> 90% → $z=1{,}645$ · 95% → $z=1{,}96$ · 96% → $z\approx2{,}05$ · 98% → $z=2{,}33$ · 99% → $z=2{,}576$

> [!example]- Ejercicio 4 — clientes (n=160, x=64)
> $\hat{p}=64/160=0{,}40$.
> - a) **IC(96%) = (0,32 ; 0,48)**: "Con 96% de confianza, el % de clientes que cambiarían de plan está entre 32% y 48%". **EM = 0,063**.
> - b) Con 90% el EM es menor → el intervalo es **más preciso**.

> [!example]- Ejercicio 5 — app (n=150, x=117)
> $\hat{p}=117/150=0{,}78$ → **IC(0,95) = (0,714 ; 0,846)**: "Hay 95% de probabilidad de que el % de usuarios interesados esté entre 71,4% y 84,6%".

---

## 3️⃣ Relación entre EM, confianza y tamaño de muestra

> [!important] Tabla clave (completar es típico de examen)
>
> | Tamaño muestra (n) | Nivel de confianza | Nivel de significación (α) | Error muestral (EM) | Precisión |
> |---|---|---|---|---|
> | Fijo | **aumenta** | disminuye | **aumenta** | **disminuye** |
> | **aumenta** | Fijo | Fijo | **disminuye** | aumenta |
> | **disminuye** | Fijo | Fijo | **aumenta** | disminuye |
>
> Reglas mentales:
> - ↑ confianza ⇒ ↑ EM ⇒ intervalo más ancho ⇒ **menos preciso**.
> - ↑ tamaño de muestra $n$ ⇒ ↓ EM ⇒ **más preciso** (EM se divide por $\sqrt{n}$).
> - Más amplitud de intervalo = menos precisión. $EM = \text{amplitud}/2$.

---

## 4️⃣ PRUEBA DE HIPÓTESIS PARA LA MEDIA ($\mu$, σ desconocido)

Estadístico de prueba (con $\nu=n-1$):
$$t_m = \frac{\bar{x}-\mu_0}{\dfrac{s}{\sqrt{n}}}$$

**Planteo de hipótesis según la dirección:**

| El enunciado dice… | $H_0$ | $H_1$ | Cola |
|---|---|---|---|
| "es mayor / supera / excede" | $\mu \le \mu_0$ | $\mu > \mu_0$ | derecha |
| "es menor / inferior a" | $\mu \ge \mu_0$ | $\mu < \mu_0$ | izquierda |
| "es distinto / difiere de" | $\mu = \mu_0$ | $\mu \ne \mu_0$ | bilateral |

### 🖥️ Con jamovi
1. Cargar la columna de datos.
2. **Pruebas t → Prueba t de una Muestra.**
3. Pasar la variable. En **Valor de prueba** poner $\mu_0$.
4. En **Hipótesis** marcar la dirección ( > , < o ≠ del Valor de prueba).
5. jamovi devuelve **Estadístico (t), gl y p**. La nota al pie indica la $H_1$ usada (el p ya viene de una cola si elegiste > o <).
6. Aplicar **CR: pv < α → Rechazar $H_0$**.

> [!example]- Ejercicio 11 — discos duros (afirmación "excede 500 MB/s")
> Datos: 540, 523, 503, 512, 498, 490, 502, 501, 480 (n=9). α=0,05.
> - $\mu_0 = 500$ ; Hipótesis de investigación: si μ > 500 → comprar.
> - $H_0:\mu \le 500 \quad H_1:\mu > 500$ (Rechazar $H_0$ = comprar).
> - jamovi: $\bar{x}=505{,}44$ · $s=17{,}763$ · $t_m=0{,}919$ · **pv = P(t>0,919)=0,192**.
> - CR: 0,192 **>** 0,05 → **NO se rechaza $H_0$**.
> - ✅ Conclusión: con α=0,05 **se aconseja NO adquirir** los dispositivos (no hay evidencia de que la velocidad supere 500 MB/s).

> [!example]- Ejercicio 12 — aeropuerto Río de Janeiro (n=12, media > 7 = "servicio superior")
> - $H_0:\mu\le7 \quad H_1:\mu>7$. **pv = 0,042 < 0,10 = α → Rechazar $H_0$**.
> - a) Sí se designa de servicio superior (riesgo 10%). Se verifica normalidad.
> - b) **IC(90%) = (7,03 ; 7,97)** → ratifica la decisión (todo el intervalo está por encima de 7).

> [!example]- Ejercicio 13 — barra de progreso (n=25, μ < 8,5 ; tm = -1,263)
> - $H_0:\mu\ge8{,}5 \quad H_1:\mu<8{,}5$. **pv = P(t<-1,263)=0,109**.
> - a) 0,109 **>** 0,08 = α → **NO se rechaza $H_0$**: no hay evidencia de que la barra tuviera el efecto deseado (α=8%).
> - b) Se puede estar cometiendo **error tipo II**: concluir que la barra NO tuvo efecto cuando en realidad el tiempo percibido sí es menor a 8,5 min.

> [!example]- Ejercicio 17 — operador de internet (n=20, "más de 50 hs")
> - a) **IC(95%) = (46,94 ; 59,86)**.
> - b) $H_0:\mu\le50 \quad H_1:\mu>50$. **pv = P(t>1,102)=0,1421 > 0,07 → NO Rechazo $H_0$**: no hay evidencia de que el directivo tenga razón (riesgo 7%). Posible **error tipo II** (queda sin efecto la propuesta, se pierde la oportunidad).

> [!example]- Ejercicio 18 — consumo de agua (μ < 136 ; pv = 0,044)
> - $H_0:\mu\ge136 \quad H_1:\mu<136$.
> - Con α=0,06: 0,044 < 0,06 → **Rechazo $H_0$**: la campaña tuvo efecto positivo.
> - Con α=0,02: 0,044 > 0,02 → **NO Rechazo**: cambia la conclusión, no se puede afirmar que tuvo efecto.

---

## 5️⃣ PRUEBA DE HIPÓTESIS PARA LA PROPORCIÓN ($p$)

Muestras grandes → distribución **normal**. Estadístico:
$$z_m = \frac{\hat{p}-p_0}{\sqrt{\dfrac{p_0\,(1-p_0)}{n}}}$$

El p-valor se calcula con la **Normal** según la dirección de $H_1$. **No se hace con la prueba t de jamovi** (esa es para medias); se resuelve a mano o con distrACTION (Normal).

> [!example]- Ejercicio 16 — antivirus (afirma 80%, muestra 75% de 400 ; α=0,01)
> - a/b) $H_0: p \ge 0{,}80 \quad H_1: p < 0{,}80$. **pv = 0,0062 < 0,01 → Rechazo $H_0$**: la proporción es menor que la estimada (riesgo 1%).
> - c) **IC(95%) = (0,7076 ; 0,7924)** para el % de compradores.

> [!example]- Ejercicio 15 — compras online (antes 23% no compraban; muestra 46 de 232 ; ¿bajó? riesgo 6%)
> $\hat p = 46/232$. $H_0: p\ge0{,}23 \quad H_1: p<0{,}23$. **pv = 0,1254 > 0,06 → NO Rechazo $H_0$**: no se puede concluir que el porcentaje haya disminuido.

> [!example]- Ejercicio del teórico (servidores, p₀=0,65, n=300, x=183, α=0,02)
> $\hat p = 183/300=0{,}61$. $H_0: p=0{,}65 \quad H_1:p<0{,}65$.
> $z_m=\dfrac{0{,}61-0{,}65}{\sqrt{0{,}65\cdot0{,}35/300}}=-1{,}4525$ → **pv=P(z<-1,4525)=0,0732 > 0,02 → NO Rechazo**.

---

## 6️⃣ Plantear hipótesis con α y β dados (sin datos numéricos)

Estos ejercicios (8, 9, 10, 14, 19, 20) solo piden **plantear** y **redactar errores/potencia**.

> [!example]- Ejercicio 9 — diseño web mejora satisfacción
> Índice previo = 75. "Mejora" = subir. Concluir erróneamente que mejoró cuando no = 3% → ese es **α**. No concluir mejora cuando sí (en realidad 78) = 10% → ese es **β**.
> - $H_0:\mu\le75 \quad H_1:\mu>75$ ; **α=0,03 ; β=0,10**.
> - Potencia: concluir que el diseño mejoró la satisfacción cuando en realidad sí mejoró.

> [!example]- Ejercicio 10 — Black Friday (ventas suben de 200)
> - $H_0:\mu\le200 \quad H_1:\mu>200$ ; α=0,05 ; β=0,02.
> - b) Con $t_m=2{,}014$ y n=28 (gl=27): **pv = P(t>2,014)=0,027** → el mínimo nivel de significación para concluir que aumentan las ventas es **0,027**.

> [!example]- Ejercicio 20 — detección de intrusiones (p=0,80)
> - $H_0: p=0{,}80 \quad H_1: p>0{,}80$ ; **β=0,07**.
> - b) Si $z_m=2{,}725$ → **pv=0,003 < 0,025 → Rechazo $H_0$**: la herramienta mejora la detección (error 0,025).

> [!example]- Ejercicio 19 — interfaz (≈65% conformes, "difiere")
> - a) $H_0:p=0{,}65 \quad H_1:p\ne0{,}65$. **Se rechazó $H_0$** (difiere del 65%). Potencia: concluir correctamente que el % difiere del 65% cuando realmente difiere.
> - b) Amplitud 0,134 → **EM = 0,134/2 = 0,067**.
> - c) Si EM = 0,072 (mayor) → intervalo más ancho → menos preciso → el nivel de confianza es **mayor**.

---

> [!success] Lo mínimo que tenés que saber del P6
> - IC media usa **t** ($\nu=n-1$); IC proporción usa **z**. Ambos: parámetro ± EM.
> - PH media → **Pruebas t → Una Muestra** (poner μ₀ en *Valor de prueba* y la dirección en *Hipótesis*).
> - PH proporción → a mano con la Normal.
> - **pv < α ⇒ Rechazo H₀.** Saber redactar errores tipo I y II y potencia.

Siguiente: [[02 - Práctico 7 - Comparación de dos muestras]]
