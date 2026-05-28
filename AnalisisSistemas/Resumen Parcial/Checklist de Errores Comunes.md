---
title: Checklist de Errores Comunes
tags:
  - analisis-sistemas
  - parcial
  - checklist
  - errores
aliases:
  - Checklist
  - Errores Comunes
---

# ⚠️ Checklist de Errores Comunes

> [!danger] Repasá esto ANTES de entregar
> Todos estos errores fueron marcados por la cátedra en los **ejemplos corregidos** (sistema de seguridad vial — radares CABA). Son los puntos donde más se pierde nota.

## 🎭 [[Casos de Uso]]

- [ ] ¿Identifiqué **todos** los actores? (incluidos **otros sistemas** —policía, Registro de Propiedad Automotor— y el rol que **configura** los radares)
- [ ] ¿Los actores del **diagrama** coinciden con los de la **narrativa**?
- [ ] ¿El **sistema** NO figura como actor de sí mismo?
- [ ] ¿El **nombre** del CU = **código + actor + acción en gerundio**?
- [ ] ¿Marqué el actor principal con **(P)**?
- [ ] ¿El **primer paso** lo ejecuta un **actor** (nunca el sistema)?
- [ ] ¿Cada paso dice **quién** lo hace (Actor / Sistema) + acción?
- [ ] ¿Refleja la **interacción** actor↔sistema con **detalle** (columnas de tablas, orden…)?
- [ ] ¿Los **mensajes** del sistema van **entre comillas**?
- [ ] ¿La **precondición** es algo realmente **previo**? (si no hay, **no** poner; es raro que un CU tenga)
- [ ] ¿Tiene **poscondición**?
- [ ] ¿Las relaciones son `<<include>>` / `<<extends>>` (**nunca `use`**) y con la **dirección correcta**?
- [ ] `<<include>>` = obligatorio · `<<extends>>` = opcional/condicional

## 🔀 [[Diagramas de Actividades]]

- [ ] ¿Cada actividad tiene **1 entrada y 1 salida**? (ninguna queda colgada)
- [ ] ¿Hay **un solo inicio**? (puede haber varios fines)
- [ ] ¿La **bifurcación** es un rombo **SIN texto adentro**, con condiciones **afuera entre corchetes**, y **NUNCA "sí/no"**?
- [ ] ¿Cerré la **bifurcación con unificación** y el **fork con join**?
- [ ] ⚠️ ¿Evité mandar ramas de una **bifurcación** directo a un **join**? (el join espera **todas**; la bifurcación ejecuta **una** → **deadlock**)
- [ ] ¿Usé **fork solo si** el enunciado dice **"al mismo tiempo"**?
- [ ] ¿Se sabe **quién** hace cada actividad? (en el nombre o con **calles**)
- [ ] ¿Figuran los **actores / el sistema**? (usar calles)
- [ ] Recordar: el DA **no** lleva precondición ni poscondición.

## 🏛️ [[Diagramas de Clases]]

- [ ] ¿Evité la **clase "Sistema" / "Procesador" que hace todo**? (*clase Dios* — **el sistema NO es una clase**; repartir responsabilidades)
- [ ] ¿Evité **clases ajenas al dominio**? (ej.: `Usuario`/login si el enunciado no lo pide)
- [ ] ¿Incluí **todas las clases del dominio**? (Cámara, Infracción, Foto…)
- [ ] ¿Puse los **atributos relevantes**? (velocidad máxima, datos del vehículo, ubicación del radar)
- [ ] ¿Modelé las **relaciones necesarias**? (ej.: Radar ↔ Multa) con su **cardinalidad**
- [ ] ¿Visibilidad correcta? `+` público · `−` privado · `#` protegido

## ⏱️ [[Diagramas de Secuencia]]

- [ ] ¿Los participantes llevan **`nombre:Clase`** (con los **dos puntos**)?
- [ ] ¿El **sistema** NO es actor ni clase? (repartir en Radar, Cámara, etc.)
- [ ] ¿Las **búsquedas** que recorren elementos están dentro de un **`loop`**?
- [ ] ¿Cada **mensaje** corresponde a un **método** de la clase receptora?
- [ ] ¿Usé `ALT` para `if`/`if-else`/`switch` y `FOR`/`loop` para los bucles, con la **guarda entre corchetes**?

---

> [!tip] Patrón transversal
> Notá que **el mismo error aparece en casi todos los diagramas**: tratar **"el sistema" como un actor/clase única que hace todo**, y **olvidar actores u objetos del dominio** (especialmente **otros sistemas** y dispositivos). Si dominás esos dos, evitás la mayoría de las correcciones.

Volver al [[Análisis de Sistemas - Índice|índice]].
