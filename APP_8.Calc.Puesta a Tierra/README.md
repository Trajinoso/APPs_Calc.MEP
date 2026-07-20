Aquí tienes el texto del documento convertido a formato Markdown de forma íntegra y estructurada:

---

# DOCUMENTACIÓN TÉCNICA

## Calculadora de Sistemas de Puesta a Tierra

### Diseño y Dimensionado según REBT

**ITC-BT-18 / ITC-BT-26 / Guía Técnica de Aplicación**

**Versión 1.0**

**6 de marzo de 2026**

---

## Índice

1. [Introducción](https://www.google.com/search?q=%231-introducci%C3%B3n)
2. [Marco Normativo](https://www.google.com/search?q=%232-marco-normativo)
3. [Fundamentos Técnicos](https://www.google.com/search?q=%233-fundamentos-t%C3%A9cnicos)
4. [Criterios de Diseño](https://www.google.com/search?q=%234-criterios-de-dise%C3%B1o)
5. [Formulación Matemática](https://www.google.com/search?q=%235-formulaci%C3%B3n-matem%C3%A1tica)
6. [Metodología de Cálculo](https://www.google.com/search?q=%236-metodolog%C3%ADa-de-c%C3%A1lculo)
7. [Especificaciones de Implementación](https://www.google.com/search?q=%237-especificaciones-de-implementaci%C3%B3n)
8. [Anexo: Prompt para IA](https://www.google.com/search?q=%238-anexo-prompt-para-ia)

---

## 1. Introducción

La instalación de puesta a tierra es un elemento fundamental de seguridad en cualquier instalación eléctrica de baja tensión. Su función principal es garantizar la protección de las personas ante contactos indirectos y derivar a tierra las corrientes de fuga, descargas atmosféricas y sobretensiones.

Este documento presenta la documentación técnica completa de una calculadora especializada para el dimensionado de sistemas de puesta a tierra en edificios, cumpliendo rigurosamente con el Reglamento Electrotécnico de Baja Tensión (REBT) y sus Instrucciones Técnicas Complementarias ITC-BT-18 e ITC-BT-26.

### 1.1. Objetivos de la Calculadora

* Dimensionar correctamente el sistema de puesta a tierra según REBT
* Calcular la necesidad de electrodos adicionales (picas) según características del terreno
* Verificar el cumplimiento simultáneo de múltiples criterios normativos
* Generar informes técnicos justificativos para proyectos eléctricos

---

## 2. Marco Normativo

### 2.1. REBT ITC-BT-18: Instalaciones de Puesta a Tierra

La ITC-BT-18 establece los requisitos de seguridad para las puestas a tierra, definiendo que el valor de la resistencia debe ser tal que cualquier masa no pueda dar lugar a tensiones de contacto superiores a:

* **24 V** en locales o emplazamientos conductores (exteriores, locales mojados, húmedos)
* **50 V** en los demás casos (locales secos interiores)

El cálculo de la resistencia máxima admisible se realiza mediante la Ley de Ohm:

$$R_{\text{máx}} = \frac{V_c}{I_d}$$

donde $V_c$ es la tensión de contacto límite (24 o 50 V) e $I_d$ es la sensibilidad del diferencial instalado.

### 2.2. Guía Técnica ITC-BT-26

La Guía Técnica de aplicación de la ITC-BT-26 establece valores de referencia de buena práctica profesional en función de la presencia o ausencia de pararrayos:

* **Edificios con pararrayos:** $R < 15\ \Omega$
* **Edificios sin pararrayos:** $R < 37\ \Omega$

Estos valores complementan los requisitos de seguridad del REBT, siendo más restrictivos en edificios con pararrayos debido a la necesidad de una evacuación eficaz de las corrientes de descarga atmosférica.

---

## 3. Fundamentos Técnicos

### 3.1. Componentes del Sistema de Puesta a Tierra

Un sistema de puesta a tierra en edificios consta de los siguientes elementos fundamentales:

#### 3.1.1. Electrodo de Tierra o Anillo de Cimentación

Conductor de cobre desnudo formando un anillo cerrado que rodea el perímetro del edificio, instalado en el fondo de las zanjas de cimentación antes de hormigonar. Características técnicas:

* **Sección mínima:** 25 mm² (habitual 50 mm²)
* **Material:** Cobre desnudo
* **Profundidad mínima:** 0,5 m (recomendado $\ge 0,8\text{ m}$)
* **Uniones:** Soldadura aluminotérmica o autógena

#### 3.1.2. Picas o Electrodos Verticales

Varillas metálicas hincadas verticalmente en el terreno cuando la resistencia del anillo no es suficiente. Las más habituales son picas de acero recubiertas de cobre de 2 metros de longitud y 14-18 mm de diámetro.

#### 3.1.3. Línea de Enlace con Tierra (LET)

Conductor que conecta el borne principal de tierra con el electrodo de puesta a tierra. Sección mínima según tabla 1 de ITC-BT-18.

#### 3.1.4. Línea Principal de Tierra (LPT)

En edificios de viviendas, conductor que va por la misma canalización que la línea general de alimentación. Secciones según ITC-BT-26:

* Si fases $< 35\text{ mm}^2$: $\text{LPT} \ge 16\text{ mm}^2$
* Si fases $\ge 35\text{ mm}^2$: $\text{LPT} \ge \frac{\text{Sección\_fase}}{2}$

#### 3.1.5. Conductores de Protección

Conductores verde-amarillo que unen las masas metálicas (enchufes, aparatos) con el borne principal de tierra. Sección según tabla 2 de ITC-BT-18.

### 3.2. Resistividad del Terreno

La resistividad ($\rho$) del terreno es el parámetro fundamental que determina la resistencia de tierra. Se mide en $\Omega\cdot\text{m}$ y varía significativamente según la naturaleza del suelo:

| Tipo de Terreno | $\rho$ mín. ($\Omega\cdot\text{m}$) | $\rho$ típico ($\Omega\cdot\text{m}$) | $\rho$ máx. ($\Omega\cdot\text{m}$) |
| --- | --- | --- | --- |
| Cenizas, turba, arcillas blandas | 1 | 30 | 100 |
| Arcilla, marga | 50 | 100 | 200 |
| Arenas arcillosas, calizas blandas | 100 | 300 | 500 |
| Arenas silíceas, granitos, basaltos | 200 | 600 | 1000 |
| Calizas compactadas, esquistos, pizarras | 500 | 1500 | 3000 |
| Roca masiva, granito compacto | 1000 | 5000 | >10000 |

*Nota: Valores en $\Omega\cdot\text{m}$. Para diseño definitivo se requiere medición in situ con telurómetro.*

---

## 4. Criterios de Diseño

### 4.1. Jerarquía de Umbrales Normativos

La calculadora implementa una verificación multi-umbral que considera simultáneamente dos criterios normativos independientes:

1. **Criterio de Seguridad REBT ITC-BT-18:**
Umbral absoluto de seguridad calculado por la Ley de Ohm en función del tipo de emplazamiento y la sensibilidad del diferencial instalado. Es un requisito obligatorio e ineludible.
2. **Criterio de Buena Práctica ITC-BT-26:**
Umbral de referencia profesional según la Guía Técnica, diferenciado por la presencia o ausencia de sistema de protección contra el rayo. Representa valores de diseño conservadores y de buena práctica.

El objetivo de diseño adoptado es el más restrictivo de ambos umbrales:

$$R_{\text{objetivo}} = \min(R_{\text{REBT}}, R_{\text{ITC-BT-26}})$$

### 4.2. Estrategia de Dimensionado

El dimensionado sigue una metodología secuencial que optimiza el uso de recursos:

* **Paso 1: Evaluación del anillo de cimentación**
Se calcula la resistencia del anillo en función del perímetro del edificio y la resistividad del terreno. Si esta resistencia es inferior al objetivo de diseño, no se requieren picas adicionales.
* **Paso 2: Cálculo de picas necesarias (si procede)**
Si el anillo es insuficiente, se determina el número mínimo de picas verticales necesarias para alcanzar el objetivo mediante el análisis del circuito paralelo formado por el anillo y las picas.
* **Paso 3: Verificación cruzada**
Se comprueba que la resistencia total obtenida cumple todos los umbrales normativos aplicables, generando un informe detallado del estado de cumplimiento.

---

## 5. Formulación Matemática

### 5.1. Resistencia Máxima Admisible (REBT)

Aplicación de la Ley de Ohm para el cálculo de la resistencia máxima admisible en función de la tensión de contacto límite y la sensibilidad del diferencial:

$$R_{\text{REBT}} = \frac{V_c}{I_d}$$

donde:

* $V_c$ = Tensión de contacto límite (24 V o 50 V según emplazamiento)
* $I_d$ = Sensibilidad del diferencial en amperios (ej: 0,03 A para 30 mA)
* $R_{\text{REBT}}$ = Resistencia máxima admisible en ohmios ($\Omega$)

### 5.2. Resistencia del Anillo de Cimentación

Para un conductor enterrado horizontalmente formando un anillo cerrado, la resistencia se calcula mediante:

$$R_{\text{anillo}} = \frac{2 \times \rho}{L}$$

donde:

* $\rho$ = Resistividad del terreno en $\Omega\cdot\text{m}$
* $L$ = Longitud total del conductor enterrado (perímetro del edificio) en metros
* $R_{\text{anillo}}$ = Resistencia del anillo en ohmios ($\Omega$)

### 5.3. Resistencia de Electrodos Verticales (Picas)

Para picas verticales de longitud $L$ hincadas en el terreno:

$$R_{\text{pica}} = \frac{\rho}{L}$$

Para $n$ picas idénticas instaladas en paralelo (conexión al mismo borne principal):

$$R_{\text{picas\_total}} = \frac{\rho}{n \times L}$$

donde:

* $n$ = Número de picas instaladas
* $L$ = Longitud de cada pica en metros (habitualmente 2 m)

### 5.4. Resistencia Total del Sistema (Circuito Paralelo)

El anillo de cimentación y las picas forman un circuito en paralelo. La resistencia total se calcula mediante:

$$\frac{1}{R_{\text{total}}} = \frac{1}{R_{\text{anillo}}} + \frac{1}{R_{\text{picas}}}$$

Despejando $R_{\text{total}}$:

$$R_{\text{total}} = \frac{R_{\text{anillo}} \times R_{\text{picas}}}{R_{\text{anillo}} + R_{\text{picas}}}$$

### 5.5. Cálculo del Número de Picas Necesarias

Si el anillo por sí solo no alcanza el objetivo ($R_{\text{anillo}} > R_{\text{objetivo}}$), se determina primero la resistencia necesaria del conjunto de picas para alcanzar el objetivo total:

$$R_{\text{picas\_necesaria}} = \frac{R_{\text{anillo}} \times R_{\text{objetivo}}}{R_{\text{anillo}} - R_{\text{objetivo}}}$$

A continuación, despejando $n$ de la fórmula de resistencia de picas en paralelo:

$$n = \frac{\rho}{R_{\text{picas\_necesaria}} \times L_{\text{pica}}}$$

El resultado se redondea al entero superior ($\lceil n \rceil$) para garantizar el cumplimiento del objetivo.

### 5.6. Separación entre Picas

Según la ITC-BT-18, cuando se instalan dos o más picas, la separación mínima recomendada entre ellas debe ser igual o mayor que su longitud enterrada:

$$\text{Separación mínima} \ge L_{\text{pica}}$$

Para más de dos picas instaladas en paralelo, se recomienda aumentar esta separación para evitar interferencias entre los campos eléctricos de cada electrodo.

---

## 6. Metodología de Cálculo

La calculadora implementa el siguiente algoritmo secuencial:

* **PASO 1: Determinación de umbrales normativos**
* a) Calcular $R_{\text{REBT}} = \frac{V_c}{I_d}$
* b) Obtener $R_{\text{ITC-BT-26}}$ según pararrayos: $15\ \Omega$ (con) / $37\ \Omega$ (sin)
* c) Determinar $R_{\text{objetivo}} = \min(R_{\text{REBT}}, R_{\text{ITC-BT-26}})$


* **PASO 2: Evaluación del anillo de cimentación**
* a) Calcular $R_{\text{anillo}} = \frac{2 \times \rho}{L}$
* b) Comparar $R_{\text{anillo}}$ con $R_{\text{objetivo}}$:
* Si $R_{\text{anillo}} \le R_{\text{objetivo}} \rightarrow$ El anillo es **SUFICIENTE** $\rightarrow$ **FIN** ($n = 0\text{ picas}$)
* Si $R_{\text{anillo}} > R_{\text{objetivo}} \rightarrow$ Continuar al PASO 3




* **PASO 3: Cálculo de picas necesarias**
* a) Calcular $R_{\text{picas\_nec}} = \frac{R_{\text{anillo}} \times R_{\text{objetivo}}}{R_{\text{anillo}} - R_{\text{objetivo}}}$
* b) Calcular $n = \frac{\rho}{R_{\text{picas\_nec}} \times L_{\text{pica}}}$
* c) Redondear $n$ al entero superior $\rightarrow n_{\text{picas}}$


* **PASO 4: Cálculo de resistencia total y verificación**
* a) Calcular $R_{\text{picas\_real}} = \frac{\rho}{n_{\text{picas}} \times L_{\text{pica}}}$
* b) Calcular $R_{\text{total}} = \frac{1}{\frac{1}{R_{\text{anillo}}} + \frac{1}{R_{\text{picas\_real}}}}$
* c) Verificar:
* $R_{\text{total}} \le R_{\text{REBT}}$ (seguridad)
* $R_{\text{total}} \le R_{\text{ITC-BT-26}}$ (buena práctica)
* $V_{c\text{\_real}} = R_{\text{total}} \times I_d \le V_c$





---

## 7. Especificaciones de Implementación

### 7.1. Datos de Entrada

La calculadora debe solicitar los siguientes parámetros al usuario:

1. **Tipo de emplazamiento:**
* Normal (interior seco): $V_c = 50\text{ V}$
* Conductor (exterior/mojado): $V_c = 24\text{ V}$


2. **Sensibilidad del diferencial ($I_d$):**
* Valores estándar: 10 mA, 30 mA, 100 mA, 300 mA, 500 mA, 1 A


3. **Presencia de pararrayos:**
* SÍ $\rightarrow R_{\text{ITC-BT-26}} = 15\ \Omega$
* NO $\rightarrow R_{\text{ITC-BT-26}} = 37\ \Omega$


4. **Características del terreno:**
* Tipo de terreno (selector con valores predefinidos)
* Resistividad $\rho$ en $\Omega\cdot\text{m}$ (campo numérico, autocompletado según tipo)


5. **Anillo de cimentación:**
* Longitud total $L$ en metros (perímetro del edificio)


6. **Picas verticales:**
* Longitud de cada pica en metros (habitual: 2 m)
* Modo de cálculo: Automático (calculado) / Manual (número fijo a verificar)



### 7.2. Resultados a Mostrar

La calculadora debe presentar de forma clara y estructurada:

* Resistencia del anillo ($R_{\text{anillo}}$ en $\Omega$)
* Número de picas necesarias (0 si el anillo es suficiente)
* Resistencia del conjunto de picas ($R_{\text{picas}}$ en $\Omega$, si aplica)
* Resistencia total del sistema ($R_{\text{total}}$ en $\Omega$)
* Tensión de contacto real ($V_{c\text{\_real}}$ en V)
* Objetivo de diseño cumplido ($R_{\text{objetivo}}$ en $\Omega$)
* Tabla de verificación normativa con estado de cumplimiento REBT e ITC-BT-26
* Desarrollo paso a paso del cálculo con todas las fórmulas aplicadas
* Justificación técnica de la necesidad o no de picas
* Separación mínima entre picas (si $n \ge 2$)
* Indicación visual clara del cumplimiento/incumplimiento normativo

### 7.3. Funcionalidad de Generación de Informes

La calculadora debe incluir un botón para generar un informe técnico en formato HTML (convertible a PDF) que contenga:

* Cabecera con título, fecha y hora de generación
* Ficha técnica con todos los datos de entrada
* Resultados principales en formato visual (cajas coloreadas)
* Tabla de verificación normativa detallada
* Desarrollo completo del cálculo con fórmulas y valores numéricos
* Conclusión técnica justificativa
* Referencias normativas y notas técnicas
* Pie de página con identificación del documento

### 7.4. Consideraciones de Diseño de Interfaz

La interfaz debe cumplir los siguientes requisitos de usabilidad y diseño:

1. **Organización clara por pestañas:**
* Pestaña principal: Calculadora con formulario y resultados
* Pestaña de referencia: Tabla de resistividades de terrenos
* Pestaña de referencia: Secciones de conductores (ITC-BT-18/26)


2. **Formulario de entrada estructurado en tarjetas:**
* Datos del edificio e instalación (emplazamiento, diferencial, pararrayos)
* Características del terreno (tipo y resistividad)
* Electrodo anillo/conductor enterrado
* Electrodos verticales (picas)


3. **Visualización de resultados:**
* Métricas principales en cuadros destacados con código de colores:
* **Verde:** Cumple normativa
* **Amarillo/Naranja:** Advertencia (anillo insuficiente, necesita picas)
* **Rojo:** No cumple (aumentar picas)


* Tabla de verificación normativa con iconos ✅ / ❌
* Desarrollo paso a paso del cálculo numerado con fórmulas destacadas
* Barra de estado final con resumen ejecutivo


4. **Estética técnica/industrial:**
* Paleta de colores profesional (azules, grises oscuros)
* Tipografías técnicas (monoespaciadas para fórmulas)
* Diseño responsive adaptable a móvil/tablet/escritorio
* Efectos hover y transiciones suaves en botones e interacciones



---

## 8. Anexo: Prompt para IA

El siguiente prompt puede utilizarse para solicitar a una IA generativa (como Claude, GPT-4, etc.) la creación de la calculadora descrita en este documento:

```text
─────────────────────────────────────────────────────────
PROMPT PARA GENERACIÓN DE CALCULADORA DE PUESTA A TIERRA
Crea una calculadora web interactiva para el dimensionado de sistemas de puesta a tierra en edificios según el Reglamento Electrotécnico de Baja Tensión (REBT) español, específicamente las Instrucciones Técnicas Complementarias ITC-BT-18 e ITC-BT-26.

REQUISITOS TÉCNICOS:
1. Implementar el siguiente algoritmo de cálculo:
   • Calcular R_REBT = Vc / Id donde Vc es 24V (emplazamiento conductor) o 50V (normal), e Id es la sensibilidad del diferencial
   • Obtener R_ITC-BT-26 = 15 Ω (edificio con pararrayos) o 37 Ω (sin pararrayos) según Guía Técnica
   • Determinar R_objetivo = min(R_REBT, R_ITC-BT-26)
   • Calcular R_anillo = (2 × ρ) / L donde ρ es resistividad del terreno y L longitud del anillo
   • Si R_anillo ≤ R_objetivo: el anillo es suficiente, n_picas = 0
   • Si R_anillo > R_objetivo: calcular R_picas_nec = (R_anillo × R_objetivo) / (R_anillo − R_objetivo)
   • Calcular n = ceil[ρ / (R_picas_nec × L_pica)] donde L_pica es longitud de cada pica (habitual 2m)
   • Calcular R_total = 1 / [(1/R_anillo) + (1/R_picas)] donde R_picas = ρ / (n × L_pica)
   • Verificar que R_total cumple REBT e ITC-BT-26

2. Datos de entrada a solicitar:
   • Tipo de emplazamiento: Normal (50V) / Conductor (24V)
   • Sensibilidad diferencial: 10, 30, 100, 300, 500, 1000 mA
   • Edificio con pararrayos: SÍ/NO (determina umbral 15 Ω o 37 Ω)
   • Tipo de terreno con valores predefinidos de resistividad o valor manual
   • Longitud del anillo de cimentación en metros
   • Longitud de cada pica (por defecto 2 m)
   • Modo de cálculo: Automático (calcular n) o Manual (verificar n dado)

3. Resultados a mostrar:
   • Resistencia del anillo, resistencia de picas, resistencia total
   • Número de picas necesarias (0 si anillo suficiente)
   • Tensión de contacto real calculada
   • Tabla de verificación normativa mostrando cumplimiento REBT e ITC-BT-26 con ✅/❌
   • Justificación técnica de necesidad/no necesidad de picas
   • Desarrollo paso a paso con todas las fórmulas aplicadas
   • Separación mínima entre picas (≥ L_pica según ITC-BT-18) si n ≥ 2
   • Barra de estado final con resumen de cumplimiento

4. Funcionalidades adicionales:
   • Incluir 3 pestañas: (1) Calculadora, (2) Tabla resistividades terrenos, (3) Secciones cables REBT
   • Botón para generar informe técnico en HTML descargable (convertible a PDF desde navegador)
   • El informe debe incluir: cabecera, ficha técnica, resultados visuales, verificación normativa, desarrollo del cálculo, conclusión justificativa, referencias normativas
   • Tabla de resistividades con 6 tipos de terreno (valores mín/típico/máx en Ω·m)
   • Tabla de secciones de conductores de protección según ITC-BT-18

5. Diseño de interfaz:
   • Estética técnica/industrial con paleta de colores profesional (azules oscuros, grises)
   • Formulario organizado en tarjetas (cards) temáticas
   • Resultados con código de colores: verde (cumple), amarillo (advertencia), rojo (no cumple)
   • Tipografías técnicas (monoespaciadas para fórmulas y valores numéricos)
   • Responsive design adaptable a móvil/tablet/escritorio
   • Grid de fondo sutil tipo blueprint técnico
   • Animaciones suaves en transiciones y botones

6. Tecnología:
   • HTML5 + CSS3 + JavaScript puro (sin frameworks, archivo único autocontenido)
   • Generación de informe HTML mediante JavaScript que construye documento completo
   • Formato listo para imprimir como PDF desde el navegador (CSS @media print optimizado)

IMPORTANTE:
• La calculadora debe priorizar SIEMPRE el umbral más restrictivo entre REBT e ITC-BT-26
• Debe mostrar claramente si el anillo solo es suficiente (mensaje: 'No se necesitan picas')
• La tabla de verificación debe evaluar REBT e ITC-BT-26 por separado, no solo el cumplimiento global
• Incluir una tercera fila en la tabla de verificación que justifique explícitamente la necesidad/no necesidad de picas
• El informe PDF debe ser profesional, con formato listo para anexar a un proyecto eléctrico oficial
─────────────────────────────────────────────────────────

```

*Nota: Este prompt ha sido diseñado para ser utilizado con modelos de lenguaje de alta capacidad (Claude Opus/Sonnet, GPT-4, etc.) que dispongan de conocimientos técnicos sobre normativa eléctrica española y sean capaces de generar código funcional completo en una sola interacción.*

---

**─── FIN DEL DOCUMENTO ───**
