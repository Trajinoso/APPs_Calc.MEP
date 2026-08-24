# Suite de Herramientas MEP-002: Cálculo de Caudales HVAC y Generación de Informes Formales

Suite de aplicaciones web del lado del cliente (*client-side*) para el cálculo termohidráulico de redes de agua climatizada (fría y caliente) en instalaciones HVAC y la correspondiente generación de informes técnicos en formato A4 listo para impresión o exportación a PDF.

---

## 📋 Estructura de la Suite

La suite está compuesta por dos módulos desacoplados que interactúan mediante transferencia de datos en formato JSON:

1. **`MEP-002 · Cálculo de Caudales (HVAC)`** (`calculo.html`): Herramienta interactiva para la parametrización de circuitos térmicos en paralelo, cálculo de caudales volumétricos/másicos, estimación de diámetros nominales (DN) y evaluación de la potencia hidráulica de bombeo.
2. **`MEP-002 · Reporte Formal de Caudales`** (`reporte.html`): Módulo de maquetación e impresión que procesa el JSON generado por la herramienta de cálculo, añade metadatos del proyecto y compila un documento formal A4 estructurado.

---

## 🧮 Módulo 1: Cálculo de Caudales (`calculo.html`)

### Descripción General
Esta aplicación permite ingresar parámetros globales del sistema de bombeo y definir múltiples circuitos térmicos independientes operating en modo Frío o Calor. Ejecuta internamente las funciones de densidad y calor específico del agua en función de la temperatura media del fluido para mayor precisión termodinámica.

### Ecuaciones y Correlaciones Implementadas

#### 1. Ecuación Base del Caudal Volumétrico
El caudal volumétrico en cada circuito se calcula despejando el balance de energía en régimen permanente:

$$Q = \frac{P}{\rho(T_m) \cdot C_p(T_m) \cdot \Delta T}$$

Donde:
- $P$: Potencia térmica del circuito $[\text{kW}]$.
- $\rho(T_m)$: Densidad del agua $[\text{kg/m}^3]$ a la temperatura media $T_m$.
- $C_p(T_m)$: Calor específico del agua $[\text{kJ/(kg}\cdot\text{K)}]$ a la temperatura media $T_m$.
- $\Delta T = |T_i - T_r|$: Salto térmico absoluto entre ida y retorno $[\text{K}]$.
- $Q$: Caudal volumétrico resultante $[\text{m}^3\text{/s}]$. *(Conversión: $Q\,(\text{m}^3\text{/h}) = Q\,(\text{m}^3\text{/s}) \times 3600$; $Q\,(\text{l/s}) = Q\,(\text{m}^3\text{/s}) \times 1000$)*.

#### 2. Ecuaciones Termodinámicas del Agua
En lugar de considerar propiedades constantes ($\rho = 1000\text{ kg/m}^3$, $C_p = 4.186\text{ kJ/kg}\cdot\text{K}$), la aplicación evalúa $T_m = \frac{T_i + T_r}{2}$ y aplica las siguientes funciones (válidas en el rango $0^\circ\text{C}$ a $100^\circ\text{C}$):

*   **Densidad del agua (Ecuación de Kell):**
    $$\rho(T) = 1000 \cdot \left( 1 - \frac{(T + 288.9414) \cdot (T - 3.9863)^2}{508929.2 \cdot (T + 68.12963)} \right) \quad [\text{kg/m}^3]$$

*   **Calor específico (Polinomio cuadrático de ajuste):**
    $$C_p(T) = 4.2176 - 0.001732264 \cdot T + 0.00001716264 \cdot T^2 \quad [\text{kJ/(kg}\cdot\text{K)}]$$

#### 3. Caudal Másico
$$\dot{m} = \rho(T_m) \cdot Q \quad [\text{kg/s}]$$

#### 4. Pre-dimensionado de Diámetro Nominal (DN)
A partir de una velocidad de diseño $v_{\text{obj}}$ (configurada en m/s), la sección teórica requerida es:

$$A_{\text{req}} = \frac{Q}{v_{\text{obj}}} \quad [\text{m}^2] \implies D_{\text{teórico}} = \sqrt{\frac{4 \cdot A_{\text{req}}}{\pi}} \cdot 1000 \quad [\text{mm}]$$

El sistema selecciona el primer DN comercial cuya área interior satisfaga $D_{\text{int}} \ge D_{\text{teórico}}$ basándose en la serie normalizada (DN15 a DN300, EN 10255/EN 10220). Posteriormente recalcula la velocidad real del fluido:

$$v_{\text{real}} = \frac{Q}{\pi \cdot \left(\frac{D_{\text{int}}}{2000}\right)^2} \quad [\text{m/s}]$$

#### 5. Potencia Hidráulica del Sistema de Bombeo
La potencia hidráulica absorbida por el conjunto de bombeo se estima mediante:

$$P_h = \frac{\rho_{\text{nom}} \cdot g \cdot Q_{\text{total}} \cdot H}{\eta \cdot 1000} \quad [\text{kW}]$$

Donde:
- $\rho_{\text{nom}} = 998\text{ kg/m}^3$ (densidad de referencia).
- $g = 9.81\text{ m/s}^2$ (aceleración de la gravedad).
- $Q_{\text{total}}$: Sumatoria de caudales de todos los circuitos operativos $[\text{m}^3\text{/s}]$.
- $H$: Altura manométrica agregada del sistema $[\text{m c.a.}]$.
- $\eta$: Rendimiento hidráulico global del grupo de bombeo ($0 < \eta \le 0.95$).

---

## 📄 Módulo 2: Reporte Formal (`reporte.html`)

### Descripción General
Módulo diseñado para transformar los datos de cálculo crudos en un documento formal técnico normalizado en formato A4. Proporciona una interfaz para ingresar metadatos de ingeniería (Nombre del Proyecto, Cliente, Localización, Autor, Fecha y Revisión) y procesa el esquema JSON importado.

### Características Técnicas de Generación
- **Motor de Recálculo Interno:** Al cargar el JSON, el reporte vuelve a ejecutar internamente las correlaciones de Kell y $C_p(T)$ garantizando la trazabilidad de los datos presentados.
- **Formato A4 Estricto (`@page` CSS):** Incluye reglas CSS nativas para control de salto de página (`page-break-before: always`) e impresión.
- **Secciones del Informe:**
  1. Portada normalizada con metadatos e integración de logotipo corporativo vía URL.
  2. Resumen ejecutivo (Parámetros globales, Caudales totales, Línea primaria y Potencia hidráulica).
  3. Desglose tabulado por circuito (Potencia, Temperaturas, Densidad, $C_p$, Caudal volumétrico/másico, DN secundario).
  4. Metodología detallada y formulación matemática empleada.
  5. Referencias normativas (RITE IT 1.2.4.2, UNE-EN 12828, CTE HE-2).
  6. Cláusula de exención de responsabilidad (*Disclaimer*) y registro de versión.

---

## 🛠️ Instrucciones de Uso y Flujo de Trabajo

### Paso 1: Cálculo e Ingeniería de Detalle
1. Abrir `calculo.html` en un navegador web.
2. Definir los **Parámetros globales**:
   - Altura manométrica $H$ (m c.a.).
   - Rendimiento del bombeo $\eta$.
   - Velocidad objetivo en secundarios $v_{\text{sec}}$ (Recomendado: 0,8 – 1,5 m/s).
   - Velocidad objetivo en colectivo primario $v_{\text{pri}}$ (Recomendado: 1,5 – 2,5 m/s).
   - Caudal mínimo de desacoplo/bypass $Q_{\text{bypass min}}$ ($	ext{m}^3	ext{/h}$).
3. Configurar la red de circuitos térmicos:
   - Hacer clic en **"➕ Añadir circuito"**.
   - Asignar nombre, seleccionar modo (Frío/Calor), potencia térmica $P$ (kW), $T_{\text{ida}}$ y $T_{\text{retorno}}$.
   - Utilizar el botón **"🎯 Sugerir ΔT"** para aplicar saltos térmicos estándar ($7/12\,^\circ\text{C}$ en frío, $50/30\,^\circ\text{C}$ en calor).
   - Marcar el circuito crítico si procede.
4. Exportar los resultados:
   - Hacer clic en el botón **"📋 Copiar"** en la barra superior. El objeto JSON completo con los datos calculados se guardará en el portapapeles.

### Paso 2: Generación del Reporte Formal
1. Abrir `reporte.html` en el navegador.
2. Completar los campos de **Metadatos del informe** (Proyecto, Cliente, Ubicación, Autor, Revisión y URL del logo).
3. Pegar el objeto JSON obtenido en el área de texto **"JSON de resultados"**.
4. Pulsar el botón **"Cargar JSON"**. La vista previa del documento A4 se actualizará automáticamente con todos los cálculos y gráficos tabulados.
5. Hacer clic en **"Imprimir / PDF"** o presionar `Ctrl + P` / `Cmd + P`.
6. En el cuadro de diálogo de impresión del navegador:
   - Destino: **Guardar como PDF** o seleccionar impresora física.
   - Tamaño de papel: **A4**.
   - Márgenes: **Predeterminados** o **Ninguno** (el CSS gestiona el sangrado del informe).
   - Opciones: Activar **Gráficos de fondo** (*Background graphics*) para conservar la jerarquía visual del documento.

---

## 💻 Estructura del Esquema JSON de Intercambio

```json
{
  "id": "MEP-002",
  "version": "1.0.1",
  "global": {
    "H_mca": 15,
    "eta": 0.65,
    "v_sec_ms": 1.2,
    "v_pri_ms": 2,
    "Q_bypass_min_m3h": 20,
    "Q_total_m3h": 138.45
  },
  "circuits": [
    {
      "name": "Frío – UTA",
      "mode": "frio",
      "P_kW": 500,
      "Ti_C": 7,
      "Tr_C": 12,
      "dT_K": 5,
      "Tm_C": 9.5,
      "rho_kgm3": 999.78,
      "cp_kJkgK": 4.2027,
      "Q_m3h": 85.69,
      "Q_ls": 23.8,
      "mdot_kgs": 23.8,
      "critico": true
    }
  ]
}
```

---

## ⚖️ Marcos Normativos de Referencia
- **RITE (Reglamento de Instalaciones Térmicas en los Edificios):** Instrucción Técnica IT 1.2.4.2 (Eficiencia energética en el transporte de fluidos).
- **UNE-EN 12828:** Sistemas de calefacción en edificios. Diseño para instalaciones de agua caliente sanitaria y calefacción.
- **CTE HE-2:** Código Técnico de la Edificación - Rendimiento de las instalaciones térmicas.
