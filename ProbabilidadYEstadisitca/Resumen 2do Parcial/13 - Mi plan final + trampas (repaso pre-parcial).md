---
tags:
  - probabilidad-estadistica
  - 2do-parcial
  - plan
  - repaso
tema: Plan final de repaso + mis trampas personales (parcial martes)
materia: Probabilidad y Estadística
parcial: 2
---

# 🎯 Mi plan final + mis trampas (parcial martes 02/06)

> [!success] Diagnóstico al domingo 31/05
> En una sesión de práctica resolví bien **~15 de 18** identificaciones (incluida la trampa más difícil: apareadas vs independientes, y la distinción z vs χ²), y **2 ejercicios completos de punta a punta** sin trabarme. **El método lo tengo.** Lo que falta son 3-4 detalles puntuales y aceitar jamovi. No es "no entiendo" — es pulir.

---

## ⚠️ MIS 3 TRAMPAS (las que se me escaparon hoy)

> [!danger] 1. No inventar dirección en la cola
> Si el enunciado NO dice "sube" ni "baja" → es **bilateral**.
> - "difiere / distinto / cambió / modifica / altera" → **bilateral** (×2)
> - "supera / mayor / más que / al menos" → derecha · "menor / inferior / baja" → izquierda
> - 🔴 **Ojo el verbo escondido:** *"promete más del 80%, ¿se cumple?"* → el "¿se cumple?" suena neutro PERO "más del 80%" tiene dirección → **derecha**. Mirá QUÉ afirma, no solo la pregunta.

> [!danger] 2. χ² y β NO se eligen la cola
> - **χ²** (bondad / indep. / homog.) → SIEMPRE **derecha**.
> - **test de β** (regresión) → SIEMPRE **bilateral** (β=0 vs β≠0).
> - La dirección de la relación (positiva/negativa) la da el signo de **b/r** DESPUÉS, no la cola.

> [!danger] 3. Apareadas vs t una muestra
> - **Apareadas** = el MISMO individuo medido DOS veces (antes/después, mismo n).
> - **t una muestra** = un grupo medido UNA vez, comparado contra un VALOR FIJO (ej: "¿el largo difiere de 50 mm?" → 50 no es una 2da medición, es μ₀).
> - 🔴 Y la otra cara: "viejo/nuevo" con **n distinto** (15 vs 18) = grupos distintos = **independientes**, NO apareadas.

> [!tip] Bonus que ya casi domino
> - **p** (proporción / %) vs **μ** (media). "X de N" o "%" → p. Promedio → μ.
> - **z (proporciones) NO lleva df** · t y χ² SÍ (t: n−1 · β: n−2 · χ² bondad: c−1 · χ² tabla: (f−1)(c−1)).
> - **×2 SOLO en bilateral.** Unilateral = una cola, sin ×2.

---

## 📅 PLAN — Lunes 01/06 (ventana real: ~14 a 18:30 hs)

> [!warning] Tengo poco tiempo: vuelvo de laburar a las 14 y a las 19 entro al labo (presentación, solo leer). Ventana útil ≈ 3-4 hs. Hay que priorizar a muerte. Orden por rentabilidad:

> [!todo] Bloque 1 (~30 min) — calentar y blindar trampas
> - [ ] Leer **mis 3 trampas** (arriba) + pasada al [[10 - Árbol de decisión (chuleta rápida)|árbol]] (los 2 diagramas).
> - [ ] Pasar el [[11 - Drill - Identificar la prueba (solo enunciados)|drill]] rápido (tapar respuestas, solo identificar).

> [!todo] Bloque 2 (~2 hs) — LO MÁS IMPORTANTE: ejercicios completos con jamovi
> - [ ] **3-4 ejercicios COMPLETOS** de [[08 - Parciales ejemplo resueltos (oficial)]] y [[06 - TODOS los ejercicios resueltos]], de los que vienen con **datos crudos → jamovi**: independientes, apareadas, χ², regresión.
> - [ ] Con **jamovi abierto**, practicar QUÉ botón toco en cada uno (rutas en [[10 - Árbol de decisión (chuleta rápida)]] secc. 6 + cheat-sheet de [[00 - GUÍA 2do Parcial]]).

> [!todo] Bloque 3 (~30 min) — fórmulas y cierre
> - [ ] Repasar la **hoja de fórmulas permitida**: qué está (una población: media y proporción) y qué va de memoria (dos grupos, χ², regresión) → [[05 - Formulario y errores típicos]].
> - [ ] Dejar lista la hoja para llevar.

> [!tip] Después del labo (noche)
> - [ ] Si tenés energía, NADA pesado: una ojeada de 10 min al árbol y listo.
> - [ ] **Dormir bien >>> estudiar de más.** El martes a la mañana se rinde mejor descansado.

---

## ☀️ Martes 02/06 — antes de entrar (10 min)

> [!check] Checklist express
> - [ ] Ojeada al **árbol** + las **3 trampas** (10 min, nada nuevo).
> - [ ] Desayunar.
> - [ ] Llevar la **hoja de fórmulas**.
> - [ ] Respirar: ya sé el método.

---

## 🧭 La regla de oro para el parcial

> [!important] Antes de calcular NADA, en cada ejercicio:
> 1. **ÁRBOL** → ¿qué prueba? (cuántos grupos + tipo de dato)
> 2. **VERBO** → ¿qué cola? (sube=derecha · baja=izquierda · difiere=bilateral · χ²=derecha · β=bilateral · IC=sin cola)
> 3. **Recién ahí**, la cuenta (jamovi si hay datos crudos, a mano si están resumidos).
> 4. **pv < α ⇒ Rechazo H0**
> 5. **Conclusión en palabras** + (si preguntan) el error: rechacé → tipo I (α) · no rechacé → tipo II (β).
>
> 💡 El 80% del puntaje está en **identificar bien** y **concluir en palabras**. No te apures a la cuenta.

> [!quote] Recordatorio
> Empecé el domingo pensando "voy a desaprobar". Los hechos dijeron otra cosa: leo cualquier enunciado y resuelvo el ciclo entero. **Llego bien. Tranquilo y a confiar en el método.** 💪

🔙 [[00 - GUÍA 2do Parcial]] · [[10 - Árbol de decisión (chuleta rápida)]] · [[11 - Drill - Identificar la prueba (solo enunciados)]] · [[12 - Glosario de letras y símbolos]]
