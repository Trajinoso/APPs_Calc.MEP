# README — App.Cal_FV 26
## Calculadora de Configuración de Instalaciones Fotovoltaicas

---

> **Documento de consulta técnica completo**
> Recoge el historial íntegro del proyecto: solicitudes, decisiones de diseño, informes técnicos, fórmulas, normativa, changelog de versiones y guía de uso.
> Válido como entrega a cliente y como punto de restauración para continuar el desarrollo en cualquier sesión de IA.

---

## ÍNDICE

1. [Descripción del Proyecto](#1-descripción-del-proyecto)
2. [Paquete de Entregables](#2-paquete-de-entregables)
3. [Protocolo de Trabajo](#3-protocolo-de-trabajo)
4. [Changelog Completo de Versiones](#4-changelog-completo-de-versiones)
5. [Historial de Solicitudes y Decisiones](#5-historial-de-solicitudes-y-decisiones)
6. [Normativa Aplicada](#6-normativa-aplicada)
7. [Constantes del Sistema](#7-constantes-del-sistema)
8. [Parámetros de Entrada — Tabla Completa](#8-parámetros-de-entrada--tabla-completa)
9. [Motores de Cálculo — Fórmulas Completas](#9-motores-de-cálculo--fórmulas-completas)
10. [Nomenclatura de Strings](#10-nomenclatura-de-strings)
11. [Arquitectura CSS — Patrón Sticky](#11-arquitectura-css--patrón-sticky)
12. [Arquitectura JavaScript](#12-arquitectura-javascript)
13. [Caso de Verificación Numérica](#13-caso-de-verificación-numérica)
14. [Guía de Uso de la Aplicación](#14-guía-de-uso-de-la-aplicación)
15. [To-Do y Mejoras Futuras](#15-to-do-y-mejoras-futuras)
16. [Registro de Informes Técnicos (ITR)](#16-registro-de-informes-técnicos-itr)

---

## 1. DESCRIPCIÓN DEL PROYECTO

### Qué es

**App.Cal_FV 26** es una herramienta de cálculo para ingeniería de instalaciones fotovoltaicas, desarrollada como aplicación web de página única (HTML5, sin dependencias externas). Permite dimensionar y verificar eléctricamente un campo solar completo con múltiples inversores.

### Para qué sirve

- Calcular el número máximo y mínimo de módulos en serie por string según las condiciones térmicas del emplazamiento
- Configurar la distribución de strings por MPPT para cada inversor
- Verificar tensiones (V_max vs Vdc_max del inversor, V_min vs rango MPPT)
- Calcular corrientes reales corregidas por temperatura (Isc_max, Impp_real)
- Calcular la potencia real con derating térmico y detectar posible clipping
- Dimensionar secciones de cable DC por caída de tensión (CDT ≤ 1,5%)
- Generar un resumen técnico de memoria de proyecto copiable
- Imprimir un informe PDF completo listo para entrega a cliente

### Tecnología

| Aspecto | Detalle |
|---|---|
| Lenguaje | HTML5 + CSS3 + JavaScript ES6 |
| Dependencias runtime | **Ninguna** (archivo único autocontenido) |
| Fuentes | Google Fonts CDN (Barlow, Barlow Condensed, Share Tech Mono) |
| Compatibilidad | Chrome, Firefox, Safari, Edge (escritorio) |
| Uso | Abrir directamente en navegador sin servidor |

---

## 2. PAQUETE DE ENTREGABLES

### Archivos de la versión actual (v4.0)

| Archivo | Descripción |
|---|---|
| `App.Cal_FV 26 v4.0.html` | Aplicación completa. Abrir en navegador. |
| `ITR.Cal_FV 26 v4.0.md` | Informe Técnico de Requisitos. Especificaciones, fórmulas, bitácora. |
| `PM.Cal_FV 26 v4.0.md` | Prompt Master. Registro de auditoría y guía para replicar en otra IA. |
| `README.md` | Este documento. Consulta técnica completa del proyecto. |

### Archivos históricos disponibles

| Versión | App | ITR | PM |
|---|---|---|---|
| v3.0 | `App.Cal_FV 26 v3.0.html` | `ITR.Cal_FV 26 v3.0.md` | `PM.Cal_FV 26 v3.0.md` |
| v3.1 | `App.Cal_FV 26 v3.1.html` | `ITR.Cal_FV 26 v3.1.md` | `PM.Cal_FV 26 v3.1.md` |
| v3.2 | `App.Cal_FV 26 v3.2.html` | `ITR.Cal_FV 26 v3.2.md` | `PM.Cal_FV 26 v3.2.md` |
| v4.0 | `App.Cal_FV 26 v4.0.html` | `ITR.Cal_FV 26 v4.0.md` | `PM.Cal_FV 26 v4.0.md` |

---

## 3. PROTOCOLO DE TRABAJO

El desarrollo ha seguido un protocolo de trazabilidad rígida en 6 fases:

```
FASE 1 → Informe de Requisitos (ITR) — SIN código
FASE 2 → Gestión de cambios en el ITR hasta aceptación
FASE 3 → Generación de código SOLO tras "ACEPTO INFORME"
FASE 4 → Snapshot de estado (punto de restauración)
FASE 5 → Paquete de entregables (App + ITR + PM)
FASE 6 → Snapshot de cierre de sesión
```

**Reglas críticas del protocolo:**
- Nunca se genera código antes de que el ITR esté validado
- Cada versión tiene su propio ID de informe `INF-YYYYMMDD-XX`
- El código siempre incluye en cabecera: `BASADO EN INFORME ACEPTADO: [ID] v[X.X]`
- El Prompt Master permite replicar el resultado exacto en otra IA
- El README es el documento de consulta acumulativo para el cliente

---

## 4. CHANGELOG COMPLETO DE VERSIONES

### v1.0 — INF-20260406-01 (2026-04-06)
**Primera versión funcional de CalcFV.**

**Solicitud original:**
> "Desarrolla una app en HTML donde se introduzcan los diferentes inputs y dé como resultado todos los datos necesarios para el cálculo de una instalación FV. Se solicitará también el número de MPPTs y canales por cada uno. Dará también el número de strings necesarios por inversor."

**Funcionalidades implementadas:**
- Inputs del módulo FV (STC): Pmax, Voc, Vmpp, Isc, Impp, βVoc, βPmpp, fusible
- Inputs del inversor: Vdc_max, Pac, Vmpp_min/max, Imax_str/mppt, n_MPPTs, canales/MPPT
- Inputs de instalación: n_módulos, Tmin, Tcell_hot, tensión red
- Cálculo de N_max y N_min de módulos por string
- Distribución automática de módulos entre strings (sliders)
- Validación de tensiones Voc_cold vs Vdc_max y Vmpp_hot vs rango MPPT
- Verificación de corrientes por MPPT (con Isc_STC)
- Ratio DC/AC con aviso de rango óptimo (1,10–1,40)
- Producción anual estimada (1600–1850 kWh/kWp)
- Protecciones orientativas (fusibles, magnetotérmico AC, diferencial, SPD)
- Resumen técnico copiable al portapapeles

**Diseño visual:** Tema oscuro industrial, acentos naranja solar (#f59e0b), fuentes técnicas.

---

### v1.1 — INF-20260406-01 (2026-04-06)
**Mejoras de UX e interactividad.**

**Solicitud:**
> "Teniendo en cuenta el número máximo de paneles por string, se podrá seleccionar desde el mínimo y el máximo para poder tener un número de paneles diferente por cada string. Si solo hay un canal por MPPT este coincide con el máximo."

**Cambios:**
- Sliders reemplazados por **inputs numéricos editables** con valor óptimo pre-rellenado
- Rango `[N_min, N_max]` con clamping automático al introducir valor fuera de rango
- **Botón "GENERAR RESUMEN"** separado del botón "COPIAR"
- El resumen ya no aparece como "calculando" — se genera bajo demanda
- **Validación Vmpp** en rango completo `[vmpp_min, vmpp_max]` del inversor (antes solo verificaba el mínimo)
- **4 pills por string:** V_max_string, V_min_string, margen hasta Vdc_max, estado rango MPPT con márgenes numéricos
- **Contador de módulos** asignados vs. declarados en tiempo real (semáforo verde/naranja/rojo)
- **Aviso de mismatch** si strings del mismo MPPT tienen diferente número de módulos

---

### v1.1 rev2 — INF-20260406-01 (2026-04-06)
**Motor de tensiones reescrito según especificación técnica formal.**

**Documento técnico adjunto procesado:**
Especificación completa del módulo de cálculo de tensiones fotovoltaicas con tratamiento de tipos de datos y lógica de flujo.

**Cambios en la lógica de cálculo:**

| Concepto | Antes | Después |
|---|---|---|
| Temperatura frío | `Voc_cold = Voc × (1 + \|bVoc\| × (25 − Tmin))` incorrecto | `t_cell_cold = t_amb_min` (célula = ambiente en amanecer) |
| Temperatura calor | Input directo de usuario `Tcell_hot` | `t_cell_hot = t_amb_max + 25°C` (offset fijo) |
| Coeficiente βVoc | `/100` sin blindaje de signo | `coeff = -(Math.abs(beta_voc)) / 100` |
| V criterio seguridad | Formula aproximada | `v_max_u = voc_stc × (1 + coeff × delta_t_cold)` |
| V criterio funcionamiento | Formula aproximada | `v_min_u = vmpp_stc × (1 + coeff × delta_t_hot)` |
| Inputs temperatura | `Tmin` + `Tcell` (absolutos) | `t_amb_min` + `t_amb_max` (ambientales históricos) |

**Añadido:**
- Barra de temperatura en resultados mostrando T_cell_cold, T_cell_hot, ΔTs, V_max/mód, V_min/mód
- Validación `v_max_u > v_min_u` con alerta descriptiva

---

### v2.0 — INF-20260407-02 (2026-04-07)
**Nuevo input αIsc, motor de corrientes y motor de potencia real.**

**Documento técnico adjunto procesado:**
Resumen completo de fórmulas de cálculo fotovoltaico con 4 bloques: temperaturas, tensiones, corrientes y potencia.

**Cambios:**
- **Nuevo input `αIsc`** (%/°C): coeficiente de temperatura de corriente (positivo, blindaje interno)
- **Motor corrientes:**
  - `Isc_max = Isc_STC × (1 + αIsc/100 × ΔT_hot)`
  - `Impp_real = Impp_STC × (1 + αIsc/100 × ΔT_hot)`
- **Verificación MPPT actualizada:** usa `Isc_max` en lugar de `Isc_STC`
- **`I_diseño_DC = 1,25 × Isc_max`** (antes usaba `Isc_STC`)
- **Motor potencia real:**
  - `P_real_mod = Pmax × (1 + βPmpp/100 × ΔT_hot)`
  - `P_real_total` y ratio de derating térmico
- **Dos ratios DC/AC:** nominal STC + real en condiciones de calor extremo
- **Aviso de clipping** cuando `ratio_real ≥ 1,0`
- **Footer MPPT ampliado:** muestra `Impp_real` e `Isc_max` en lugar de valores STC
- **Pestaña de Referencia Técnica** con 5 secciones + leyenda de variables

---

### v3.0 — INF-20260408-03 (2026-04-08)
**UX rediseñada + nueva pestaña de Secciones CC.**

**Solicitudes:**
1. UX sidebar: inputs no visibles al desplegar — propuesta de 3 opciones, elegida **Opción B**
2. Nueva pestaña para cálculo de secciones de cable DC por caída de tensión

**Decisiones de diseño confirmadas:**
- Punto 1: Sección calculada automáticamente + posibilidad de modificarla manualmente
- Punto 2: Longitudes introducidas por usuario por tramo y por string individualmente
- Punto 3: Identificadores generados automáticamente por la app
- Punto 4: Cable PV1-F cobre, ρ = 0,024 Ω·mm²/m
- Punto 5: Fórmula CDT DC: `cdt_V = 2 × L × I × ρ / S`; `cdt_% = cdt_V / V_string × 100`
- Corriente de diseño confirmada: `I = 1,25 × Isc_max` (UNE-HD 60364-7-712)
- ρ conservador para exterior: 0,024 Ω·mm²/m

**Cambios:**

**UX Sidebar (Opción B):**
- Zona fija superior (sticky): botones Calcular, PDF y Restaurar — siempre visibles
- Zona scrollable: inputs en cards colapsables
- Ancho aumentado a 430px
- Sin box de constantes ni normativa en el sidebar (movidos a Referencia Técnica)

**Pestaña Secciones CC:**
- Generación automática de una línea CC por cada string configurado
- Identificadores `INV.XX.XX.XX Tramo CC` (formato anterior, actualizado en v4.0)
- 3 tramos fijos por string: Cableado paneles / Paneles–Fusibles / Fusibles–Inversor
- Por tramo: selector de sección normalizada + input de longitud → cálculo cdt(V) y cdt(%)
- Footer por línea: TOTAL CDT (V) | TOTAL CDT (%) | semáforo ≤1,5%
- Botón "Aplicar S_mín automática": distribución proporcional por longitud

**Referencia Técnica ampliada:**
- Sección de constantes del sistema
- Sección de normativa de aplicación
- Fórmula CDT CC añadida
- Secciones normalizadas PV1-F

---

### v3.1 — INF-20260408-04 (2026-04-08)
**Corrección de bugs de layout + botón PDF + sección CC=0.**

**Solicitudes:**
1. Inputs del sidebar no visibles (bug)
2. Referencia Técnica: solo se ven títulos, el contenido no renderiza (bug)
3. Botón imprimir PDF con resumen técnico completo
4. Sección CC = 0 para tramos inexistentes

**Diagnóstico de bugs:**
- `.tab-panel.active { display:flex }` colapsaba hijos con `height:calc(100vh - N)` al no tener altura de referencia
- `.sb-scroll` sin `min-height:0` impedía el scroll en Firefox/Safari (bug de flex containers)
- `.fc-f { white-space:pre }` con ancho restringido por flex padre desbordaba sin scroll

**Correcciones:**
- `display:block` en `.tab-panel.active` + `height:calc(100vh - 90px)` explícita
- `min-height:0` en `.sb-scroll` — fix estándar para scroll en hijo flex
- `white-space:pre-wrap` + `overflow-x:auto` en `.fc-f`
- `overflow:visible` en `.ts` (secciones de teoría)

**Botón PDF (`🖨 PDF`):**
- Implementación: `window.print()` + `@media print` CSS
- Sin dependencias externas
- 9 bloques: módulo FV, inversor, temperaturas/tensiones, corrientes/potencia, potencias/ratios, strings, protecciones, secciones CC, normativa
- Activo solo si hay cálculo realizado
- Fondo blanco, texto negro, tablas colapsadas en impresión

**Sección CC = 0:**
- Opción `0 mm² (no existe)` en selector de sección de cada tramo
- Si S=0: CDT=0, fila inactiva visualmente (tachada, opacidad reducida)
- S_mín proporcional: omite los tramos con S=0 en el reparto del 1,5%
- Botón S_mín: no toca los tramos con S=0

---

### v3.2 — INF-20260409-05 (2026-04-09)
**Corrección definitiva del scroll del sidebar.**

**Solicitud:**
> "Sigue existiendo problemas con la visibilidad de los box de entrada de datos. Modifica para poder hacer scroll en la sidebar izquierda cuando estén todos abiertos."

**Diagnóstico:**
La arquitectura `sidebar { height:100%; overflow:hidden }` con hijo `sb-scroll { flex:1; min-height:0; overflow-y:auto }` fija la altura del sidebar al viewport. Los cards desplegados no pueden crecer más allá de ese límite. El scroll interno de `sb-scroll` solo funciona hasta esa altura fija.

**Solución — Arquitectura `position:sticky`:**

```
ANTES (fallaba):
  .sidebar { height:100%; overflow:hidden }  ← altura fija
  .sb-scroll { flex:1; min-height:0; overflow-y:auto }  ← scroll confinado

DESPUÉS (funciona):
  .sidebar { position:sticky; top:90px; overflow-y:auto }  ← sidebar ES el scroll
  .sb-fixed { position:sticky; top:0 }  ← botones siempre visibles dentro del sidebar
  .sb-scroll { padding normal, sin flex tricks }  ← flujo libre
```

**Por qué funciona:**
- El sidebar completo scrollea. Su contenido puede tener cualquier altura.
- `position:sticky` mantiene el sidebar visible en el viewport.
- Los botones usan `position:sticky; top:0` dentro del sidebar → siempre visibles.
- `.calc-layout` usa `align-items:start` → columnas con altura natural.

**Comparativa completa:**

| Elemento | v3.1 | v3.2 |
|---|---|---|
| `.tab-panel.active` | `display:block; height:calc(100vh-90px); overflow:hidden` | `display:block` solo |
| `.calc-layout` | `height:100%` | `align-items:start` |
| `.sidebar` | `height:100%; overflow:hidden` | `position:sticky; overflow-y:auto` |
| `.sb-fixed` | `flex-shrink:0` | `position:sticky; top:0` |
| `.sb-scroll` | `flex:1; min-height:0; overflow-y:auto` | `padding normal` |
| `.main` | `flex:1; min-height:0; overflow-y:auto` | `position:sticky; overflow-y:auto` |

---

### v4.0 — INF-20260409-06 (2026-04-09)
**Multi-inversor — visión global de campo solar.**

**Solicitud:**
> "Vamos a añadir opción de colocar múltiples inversores para tener una visión global de un campo solar. Se solicitará además del número de MPPTs y canales, el número de inversores. Se verán en pantalla y se nombrarán como INV01-MPPT1-01 o -02, dependiendo del número de canales por MPPT."

**Preguntas de diseño y respuestas:**
- A — ¿Todos los inversores son del mismo modelo? → **Cada inversor diferente** (parámetros propios)
- B — ¿Los módulos FV son iguales? → **Único módulo para toda la planta**
- C — ¿Distribución de módulos independiente? → **Sí, independiente por inversor**
- D — ¿Secciones CC independientes? → **Sí, independientes por inversor**

**Cambios:**

**Sidebar — Panel de inversores:**
- Input `Nº de inversores` con generación automática de cards
- Botón `+ Añadir inversor`
- Cada inversor: card colapsable con nombre libre + todos los parámetros DC/AC + módulos asignados
- Botón `×` para eliminar un inversor individual

**Motor de cálculo actualizado:**
- Motores comunes (módulo único): v_max_u, v_min_u, Isc_max, Impp_real, P_real_mod → iguales para todos
- Por cada inversor: N_max[i], N_min[i], cfgArr[i][m][c], Pdc_stc[i], P_real[i], ratio_STC[i], ratio_real[i], clipping[i]
- Resumen global: Pdc_total, Pac_total, P_real_total, ratio_global, E_anual

**Resultados — Panel principal:**
- Barra de temperatura común (módulo único, valores iguales para toda la planta)
- Banner global de planta: N_inv, módulos totales, Pdc/Pac totales, ratio global, P_real, energía anual
- Acordeón por inversor: cada uno muestra nombre, Pdc, Pac, ratio y rango N en cabecera; desplegable con banner rango, contador, grid MPPTs, 4 cards de resultados

**Nomenclatura:**
```
INV[ii]-MPPT[m]-[cc]
  ii = inversor 2 dígitos
  m  = MPPT sin ceros
  cc = canal 2 dígitos
```

**Secciones CC:**
- Organizadas en acordeón por inversor
- Líneas CC identificadas con nueva nomenclatura `INV01-MPPT1-01 Tramo CC`

**PDF:**
- Tabla resumen global de planta al inicio
- Sección independiente por inversor con salto de página automático
- Cada sección incluye: parámetros del inversor, tabla de strings, corrientes, protecciones, CDT CC

---

## 5. HISTORIAL DE SOLICITUDES Y DECISIONES

### Solicitud inicial
Aplicación HTML para cálculo de instalación FV con módulo Trina TSM-460 NEG9R.28 (460Wp) e inversor Huawei SUN2000-25KTL-M5 como caso de referencia.

### Decisiones técnicas relevantes adoptadas durante el desarrollo

| Versión | Decisión | Justificación |
|---|---|---|
| v1.0 | Tema oscuro industrial, acentos naranja solar | Coherencia con herramientas de ingeniería profesionales |
| v1.1 | Inputs numéricos en lugar de sliders | Los sliders no actualizaban el valor correctamente |
| v1.1 rev2 | `t_cell_cold = t_amb_min` (no `25 − Tmin`) | Especificación técnica formal: célula en frío = temperatura ambiente al amanecer |
| v1.1 rev2 | `t_cell_hot = t_amb_max + 25°C` (offset fijo) | Offset de 25°C es la regla práctica de ingeniería para calentamiento por radiación |
| v2.0 | `I_diseño = 1,25 × Isc_max` (no `Isc_STC`) | Verificación de MPPT debe hacerse con la corriente real en caliente, no la STC |
| v2.0 | `ρ = 0,024 Ω·mm²/m` | Valor conservador para cobre en exterior. Rango habitual 0,022–0,024 Ω·mm²/m |
| v3.0 | UX Opción B (sticky buttons + scroll natural) | Opciones A, B y C propuestas; B elegida por mantener el flujo visual del sidebar |
| v3.0 | V_ref CDT = V_max_string (frío) | Práctica habitual en proyectos FV: da el % más bajo, criterio favorable para el diseñador |
| v3.2 | Arquitectura `position:sticky` | Única solución que funciona en todos los navegadores sin restricciones de altura |
| v4.0 | Módulo FV único para toda la planta | Confirmado por el usuario; simplifica el modelo de datos |
| v4.0 | Inversores con parámetros completamente independientes | Cada inversor puede ser de modelo diferente |

---

## 6. NORMATIVA APLICADA

| Norma | Ámbito de aplicación en la herramienta |
|---|---|
| **REBT ITC-BT-40** | Instalaciones generadoras de baja tensión. Base para criterios de conexión a red, protecciones y medida. |
| **REBT ITC-BT-23** | Protección contra sobretensiones transitorias. Criterios de selección y coordinación de SPDs. |
| **IEC 62548** | Requisitos de diseño para sistemas FV. Criterio de fusibles de string (obligatorios cuando n_ch ≥ 3 strings en paralelo por MPPT). |
| **UNE-HD 60364-7-712** | Instalaciones de suministro solar fotovoltaico. Factor de diseño de conductores DC: `I_diseño = 1,25 × Isc_max`. |
| **UNE-EN 61643** | Dispositivos de protección contra sobretensiones (SPD). Tipos I, II y III. |
| **IEC 61215 / IEC 61730** | Cualificación y seguridad de módulos fotovoltaicos. Base para parámetros STC utilizados como inputs. |
| **IEC 60364-5-52** | Selección e instalación de conductores. Secciones normalizadas y criterios térmicos. |
| **UNE-EN 60228** | Conductores de cables eléctricos. Base para resistividad del cobre utilizada en la app. |

---

## 7. CONSTANTES DEL SISTEMA

Estas constantes están fijas en el código de la aplicación y **no son modificables por el usuario**:

| Constante | Valor | Descripción |
|---|---|---|
| `T_REF` | 25 °C | Temperatura de referencia STC (Standard Test Conditions) |
| `T_OFFSET` | 25 °C | Incremento térmico de la célula sobre la temperatura ambiente en condición de pleno sol (regla práctica de ingeniería) |
| `G_STC` | 1000 W/m² | Irradiancia de referencia en condiciones STC |
| `ρ_Cu` | 0,024 Ω·mm²/m | Resistividad del cobre en exterior, valor conservador para cable PV1-F (rango típico: 0,022–0,024) |
| `CDT_MAX` | 1,5 % | Caída de tensión máxima total admisible en los tramos CC de un string |
| `Factor_I` | 1,25 | Factor de mayoración de la corriente de diseño para conductores DC FV (UNE-HD 60364-7-712) |

---

## 8. PARÁMETROS DE ENTRADA — TABLA COMPLETA

### Módulo FV (único para toda la planta)

| Variable JS | Descripción | Unidad | Signo | Ejemplo | Validación |
|---|---|---|---|---|---|
| `pmax` | Potencia pico en STC | Wp | + | 460 | > 0 |
| `voc_stc` | Tensión de circuito abierto en STC | V | + | 52,1 | > vmpp_stc |
| `vmpp_stc` | Tensión en punto de máxima potencia en STC | V | + | 43,5 | < voc_stc |
| `isc` | Corriente de cortocircuito en STC | A | + | 11,20 | > impp |
| `impp` | Corriente en punto de máxima potencia en STC | A | + | 10,58 | < isc |
| `beta_voc` | Coeficiente de temperatura de tensión βVoc | %/°C | **negativo** | −0,24 | Forzado negativo internamente |
| `alpha_isc` | Coeficiente de temperatura de corriente αIsc | %/°C | **positivo** | +0,04 | Forzado positivo internamente |
| `bpmpp` | Coeficiente de temperatura de potencia βPmpp | %/°C | **negativo** | −0,30 | Forzado negativo internamente |
| `ifus` | Fusible máximo del módulo | A | + | 25 | > 0 |

### Condiciones de sitio (únicas para toda la planta)

| Variable JS | Descripción | Unidad | Ejemplo | Nota |
|---|---|---|---|---|
| `t_amb_min` | Temperatura ambiente mínima histórica | °C | −5 | Para calcular V_max (peor caso frío) |
| `t_amb_max` | Temperatura ambiente máxima histórica | °C | 40 | Para calcular V_min, Isc_max, P_real |
| `v_red` | Tensión de red AC | V | 400 | 400V trifásico / 230V monofásico |

### Por cada inversor i (v4.0)

| Variable JS | Descripción | Unidad | Ejemplo | Validación |
|---|---|---|---|---|
| `inv[i].nombre` | Nombre o modelo del inversor | texto | "Huawei SUN2000-25K" | Libre |
| `inv[i].vdc_max` | Tensión DC máxima de entrada | V | 1100 | > 0 |
| `inv[i].pac` | Potencia AC nominal | kW | 25 | > 0 |
| `inv[i].vmpp_min` | Límite inferior del rango MPPT | V | 200 | > 0 |
| `inv[i].vmpp_max` | Límite superior del rango MPPT | V | 1000 | < vdc_max |
| `inv[i].imax_str` | Corriente máxima por string (entrada) | A | 13 | > 0 |
| `inv[i].imax_mppt` | Corriente máxima por MPPT | A | 26 | > 0 |
| `inv[i].n_mppt` | Número de MPPTs del inversor | — | 2 | Entero 1–12 |
| `inv[i].n_ch` | Número de canales (strings) por MPPT | — | 2 | Entero 1–6 |
| `inv[i].n_mod` | Número de módulos asignados a este inversor | — | 70 | Entero > 0 |

### Pestaña Secciones CC (por tramo de cada string)

| Variable | Descripción | Unidad | Opciones |
|---|---|---|---|
| `S[t]` | Sección del conductor del tramo t | mm² | 0(no existe)/1,5/2,5/4/6/10/16/25/35/50/70/95/120 |
| `L[t]` | Longitud del tramo t | m | Input libre > 0 |

---

## 9. MOTORES DE CÁLCULO — FÓRMULAS COMPLETAS

### Motor 1 — Temperaturas y Tensiones

```
T_REF    = 25 °C
T_OFFSET = 25 °C

# Temperatura de célula en escenarios extremos
t_cell_cold = t_amb_min                          # frío: célula = ambiente (amanecer sin sol)
t_cell_hot  = t_amb_max + T_OFFSET               # caliente: ambiente + 25°C

# Variaciones respecto a STC
delta_t_cold = t_cell_cold - T_REF               # negativo → tensión sube
delta_t_hot  = t_cell_hot  - T_REF               # positivo → tensión baja

# Coeficiente normalizado (blindaje de signo)
coeff_V = -(Math.abs(beta_voc)) / 100            # siempre negativo

# Tensiones unitarias (por módulo)
v_max_u = voc_stc  × (1 + coeff_V × delta_t_cold)  # V_max → criterio SEGURIDAD
v_min_u = vmpp_stc × (1 + coeff_V × delta_t_hot)   # V_min → criterio FUNCIONAMIENTO MPPT

# Validación de coherencia
ASSERT v_max_u > v_min_u

# Rango válido de módulos en serie (POR INVERSOR i)
N_max[i] = floor(inv[i].vdc_max  / v_max_u)     # límite superior: seguridad
N_min[i] = ceil (inv[i].vmpp_min / v_min_u)     # límite inferior: funcionamiento MPPT

# Tensiones del string (N módulos en serie)
V_max_string(N) = v_max_u × N   →  debe ser < inv[i].vdc_max
V_min_string(N) = v_min_u × N   →  debe estar en [inv[i].vmpp_min, inv[i].vmpp_max]
```

### Motor 2 — Corrientes

```
# Coeficiente normalizado (blindaje de signo)
coeff_I = Math.abs(alpha_isc) / 100              # siempre positivo

# Corrientes en condición de calor extremo (COMÚN para todos los inversores)
Isc_max   = isc  × (1 + coeff_I × delta_t_hot)  # Isc real en caliente
Impp_real = impp × (1 + coeff_I × delta_t_hot)  # Impp real en caliente

# Corriente de diseño para conductores DC (UNE-HD 60364-7-712)
I_diseño = 1,25 × Isc_max

# Verificación por MPPT (POR INVERSOR i)
Isc_max_MPPT[i] = inv[i].n_ch × Isc_max   →  debe ser ≤ inv[i].imax_mppt

# Corriente AC nominal
I_AC = Pac × 1000 / (√3 × 400)    # trifásico 400V
I_AC = Pac × 1000 / 230            # monofásico 230V

# Magnetotérmico AC (calibre normalizado inmediatamente superior a I_AC)
Calibres normalizados: 10/16/20/25/32/40/50/63/80/100/125 A

# Fusibles de string (IEC 62548)
needFuse = (n_ch ≥ 3 strings en paralelo por MPPT)
```

### Motor 3 — Potencia Real y Derating

```
# Coeficiente normalizado (blindaje de signo)
coeff_P = -(Math.abs(bpmpp)) / 100               # siempre negativo

# Potencia real por módulo en caliente (COMÚN para todos los inversores)
P_real_mod = pmax × (1 + coeff_P × delta_t_hot)  # W/módulo

# Por cada inversor i
Pdc_stc[i]  = inv[i].n_mod × pmax / 1000         # kWp (potencia nominal STC)
P_real[i]   = P_real_mod × inv[i].n_mod / 1000   # kW (potencia real en caliente)
ratio_STC[i]  = Pdc_stc[i] / inv[i].pac          # ratio DC/AC nominal
ratio_real[i] = P_real[i]  / inv[i].pac          # ratio DC/AC real (verano extremo)
clipping[i]   = ratio_real[i] >= 1,0             # inversor limitará si es true

# Rango óptimo ratio DC/AC: 1,10 – 1,40

# Resumen global de planta
Pdc_total    = Σ Pdc_stc[i]
Pac_total    = Σ inv[i].pac
P_real_total = Σ P_real[i]
ratio_global = Pdc_total / Pac_total
E_baja       = Pdc_total × 1600 / 1000   # MWh/año (factor bajo)
E_alta       = Pdc_total × 1850 / 1000   # MWh/año (factor alto)
```

### Motor 4 — Caída de Tensión en Tramos CC

```
# Constantes
ρ       = 0,024 Ω·mm²/m         # resistividad Cu exterior, conservador
CDT_MAX = 1,5 %                  # caída máxima admisible total

# Corriente de diseño (misma que Motor 2)
I_diseño = 1,25 × Isc_max

# Por cada tramo t de cada string
# Si S[t] = 0: tramo inexistente → cdt_V = 0, cdt_% = 0 (excluido del total)
cdt_V[t]   = 2 × L[t] × I_diseño × ρ / S[t]           # [V] — circuito DC 2 conductores
cdt_%[t]   = (cdt_V[t] / V_max_string) × 100           # [%]

# Total de la línea CC (suma de tramos activos S > 0)
CDT_total_V   = Σ cdt_V[t]   (t con S[t] > 0)
CDT_total_%   = Σ cdt_%[t]   →  debe ser ≤ 1,5 %

# Sección mínima (distribución proporcional por longitud entre tramos activos)
L_total_activa       = Σ L[t]   (t con S[t] > 0)
CDT_disponible_t     = V_max_string × 0,015 × (L[t] / L_total_activa)
S_min[t]             = 2 × L[t] × I_diseño × ρ / CDT_disponible_t

# Secciones normalizadas PV1-F [mm²]:
# 0(no existe) / 1,5 / 2,5 / 4 / 6 / 10 / 16 / 25 / 35 / 50 / 70 / 95 / 120
# → Se toma la sección normalizada inmediatamente superior a S_min

# Nota sobre V_ref:
# Se usa V_max_string (frío) como referencia para el %CDT.
# Esto da el % más bajo posible (criterio más favorable para el diseñador).
# Práctica habitual en proyectos FV en España.
```

---

## 10. NOMENCLATURA DE STRINGS

### Formato (v4.0)

```
INV[ii]-MPPT[m]-[cc]

  ii  = número de inversor, 2 dígitos con cero a la izquierda (01, 02, 03...)
  m   = número de MPPT, sin ceros (1, 2, 3...)
  cc  = número de canal (string), 2 dígitos con cero a la izquierda (01, 02, 03...)
```

### Ejemplos

```
INV01-MPPT1-01  →  Inversor 1, MPPT 1, Canal 1
INV01-MPPT1-02  →  Inversor 1, MPPT 1, Canal 2
INV01-MPPT2-01  →  Inversor 1, MPPT 2, Canal 1
INV02-MPPT1-01  →  Inversor 2, MPPT 1, Canal 1
INV02-MPPT3-02  →  Inversor 2, MPPT 3, Canal 2
```

### Identificador de línea CC

```
INV01-MPPT1-01 Tramo CC
```

### Nomenclatura histórica (v1.0 a v3.2)

```
INV.01.01.01   →  formato anterior con puntos
```

---

## 11. ARQUITECTURA CSS — PATRÓN STICKY

Este patrón es el resultado de múltiples iteraciones para resolver los problemas de scroll del sidebar. **Implementar exactamente así** para que funcione en todos los navegadores.

```css
/* Variables de layout */
:root {
  --hdr:   52px;   /* altura del header */
  --tabs:  38px;   /* altura de la barra de pestañas */
  --shell: 90px;   /* --hdr + --tabs */
}

/* PESTAÑAS: solo show/hide, sin restricciones de tamaño */
.tab-panel { display: none }
.tab-panel.active { display: block }
/* CRÍTICO: NO usar height, NO usar overflow aquí */

/* GRID CALCULADORA: align-items:start permite alturas naturales */
.calc-layout {
  display: grid;
  grid-template-columns: 440px 1fr;
  align-items: start;   /* CRÍTICO: sin esto las columnas se estiran */
}

/* SIDEBAR: sticky + scrolleable completo */
.sidebar {
  position: sticky;
  top: var(--shell);                    /* pegado bajo header + tabs */
  height: calc(100vh - var(--shell));   /* alto visible disponible */
  overflow-y: auto;                     /* EL SIDEBAR ES EL SCROLL */
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
}

/* BOTONES: sticky dentro del sidebar → siempre visibles */
.sb-fixed {
  position: sticky;
  top: 0;             /* se pega al tope del sidebar */
  z-index: 10;
  flex-shrink: 0;
  background: var(--bg-p);   /* fondo opaco para tapar el contenido */
}

/* INPUTS: flujo natural, sin restricciones */
.sb-scroll {
  padding: 11px 13px;
  display: flex;
  flex-direction: column;
  gap: 8px;
  /* SIN height, SIN overflow, SIN min-height:0 */
}

/* COLUMNA DERECHA: mismo patrón sticky */
.main {
  position: sticky;
  top: var(--shell);
  height: calc(100vh - var(--shell));
  overflow-y: auto;
}

/* PESTAÑAS CC Y TEORÍA: mismo patrón */
.cc-wrap, .teoria-wrap {
  position: sticky;
  top: var(--shell);
  height: calc(100vh - var(--shell));
  overflow-y: auto;
}

/* RESPONSIVE: desactivar sticky en móvil */
@media (max-width: 860px) {
  .calc-layout { grid-template-columns: 1fr }
  .sidebar, .main { position: static; height: auto }
}
```

---

## 12. ARQUITECTURA JAVASCRIPT

### Estado global (v4.0)

```javascript
let invDefs = [           // array de N inversores
  { nombre, vdc_max, pac, vmpp_min, vmpp_max,
    imax_str, imax_mppt, n_mppt, n_ch, n_mod }
]

let cfgArr = [            // [inversor][mppt][canal] = nº módulos
  [ [18, 18], [17, 17] ],   // inversor 0: 2 MPPTs × 2 canales
  [ [16, 16], [15, 15] ],   // inversor 1: configuración distinta
]

let ccData = {            // datos de caída de tensión por línea CC
  'INV01-MPPT1-01': { L: [10, 5, 50], S: [4, 4, 6] },
  'INV01-MPPT1-02': { L: [10, 5, 50], S: [4, 4, 6] },
  ...
}

let P = {                 // resultados calculados (generado por calculate())
  panel, env, Vred,
  T,                      // objeto del motor de tensiones
  CI,                     // objeto del motor de corrientes
  CP,                     // objeto del motor de potencia
  invResults: [           // array de resultados por inversor
    { inv, Nmax, Nmin, Nstr, Pdc_stc, P_real,
      ratio_STC, ratio_real, clipping, I_dc, I_ac, needF }
  ],
  Pdc_total, Pac_total, Preal_total, N_mod_total, ratio_global
}
```

### Funciones principales

```
initInvs()          → Inicializa el array de inversores con 1 inversor por defecto
onNInvChange(n)     → Ajusta el número de inversores
addInversor()       → Añade un inversor al final
delInversor(i)      → Elimina el inversor i
readInvFromDOM()    → Lee todos los parámetros del DOM antes de calcular
renderInvList()     → Regenera los cards de inversores en el sidebar

calculate()         → Motor principal: lee inputs, ejecuta motores, genera UI
buildUI()           → Construye el HTML de resultados (tempBar + plantBanner + invBlocks)
refreshStr(i,m,c)   → Actualiza las 4 pills y el alert de un string concreto
refreshMPPT(i)      → Actualiza el footer de un MPPT: Isc_max_MPPT, estado
refreshCounter(i)   → Actualiza el contador de módulos asignados del inversor i
onInp(i,m,c)        → Maneja el cambio de un input de módulos de un string

renderCC()          → Genera la pestaña de Secciones CC completa
refreshCCLine(id)   → Recalcula CDTs de una línea al cambiar L o S
onCClen(id,t,val)   → Maneja cambio de longitud de un tramo
onCCsec(id,t,val)   → Maneja cambio de sección de un tramo
applySmins(id,i,m,c) → Calcula y aplica secciones mínimas proporcionales

generateSummary()   → Genera el texto de memoria técnica
copySummary()       → Copia el texto al portapapeles
printPDF()          → Genera el HTML de impresión y llama a window.print()
resetAll()          → Restaura todos los valores a los ejemplos por defecto
```

---

## 13. CASO DE VERIFICACIÓN NUMÉRICA

Usar para validar que los cálculos son correctos después de cualquier modificación:

### Módulo y condiciones de sitio

```
Módulo:       Trina TSM-460 NEG9R.28
  voc_stc   = 52,1 V      vmpp_stc  = 43,5 V
  isc       = 11,20 A     impp      = 10,58 A     pmax = 460 W
  beta_voc  = −0,24 %/°C  alpha_isc = +0,04 %/°C  bpmpp = −0,30 %/°C

Emplazamiento:  Úbeda, Jaén (España)
  t_amb_min = −5 °C    t_amb_max = 40 °C
```

### Resultados comunes (Motor 1 + 2 + 3)

```
coeff_V = −0,0024 /°C
coeff_I = +0,0004 /°C
coeff_P = −0,0030 /°C

t_cell_cold = −5 °C    →  delta_t_cold = −30 °C
t_cell_hot  = 65 °C    →  delta_t_hot  = +40 °C

v_max_u = 52,1 × (1 + (−0,0024) × (−30)) = 52,1 × 1,072 = 55,851 V/módulo
v_min_u = 43,5 × (1 + (−0,0024) × (+40)) = 43,5 × 0,904 = 39,324 V/módulo

Isc_max   = 11,20 × (1 + 0,0004 × 40) = 11,20 × 1,016 = 11,379 A
Impp_real = 10,58 × (1 + 0,0004 × 40) = 10,58 × 1,016 = 10,749 A
I_diseño  = 1,25 × 11,379 = 14,224 A

P_real_mod = 460 × (1 + (−0,003) × 40) = 460 × 0,880 = 404,8 W/módulo
```

### INV01 — Huawei SUN2000-25KTL-M5

```
vdc_max=1100V, vmpp_min=200V, vmpp_max=1000V, pac=25kW, n_mod=70
n_mppt=2, n_ch=2

N_max = floor(1100 / 55,851) = 19 módulos
N_min = ceil (200  / 39,324) = 6  módulos

Configuración (70 mód, 4 strings):
  INV01-MPPT1-01: 18 mód → V_max=18×55,851=1005,3V (<1100V ✓) V_min=18×39,324=707,8V (en [200,1000]V ✓)
  INV01-MPPT1-02: 18 mód → ídem ✓
  INV01-MPPT2-01: 17 mód → V_max=17×55,851=949,5V ✓  V_min=17×39,324=668,5V ✓
  INV01-MPPT2-02: 17 mód → ídem ✓

Isc_max_MPPT = 2 × 11,379 = 22,758 A ≤ 26 A ✓
Pdc_stc  = 70 × 460 / 1000 = 32,20 kWp
P_real   = 404,8 × 70 / 1000 = 28,34 kW
ratio_STC  = 32,20 / 25 = 1,288  ✓ ÓPTIMO
ratio_real = 28,34 / 25 = 1,134  ⚠ CLIPPING
```

### Verificación CDT CC — Línea INV01-MPPT1-01 (18 módulos)

```
V_max_string = 1005,3 V   I_diseño = 14,224 A   ρ = 0,024 Ω·mm²/m

Tramo 1 — Cableado paneles:  L=10m, S=4mm²
  cdt_V = 2 × 10 × 14,224 × 0,024 / 4 = 1,707 V → 1,707/1005,3×100 = 0,170%

Tramo 2 — Paneles–Fusibles:  L=5m, S=4mm²
  cdt_V = 2 × 5 × 14,224 × 0,024 / 4  = 0,854 V → 0,085%

Tramo 3 — Fusibles–Inversor: L=50m, S=6mm²
  cdt_V = 2 × 50 × 14,224 × 0,024 / 6 = 5,690 V → 0,566%

TOTAL CDT = 0,170 + 0,085 + 0,566 = 0,821%  ✓ ≤ 1,5%
```

---

## 14. GUÍA DE USO DE LA APLICACIÓN

### Pestaña ⚡ Calculadora

**Paso 1 — Módulo FV:**
Introduce los parámetros del módulo fotovoltaico desde la ficha técnica del fabricante:
- Pmax, Voc, Vmpp, Isc, Impp en condiciones STC (25°C, 1000W/m²)
- Coeficientes de temperatura βVoc, αIsc, βPmpp (con signo según ficha: βVoc y βPmpp negativos, αIsc positivo)
- La app fuerza el signo correcto internamente si se introduce mal

**Paso 2 — Instalación y Ubicación:**
- T_amb_min: temperatura mínima histórica del emplazamiento (para V_max, criterio de seguridad)
- T_amb_max: temperatura máxima histórica del emplazamiento (para V_min, Isc_max, P_real)
- Fuente recomendada: AEMET, tablas de norma o datos históricos de la zona

**Paso 3 — Inversores:**
- Introduce el número de inversores del campo solar
- Para cada inversor: nombre/modelo, parámetros DC (Vdc_max, rango MPPT, Imax), potencia AC, número de MPPTs y canales, y módulos asignados
- Los parámetros DC se encuentran en la ficha técnica del inversor

**Paso 4 — Calcular:**
- Pulsa **⚡ CALCULAR**
- La app genera automáticamente la distribución óptima de módulos por string
- Verifica todas las condiciones y muestra semáforo de estado por string

**Paso 5 — Ajustar distribución:**
- Cada string tiene un input numérico con el valor calculado
- Se puede modificar manualmente dentro del rango [N_min, N_max]
- Si strings del mismo MPPT tienen diferente N → aviso de mismatch

**Paso 6 — Resumen:**
- Pulsa **📋 GENERAR** para obtener el texto de memoria técnica
- Pulsa **⧉ COPIAR** para copiar al portapapeles
- Pulsa **🖨 PDF** para imprimir o exportar como PDF

### Pestaña 📐 Secciones CC

- Las líneas CC se generan automáticamente desde la configuración de la pestaña Calculadora
- Para cada tramo: selecciona la sección del cable y introduce la longitud
- Si un tramo no existe en la instalación real: selecciona **0 (no existe)** en el selector de sección
- El botón **⚡ Aplicar S_mín** calcula y propone la sección mínima que cumple CDT ≤ 1,5%, distribuyendo el presupuesto de caída proporcionalmente a la longitud de cada tramo activo

### Pestaña 📋 Referencia Técnica

- Consulta estática de todas las fórmulas, constantes, normativa y leyenda de variables
- Disponible sin necesidad de calcular nada

### Botón 🖨 PDF

- Genera un informe imprimible completo con todos los resultados
- Requiere que se haya realizado un cálculo previo
- El PDF incluye: módulo FV, inversores, temperaturas, tensiones, corrientes, potencias, configuración de strings, protecciones, secciones CC y referencias normativas
- Para guardar como PDF: en el diálogo de impresión del navegador, seleccionar "Guardar como PDF" como destino

---

## 15. TO-DO Y MEJORAS FUTURAS

### Confirmadas como pendientes

| Prioridad | Mejora | Descripción |
|---|---|---|
| Alta | CDT tramo AC | Cálculo de caída de tensión en el tramo inversor → cuadro general AC |
| Alta | Modo "todos iguales" | Configurar N inversores idénticos en un solo paso |
| Alta | Copia de parámetros entre inversores | Botón para duplicar la configuración de un inversor en otro |
| Media | Exportación Excel | Tabla de strings y CDT exportable a .xlsx |
| Media | Integración PVGIS | Producción real por coordenadas GPS en lugar de factor kWh/kWp genérico |
| Media | Longitudes CC en modo copia | Aplicar las mismas longitudes a todos los strings con un clic |
| Baja | PDF personalizable | Cabecera con logo y datos de empresa |
| Baja | Multi-inversor modo rápido | Si todos los inversores son iguales, configurar una vez y replicar |
| Baja | Coeficiente βVmpp independiente | Para fichas técnicas que lo incluyen por separado |
| Baja | Pérdidas mismatch numéricas | Cálculo cuantitativo de pérdidas entre strings de distinta longitud |

---

## 16. REGISTRO DE INFORMES TÉCNICOS (ITR)

| ID Informe | Versión ITR | Fecha | Versión App | Estado |
|---|---|---|---|---|
| INF-20260406-01 | v1.0 | 2026-04-06 | CalcFV v1.0 | Aceptado |
| INF-20260406-01 | v1.1 | 2026-04-06 | CalcFV v1.1 | Aceptado |
| INF-20260406-01 | v1.1 rev2 | 2026-04-06 | CalcFV v1.1 rev2 | Aceptado |
| INF-20260407-02 | v1.0 | 2026-04-07 | CalcFV v2.0 | Aceptado |
| INF-20260408-03 | v1.0 | 2026-04-08 | CalcFV v3.0 | Aceptado |
| INF-20260408-04 | v2.0 | 2026-04-08 | CalcFV v3.1 | Aceptado |
| INF-20260409-05 | v1.0 | 2026-04-09 | CalcFV v3.2 | Aceptado |
| INF-20260409-06 | v1.0 | 2026-04-09 | CalcFV v4.0 | Aceptado |

---

## METADATOS DEL DOCUMENTO

| Campo | Valor |
|---|---|
| **Proyecto** | App.Cal_FV 26 |
| **Versión actual** | v4.0 |
| **Última actualización** | 2026-04-09 |
| **Modelo IA utilizado** | Claude Sonnet 4.6 (Anthropic) |
| **Protocolo de desarrollo** | Trazabilidad rígida 6 fases (ITR → Aceptación → Código → Snapshot → Paquete → Cierre) |
| **Ingeniero responsable** | Fali (Ingeniero MEP) |
| **Normativa principal** | REBT ITC-BT-40 · IEC 62548 · UNE-HD 60364-7-712 |

---

*Documento generado acumulativamente desde la sesión inicial del proyecto. Válido como entrega técnica a cliente y como punto de restauración para continuar el desarrollo en sesiones futuras de IA.*
