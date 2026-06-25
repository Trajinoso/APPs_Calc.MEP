He comprendido el protocolo de ingeniería MEP y el sistema de ITR incremental. Estoy listo para indexar la información. Por favor, proporcione el nombre del proyecto y los detalles de la app a desarrollar.

Información indexada correctamente para el contexto del proyecto. A continuación, presento el **Informe Técnico de Requisitos (ITR)** para su validación antes de proceder con el desarrollo de la aplicación.

# INFORME TÉCNICO DE REQUISITOS: CALCULADORA PARARRAYOS CTE SUA 8

**ID_INFORME:** ITR.SUA8_260624-01 | **VERSIÓN:** v1.0

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS

- **Orden del Usuario:** Desarrollar una aplicación web interactiva en un único archivo HTML5 para evaluar la necesidad de instalación de un sistema de protección contra el rayo (pararrayos) y determinar su nivel de protección de acuerdo con la sección CTE DB-SUA 8. La aplicación debe guardar los datos de entrada en el almacenamiento local (`localStorage`), mostrar detalladamente el desarrollo analítico de las fórmulas en pantalla y generar un informe técnico imprimible que replique y amplíe el diseño CSS corporativo suministrado (azul/naranja, tipografías claras, tablas formateadas).
- **Normativa:** Código Técnico de la Edificación (CTE), Documento Básico de Seguridad de Utilización y Accesibilidad, Sección SUA 8 ("Seguridad frente al riesgo causado por la acción del rayo").
- **Parámetros Validados:** - Dimensiones del edificio: Largo ($L > 0$), Ancho ($W > 0$) y Altura ($H > 0$) en metros.
    - Densidad de impactos sobre el terreno ($N_g \ge 0$) en $\text{impactos/km}^2\cdot\text{año}$.
    - Coeficientes adimensionales normativos ($C_1, C_2, C_3, C_4, C_5$) seleccionados dinámicamente mediante selectores/checks con validación automática.
- **Hipótesis y Simplificaciones Adoptadas:** El edificio se modela como un paralelepípedo rectangular aislado o integrado dentro de un entorno regular para el cálculo automático de su superficie de captura equivalente ($A_e$). No se contemplan geometrías complejas salientes o multinivel que requieran una discretización volumétrica tridimensional manual por parte del proyectista, asumiendo la cota de altura máxima ($H$) para todo el perímetro.

### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

- **Check de Indexación:** Se confirma la correcta indexación de las variables y coeficientes extraídos de la sección SUA 8 del CTE y del informe de referencia de la "Casa Villa Hanna" en Marbella.
- **Hipótesis y Simplificaciones:** Se asume que el volumen exterior envolvente define el perímetro crítico de captura.
- **Desarrollo de los Cálculos e Implementación:**
    1. **Superficie de captura equivalente ($A_e$):** Para un edificio rectangular aislado de altura uniforme $H$, la superficie delimitada por la línea trazada a una distancia $3H$ se calcula matemáticamente mediante la expresión expandida:
        
        $$A_e = (L \cdot W) + 6H \cdot (L + W) + 9\pi \cdot H^2$$
        
    2. **recuencia esperada de impactos ($N_e$):** Número previsible de impactos anuales sobre la estructura:
        
        $$N_e = N_g \cdot A_e \cdot C_1 \cdot 10^{-6}$$
        
    3. **Riesgo admisible ($N_a$):** Umbral de tolerancia normativo en función de las características constructivas, uso y entorno:
        
        $$N_a = \frac{5,5}{C_2 \cdot C_3 \cdot C_4 \cdot C_5} \cdot 10^{-3}$$
        
    4. **Criterio de Obligatoriedad:** La instalación es obligatoria si y solo si $N_e > N_a$. Además, se añade la regla condicional estricta del CTE: si el edificio manipula sustancias tóxicas/inflamables o su altura $H > 43\text{ m}$, el sistema es obligatorio con nivel de protección 1 ($E \ge 0,98$) independientemente del cálculo de frecuencias.
    5. **Cálculo de la Eficiencia ($E$) y Nivel de Protección:** Si se requiere protección, se calcula la eficiencia mínima mediante:
        
        $$E = 1 - \frac{N_a}{N_e}$$
        
        El nivel se selecciona según los rangos establecidos en la Tabla 2.1 del CTE.
        

### 3. HISTORIAL DE REQUERIMIENTOS ACUMULADOS

- **v1.0 (Pendiente de desarrollo):** - Módulo 1: Panel de introducción de datos de proyecto (Nombre, Ubicación, Expediente).
    - Módulo 2: Configuración geométrica ($L, W, H, N_g$) con almacenamiento automático en `localStorage`.
    - Módulo 3: Matriz interactiva de selección para coeficientes $C_1$ a $C_5$ con descripción normativa visible.
    - Módulo 4: Bloque de cálculo en tiempo real con desglose matemático dinámico en LaTeX.
    - Módulo 5: Generador de Informes Técnicos de Ingeniería con vista previa de impresión y maquetación CSS idéntica al estándar provisto.
    - Módulo 6: Pestaña informativa con el volcado completo de este ITR en Markdown para asegurar la auto-documentación de la herramienta.

### 4. ESPECIFICACIONES DE LA VERSIÓN ACTUAL (v1.0)

- **Alcance:** Programación íntegra de la interfaz de usuario en un solo documento HTML5 sin librerías de servidor.
- **Lógica de Negocio:** Actualización reactiva (evento `oninput` / `onchange`) que recalcula todos los parámetros de manera inmediata al modificar una variable.
- **Estrategia de Manejo de Errores:** - Bloqueo de valores negativos o incoherentes en los campos numéricos mediante atributos `min="0"` y validaciones personalizadas por JS.
    - Alertas visuales contextuales (en color rojo) si la altura supera los $43\text{ m}$ para recordar la obligatoriedad automática por altura según el artículo SUA 8.1.2.

### 5. STACK TECNOLÓGICO

- **Arquitectura:** Cliente puro (Single File App).
- **Frontend:** HTML5 semántico y CSS3 con diseño responsivo (CSS Grid y Flexbox).
- **Motor de Renderizado Matemático:** MathJax v3 (vía CDN oficial de carga asíncrona) para visualizar las ecuaciones de forma nativa y profesional.
- **Persistencia:** Web Storage API nativa (`window.localStorage`).

### 6. BITÁCORA DE CAMBIOS (CHANGELOG)

- **v1.0:** Definición de requisitos iniciales, estructuración de las ecuaciones de la sección DB-SUA 8 y diseño de la lógica de persistencia y exportación de informes.

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA

El cálculo se rige bajo las unidades del Sistema Internacional (SI). Los coeficientes de penalización o minoración del riesgo se definen matemáticamente de la siguiente manera:

- **Densidad de impactos ($N_g$):** Expresada en $\text{impactos/km}^2\cdot\text{año}$.
- **Superficie de captura ($A_e$):** Expresada en $\text{m}^2$.
- **Eficiencia requerida ($E$):** Valor adimensional entre $0$ y $1$ que cuantifica la capacidad de interceptación requerida para el pararrayos.

### 8. LIBRERÍAS TÉCNICAS

- **JavaScript/CDNs:** Carga externa únicamente de `MathJax` para la traducción sintáctica de expresiones LaTeX a HTML matemático interactivo. No se requiere manipulación externa mediante frameworks complejos (como React o Vue) para garantizar la portabilidad absoluta del archivo local.

⚠️ ESPERANDO ACEPTACIÓN TÉCNICA. Por favor, revisa los puntos anteriores y escribe 'ACEPTO INFORME' para proceder con el código, o indica los cambios necesarios. En caso de aceptar el informe, solicita también el lenguaje en cual se hará el código.
