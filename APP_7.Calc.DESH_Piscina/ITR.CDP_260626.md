Como Ingeniero Senior, he revisado las fórmulas propuestas. Son un excelente paso hacia la rigurosidad térmica (el modelo VDI 2089 es el estándar de oro en Europa para piscinas). Sin embargo, atendiendo a tu petición de mejora, realizo las siguientes **observaciones críticas de ingeniería** que he incorporado al informe:

1. **Sobre el Balance de Aire Exterior ($V_{ae}$):** La fórmula que propones utiliza solo la evaporación ($m_e$). En la realidad, el caudal de aire exterior debe vencer **toda** la carga latente del recinto antes de la ventilación, es decir, la evaporación más la generada por las personas ($\dot{m}_e + \dot{m}_p$). Si no sumamos a los bañistas, el recinto acabará condensando con el local lleno.
2. **Condición de Imposibilidad Física:** La fórmula $V_{ae} = \frac{v_e \cdot m_{req}}{W_{ai} - W_{ae}}$ asume que el aire exterior es más seco que el interior ($W_{ai} > W_{ae}$). Si en verano el aire exterior es más húmedo, el denominador se vuelve negativo y el caudal carece de sentido físico (no se puede secar con aire húmedo). Añadiré un condicional lógico para advertir de esto.
3. **Volumen Específico ($v_e$):** Para mantener la coherencia con el Módulo 1 (donde usamos $\rho = 1.2 \text{ kg/m}^3$), asumiremos un volumen específico estándar de $v_e = \frac{1}{\rho} \approx 0.833 \text{ m}^3/\text{kg}$, o bien lo calcularemos dinámicamente con las propiedades psicrométricas.

A continuación, presento la versión actualizada del Informe Técnico de Requisitos (ITR v1.3) para tu validación.

---

# CALCULADORA AVANZADA MEP: DESHUMECTACIÓN Y POTENCIA TÉRMICA EN PISCINAS

**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.3

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS

* **Orden del Usuario:** 1. Modificar el cálculo de evaporación de la lámina de agua al método **VDI 2089** basado en la diferencia de humedades absolutas (g/kg) y coeficientes empíricos $\beta$.
2. Añadir un tercer módulo para el cálculo del **Caudal de Aire Exterior de Deshumectación** (Free-Drying), que determine los m³/h necesarios si la instalación depende 100% de aire exterior para secar el ambiente.
* **Normativa:** VDI 2089 (Building services in swimming baths), ASHRAE Fundamentals.
* **Parámetros Validados:**
* *Coeficiente de Uso VDI ($\beta$):* 0.5 (Reposo), 1.0 (Residencial), 2.0 (Hidromasaje), 4.0 (Atracciones/Olas). Sustituye al antiguo factor $F$.
* Resto de parámetros geométricos y termodinámicos ($A, h_{vaso}, T_w, T_{int}, HR_{int}$, etc.) se mantienen de la v1.2.


* **Hipótesis y simplificaciones adoptadas:**
* El aire exterior utilizado para el cálculo de $V_{ae}$ entra al recinto sin tratamiento previo (Free-Cooling/Drying latente).
* La densidad del aire se mantiene en $1.2 \text{ kg/m}^3$ para un volumen específico $v_e = 0.833 \text{ m}^3/\text{kg}$.



### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

* **Check de comprensión:** Comprendida la transición de diferencias de presión de vapor ($\Delta P$) a diferencias de humedad absoluta ($\Delta x$) en g/kg para el cálculo latente de la lámina, y la integración de la ventilación como método de resolución mecánica.
* **Desarrollo completo de los cálculos:**
1. **Psicrometría (Magnus-Tetens):**
* $P_{sat}(T) = 0.6112 \cdot e^{\frac{17.67 \cdot T}{T + 243.5}} \text{ [kPa]}$
* $P_v = \frac{HR}{100} \cdot P_{sat} \text{ [kPa]}$
* $x = 622 \cdot \frac{P_v}{101.325 - P_v} \text{ [g/kg]}$ (Nota: Dividir por 1000 para pasar a kg/kg en cálculos de masas).


2. **Módulo A: Deshumectación (VDI 2089):**
* $x_{sup}$: Humedad absoluta evaluada a $T_w$ y $HR = 100\%$.
* $x_{int}$: Humedad absoluta evaluada a $T_{int}$ y $HR_{int}$.
* $\dot{m}_e = \beta \cdot A \cdot (x_{sup} - x_{int}) \cdot \frac{1}{1000} \text{ [kg/h]}$
* Carga Base a secar ($m_{req}$) = $\dot{m}_e + \dot{m}_p$


3. **Módulo B: Caudal de Aire Exterior (Nuevo):**
* $V_{ae} = \frac{v_e \cdot (\dot{m}_e + \dot{m}_p)}{W_{ai} - W_{ae}} \text{ [m}^3\text{/h]}$
* *Condición MEP:* Si $W_{ae} \ge W_{ai}$, el aire exterior no puede secar. $V_{ae} = \text{No Viable (Requisito de Máquina Frigorífica)}$.





### 3. HISTORIAL DE REQUERIMIENTOS ACUMULADOS

* [Implementado v1.0] Interfaz base con 3 pestañas y cálculo psicrométrico.
* [Implementado v1.1] Módulo térmico del vaso (arranque y mantenimiento).
* [Implementado v1.2] Volumen de vaso automático y modelo vectorial de ventilación.
* **[Pendiente v1.3]** Reemplazar motor de evaporación por VDI 2089 ($\beta$ y $\Delta x$).
* **[Pendiente v1.3]** Añadir cálculo analítico de $V_{ae}$ (Free-Drying) informando al usuario sobre la viabilidad de usar solo ventilación exterior.

### 4. ESPECIFICACIONES DE LA VERSIÓN ACTUAL (v1.3)

* **Descripción:** Actualización del núcleo algorítmico. Se modifica el selector de "Factor de Actividad" para albergar los valores $\beta$ de la VDI 2089. Se añade un nuevo panel de resultados para mostrar el "Caudal Volumétrico de Secado ($V_{ae}$)".
* **Lógica de negocio:** La UI mostrará el caudal necesario en m³/h. Si las condiciones exteriores ($T_{ext}, HR_{ext}$) resultan en un aire más húmedo que el interior, el panel mostrará un aviso de "Secado exterior imposible" bloqueando resultados negativos irreales.
* **Estrategia de manejo de errores:** Condicional de división por cero o divisiones negativas en la ecuación de $V_{ae}$ si $W_{ai} \le W_{ae}$.

### 5. STACK TECNOLÓGICO

* JavaScript ES6 (Mantenido). MathJax para la nueva fórmula VDI.

### 6. BITÁCORA DE CAMBIOS (Changelog)

* **v1.0 - 1.2:** [Implementados previamente] Balance base, módulo térmico, automatización geométrica.
* **v1.3:** Migración al estándar VDI 2089 para evaporación y adición de módulo de dimensionado de ventilador para control de humedad por Free-Drying.

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA

* **Marco Normativo:** VDI 2089 Part 1 (Heating, ventilation and air-conditioning in indoor swimming pools).
* **Fórmulas:** Ecuación de evaporación VDI validada internacionalmente, superior en precisión a estimaciones estáticas antiguas.

### 8. LIBRERÍAS TÉCNICAS

* Nativo (cero dependencias). MathJax vía CDN.

### 9. IMPLEMENTACIÓN

*(Pendiente de aprobación)*

---



