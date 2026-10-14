# App.Cal_FV 26 — Configurador Fotovoltaico MEP
**ID_INFORME VIGENTE:** INF-20260410-08 | **VERSIÓN:** 5.0
**ESTADO:** ACEPTADO Y CODIFICADO

Este documento es la única fuente de verdad (Single Source of Truth) acumulativa del proyecto. Contiene la trazabilidad completa desde la versión 1.0 hasta la versión 5.0 actual.

## 1. ESPECIFICACIONES TÉCNICAS (ESTADO ACTUAL)
Herramienta MEP autocontenida (HTML/JS/CSS puro) para el dimensionado termo-eléctrico de campos solares fotovoltaicos multi-inversor. 
- **Funciones Principales:** - Cálculo de strings (Vmax en frío, Vmin en calor).
  - Verificación de corrientes (Isc_max, I_diseño) y protecciones (Fusibles, Magnetotérmicos).
  - Cálculo de caída de tensión (CDT) por tramos de cableado CC.
  - Análisis avanzado de Clipping térmico basado en Ratio Real DC/AC.
  - Generación de memoria técnica en texto y exportación a PDF.
  - Persistencia de datos de sesión mediante `localStorage`.
- **Normativa Integrada:** REBT ITC-BT-40, REBT ITC-BT-23, IEC 62548, UNE-HD 60364-7-712, IEC 60364-5-52.
- **Unidades:** Sistema Internacional (SI).

## 2. LÓGICA DE INGENIERÍA (CÁLCULOS CRÍTICOS)
### 2.1 Temperaturas y Tensiones
- `T_cell_cold` = `T_amb_min`
- `T_cell_hot` = `T_amb_max` + 25°C
- `V_max_string` = `Voc_STC` × [1 + (βVoc/100) × (`T_cell_cold` - 25)] × N
- `V_min_string` = `Vmpp_STC` × [1 + (βVoc/100) × (`T_cell_hot` - 25)] × N

### 2.2 Derating Térmico y Clipping (Motor v5.0)
- `dT_hot` = `T_cell_hot` - 25
- `P_real_mod` = `Pmax` × [1 - (|βPmpp|/100) × `dT_hot`]
- `P_DC_Treal[i]` = (`P_real_mod` × `n_mod[i]` / 1000) × `f_loss_dc`
- `P_clip_T[i]` = max(0, `P_DC_Treal[i]` - `inv[i].pac`)
- **Indicador Director (Ratio Real):** `Ratio_Real` = `P_DC_Treal[i]` / `inv[i].pac`

### 2.3 Semáforo MEP (Umbrales de Ratio Real DC/AC)
- **≤ 1.00:** 🟢 Sin clipping (Diseño conservador).
- **1.00 - 1.05:** 🟢 Excelente (Pérdidas mínimas).
- **1.05 - 1.10:** ✅ Óptimo (Equilibrio de ingeniería).
- **1.10 - 1.15:** ✅ Aceptable (Justificado por coste de panel).
- **1.15 - 1.20:** ⚠️ Justificar (Pérdida económica notable).
- **1.20 - 1.25:** ⚠️ Alto (Solo para Orientación E/O).
- **≥ 1.25:** 🚨 Excesivo (Alarma CLP-01: Estrés térmico Inversor).

### 2.4 Autoguardado (Persistencia v5.0)
- **Clave:** `calcfv_v5_state`.
- Captura de inputs y arrays (`invDefs`, `cfgArr`, `ccData`) al disparar la función `calculate()`.

## 3. BITÁCORA DE CAMBIOS (CHANGELOG ACUMULATIVO)
- **v1.0 (INF-20260406-01):** Calculadora base STC, validación de rangos V_max y V_min.
- **v2.0 (INF-20260407-02):** Implementación de derating térmico (αIsc, Isc_max, P_real).
- **v3.0 - v3.2 (INF-20260408-xx):** Cálculos de sección de cables CC, CDT y layout sticky.
- **v4.0 (INF-20260409-06):** Arquitectura multi-inversor, banner global y arrays dinámicos.
- **v4.1 (INF-20260409-07):** Módulo de clipping inicial basado en condiciones STC y `f_loss_DC`.
- **v5.0 (INF-20260410-08) [ACTUAL]:** Motor de clipping corregido. Utiliza `P_real_mod` × `f_loss_DC` (derating térmico real). El indicador principal pasa a ser el Ratio Real DC/AC con escala MEP de 7 tramos. El %Clipping queda relegado a dato informativo (8 métricas en UI). Integración de autoguardado vía `localStorage`. Se mantuvo intacto el motor de cálculo CC de v4.1.