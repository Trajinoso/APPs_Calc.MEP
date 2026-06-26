# CALCULADORA AVANZADA MEP: DESHUMECTACIÓN Y POTENCIA TÉRMICA EN PISCINAS

**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.4

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS

* **Orden del Usuario:** 1. Descartar la implementación del módulo de "Aire exterior para deshumectación" (Free-Drying).
2. Rediseñar por completo la tercera pestaña de la aplicación: sustituir la exportación cruda del ITR en formato texto por una interfaz estructurada ("Prontuario Técnico") que contenga la descripción detallada, el formulario matemático (MathJax) y las consideraciones técnicas tomadas para todos los cálculos de la aplicación.
* **Normativa Base (Consolidada):** VDI 2089 (Piscinas), ASHRAE Fundamentals (Psicrometría), RITE (Bases de diseño de ventilación y cargas térmicas).

### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

La algoritmia de cálculo (Pestaña 1 y 2) se mantiene intacta respecto a la v1.3 (Evaporación VDI 2089 y balance de masas con vector de ventilación psicrométrico). El cambio principal radica en la arquitectura de la información de la **Pestaña 3**, que ahora se dividirá en las siguientes subsecciones teóricas:

* **Sección A: Termodinámica del Aire Húmedo:**
Explicación del uso de la ecuación de Magnus-Tetens para calcular presiones de saturación y humedades absolutas (g/kg), prescindiendo de ábacos y garantizando precisión computacional.
* **Sección B: Balance de Masa Latente (VDI 2089):**
Desarrollo de la fórmula $\dot{m}_e = \beta \cdot A \cdot (x_{sup} - x_{int}) \cdot \frac{1}{1000}$. Explicación de los coeficientes de agitación $\beta$ y de cómo interactúa la humedad absoluta exterior ($x_{ext}$) extraída por el caudal de la máquina ($Q_v$).
* **Sección C: Balance de Energía del Vaso:**
Justificación del cálculo de potencia de choque (sensible) basada en volumen, gradiente térmico y tiempo de concesión ($t_{cal}$); y potencia de mantenimiento (latente) ligada a los $0.68 \text{ kWh/kg}$ de agua evaporada.

### 3. HISTORIAL DE REQUERIMIENTOS ACUMULADOS

* [Implementado v1.0] Interfaz base, cálculo psicrométrico automático.
* [Implementado v1.1] Módulo térmico del vaso (arranque y mantenimiento).
* [Implementado v1.2] Volumen de vaso automático y modelo vectorial de ventilación para deshumectadoras.
* [Implementado v1.3] Migración a normativa alemana VDI 2089 para evaporación ($\beta$ y $\Delta x$).
* **[Descartado v1.4]** Implementación de módulo Free-Drying (Todo-Aire Exterior) cancelada por orden directa.
* **[Pendiente v1.4]** Rediseño UI/UX de la pestaña 3 para convertirla en un Prontuario Técnico visual con renderizado MathJax y justificación de hipótesis de diseño.

### 4. ESPECIFICACIONES DE LA VERSIÓN ACTUAL (v1.4)

* **Descripción:** Limpieza de requerimientos cancelados y foco absoluto en la calidad documental de la herramienta. La pestaña 3 abandonará el campo `<textarea>` para adoptar un formato de reporte técnico de alto nivel.
* **Lógica de negocio:** Sin alteraciones en el cálculo de resultados de las pestañas 1 y 2.
* **Estrategia de visualización:** Uso de cajas de texto (normativa-box o similares) para separar visualmente los fundamentos psicrométricos de los fundamentos térmicos.

### 5. STACK TECNOLÓGICO

* HTML5 semántico, CSS3, Vanilla JS.
* MathJax (Uso intensivo en la nueva Pestaña 3 para renderizado de ecuaciones analíticas).

### 6. BITÁCORA DE CAMBIOS (Changelog)

* **v1.0 - 1.2:** Balance masa/energía, volumen automático, motor termodinámico interno.
* **v1.3:** Migración al motor evaporativo de la VDI 2089.
* **v1.4:** Reestructuración profunda de la documentación embebida (Pestaña 3) para presentar un Prontuario Matemático en lugar del log del ITR.

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA

* **Hipótesis principales reflejadas en Pestaña 3:** * Densidad del aire $\rho = 1.2 \text{ kg/m}^3$.
* Calor Latente de vaporización $h_{fg} = 0.68 \text{ kWh/kg}$.
* Calor Específico del agua $c_p = 1.16 \text{ Wh/(kg·K)}$.



### 8. LIBRERÍAS TÉCNICAS

* Nativo (cero dependencias externas). MathJax v3 (CDN).

### 9. IMPLEMENTACIÓN

*(Pendiente de aprobación)*

