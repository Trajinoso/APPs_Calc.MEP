# CALCULADORA PARARRAYOS CTE SUA 8

**ID_INFORME:** ITR.SUA8_260624-01 | **VERSIÓN:** v1.2

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS

- **Orden del Usuario:** Sin modificar ninguna otra funcionalidad existente, integrar dos nuevos contenedores visuales (Boxes) específicos en el Panel de Cálculo que muestren de forma explícita el desarrollo numérico sustituido de las fórmulas de la Frecuencia Esperada de Impactos ($N_e$) y la Frecuencia de Riesgo Admisible ($N_a$), replicando exactamente el diseño limpio de la imagen aportada (`image_cc9edf.png`): texto en negrita, recuadro blanco con borde izquierdo grueso en azul corporativo y renderizado de la operación aritmética completa.
- **Normativa:** Código Técnico de la Edificación (CTE), Documento Básico SUA 8.
- **Parámetros Validados:** - Coeficientes y variables geométricas introducidas por el usuario.
    - Formato sintáctico de la multiplicación explícita empleando el punto centrado ($\cdot$) para la separación de términos en LaTeX.

### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

- **Check de Indexación:** Se confirma la indexación de la captura de pantalla (`image_cc9edf.png`), identificando la estructura exacta del bloque visual solicitado.
- **Hipótesis y Simplificaciones Adoptadas:** El motor de cálculo modificará las cadenas de texto dinámicas inyectadas en el DOM para que, en lugar de mostrar únicamente la ecuación abstracta y el resultado, se realice una sustitución ordenada paso a paso de los valores activos en el formulario.
- **Justificación Técnica y Normativa (MEP Focus):**
    - **Formulación de la Frecuencia Esperada ($N_e$):** La inyección mostrará explícitamente los valores de $N_g$, $A_e$ y $C_1$ operados:
        
        $$N_e = [N_g] \cdot [A_e] \cdot [C_1] \cdot 10^{-6} = [\text{Resultado}] \text{ imp/año}$$
        
    - **Formulación del Riesgo Admisible ($N_a$):** Se desglosará el denominador con el producto ordenado de los coeficientes constructivos y de uso ($C_2 \cdot C_3 \cdot C_4 \cdot C_5$):
        
        $$N_a = \frac{5,5}{[C_2] \cdot [C_3] \cdot [C_4] \cdot [C_5]} \cdot 10^{-3} = [\text{Resultado}] \text{ imp/año}$$
        

### 3. Historial de Requerimientos Acumulados:

- **v1.0 (Implementado):** Estructura básica de la calculadora reactiva con almacenamiento persistente en `localStorage`, cálculo analítico de fórmulas en LaTeX y plantilla de informe base para impresión.
- **v1.1 (Implementado):** Ajuste estético de textos e inclusión del Box comparativo reactivo para la inecuación $N_e > N_a$.
- **v1.2 (Pendiente de generación de código):**
    - Inclusión de los dos Boxes de desglose analítico en el panel derecho de la aplicación según la muestra visual de `image_cc9edf.png`.
    - Configuración del script para formatear los decimales fijos ($A_e$ a 2 decimales, frecuencias a 6 decimales) simulando de forma idéntica el comportamiento del motor matemático del documento de referencia.

### 4. Especificaciones de la Versión Actual (v1.2):

- **Descripción:** Se adaptará el método `runCalculationsAndSync()` para actualizar los contenedores HTML con la estructuración exacta demandada. No se modificará ningún estilo base, cabeceras o lógica de persistencia local previa.
- **Estrategia de manejo de errores:** En caso de que un coeficiente contenga un valor vacío o erróneo transitorio, la interfaz mantendrá el formato de la ecuación utilizando guiones o el valor `0.00` para evitar la ruptura visual del renderizado de MathJax.

### 5. Stack Tecnológico:

- HTML5 semántico, CSS3 y JavaScript nativo.
- MathJax v3 para el procesado de las ecuaciones dinámicas.

### 6. BITÁCORA DE CAMBIOS (Changelog)

- **v1.0:** Definición inicial y estructuración de la app.
- **v1.1:** Remoción de términos de trazabilidad e introducción del Box comparativo.
- **v1.2:** Implementación de los Boxes de desarrollo numérico explícito para $N_e$ y $N_a$ basados en la captura técnica provista.

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA:

- **Sistema Internacional (SI):** Unidades de frecuencia temporal expresadas de forma estricta en impactos por año ($\text{imp/año}$).
- **Fórmulas:** Desglose en LaTeX adaptado para cumplir con la visualización exacta requerida por la dirección técnica del proyecto.

### 8. LIBRERÍAS TÉCNICAS:

- HTML/JS: `MathJax.js` (Carga vía CDN).
