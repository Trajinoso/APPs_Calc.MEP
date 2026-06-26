# ENTORNO DE INSTALACIONES MEP: CALCULADORA DE DESHUMECTACIÓN Y TÉRMICA DE PISCINAS
**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.3  
**ESTADO:** ACEPTADO E IMPLEMENTADO

Este repositorio contiene la herramienta profesional autocontenida para el dimensionado mecánico y de fluidos en piscinas cubiertas climatizadas (natatorios). El aplicativo garantiza una trazabilidad completa desde los requisitos técnicos hasta las ecuaciones aplicadas de balance de masa y energía térmica.

## HISTORIAL ACUMULADO DE INFORMES TÉCNICOS (ITR)

### Versión 1.0 - Balance de Masa Básico
* **Alcance:** Modelado analítico de la carga latente de deshumectación considerando evaporación libre superficial, ocupación y caudal de renovación exterior. Metodología ASHRAE.

### Versión 1.1 - Balance de Energía Térmica
* **Alcance:** Adición del módulo de transferencia de calor para el agua del vaso. Diferenciación entre la potencia de arranque inicial sensible en función del tiempo y la potencia de mantenimiento continuo (pérdida latente + transmisión).

### Versión 1.2 - Optimización Geométrica y Psicrometría Vectorial
* **Alcance:** Cálculo automático de volumen ($V = A \cdot h_{vaso}$) y corrección algebraica del aire exterior para evaluar su vector de secado (resta carga si es más seco que el interior, suma si es más húmedo). Integración de motor psicrométrico analítico (Magnus-Tetens).

### Versión 1.3 - Estándar VDI 2089 y Free-Drying (Versión Actual)
* **Cambio Evaporativo:** Implementación de la norma alemana VDI 2089. La evaporación se calcula en base al salto de humedad absoluta en g/kg y coeficientes de agitación $\beta$ (0.5 a 4.0):
  $\dot{m}_e = \beta \cdot A \cdot (x_{sup} - x_{int}) \cdot \frac{1}{1000}$
* **Módulo Free-Drying:** Adición del cálculo de caudal de aire exterior necesario si la estrategia de deshumectación es 100% aire exterior (sin ciclo frigorífico cerrado):
  $V_{ae} = \frac{v_e \cdot m_{req}}{W_{ai} - W_{ae}}$
  *(Con evaluación lógica de viabilidad física si el aire exterior es demasiado húmedo).*

---

## BITÁCORA DE CAMBIOS (CHANGELOG)
* **v1.0 (26/06/2026):** Definición de variables base y factores de actividad de ASHRAE.
* **v1.1 (26/06/2026):** Interconexión evaporación-pérdida térmica latente y balance de placas térmicas.
* **v1.2 (26/06/2026):** Refactorización del balance psicrométrico y automatización de volumen del vaso.
* **v1.3 (26/06/2026):** Migración del cálculo de evaporación a VDI 2089 e inclusión del caudal volumétrico $V_{ae}$ para sistemas All-Air (Todo Aire).

---

## STACK TECNOLÓGICO
* **Frontend:** HTML5 Semántico y CSS3 con optimización Report-Ready (A4 Print).
* **Lógica:** JavaScript ES6 Nativo (Vanilla JS). Motor termodinámico propio.
* **Renderizado Matemático:** MathJax v3 para justificación formal en LaTeX.