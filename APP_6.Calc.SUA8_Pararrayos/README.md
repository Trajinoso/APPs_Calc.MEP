# CALCULADORA PARARRAYOS CTE SUA 8 - MANUAL TÉCNICO Y DOCUMENTACIÓN
**ID_DOCUMENTO:** RE_SUA8_2026-DOC | **VERSIÓN ACUMULADA:** v1.2  
**ESTADO DEL PROYECTO:** CONSOLIDADO Y COMPLETO

---

Este documento técnico funciona como la **Única Fuente de Verdad** y el manual de ingeniería definitivo para la aplicación interactiva de cálculo y justificación de protección contra el rayo, estructurada estrictamente bajo los parámetros del Código Técnico de la Edificación (CTE)[cite: 1, 2].

---

## 1. DESCRIPCIÓN GENERAL Y FUNCIONAMIENTO

La aplicación es una herramienta web interactiva autocontenida (Single File App) diseñada para ingenieros y proyectistas MEP[cite: 2]. Permite realizar la evaluación de riesgo frente a la acción del rayo en cualquier edificación dentro del territorio español[cite: 1, 2].

### Características Principales de Operación
* **Cálculo Reactivo**: Toda modificación en los campos geométricos ($L, W, H$) o selección de coeficientes normativos recalculan de forma inmediata los resultados analíticos en pantalla mediante eventos del DOM.
* **Persistencia de Datos**: Almacenamiento automático y transparente en el navegador (`window.localStorage`), asegurando que al recargar o reabrir el archivo aparezcan siempre los parámetros del último proyecto analizado.
* **Interfaz Profesional Impresionable**: Incluye un layout de impresión optimizado CSS (formato DIN A4) que hereda la paleta de color corporativa (Azul Elecnor `--elecnor-blue: #004687` y Naranja `--elecnor-orange: #FF8200`), ideal para anexar directamente a memorias de proyectos de edificación[cite: 2].

---

## 2. MARCO NORMATIVO (CTE DB-SUA 8)

El motor analítico de la aplicación se rige de manera estricta por las especificaciones de la **Sección SUA 8 del Documento Básico de Seguridad de Utilización y Accesibilidad**[cite: 1, 2].

### Parámetros de Configuración del Entorno y Edificación

#### Coeficiente $C_1$: Relacionado con el entorno del edificio[cite: 1, 2]
| Situación del Edificio | Valor de $C_1$ |
| :--- | :---: |
| Próximo a otros edificios o árboles de la misma altura o más altos | **0,50** |
| Rodeado de edificios más bajos | **0,75** |
| Aislado | **1,00** |
| Aislado sobre una colina o promontorio | **2,00** |

#### Coeficiente $C_2$: En función del tipo de construcción (Estructura / Cubierta)[cite: 1, 2]
| Estructura / Cubierta | Cubierta Metálica | Cubierta de Hormigón | Cubierta de Madera |
| :--- | :---: | :---: | :---: |
| **Metálica** | 0,50 | 1,00 | 2,00 |
| **Hormigón** | 1,00 | 1,00 | 2,50 |
| **Madera** | 2,00 | 2,50 | 3,00 |

#### Coeficientes Adicionales de Uso e Inflamabilidad[cite: 1, 2]
* **$C_3$ (Contenido)**: **3,00** para edificios con contenido inflamable o reactivo; **1,00** para otros contenidos ordinarios[cite: 2].
* **$C_4$ (Ocupación)**: **0,50** para edificios no ocupados normalmente; **3,00** para Pública Concurrencia, Sanitario, Comercial o Docente; **1,00** para el resto de usos (residencial, administrativo, etc.)[cite: 2].
* **$C_5$ (Continuidad pública)**: **5,00** para edificios de servicios imprescindibles (hospitales, bomberos) o con riesgo de impacto ambiental grave; **1,00** para el resto de edificios[cite: 2].

---

## 3. FORMULACIÓN Y DESARROLLO ANALÍTICO

La herramienta evalúa y desglosa las operaciones matemáticas paso a paso utilizando el punto centrado ($\cdot$) como operador explícito de multiplicación para facilitar su comprobación manual directa.

### 3.1. Superficie de Captura Equivalente ($A_e$)
Representa el área crítica de la estructura aislada delimitada por una línea trazada a una distancia $3H$ del perímetro[cite: 1]. Para un edificio de base rectangular simétrica se expande analíticamente como[cite: 2]:

$$A_e = (L \cdot W) + 6H \cdot (L + W) + 9\pi \cdot H^2$$

### 3.2. Frecuencia Esperada de Impactos ($N_e$)
Es el número previsible de impactos directos de rayos anuales calculados sobre la superficie del inmueble[cite: 1]:

$$N_e = N_g \cdot A_e \cdot C_1 \cdot 10^{-6}$$

### 3.3. Frecuencia de Riesgo Admisible ($N_a$)
Constituye el umbral de seguridad normativo máximo tolerado para la configuración arquitectónica y operativa propuesta[cite: 1]:

$$N_a = \frac{5,5}{C_2 \cdot C_3 \cdot C_4 \cdot C_5} \cdot 10^{-3}$$

### 3.4. Eficiencia Requerida del Sistema ($E$)
Determina la tasa de efectividad mínima que debe garantizar la instalación externa captadora (pararrayos)[cite: 2]:

$$E = 1 - \frac{N_a}{N_e}$$

---

## 4. CONDICIONES DE OBLIGATORIEDAD Y EXCEPCIONES ESPECIALES

El dictamen definitivo de instalación obligatoria se activa bajo tres supuestos independientes controlados por lógica booleana por el script[cite: 1, 2]:

1. **Evaluación de Frecuencias**: Condición estándar si la tasa anual esperada sobrepasa el umbral admisible ($N_e > N_a$)[cite: 1].
2. **Excepción por Altura Crítica**: Si la cota máxima del perímetro es estrictamente superior a los $43\text{ m}$ ($H > 43\text{ m}$), la instalación se declara **Obligatoria** con un requerimiento mínimo de eficiencia $E \ge 0,98$ (Nivel de Protección 1), ignorando el balance de frecuencias[cite: 1].
3. **Excepción por Sustancias Peligrosas**: Edificios destinados a la manipulación de sustancias tóxicas, radiactivas, altamente inflamables o explosivas activan de forma automática la obligatoriedad con eficiencia máxima ($E \ge 0,98$, Nivel de Protección 1)[cite: 1].

### Determinación del Nivel de Protección Exigido (Tabla 2.1)[cite: 2]
Si el sistema es requerido, se mapea la eficiencia dentro de los rangos oficiales:
* **Nivel 1**: $E \ge 0,98$[cite: 2]
* **Nivel 2**: $0,95 \le E < 0,98$[cite: 2]
* **Nivel 3**: $0,80 \le E < 0,95$[cite: 2]
* **Nivel 4**: $0 \le E < 0,80$ *(Nota: Dentro de este rango, la instalación no es obligatoria por el mero cálculo de frecuencias según nota al pie de tabla)*[cite: 2].

---

## 5. CONFIGURACIÓN GRÁFICA DE LA INTERFAZ (BOXES EXCLUSIVOS)

Para garantizar la máxima claridad técnica y cumplir de forma fidedigna con la documentación gráfica de referencia (`image_cc9edf.png`), se han estructurado los siguientes componentes en el panel de control:

### A. Boxes de Desarrollo Analítico Detallado
* **Formato Estético**: Bloques con fondo blanco puro (`#ffffff`), sombra sutil y un borde izquierdo de realce de $5\text{ px}$ en azul corporativo (`--elecnor-blue`).
* **Inyección de Variables**: Muestran la sustitución en tiempo real de los valores activos del formulario dentro de la expresión LaTeX, permitiendo ver de un vistazo el peso aritmético de la densidad de impactos $N_g$ o el producto de la matriz de coeficientes $C$.

### B. Box de Demostración Visual de Riesgo ($N_e > N_a$)
* Un contenedor interactivo centralizado que contrasta ambos valores de manera directa.
* Cambia dinámicamente de operador lógico ($>$ o $\le$) y altera sus propiedades cromáticas de forma contextual: si el riesgo se supera, los indicadores conmutan a rojo de advertencia (`--danger-color`); si se mantiene dentro del umbral tolerado, el operador se muestra en verde de seguridad (`--success-color`).

### C. Box Exclusivo de Nivel de Protección en el Informe
* Ubicado estratégicamente en la sección 4 de la memoria imprimible, destaca visualmente sobre fondo suave el nivel de diseño requerido (Nivel 1 al 4) facilitando la rápida lectura por parte de las entidades de control técnico u organismos de inspección (OCT)[cite: 2].

---

## 6. BITÁCORA DE CAMBIOS ACUMULATIVA (CHANGELOG)

### v1.0 (24/06/2026) - Entrega Inicial Basada en CTE SUA 8
* Consolidación del motor matemático para $A_e$, $N_e$, $N_a$ y eficiencia $E$[cite: 1, 2].
* Desarrollo de la interfaz inicial con selectores para coeficientes $C_1$ a $C_5$[cite: 1, 2].
* Implementación de la persistencia persistente mediante la API `localStorage`.
* Diseño preliminar del formato de impresión con CSS adaptado.

### v1.1 (24/06/2026) - Refinamiento Semántico e Indicadores de Riesgo
* Supresión de etiquetas de control y textos no funcionales del entorno visual.
* Adición del Box interactivo biestable para contrastar en caliente la inecuación de riesgo $N_e > N_a$.
* Inclusión del Box independiente en el informe técnico DIN A4 para indicar de manera clara el nivel de protección de la instalación[cite: 2].

### v1.2 (24/06/2026) - Integración de Bloques Analíticos de Referencia (Versión Actual)
* Programación de los contenedores de desarrollo analítico en el panel derecho según la morfología gráfica de `image_cc9edf.png`.
* Ajuste sintáctico en las ecuaciones matemáticas para la sustitución explícita de valores numéricos reales con uso estricto de puntos centrados ($\cdot$).
* Sincronización del módulo de auto-documentación interna en la pestaña de soporte técnico.