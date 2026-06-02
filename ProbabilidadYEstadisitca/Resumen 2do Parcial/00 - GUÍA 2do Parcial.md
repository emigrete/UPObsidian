---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - resumen
  - jamovi
materia: Probabilidad y Estadística
parcial: 2
alcance: Prácticos 6 a 9
---

# 📊 GUÍA 2do Parcial — Probabilidad y Estadística

> [!abstract] ¿Qué entra?
> El **segundo parcial empieza en la Unidad 6 del práctico** y va hasta la 9. Es todo **inferencia estadística**:
> - [[01 - Práctico 6 - Inferencia para una población|Práctico 6]] → Inferencia para **una** población (Intervalos de Confianza + Prueba de Hipótesis)
> - [[02 - Práctico 7 - Comparación de dos muestras|Práctico 7]] → Comparación de **dos** muestras
> - [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)|Práctico 8]] → Datos categóricos / Pruebas no paramétricas (**Chi-cuadrado**)
> - [[04 - Práctico 9 - Regresión y correlación|Práctico 9]] → Regresión y correlación lineal

---

## 🌳 ÁRBOL DE DECISIÓN: ¿QUÉ PRUEBA USO?

Esto es lo que más se evalúa. **Antes de calcular nada, identificá:**

> [!question] Paso 1 — ¿Cuántas poblaciones y qué tipo de variable?

```
¿Qué quiero analizar?
│
├── UNA población  → Práctico 6
│   ├── Media (variable cuantitativa, σ desconocido) ........ PRUEBA t DE UNA MUESTRA
│   └── Proporción / porcentaje (% de éxitos) .............. PRUEBA z PARA PROPORCIÓN
│
├── DOS poblaciones → Práctico 7
│   ├── Medias, DOS grupos DISTINTOS de individuos ......... t MUESTRAS INDEPENDIENTES
│   │      (primero test de Levene; si varianzas ≠ → Welch)
│   ├── Medias, MISMO individuo medido 2 veces ............. t MUESTRAS APAREADAS (pareadas)
│   │      (antes/después, pre-post, test-retest)
│   └── Proporciones de dos grupos ........................ z DIFERENCIA DE PROPORCIONES
│
├── DATOS CATEGÓRICOS / conteos en tablas → Práctico 8 (Chi-cuadrado χ²)
│   ├── ¿Una muestra sigue proporciones dadas/históricas/uniforme? .. χ² BONDAD DE AJUSTE
│   ├── ¿Dos variables son independientes? (1 muestra, 2 variables) .. χ² INDEPENDENCIA
│   └── ¿Una variable categórica igual en k poblaciones? ............ χ² HOMOGENEIDAD
│       (independencia y homogeneidad → misma operación en jamovi)
│
└── RELACIÓN entre dos variables CUANTITATIVAS → Práctico 9
    └── Recta de regresión + correlación (r, r²) + PH para β
```

> [!tip] Palabras clave que delatan la prueba
> - **"antes y después" / "pre-post" / "el mismo grupo"** → t **apareadas**
> - **"dos grupos / dos sucursales / hombres vs mujeres / dos algoritmos"** (distintos individuos) → t **independientes**
> - **"porcentaje / proporción / % de…"** una sola → z proporción ; dos → z diferencia de proporciones
> - **"¿depende de? / ¿está relacionado con? / ¿es independiente de?"** con tabla de conteos → χ² **independencia**
> - **"¿se mantienen las proporciones históricas? / ¿es uniforme? / ¿sigue tal distribución?"** → χ² **bondad de ajuste**
> - **"¿diferencias según la línea/proveedor/rubro?"** con tabla → χ² **homogeneidad**
> - **"estimar y en función de x / recta / asociación lineal"** → regresión

---

## 🧭 MÉTODO GENERAL DE UNA PRUEBA DE HIPÓTESIS

Siempre los mismos pasos (sirve para TODOS los prácticos 6 a 8):

1. **Hipótesis de investigación** (en palabras: lo que se quiere demostrar).
2. **Hipótesis estadísticas:**
   - $H_0$ (nula): afirma que **NO** hay efecto/diferencia/cambio. Lleva el `=`.
   - $H_1$ (alternativa): lo que se quiere probar. **Rechazar $H_0$ = tomar la acción.**
3. **Elegir la prueba** (ver árbol de decisión).
4. **Calcular el p-valor** (con jamovi).
5. **Condición de Rechazo (CR):** $\boxed{\text{si } p\text{-valor} < \alpha \Rightarrow \text{Rechazar } H_0}$
6. **Conclusión** en términos del problema.

> [!warning] La regla de oro (vale para casi todo)
> $$p\text{-valor} < \alpha \;\Rightarrow\; \text{Rechazo } H_0$$
> $$p\text{-valor} > \alpha \;\Rightarrow\; \text{NO Rechazo } H_0$$
> Donde $\alpha$ = nivel de significación = riesgo (te lo dan en el enunciado: 0,05; 0,04; 0,01; etc.).

---

## 🔑 CONCEPTOS CLAVE (caen siempre en teoría)

> [!info] Definiciones
> - **Parámetro:** característica de la **población** (constante). Ej: $\mu$, $\sigma^2$, $p$.
> - **Estadístico / Estimador:** característica de la **muestra** (variable aleatoria). Ej: $\bar{x}$, $S^2$, $\hat{p}$.
> - **Estimadores puntuales:** $\bar{x}$ estima $\mu$ · $S^2$ estima $\sigma^2$ · $S$ estima $\sigma$ · $\hat{p}$ estima $p$.
> - **Nivel de confianza** $(1-\alpha)$: de cada 100 intervalos construidos con muestras distintas, $(1-\alpha)\cdot100$ contienen al verdadero parámetro.
> - **p-valor:** mínimo nivel de significación para el que se rechazaría $H_0$.

### Errores y potencia

| | $H_0$ Verdadera | $H_0$ Falsa |
|---|---|---|
| **Rechazo $H_0$** | ❌ Error tipo I ($\alpha$) | ✅ Decisión correcta |
| **No Rechazo $H_0$** | ✅ Decisión correcta | ❌ Error tipo II ($\beta$) |

- $\alpha = P(\text{Rechazar } H_0 \mid H_0 \text{ verdadera})$ → **error tipo I** (el más grave, = nivel de significación).
- $\beta = P(\text{No rechazar } H_0 \mid H_0 \text{ falsa})$ → **error tipo II**.
- **Potencia** $= 1-\beta = P(\text{Rechazar } H_0 \mid H_0 \text{ falsa})$ → probabilidad de tomar la decisión correcta (detectar el efecto que sí existe).

> [!example] Cómo redactar los errores (modelo de examen)
> "Si rechazar $H_0$ significa **comprar la máquina**":
> - **Error tipo I:** comprar la máquina cuando en realidad NO mejora la producción.
> - **Error tipo II:** NO comprar la máquina cuando en realidad SÍ mejora la producción.
>
> ¿Qué α conviene? Quien teme el error tipo I (comprar de más) elige **α chico (0,01)**. Quien quiere que la acción se tome (al vendedor le interesa vender) le conviene **α grande (0,10)**.

---

## 🖥️ CHEAT-SHEET DE JAMOVI (rutas de menú)

> [!tip] Cómo cargar datos
> Menú ☰ → **Importar especial** → seleccionar el Excel/CSV. O escribir los datos directamente en columnas.

| Necesito… | Ruta en jamovi |
|---|---|
| Medidas resumen (media, mediana, modo, DE, percentiles) | **Exploración → Descriptivas** |
| Gráfico de dispersión (regresión) | **Exploración → Gráfico de dispersión** |
| PH media, 1 población | **Pruebas t → Prueba t de una Muestra** |
| Comparar medias, 2 grupos distintos | **Pruebas t → Prueba t para Muestras Independientes** |
| Comparar medias, antes/después | **Pruebas t → Prueba t para Muestras Pareadas** |
| Comparar 2 proporciones | **Recuento → Muestras independientes (Prueba de independencia χ²)** → tildar *Prueba z para diferencia de 2 proporciones* |
| χ² bondad de ajuste | **Recuento → N Resultados (χ² de bondad de ajuste)** |
| χ² independencia / homogeneidad | **Recuento → Prueba de independencia χ²** |
| Recta de regresión, r, r², test de β | **Regresión → Regresión lineal** |
| Buscar fractiles / probabilidades de t, z, χ² | módulo **distrACTION** (T-Distribution, etc.) |

> [!note] Comprobación de supuestos en jamovi (pruebas t)
> - **Prueba de Normalidad (Shapiro-Wilk):** $H_0$: los datos son normales. Si **pv > α → se cumple normalidad** (NO se rechaza).
> - **Prueba de homogeneidad (Levene):** $H_0$: varianzas iguales. Si **pv > α → varianzas iguales** → uso *t de Student*. Si **pv < α → varianzas distintas** → destildar t de Student y tildar **t de Welch**.

---

## 📂 Notas del parcial

- [[09 - Explicado de CERO (teoría + jamovi paso a paso)]] 🐣 — **empezá por acá si no entendés nada**: la teoría en criollo (μ, p-valor, α) + jamovi clic por clic
- [[Hoja de Resumen - 2do Parcial]] 🏆 — **chuleta integral**: fórmulas permitidas + cómo se hace + ejercicios integradores (todo en una página)
- [[01 - Práctico 6 - Inferencia para una población]]
- [[02 - Práctico 7 - Comparación de dos muestras]]
- [[03 - Práctico 8 - Datos categóricos (Chi cuadrado)]]
- [[04 - Práctico 9 - Regresión y correlación]]
- [[05 - Formulario y errores típicos]]
- [[06 - TODOS los ejercicios resueltos]] ⭐ — los ~52 ejercicios del parcial resueltos uno por uno
- [[07 - Ejercicios de repaso resueltos]] 📝 — la ejercitación de repaso del parcial resuelta y explicada
- [[08 - Parciales ejemplo resueltos (oficial)]] 🧪 — los **2 parciales ejemplo de la cátedra** resueltos paso a paso y verificados contra las respuestas oficiales
- [[Preguntas Integradoras - Probabilidad (2do Parcial)]] ❓ — preguntas estilo parcial (teoría + aplicadas) con respuestas plegadas
- [[11 - Drill - Identificar la prueba (solo enunciados)]] 🎯 — **20 enunciados con formas variadas y trampas**: solo identificar qué prueba es (con las 3 preguntas) y la respuesta en desplegable
- [[12 - Glosario de letras y símbolos]] 🔠 — **qué significa cada letra** (μ, σ, p̂, α, β, r…) y cuáles cambian de significado según el contexto (la β: error tipo II vs pendiente)
- [[14 - Cómo calcular el estadístico (tm y zm) a mano]] 🧮 — **las 5 fórmulas del tm/zm** + por qué la cola NO cambia el estadístico + ejemplo de apareadas resuelto a mano paso a paso
- [[13 - Mi plan final + trampas (repaso pre-parcial)]] 🎯 — **diagnóstico + mis 3 trampas + plan del lunes y checklist del martes** (parcial 02/06)
