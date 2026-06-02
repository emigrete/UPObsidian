---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - drill
  - identificar-prueba
tema: Drill de identificación — leer el enunciado y decir qué prueba es
materia: Probabilidad y Estadística
parcial: 2
aliases:
  - Drill identificar prueba
  - Qué prueba es
---

# 🎯 Drill — ¿Qué prueba es? (solo identificar)

> [!abstract] Cómo usar esta página
> El objetivo **NO es resolver**, es **identificar**. Para cada enunciado, antes de abrir la respuesta, contestá en voz alta las **3 preguntas** y decí el nombre de la prueba + la cola. Recién ahí abrí el desplegable.
>
> **Las 3 preguntas (siempre, en orden):**
> 1. **¿Cuántos grupos?** → uno · dos · tabla de conteos · dos variables numéricas
> 2. **¿Qué dato?** → media (se promedia) · proporción (% de éxitos) · categorías (se cuentan)
> 3. **¿Probar o estimar?** → "¿hay evidencia / supera / difiere?" = **PH** · "estimar / intervalo / X% de confianza" = **IC**
>
> Y la **cola** sale del verbo: *supera/mayor* → `>` · *menor/baja* → `<` · *difiere/distinto* → `≠` (bilateral, pv ×2).
>
> 🌳 Apoyo: [[10 - Árbol de decisión (chuleta rápida)]] · [[00 - GUÍA 2do Parcial]] · [[05 - Formulario y errores típicos]]

> [!tip] Tip de oro
> El **contexto (el cuento) es decorado**. Semiconductores, plantas, perfumes o cripto da igual: cada enunciado es **uno de 7 tipos**. Entrená el reflejo de ignorar el tema y mirar solo la estructura.

---

## 🟢 Bloque 1 — Una población (Práctico 6)

### 1. Batería de drones
> Una fábrica afirma que sus drones vuelan en promedio **más de 28 minutos** por carga. Se toma una muestra de 15 drones, se mide la duración de cada vuelo y se obtiene la lista de tiempos. ¿Hay evidencia para sostener la afirmación? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Grupos?** Uno. · **¿Dato?** Media (minutos que se promedian). · **¿Probar o estimar?** Probar ("¿hay evidencia?").
> - → **Prueba t de una muestra** (media, σ desconocido).
> - **Cola:** "más de" → `>` → unilateral derecha. $H_0:\mu\le28$ ; $H_1:\mu>28$.
> - **Cómo:** datos crudos (lista) → **jamovi** (Pruebas t → Una muestra). ✅ Fórmula en la hoja.

### 2. Usuarios que abandonan el carrito
> En un e-commerce, de **240 visitas** registradas, **86 abandonaron** el carrito. El equipo sospecha que la tasa de abandono **supera el 30%**. ¿Los datos lo respaldan? ($\alpha=0{,}04$)

> [!success]- Ver respuesta
> - **¿Grupos?** Uno. · **¿Dato?** Proporción ("86 de 240", %). · **¿Probar o estimar?** Probar.
> - → **Prueba z para una proporción**.
> - **Cola:** "supera el 30%" → `>`. $H_0:p\le0{,}30$ ; $H_1:p>0{,}30$.
> - **Cómo:** dato resumido (x/n) → **a mano** + distrACTION (Normal). ✅ Fórmula en la hoja.

### 3. Tiempo de respuesta de un servidor
> Se midió el tiempo de respuesta (ms) de **18 peticiones** a un servidor. Con esos datos, **estimá con un 95% de confianza** el tiempo medio de respuesta.

> [!success]- Ver respuesta
> - **¿Grupos?** Uno. · **¿Dato?** Media. · **¿Probar o estimar?** **Estimar** ("estimá con 95% de confianza") → **NO hay H₀**.
> - → **Intervalo de confianza para la media (t)**, $\nu=n-1=17$.
> - **Trampa:** la palabra *estimar / intervalo / X% de confianza* delata un **IC**, no una PH. No plantees hipótesis.
> - **Cómo:** datos crudos → jamovi (la misma prueba t, tildando IC, dejando hipótesis en `≠` para que salga bilateral). ✅

### 4. Satisfacción de clientes (IC proporción)
> Una encuesta a **150 clientes** mostró que **117 están satisfechos**. Construí un **intervalo de confianza del 90%** para la proporción de clientes satisfechos.

> [!success]- Ver respuesta
> - **¿Grupos?** Uno. · **¿Dato?** Proporción (117/150). · **¿Probar o estimar?** **Estimar** → IC.
> - → **Intervalo de confianza para la proporción (z)**, $\hat p\pm z_{(1-\alpha/2)}\sqrt{\hat p(1-\hat p)/n}$.
> - **Cómo:** a mano. ✅ Fórmula en la hoja. (Para 90%: $z=1{,}645$.)

### 5. ¿Cambió el peso? (bilateral, una muestra)
> El peso de un producto debería ser de **500 g**. Se controlan **20 envases** y se quiere saber si el peso medio **difiere** del valor nominal. ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Grupos?** Uno. · **¿Dato?** Media. · **¿Probar o estimar?** Probar.
> - → **Prueba t de una muestra**.
> - **Cola:** "difiere de" → `≠` → **bilateral** (el pv se **duplica**). $H_0:\mu=500$ ; $H_1:\mu\ne500$.
> - **Cómo:** jamovi. ✅

---

## 🔵 Bloque 2 — Dos poblaciones (Práctico 7)

### 6. Dos proveedores de baterías
> Se prueban las baterías de **Proveedor A** (12 unidades) y **Proveedor B** (14 unidades) midiendo su autonomía. ¿**Difiere** la autonomía media entre proveedores? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Grupos?** **Dos** (A y B, **individuos distintos**). · **¿Dato?** Media. · **¿Probar?** Sí.
> - → **t para muestras independientes**. ⚠️ Antes va **Levene** (varianzas): si pv<α → **Welch**.
> - **Cola:** "difiere" → `≠` bilateral. $H_0:\mu_A=\mu_B$ ; $H_1:\mu_A\ne\mu_B$.
> - **Cómo:** datos crudos → jamovi. ❌ No está en la hoja (memorizar / jamovi).

### 7. Dieta: peso antes y después
> A **10 personas** se les midió el peso **antes** y **después** de 8 semanas de un plan nutricional. ¿El plan **reduce** el peso? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Grupos?** Dos medidas, **pero del MISMO individuo** ("antes y después", n igual) → **APAREADAS**.
> - **¿Dato?** Media (de la diferencia). · **¿Probar?** Sí.
> - → **t para muestras apareadas**. Se define $d=\text{antes}-\text{después}$ y se hace una t "de una muestra" sobre $d$. $H_0:\mu_d=0$.
> - **Cola:** "reduce el peso" → $H_1:\mu_d>0$ (si antes − después > 0, bajó). Unilateral.
> - **Cómo:** jamovi (Pruebas t → Pareadas). ❌ No está en la hoja.

### 8. ⚠️ TRAMPA: antes/después con n distinto
> Se midió la satisfacción de **10 usuarios** con la versión vieja de una app y de **13 usuarios** con la versión nueva. ¿Mejoró la satisfacción con la nueva versión? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¡OJO!** Dice "vieja/nueva" (suena a antes/después), **pero son 10 vs 13 → individuos DISTINTOS** (n distinto ⇒ no puede ser el mismo medido dos veces).
> - → **t para muestras INDEPENDIENTES** (NO apareadas). Levene antes.
> - **Regla:** apareadas exige el **mismo n** y el **mismo individuo**. Si los n difieren, son grupos distintos.
> - **Cola:** "mejoró" → unilateral en la dirección de la mejora.

### 9. Conversión de dos landing pages
> La landing **A** convirtió **54 de 120** visitas; la landing **B**, **72 de 200**. ¿**Difieren** las tasas de conversión? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Grupos?** Dos. · **¿Dato?** **Proporción** ("54 de 120", "72 de 200" → %). · **¿Probar?** Sí.
> - → **z para diferencia de dos proporciones** (usa la proporción combinada $\bar p$).
> - **Cola:** "difieren" → `≠` bilateral. $H_0:p_A=p_B$ ; $H_1:p_A\ne p_B$.
> - **Cómo:** a mano con estadístico, o jamovi (Recuento → independencia → "comparing proportions"). ❌ Memorizar.

---

## 🟣 Bloque 3 — Categóricos / χ² (Práctico 8)

### 10. ¿Los colores siguen el patrón histórico?
> Una marca dice que sus cajas vienen en **4 colores** con proporciones **40/30/20/10%**. En un lote de 300 cajas se cuentan los colores. ¿Se mantienen las proporciones históricas? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Grupos/forma?** Tabla de **conteos** → χ². · **¿Cuál χ²?** **UNA sola** variable (color) contra **porcentajes fijos** → **bondad de ajuste**.
> - → **χ² de bondad de ajuste**, $e_i=n\cdot p_i$, $\nu=c-1=3$.
> - **Cola:** χ² **SIEMPRE unilateral derecha** ($pv=P(\chi^2>\chi^2_m)$, nunca ×2).
> - $H_0$: sigue 40/30/20/10 ; $H_1$: no las sigue.

### 11. ¿El plan elegido depende de la edad?
> Se encuesta a 400 usuarios y se arma una **tabla** cruzando el **plan contratado** (Básico/Pro/Premium) con el **grupo de edad** (joven/adulto/mayor). ¿El plan elegido **depende de** la edad? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Forma?** Tabla de conteos → χ². · **¿Cuál?** **DOS variables** cruzadas (plan × edad) → **independencia**.
> - → **χ² de independencia**, $e_{ij}=\dfrac{\text{fila}\cdot\text{col}}{n}$, $\nu=(f-1)(c-1)=(3-1)(3-1)=4$.
> - **Cola:** unilateral derecha.
> - $H_0$: el plan **es independiente** de la edad ; $H_1$: depende.

### 12. ¿La tasa de defectos difiere según la línea?
> Tres **líneas de producción** (1, 2, 3) producen piezas clasificadas como **OK / defectuosa**. ¿La proporción de defectos **difiere según la línea**? ($\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Forma?** Tabla de conteos (línea × estado) → χ². · **¿Cuál?** Misma variable (defecto) comparada **entre varias poblaciones** (líneas) → la cátedra lo llama **homogeneidad**.
> - → **χ² de homogeneidad** (operativamente **idéntico a independencia**: misma fórmula, mismos gl, misma cola). $\nu=(f-1)(c-1)$.
> - **Cola:** unilateral derecha.
> - **Truco:** "¿difiere **según** la línea/proveedor/rubro?" + tabla → homogeneidad.

---

## 🟠 Bloque 4 — Regresión (Práctico 9)

### 13. Horas de estudio vs nota
> Se registran las **horas de estudio** ($x$) y la **nota final** ($y$) de 12 alumnos. Se obtiene la recta $\hat y = 2{,}1 + 0{,}8x$ con $r^2=0{,}64$. ¿**Existe asociación lineal** entre horas y nota? ($S_b=0{,}21$, $\alpha=0{,}05$)

> [!success]- Ver respuesta
> - **¿Forma?** **Dos variables cuantitativas** ($x$, $y$) + recta dada → **regresión**.
> - "¿Hay asociación lineal?" se traduce **siempre** a la **PH para la pendiente** $\beta$: $H_0:\beta=0$ (no hay) ; $H_1:\beta\ne0$ (**bilateral**).
> - → **Prueba t para β**: $t_m=b/S_b$, $\nu=n-2=10$, $pv=2\,P(t>|t_m|)$.
> - Correlación: $r=+\sqrt{0{,}64}=0{,}8$ (signo de $b$, positiva). $r^2=0{,}64$ → el 64% de la variación de $y$ se explica por $x$.
> - **Cómo:** jamovi (Regresión → Lineal). ❌ Memorizar.

### 14. Interpretar un coeficiente (sin probar)
> En la regresión $\hat y = 50 - 1{,}2x$ ($y$ = ventas en miles, $x$ = precio en \$), **interpretá** el coeficiente de la pendiente y decí qué pasa con la correlación.

> [!success]- Ver respuesta
> - **¿Forma?** Regresión, pero piden **interpretación**, no una prueba.
> - **Pendiente $b=-1{,}2$:** por cada \$1 que sube el precio, las ventas **bajan** 1,2 mil unidades (en promedio).
> - Como $b<0$ → la **correlación es negativa** ($r=-\sqrt{r^2}$) → asociación lineal **inversa**.
> - ⚠️ No extrapolar fuera del rango de $x$; cuidado con interpretar el intercepto si $x=0$ no tiene sentido.

---

## 🔴 Bloque 5 — Mezcla difícil (sin pista del tema)

### 15. Cripto: rendimiento de dos estrategias
> Un trader corre la estrategia **Alfa** durante 30 días y la estrategia **Beta** durante otros 30 días distintos, anotando el rendimiento diario (%). ¿La estrategia Alfa **supera** a Beta en rendimiento medio?

> [!success]- Ver respuesta
> - Dos grupos **distintos** (días distintos para cada una) · media · probar → **t independientes** (+Levene).
> - "Alfa supera a Beta" → unilateral derecha: $H_0:\mu_A\le\mu_B$ ; $H_1:\mu_A>\mu_B$.

### 16. Reciclaje: misma gente, dos campañas
> A las **mismas 25 familias** se les midió cuántos kg reciclan por mes **antes** y **después** de una campaña educativa. ¿La campaña **aumentó** el reciclaje?

> [!success]- Ver respuesta
> - Mismo individuo medido 2 veces ("mismas 25 familias, antes/después") → **t apareadas**. $d=\text{después}-\text{antes}$, $H_0:\mu_d=0$.
> - "aumentó" → unilateral. **No** es independientes (es el mismo n y el mismo sujeto).

### 17. App: ¿la valoración media llega a 4?
> Una app pide que la **valoración media** de los usuarios sea **al menos 4 estrellas**. Sobre una muestra de 40 reseñas, ¿se alcanza ese estándar?

> [!success]- Ver respuesta
> - Un grupo · media · probar → **t de una muestra**.
> - "al menos 4 / ¿se alcanza?" → se quiere ver si **NO llega**: planteo según lo que se busca demostrar; típicamente $H_0:\mu\ge4$ ; $H_1:\mu<4$ (unilateral izquierda).

### 18. ¿La marca preferida depende del país?
> Se encuesta en 3 países y se cuenta cuántos prefieren cada una de 4 marcas, en una **tabla**. ¿La preferencia de marca **se relaciona con** el país?

> [!success]- Ver respuesta
> - Tabla de conteos, **dos variables** (marca × país) → **χ² de independencia/homogeneidad**, $\nu=(4-1)(3-1)=6$, **unilateral derecha**.

### 19. Spam: ¿una sola proporción o dos?
> Un filtro antispam dice bloquear el **95%** de los correos no deseados. En una prueba con 500 spams, bloqueó 463. ¿El filtro **cumple** lo prometido?

> [!success]- Ver respuesta
> - **Un** grupo · **proporción** (463/500) · probar → **z para una proporción** (¡NO diferencia de proporciones! hay un solo grupo).
> - "¿cumple el 95%?" se suele plantear $H_0:p\ge0{,}95$ ; $H_1:p<0{,}95$.
> - **Trampa:** que aparezca un % no significa "diferencia de proporciones"; eso requiere **dos** grupos.

### 20. ⚠️ IC, no PH
> Sobre 30 mediciones de consumo eléctrico (kWh), se pide **acotar con un 98% de confianza** el consumo medio mensual del edificio.

> [!success]- Ver respuesta
> - Un grupo · media · **estimar** ("acotar con 98% de confianza") → **IC para la media (t)**, $\nu=29$.
> - **No hay hipótesis.** El verbo *acotar/estimar/confianza* manda: es IC, no PH. (98% → si fuera por z, $z=2{,}33$; acá va t por gl.)

---

> [!quote] El reflejo que tenés que automatizar
> Leés el enunciado → **tapás los números** → respondés las 3 preguntas → decís la prueba y la cola **antes** de calcular nada. Si lográs hacer esto en 10 segundos con cualquier cuento, la "forma" ya no te puede sorprender.

🔙 Volver: [[00 - GUÍA 2do Parcial]] · [[10 - Árbol de decisión (chuleta rápida)]] · Resueltos paso a paso: [[08 - Parciales ejemplo resueltos (oficial)]] · [[06 - TODOS los ejercicios resueltos]]
