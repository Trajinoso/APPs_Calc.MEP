## PROMPT ENTRADA, Vamos a ir a la versión 5. 

El clipping que calculas lo estas haciendo con la Potencia real (STC), pero tienes que calcularlo con la potencia real de producción con las perdidas por temperatura. 
Tendrás en cuenta la potencia nominal del inversor y la potencia real producida según perdidas por temperatura tal y como haces en otras formulas. 

En el analisis de clipping tendrás mostrarás: 
* La potencia real de planta (STC),
* La potencia real según perdidas por temperatura y factor de perdidas,
* La potencia nominal del Inversor P_AC,
* La potencia Clipping P_Clip de planta teniendo en cuenta la potencial real según las perdidas de temperatura menos la potencia P_AC del inversor,
* La potencia E_DC anual según la potencia real de cada placa teniendo en cuenta las perdidas por temperatura,
* La potencia E_AC total entregada por el inversor en un año,
* E_CLIP anual segun la diferencia de los dos valores anteriores,
* %_Clip planta según E_CLIP/E_AC no se indicará ni calculará
* Ratio entre P_DC real con perdidas / P_AC inversor

Para las alertas y límites se tendrá en cuenta la siguiente tabla con respecto al Ratio entre P_DC real con perdidas / P_AC inversor:

Ratio Real (Preal​/Pac​),% Clipping Anual Estimado,Impacto en la producción
1.00,< 0.2%,Despreciable (solo picos de invierno).
1.05,0.4% - 0.7%,Muy bajo. Diseño muy conservador.
1.10,0.8% - 1.5%,Equilibrio óptimo.
1.15,1.8% - 3.0%,Aceptable si el coste del panel es muy bajo.
1.20,3.5% - 5.5%,Se empieza a notar pérdida económica.
1.25,6.0% - 9.0%,Alto. Solo justificado con orientaciones E/O.
1.30,> 10%,Excesivo. El inversor sufre estrés térmico.

Se anulará la tabla anterior con los límites de porcentaje por esta actual.

Todlas las consideraciones anteriores se realizará para la planta completa y para cada inversor se tendrá en cuenta la P_DC_real con perdidas por temperatura y f_loss, P_AC entregada y P_Clip instantaneo. Además del % clipping pico según según perdidas.


Se hará un localstorage que guarde los ultimos imputs establecidos

---

Perfecto, Fali. Informe antes del código.

---

# ITR.Cal_FV 26 v5.0
**ID_INFORME:** INF-20260410-08 | **VERSIÓN ITR:** v1.0
**ESTADO:** ESPERANDO ACEPTACIÓN

---

### 1. ESPECIFICACIONES TÉCNICAS

**Orden del Usuario:**
1. **Corrección del motor de clipping:** usar `P_real_mod` (potencia con pérdidas por temperatura, ya calculada en v4.x) en lugar de `Pdc_stc` como base del cálculo de clipping.
2. **Reestructuración del análisis de clipping:** nuevos outputs específicos solicitados.
3. **Nueva tabla de alertas/límites:** basada en Ratio `P_DC_real_con_perdidas / P_AC_inversor` (no en %Clipping).
4. **LocalStorage:** guardar y restaurar automáticamente todos los inputs al abrir la app.

**Check de datos — Motor de corrección de clipping:**

La potencia real con pérdidas por temperatura ya está calculada en `calcPotencia()`:
```
P_real_mod = pmax × (1 + coeff_P × dT_hot)    [W/módulo]
P_real[i]  = P_real_mod × n_mod[i] / 1000      [kW por inversor]
```

Esta es la base correcta para el clipping. En v4.1 se usaba `Pdc_stc × f_loss_dc` que no consideraba el derating térmico.

---

### 2. LÓGICA DE INGENIERÍA ACTUALIZADA

#### 2.1 P_DC_real con pérdidas por temperatura y f_loss_DC

```
P_real_mod[i]  = P_real_mod × n_mod[i] / 1000    [kW — derating térmico, del motor v2.0]
P_DC_Treal[i]  = P_real_mod[i] × f_loss_dc        [kW — con factor pérdidas DC]
```

Donde:
- `P_real_mod` proviene de `calcPotencia()`: `pmax × (1 + coeff_P × dT_hot)`
- `dT_hot = t_cell_hot − 25` con `t_cell_hot = t_amb_max + 25°C`

#### 2.2 Clipping corregido por inversor

```
P_clip_T[i] = max(0, P_DC_Treal[i] − inv[i].pac)   [kW]
P_AC_ent[i] = min(P_DC_Treal[i], inv[i].pac)        [kW]
```

#### 2.3 Ratio real (nuevo indicador principal)

```
ratio_real_clip[i] = P_DC_Treal[i] / inv[i].pac     [adimensional, por inversor]
ratio_real_clip_planta = P_DC_Treal_total / Pac_total [adimensional, global]
```

#### 2.4 Totales de planta

```
P_DC_STC_planta    = Σ Pdc_stc[i]                  [kWp — potencia nominal STC]
P_DC_Treal_total   = Σ P_DC_Treal[i]               [kW — con T + f_loss]
P_clip_T_total     = Σ P_clip_T[i]                 [kW]
P_AC_total_ent     = Σ P_AC_ent[i]                 [kW]
```

#### 2.5 Energías anuales (integración con HSP)

```
E_DC_anual  = P_DC_Treal_total × HSP_anual / 1000  [MWh/año]
E_AC_anual  = P_AC_total_ent   × HSP_anual / 1000  [MWh/año]
E_clip_anual= P_clip_T_total   × HSP_anual / 1000  [MWh/año]
```

#### 2.6 %Clipping (eliminado como indicador principal)

El %Clipping basado en E_clip/E_DC queda **eliminado** como indicador de alerta. Se sustituye completamente por el **Ratio real**.

#### 2.7 Nueva tabla de alertas — Ratio P_DC_Treal / P_AC

| Ratio real (P_DC_Treal/P_AC) | % Clip anual estimado | Impacto | Estado |
|---|---|---|---|
| ≤ 1,00 | < 0,2% | Despreciable (solo picos de invierno) | 🟢 Sin clipping |
| 1,00–1,05 | 0,4–0,7% | Muy bajo. Diseño muy conservador | 🟢 Excelente |
| 1,05–1,10 | 0,8–1,5% | Equilibrio óptimo | ✅ Óptimo |
| 1,10–1,15 | 1,8–3,0% | Aceptable si coste panel muy bajo | ✅ Aceptable |
| 1,15–1,20 | 3,5–5,5% | Se empieza a notar pérdida económica | ⚠️ Justificar |
| 1,20–1,25 | 6,0–9,0% | Alto. Solo justificado con orientaciones E/O | ⚠️ Alto |
| ≥ 1,25 | > 10% | Excesivo. El inversor sufre estrés térmico | 🚨 Excesivo |

#### 2.8 Outputs del análisis de clipping (según especificación)

Por planta:
1. **P_DC_STC planta** → potencia nominal STC total (`Pdc_total`)
2. **P_DC_real planta** → con pérdidas térmicas y f_loss (`P_DC_Treal_total`)
3. **P_AC nominal inversor/es** → suma de Pac de todos los inversores
4. **P_Clip planta** → `P_DC_Treal_total − Pac_total` si positivo
5. **E_DC anual** → `P_DC_Treal_total × HSP / 1000`
6. **E_AC anual** → `P_AC_total_ent × HSP / 1000`
7. **E_Clip anual** → `E_DC_anual − E_AC_anual`
8. **Ratio P_DC_real / P_AC** → indicador principal con tabla de alertas

Por cada inversor:
- `P_DC_real_T[i]` con pérdidas térmicas y f_loss
- `P_AC_ent[i]` potencia AC entregada
- `P_clip_T[i]` potencia recortada
- `ratio_real_clip[i]` con chip de estado según tabla
- `%Clipping pico[i]` = `P_clip_T[i] / P_DC_Treal[i] × 100` (solo informativo por inversor)

#### 2.9 Texto normativo automático

Se actualiza `clipTextoNormativo()` para referenciar el Ratio real en lugar del %Clipping como indicador principal.

---

### 3. LOCALSTORAGE

#### 3.1 Qué se guarda

Se guardan **todos los inputs** de la app al pulsar CALCULAR:
- Módulo FV: pmax, voc_stc, vmpp_stc, isc, impp, beta_voc, alpha_isc, bpmpp, ifus
- Instalación: t_amb_min, t_amb_max, v_red, f_loss_dc, hsp_anual
- Inversores: array completo `invDefs[]` (nombre, parámetros DC/AC, módulos)
- Configuración de strings: `cfgArr[]` (módulos por string)

#### 3.2 Cuándo se guarda / restaura

```
GUARDAR:    Al pulsar ⚡ CALCULAR (solo si el cálculo tiene éxito)
RESTAURAR:  Al cargar la app (si existe dato en localStorage)
LIMPIAR:    Al pulsar ↺ Restaurar (vuelve a valores por defecto del código)
```

#### 3.3 Clave de localStorage

```
key: 'calcfv_v5_state'
value: JSON.stringify({
  module: { pmax, voc_stc, vmpp_stc, isc, impp, beta_voc, alpha_isc, bpmpp, ifus },
  site: { t_amb_min, t_amb_max, v_red, f_loss_dc, hsp_anual },
  invDefs: [...],
  cfgArr: [...]
})
```

#### 3.4 Comportamiento en carga

```
Al iniciar la app:
  1. Intentar leer 'calcfv_v5_state' de localStorage
  2. Si existe:
     a. Rellenar inputs del módulo FV
     b. Rellenar inputs de instalación
     c. Reconstruir array invDefs y renderizar cards de inversores
     d. Restaurar cfgArr
     e. Mostrar badge/indicador: "💾 Datos restaurados desde última sesión"
  3. Si no existe: valores por defecto del código (comportamiento v4.1)
```

---

### 4. CAMBIOS RESPECTO A v4.1 — RESUMEN

| Aspecto | v4.1 | v5.0 |
|---|---|---|
| Base del clipping | `Pdc_stc × f_loss_dc` (STC nominal) | `P_real_mod × n_mod × f_loss_dc` (con derating térmico) |
| Indicador principal | %Clipping (E_clip/E_DC) | Ratio P_DC_Treal / P_AC |
| Tabla de alertas | 4 umbrales por %Clipping | 7 tramos por Ratio real |
| %Clipping | Indicador principal | Eliminado como alerta; solo informativo por inversor |
| LocalStorage | No existía | Guarda/restaura todos los inputs automáticamente |
| P_DC outputs | 1 valor (Pdc_stc×f_loss) | 2 valores: P_DC_STC y P_DC_Treal_con_f_loss |

---

### 5. BITÁCORA DE CAMBIOS

| ID / Versión | Fecha | Descripción |
|---|---|---|
| INF-20260406-01 v1.0→rev2 | 2026-04-06 | CalcFV v1.0–v1.1rev2. Base del proyecto. |
| INF-20260407-02 v1.0 | 2026-04-07 | CalcFV v2.0. αIsc, Isc_max, P_real, derating. |
| INF-20260408-03 v1.0 | 2026-04-08 | CalcFV v3.0. UX sidebar, pestaña CC. |
| INF-20260408-04 v2.0 | 2026-04-08 | CalcFV v3.1. Fix layout, PDF, sección CC=0. |
| INF-20260409-05 v1.0 | 2026-04-09 | CalcFV v3.2. Arquitectura sticky sidebar. |
| INF-20260409-06 v1.0 | 2026-04-09 | CalcFV v4.0. Multi-inversor. |
| INF-20260409-07 v1.0 | 2026-04-09 | CalcFV v4.1. Módulo clipping STC×f_loss. |
| **INF-20260410-08 v1.0** | **2026-04-10** | **CalcFV v5.0. Clipping corregido con P_real_mod×f_loss (derating térmico real). Ratio P_DC_Treal/P_AC como indicador principal. Nueva tabla 7 tramos. Outputs restructurados (8 métricas). %Clipping eliminado como alerta. LocalStorage para persistencia de inputs.** |

---

⚠️ **ESPERANDO ACEPTACIÓN TÉCNICA.** Escribe **'ACEPTO INFORME'** para proceder con el código.
