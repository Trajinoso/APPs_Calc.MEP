# Documento Unificado de Requisitos Técnicos (ITR) — MEP-002

**Proyecto:** Herramienta de Cálculo de Caudales para Climatización (HVAC)

**ID del Proyecto:** MEP-002

**Disciplina:** HVAC / Ingeniería Mecánica y de Fluidos

**Tecnología:** HTML / CSS / JS (*Client-side* puro, sin librerías ni dependencias externas)

**Versión Consolidada:** v1.2 (Versión Final Aprobada)

---

## 📄 Resumen Ejecutivo

El presente archivo constituye la consolidación única y definitiva del Informe Técnico de Requisitos (ITR) para el desarrollo de la suite de herramientas **MEP-002: Cálculo de Caudales (HVAC)** y **Reporte Formal de Caudales**. Integre todas las especificaciones funcionales, operativas, termodinámicas y de maquetación acordadas a lo largo de las iteraciones de la FASE 1 y la FASE 2.

La herramienta opera exclusivamente en el lado del cliente (*client-side*), permitiendo dimensionar caudales volumétricos y másicos, predimensionar diámetros nominales (DN), estimar la potencia hidráulica de bombeo y transferir los resultados mediante un esquema JSON estructurado hacia un módulo independiente de generación de informes técnicos impresos en formato A4.

---

## 🎯 1. Alcance Funcional y Casos de Uso

### 1.1. Ámbito de Aplicación

* **Cálculo por Circuito / Servicio:** La aplicación permite definir de forma independiente múltiples circuitos térmicos en paralelo (UTAs, fancoils, intercambiadores de placas, climatizadores, etc.).
* **Suma de Caudales:** La herramienta efectúa el balance hidráulico global sumando los caudales de todos los circuitos definidos, sin aplicar factores de simultaneidad o diversidad.
* **Arquitectura Hidráulica:** Configuración para esquemas **primario-secundario** con régimen de caudal variable mediante variadores de frecuencia (VFD) y válvulas de control de 2 vías.

### 1.2. Casos de Uso Principales

* **Refrigeración:** Redes de agua fría de climatización (p. ej. producción mediante enfriadoras).
* **Calefacción:** Redes de agua caliente (calderas de condensación, bombas de calor, aerotermia).
* **Circuitos Mixtos:** Equipos con cambio estacional (ida/retorno según régimen de verano o invierno).
* **Free-Cooling:** Integración de circuitos de enfriamiento gratuito (evaluación hidráulica en el lado agua de consumo o lado agua de torre).

### 1.3. Régimen de Operación y Estabilidad

* **Caudal Mínimo de Bypass:** Entrada de valor absoluto editable ($Q_{\text{bypass min}}$ en $\text{m}^3\text{/h}$) para garantizar el caudal mínimo de circulación en la aguja hidráulica o colector de desacoplo.
* **Evaluación de Circuito Crítico:** Identificación del circuito con mayor exigencia o pérdida de carga a efectos de referencia de estabilidad hidráulica del sistema.

---

## 🧮 2. Entradas de Cálculo (Parámetros del Usuario)

### 2.1. Parámetros Globales del Sistema

* **Altura Manométrica ($H$):** Pérdida de carga agregada del circuito expresada en metros de columna de agua [m c.a.] (Valor único introducido por el usuario; $H \ge 0$).
* **Rendimiento Hidráulico ($\eta$):** Rendimiento global estimado del grupo de bombeo (Valor escalar entre $0.05$ y $0.95$; valor por defecto: $0.65$).
* **Velocidad Objetivo en Secundarios ($v_{\text{sec}}$):** Criterio de velocidad para el predimensionado de tuberías en circuitos terminales (Valor en m/s; rango recomendado: $0.8 \text{ a } 1.5 \text{ m/s}$; defecto: $1.2 \text{ m/s}$).
* **Velocidad Objetivo en Primario ($v_{\text{pri}}$):** Criterio de velocidad para el colector/línea principal (Valor en m/s; rango recomendado: $1.5 \text{ a } 2.5 \text{ m/s}$; defecto: $2.0 \text{ m/s}$).
* **Caudal Mínimo de Bypass ($Q_{\text{bypass min}}$):** Parámetro absoluto de seguridad hidráulica en $\text{m}^3\text{/h}$ (Defecto: $20 \text{ m}^3\text{/h}$).
* **URL del Logotipo:** Dirección opcional de una imagen remota para personalización corporativa en la vista de informe.

### 2.2. Parámetros por Circuito

* **Nombre:** Identificador del servicio (ej. "UTA Planta 1", "Fancoils Norte").
* **Modo Térmico:** Selección entre `Frío` o `Calor`.
* **Potencia Térmica ($P$):** Carga térmica de diseño expressada en kilowatts [$P > 0\text{ kW}$].
* **Temperatura de Ida ($T_i$):** Temperatura del fluido impulsado [°C].
* **Temperatura de Retorno ($T_r$):** Temperatura del fluido retornado [°C].
* **Marcador de Circuito Crítico:** Checkbox para identificar el circuito determinante de la red.
* **Acción "Sugerir $\Delta T$":** Asigna automáticamente parámetros típicos de diseño:
* Modo Frío: $T_i = 7.0\,^\circ\text{C}$, $T_r = 12.0\,^\circ\text{C}$ ($\Delta T = 5.0\text{ K}$).
* Modo Calor: $T_i = 50.0\,^\circ\text{C}$, $T_r = 30.0\,^\circ\text{C}$ ($\Delta T = 20.0\text{ K}$).



---

## 📐 3. Ecuaciones, Correlaciones Termodinámicas y Algoritmos

### 3.1. Balance Térmico e Hidráulico Base

El caudal volumétrico instantáneo $Q$ ($\text{m}^3\text{/s}$) en régimen permanente se determina mediante:

$$Q = \frac{P}{\rho(T_m) \cdot C_p(T_m) \cdot \Delta T}$$

Donde:

* $P$: Potencia térmica del circuito $[\text{kW}]$.
* $\Delta T = \vert{}T_i - T_r\vert{}$: Salto térmico absoluto $[\text{K} \text{ o } ^\circ\text{C}]$.
* $T_m = \frac{T_i + T_r}{2}$: Temperatura media operativa del circuito [$^\circ\text{C}$].
* $\rho(T_m)$: Densidad del fluido $[\text{kg/m}^3]$.
* $C_p(T_m)$: Calor específico del fluido $[\text{kJ/(kg}\cdot\text{K)}]$.

*Conversión de Unidades de Caudales:*


$$Q\,[\text{m}^3\text{/h}] = Q\,[\text{m}^3\text{/s}] \times 3600$$

$$Q\,[\text{l/s}] = Q\,[\text{m}^3\text{/s}] \times 1000$$

### 3.2. Caudal Másico

$$\dot{m} = \rho(T_m) \cdot Q \quad [\text{kg/s}]$$

### 3.3. Propiedades Thermofísicas del Agua Pura

Para garantizar máxima precisión sin recurrir a bibliotecas externas ni tablas discretas, se utilizan correlaciones analíticas continuas válidas en el rango de climatización ($0^\circ\text{C} \le T_m \le 100^\circ\text{C}$, con rango recomendado de operación $5^\circ\text{C} \le T_m \le 80^\circ\text{C}$):

1. **Densidad del Agua ($\rho$) — Ecuación de Kell (1975):**

$$\rho(T) = 1000 \cdot \left( 1 - \frac{(T + 288.9414) \cdot (T - 3.9863)^2}{508929.2 \cdot (T + 68.12963)} \right) \quad [\text{kg/m}^3]$$


2. **Calor Específico del Agua ($C_p$) — Polinomio Cuadrático de Ajuste:**

$$C_p(T) = 4.2176 - 0.001732264 \cdot T + 0.00001716264 \cdot T^2 \quad [\text{kJ/(kg}\cdot\text{K)}]$$



### 3.4. Algoritmo de Predimensionado de Diámetro Nominal (DN)

Basado en una velocidad objetivo $v_{\text{obj}}$ ($\text{m/s}$):

1. Cálculo de área teórica requerida: $A_{\text{req}} = \frac{Q}{v_{\text{obj}}} \quad [\text{m}^2]$
2. Diámetro interior teórico: $D_{\text{int, teor}} = \sqrt{\frac{4 \cdot A_{\text{req}}}{\pi}} \cdot 1000 \quad [\text{mm}]$
3. Selección por catálogo: La aplicación compara $D_{\text{int, teor}}$ contra la serie comercial normalizada (EN 10255 / EN 10220) asignando el primer $DN$ cuyo diámetro interior sea mayor o igual al teórico:

| DN comercial | Diámetro Interior Aprox. ($D_{\text{int}}$, mm) |
| --- | --- |
| **DN15** | 16 |
| **DN20** | 21 |
| **DN25** | 27 |
| **DN32** | 35 |
| **DN40** | 41 |
| **DN50** | 53 |
| **DN65** | 69 |
| **DN80** | 81 |
| **DN100** | 102 |
| **DN125** | 127 |
| **DN150** | 153 |
| **DN200** | 202 |
| **DN250** | 254 |
| **DN300** | 303 |

4. Recálculo de la velocidad real resultante en la conducción:

$$v_{\text{real}} = \frac{Q}{\pi \cdot \left(\frac{D_{\text{int}}}{2000}\right)^2} \quad [\text{m/s}]$$



### 3.5. Potencia Hidráulica del Sistema de Bombeo

La potencia hidráulica requerida $P_h$ ($\text{kW}$) en el punto de diseño ($Q_{\text{total}}, H$) se evalúa según:

$$P_h = \frac{\rho_{\text{nom}} \cdot g \cdot Q_{\text{total}} \cdot H}{\eta \cdot 1000} \quad [\text{kW}]$$

Donde $\rho_{\text{nom}} = 998 \text{ kg/m}^3$, $g = 9.81 \text{ m/s}^2$, $Q_{\text{total}}$ en $\text{m}^3\text{/s}$, $H$ en $\text{m c.a.}$ y $\eta$ es el rendimiento hidráulico.

---

## 📊 4. Salidas de Cálculo y Resultados Mostrados

### 4.1. Métrica Tabular por Circuito

* Nombre del servicio y Modo (`Frío` / `Calor`).
* Potencia Térmica $P$ [kW].
* Temperaturas $T_i / T_r$ [°C] y Salto Térmico $\Delta T$ [K].
* Temperatura media $T_m$ [°C].
* Densidad $\rho$ [kg/m³] y Calor Específico $C_p$ [kJ/kg·K] evaluados a $T_m$.
* Caudal Volumétrico en $\text{m}^3\text{/h}$ y en $\text{l/s}$.
* Caudal Másico $\dot{m}$ [kg/s].
* Diámetro Nominal sugerido para la red secundaria ($DN_{\text{sec}}$).
* Indicador visual de Circuito Crítico.

### 4.2. Indicadores Globales KPI

* **$\Sigma Q$ Total:** Sumatoria en $\text{m}^3\text{/h}$ y $\text{l/s}$.
* **DN Primario:** Diámetro sugerido para el colector principal en función de $v_{\text{pri}}$ y la velocidad resultante aproximada.
* **Relación de Bypass:** Proporción entre $Q_{\text{bypass min}}$ y $Q_{\text{total}}$ expresada en porcentaje (alerta visual en caso de superar el 20%).
* **Caudal Crítico:** Caudal específico del circuito marcado como crítico.
* **Potencia Hidráulica Estimada ($P_h$):** Expresada en kW asociada al punto de diseño $(Q_{\text{total}}, H)$.

---

## 🖥️ 5. Arquitectura del Software, UI y Módulo de Reportes

### 5.1. Arquitectura Técnica

* **Modalidad:** Client-side puro (*Single Page Application* en archivos HTML sin dependencias de servidor ni Node.js).
* **Lenguajes:** HTML5, CSS3 nativo (usando CSS Variables), JavaScript ES6+.
* **Dependencias:** Ninguna (Prohibida la inclusión de bibliotecas externas como React, Vue, jQuery o Bootstrap).
* **Interfaz y Tema:** Diseño receptivo (*responsive*) con paleta en Modo Oscuro de alto contraste para la aplicación de cálculo.

### 5.2. Módulo de Reportes e Impresión A4 (`reporte.html`)

* **Transferencia de Datos:** Exportación mediante un clic en la herramienta de cálculo que genera una estructura en el portapapeles en formato JSON.
* **Importación:** El usuario pega la cadena JSON en la interfaz del reporte formal para procesar los datos.
* **Recálculo de Validación:** El módulo de reporte re-ejecuta internamente las funciones de Kell y $C_p(T)$ para certificar la integridad técnica de los datos.
* **Formato A4 Estricto:** Reglas CSS `@page { size: A4; margin: 18mm 15mm 20mm 15mm; }` y directivas `page-break-before: always` para división de portadas y secciones.
* **Secciones del Reporte:**
1. Portada formal con metadatos del proyecto (Nombre, Cliente, Localización, Autor, Fecha, Revisión) y logotipo.
2. Resumen ejecutivo (Parámetros globales, caudales acumulados, colectores primarios).
3. Detalle analítico por circuito tabulado.
4. Formulación, metodología y referencias normativas.
5. Avisos legales, exención de responsabilidad y registro de versión.



### 5.3. Esquema de Intercambio de Datos (JSON Standard)

```json
{
  "id": "MEP-002",
  "version": "1.0.1",
  "global": {
    "H_mca": 15,
    "eta": 0.65,
    "v_sec_ms": 1.2,
    "v_pri_ms": 2.0,
    "Q_bypass_min_m3h": 20,
    "Q_total_m3h": 138.45
  },
  "circuits": [
    {
      "name": "Frío – UTA 01",
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

## ⚖️ 6. Normativa, Requisitos Legales y Validaciones

### 6.1. Marco Normativo Técnico (Informativo)

* **RITE (Reglamento de Instalaciones Térmicas en los Edificios):** Instrucción Técnica IT 1.2.4.2 sobre eficiencia en el transporte de fluidos y optimización del salto térmico $\Delta T$.
* **UNE-EN 12828:** Sistemas de calefacción en edificios.
* **CTE HE-2:** Rendimiento de las instalaciones térmicas.

### 6.2. Reglas de Validación

* **Bloqueantes (Error en pantalla):**
* Potencia térmica $P \le 0$.
* Salto térmico $\Delta T \le 0$ o temperaturas incoherentes ($T_i = T_r$).
* Rendimiento $\eta \le 0$ o $\eta > 0.95$.


* **Advertencias (Avisos de color/chips informativos):**
* Temperatura media $T_m < 5\,^\circ\text{C}$ o $T_m > 80\,^\circ\text{C}$ (Superación del rango recomendado para las correlaciones).
* Incoherencia de modo (ej. Modo Frío con $T_r < T_i$).
* Relación $Q_{\text{bypass min}} / Q_{\text{total}} > 20\%$.



### 6.3. Aviso Legal (*Disclaimer*)

La aplicación incluye de forma visible la siguiente cláusula obligatoria:

> *"Herramienta de apoyo al cálculo técnico. No sustituye el proyecto de ingeniería detallado ni la selección definitiva de equipos y bombas según las curvas hidráulicas del fabricante. El fenómeno de NPSH no es evaluado por este software."*

---

## 📜 7. Registro de Cambios y Control de Versiones

| Versión | Fecha | Resumen de Cambios |
| --- | --- | --- |
| **v1.0** | Marzo 2026 | Definición inicial de requisitos, preguntas de alcance funcional y propuesta de motor de cálculo. |
| **v1.1** | Julio 2026 | Consolidación de respuestas de usuario: selección de fluido agua pura, velocidad objetivo por defecto, bypass absoluto en $\text{m}^3\text{/h}$, integración de correlaciones de Kell y $C_p$ cuadrático. |
| **v1.2** | Agosto 2026 | **Versión Unificada Final.** Integración completa del informe ITR, especificación del esquema de intercambio JSON y definición del motor CSS `@page` para exportación normalizada A4. |
