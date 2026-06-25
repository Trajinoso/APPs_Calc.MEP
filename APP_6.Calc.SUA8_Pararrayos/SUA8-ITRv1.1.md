# CALCULADORA PARARRAYOS CTE SUA 8

**ID_INFORME:** ITR.SUA8_260624-01 | **VERSIÓN:** v1.1

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS

- **Orden del Usuario:** Modificar la aplicación para eliminar por completo el término "Trazabilidad rígida" tanto del entorno de la interfaz como del informe técnico. Incluir un nuevo contenedor visual (Box) interactivo que demuestre de forma gráfica y clara la comparación directa de si la frecuencia esperada de impactos supera el riesgo admisible ($N_e > N_a$). Asimismo, integrar un Box específico dentro del informe imprimible que destaque de forma limpia el nivel de protección requerido según los resultados analíticos de eficiencia.
- **Normativa:** Código Técnico de la Edificación (CTE), Documento Básico de Seguridad de Utilización y Accesibilidad, Sección SUA 8.
- **Parámetros Validados:** - Dimensionado geométrico ($L, W, H$) y densidad de impactos local ($N_g$).
    - Factores correctores adimensionales normativos ($C_1$ a $C_5$).
- **Hipótesis y simplificaciones adoptadas:** El cálculo de la relación de riesgo se mantiene automatizado bajo un enfoque reactivo continuo, evaluando simultáneamente las condiciones geométricas de contorno y las excepciones directas de la norma.

### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

- **Check de Indexación:** Se confirma la indexación y asimilación de los nuevos requisitos visuales sobre las variables $N_e$ y $N_a$ ya establecidas en la normativa del documento de entrada.
- **Hipótesis y simplificaciones adoptadas:** El Box comparativo procesará los valores matemáticos de $N_e$ y $N_a$ en escala comparativa o mediante indicadores de umbral crítico para validar visualmente la inecuación fundamental:
    
    $$\text{Condición de Obligatoriedad: } N_e > N_a$$
    
- **Desarrollo completo de los cálculos y consideraciones adoptadas:**
    - Si $N_e > N_a$, el Box de comparación visual cambiará a un estado de alerta técnica (color de advertencia/crítico), mostrando gráficamente la superación del umbral permitido.
    - La asignación del nivel de protección se mantiene vinculada de forma estricta a la Tabla 2.1 del CTE en función de la eficiencia calculada $E = 1 - (N_a/N_e)$:
        - $E \ge 0,98 \rightarrow$ Nivel 1
        - $0,95 \le E < 0,98 \rightarrow$ Nivel 2
        - $0,80 \le E < 0,95 \rightarrow$ Nivel 3
        - $0 \le E < 0,80 \rightarrow$ Nivel 4 (No obligatorio por cálculo)

### 3. Historial de Requerimientos Acumulados:

- **v1.0 (Implementado):** Estructura básica de la calculadora reactiva con almacenamiento persistente en `localStorage`, cálculo analítico de fórmulas en LaTeX y plantilla de informe base para impresión.
- **v1.1 (Pendiente de generación de código):**
    - Remoción completa del término "Trazabilidad rígida" de toda la aplicación.
    - Adición de un componente visual (Box) en la interfaz para la demostración gráfica de la inecuación $N_e > N_a$.
    - Inclusión de un Box destacado en el layout de impresión del informe para indicar el Nivel de Protección asignado de manera limpia y profesional.

### 4. Especificaciones de la Versión Actual (v1.1):

- **Descripción:** Se estructurará el código para incluir contenedores CSS específicos (`.comparison-box` y `.protection-level-box`) con estilos alineados a la identidad visual establecida (azul/naranja). El motor de cálculo en JavaScript actualizará dinámicamente tanto el estado gráfico de la inecuación como el cuadro del informe técnico de impresión.
- **Lógica de negocio:** Vinculación directa mediante manipulación del DOM nativo de las variables calculadas con los nuevos elementos de la vista.
- **Estrategia de manejo de errores:** Validación de divisiones por cero en el cálculo de la eficiencia cuando $N_e$ sea idéntico a cero (terreno con densidad nula o dimensiones virtuales cero), previniendo valores del tipo `Infinity` o `NaN` en los nuevos componentes visuales.

### 5. Stack Tecnológico:

- HTML5 semántico, CSS3 avanzado (Layouts Flexbox/Grid) y JavaScript nativo (ES6+).
- Motor matemático MathJax v3 vía CDN oficial.

### 6. BITÁCORA DE CAMBIOS (Changelog)

- **v1.0:** Versión base inicial aprobada.
- **v1.1:** Ajuste de diseño y semántica: eliminación del concepto textual de trazabilidad, integración del Box de comparación gráfica de frecuencias y diseño del Box de nivel de protección dentro de la memoria imprimible.

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA:

- **Sistema Internacional (SI):** Frecuencias expresadas en impactos por año ($\text{imp/año}$).
- **Marco Normativo:** CTE DB-SUA 8.
- **Fórmulas:** Representación formalizada mediante MathJax de la inecuación de riesgo y los niveles de protección técnica.

### 8. LIBRERÍAS TÉCNICAS:

- HTML/JS: `MathJax.js` (Renderizado de notación científica y LaTeX).

### 9. IMPLEMENTACIÓN (SOLO SI EL ESTADO ES "ACEPTADO")

*(Bloque de código funcional retenido de acuerdo al protocolo técnico de Fase 2)*
