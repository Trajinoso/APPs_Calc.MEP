# ENTORNO DE INSTALACIONES MEP: CALCULADORA DE DESHUMECTACIÓN Y TÉRMICA DE PISCINAS
**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.4  
**ESTADO:** ACEPTADO E IMPLEMENTADO

Este repositorio contiene la herramienta profesional autocontenida para el dimensionado mecánico y de fluidos en piscinas cubiertas climatizadas (natatorios). El aplicativo garantiza una trazabilidad completa desde los requisitos técnicos hasta las ecuaciones aplicadas de balance de masa y energía térmica.

## HISTORIAL ACUMULADO DE INFORMES TÉCNICOS (ITR)

### Versión 1.0 - Balance de Masa Básico
* **Alcance:** Modelado analítico de la carga latente de deshumectación considerando evaporación libre superficial (ASHRAE), ocupación y caudal de renovación exterior.

### Versión 1.1 - Balance de Energía Térmica
* **Alcance:** Adición del módulo de transferencia de calor para el agua del vaso (potencia sensible de arranque inicial y potencia latente de mantenimiento).

### Versión 1.2 - Optimización Geométrica y Psicrometría Vectorial
* **Alcance:** Cálculo automático de volumen por profundidad y corrección algebraica del aire exterior para evaluar su vector de secado dinámico mediante motor psicrométrico interno (Magnus-Tetens).

### Versión 1.3 - Estándar VDI 2089
* **Alcance:** Migración del bloque evaporativo de la lámina de agua al estándar alemán VDI 2089, empleando diferencias de humedades absolutas (g/kg) y el coeficiente de agitación empírico $\beta$.

### Versión 1.4 - Prontuario Técnico Integrado (Versión Actual)
* **Alcance UI/UX:** Transformación de la pestaña de "Auditoría ITR" en un **Prontuario Técnico** elegante y didáctico. Se ha desplegado el formulario matemático completo (renderizado en LaTeX) y la justificación de las consideraciones termodinámicas adoptadas por la herramienta. 
* **Lógica:** Cancelación de la vía de desarrollo de sistemas All-Air (Todo-Aire Exterior) por decisión técnica de la dirección de ingeniería, focalizando el cálculo en el balance de carga neta para máquinas frigoríficas deshumectadoras.

---

## STACK TECNOLÓGICO
* **Frontend:** HTML5 Semántico y CSS3 con optimización Report-Ready (A4 Print).
* **Lógica:** JavaScript ES6 Nativo (Vanilla JS). Motor termodinámico propio sin dependencias.
* **Renderizado Matemático:** MathJax v3 para justificación formal en LaTeX.
