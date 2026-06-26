Primero, la simplificación geométrica del vaso es un acierto para facilitar la entrada de datos al usuario. Segundo, respecto a los datos psicrométricos: **no te preocupes, ya implementé en la versión anterior un motor de cálculo termodinámico**. La aplicación calcula internamente y de forma exacta la humedad absoluta ($x_i$, $x_e$) a partir de las temperaturas y humedades relativas que introduce el usuario usando la ecuación de Magnus-Tetens. No hace falta meter tablas manuales.

Respecto a la fórmula de ventilación $\dot{m}_v = Q_v \cdot \rho_{aire} \cdot (x_i - x_e)$: este enfoque calcula la **capacidad de secado** del aire exterior. Si el aire interior tiene más agua ($x_i$) que el exterior ($x_e$), ventilar *resta* trabajo a la máquina deshumectadora. Lo ajustaremos en el balance de masas para que funcione exactamente así.


# CALCULADORA AVANZADA MEP: DESHUMECTACIÓN Y POTENCIA TÉRMICA EN PISCINAS

**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.2

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS 

* **Orden del Usuario:** Modificar la aplicación SPA de cálculo integral para recintos de piscinas. Se elimina el input directo de volumen del vaso, calculándolo geométricamente a partir de la profundidad. Se ajusta la ecuación de ventilación para evaluar correctamente el potencial de secado del aire exterior y se confirma la automatización del cálculo psicrométrico.
* **Normativa:** RITE, ASHRAE Handbook (Natatoriums), VDI 2089.
* **Parámetros Validados (Actualizados):**
* *Superficie de agua ($A$):* [m²] (Validación: > 0)
* **[NUEVO]** *Profundidad media del vaso ($h_{vaso}$):* [m] (Validación: > 0) -> El Volumen ($V$) pasará a ser un campo calculado ($A \cdot h_{vaso}$).
* *Temperatura del agua ($T_w$):* [ºC]
* *Temperatura inicial/red ($T_{red}$):* [ºC]
* *Tiempo de calentamiento inicial ($t_{cal}$):* [h]
* *Temperatura del aire interior ($T_{int}$) y Exterior ($T_{ext}$):* [ºC]
* *Humedad Relativa interior ($HR_{int}$) y Exterior ($HR_{ext}$):* [%]
* *Caudal de ventilación exterior ($Q_v$):* [m³/h]
* *Ocupantes ($N$) y Factor de Actividad ($F$).*



### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

Se mantiene la base termodinámica de la versión 1.1 y se modifica el balance de masa latente.

* **Check de datos aportados:** Comprendida la modificación geométrica. Confirmada la capacidad de la herramienta para autocalcular las propiedades del aire (Humedad Absoluta y Presión de Saturación) sin depender de ábacos externos.
* **Módulo 1: Carga Latente y Deshumectación (Modificado)**
1. **Cálculo Psicrométrico Automático:** Se usa Magnus-Tetens para calcular $P_{sat}$ y deducir $x_i$ y $x_e$ en kg/kg.
2. **Aportes de Humedad:**
* Evaporación: $\dot{m}_e = A \cdot (P_w - P_{a,int}) \cdot F$ [kg/h]
* Ocupación: $\dot{m}_p = N \cdot 0.15$ [kg/h]


3. **Extracción por Ventilación (Nueva Lógica):**
* Capacidad de secado: $\dot{m}_v = Q_v \cdot \rho_{aire} \cdot (x_i - x_e)$ [kg/h]
* *(Nota: Si $x_i > x_e$, la ventilación seca el local y el valor es positivo. Si $x_e > x_i$, el aire exterior es más húmedo, introduciendo humedad al local, por lo que el valor resultará negativo y sumará carga).*


4. **Carga Neta de la Deshumectadora:**
* $\dot{m}_{neta} = \dot{m}_e + \dot{m}_p - \dot{m}_v$ [kg/h]
* $\dot{m}_{total\_diseño} = \max(0, \dot{m}_{neta}) \cdot \left(1 + \frac{\text{Margen}}{100}\right)$ [kg/h]




* **Módulo 2: Potencia Térmica del Vaso (Actualizado)**
1. **Volumen Dinámico:** $V = A \cdot h_{vaso}$ [m³]
2. **Energía de Calentamiento:** $Q_{ini} = V \cdot 1.16 \cdot (T_w - T_{red})$ [kWh]
3. **Potencia de Arranque:** $P_{arranque} = \frac{Q_{ini}}{t_{cal}}$ [kW]
4. **Potencia de Mantenimiento:** $P_{maint} = (\dot{m}_e \cdot 0.68) + P_{otras}$ [kW]



### 3. HISTORIAL DE REQUERIMIENTOS ACUMULADOS

* [Implementado] Interfaz con 3 pestañas (Cálculo, Informe, Documentación).
* [Implementado] Motor de cálculo psicrométrico exacto en JS para $x_i$ y $x_e$.
* [Implementado] Generación de informe justificativo imprimible (MathJax).
* **[Pendiente v1.2]** Sustituir el input de "Volumen" por "Profundidad Media" y automatizar el cálculo del volumen.
* **[Pendiente v1.2]** Modificar el balance de masas: $\text{Carga Total} = \text{Evaporación} + \text{Personas} - \text{Ventilación}$. Reflejar visualmente si la ventilación ayuda a secar o si penaliza.

### 4. ESPECIFICACIONES DE LA VERSIÓN ACTUAL (v1.2)

* **Descripción:** Actualización menor de la interfaz para incluir la profundidad del vaso. Refactorización del algoritmo de cálculo latente para que el aire de ventilación actúe algebraicamente (restando carga si es seco, sumando si es húmedo) acorde a la fórmula solicitada por el usuario.
* **Manejo de errores:** Si el aire exterior es tan seco y el caudal tan alto que la carga neta es negativa (la ventilación seca más de lo que evapora la piscina), la herramienta dictaminará una carga de 0 kg/h para la deshumectadora (no se requiere encenderla bajo esas condiciones).

### 5. STACK TECNOLÓGICO

* HTML5 / CSS3 / Vanilla JS (Mantenido).

### 6. BITÁCORA DE CAMBIOS (Changelog)

* **v1.0:** ITR inicial (balance de masa de agua / deshumectación).
* **v1.1:** Incorporación térmica del vaso (calentamiento y mantenimiento).
* **v1.2:** Cambio geométrico (Volumen calculado vía Área x Profundidad). Corrección del enfoque de ventilación para tratarlo como vector de extracción de humedad $\dot{m} = Q \cdot \rho \cdot (x_i - x_e)$ integrando el cálculo psicrométrico automático.

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA

* **Densidad del aire ($\rho_{aire}$):** 1.2 kg/m³.
* **Balance de masa de agua:** Conservación de la masa en volumen de control. La masa que se retira por ventilación natural/forzada se resta de la que debe procesar el equipo frigorífico.

### 8. LIBRERÍAS TÉCNICAS

* HTML/JS: `MathJax` (v3).

### 9. IMPLEMENTACIÓN

*(Pendiente de aprobación)*


