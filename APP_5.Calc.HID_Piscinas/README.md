# CalcPisc Multi-Vaso v1.4 — Calculadora Hidráulica de Piscinas

> **Herramienta de cálculo, dimensionamiento y justificación hidráulica para proyectos de piscinas multi-vaso conforme a la normativa española y europea.**  
> *Referencia técnica: ITR.CPSC_260410-01 v1.4*

---

## 📋 Normativa Técnica Aplicada

* **RD 742/2013**: Criterios técnico-sanitarios de las piscinas (tiempos máximos de renovación, velocidad máxima en sumideros $\le 0,5\text{ m/s}$ contra atrapamiento).
* **CTE DB-HS4**: Código Técnico de la Edificación — Suministro y evacuación de agua (rangos de velocidades de diseño en tuberías).
* **UNE-EN 13451**: Equipamiento para piscinas (requisitos de seguridad hidráulica y elementos empotrados).
* **UNE-EN 16713-1**: Sistemas de filtración para piscinas (velocidades superficiales de diseño y diámetros comerciales normalizados).
* **UNE-EN ISO 1452 / PVC PN10**: Serie de tuberías de policloruro de vinilo no plastificado para conducciones de agua a presión.

---

## 🚀 Módulos de Cálculo Hidráulico

1. **M1 — Geometría y Volúmenes**:
   * Cálculo del volumen del vaso principal ($V_{\text{vaso}} = L \times A \times h_{\text{med}}$).
   * Vaso de compensación para piscinas desbordantes ($V_{\text{comp}}$).
   * Caudal de recirculación simultáneo según tiempo de renovación normativo ($Q_{\text{total}} = V_{\text{total}} / T_{\text{renov}}$).
   * Tipologías admitidas: Privada/Residencial, Pública/Polivalente, Infantil/Chapoteo, Deportiva/Competición, Spa/Hidromasaje.

2. **M2 — Sistema de Filtración**:
   * Cálculo de superficie filtrante mínima necesaria ($S_{\text{tot}} = Q / v_{\text{filtr}}$).
   * Selector automático y manual de diámetros comerciales normalizados (**Ø400 mm hasta Ø1800 mm**).
   * Verificación de velocidad real de paso y caudal admisible máximo ($Q_{\text{máx}}$).
   * Alertas automáticas de filtro suficiente o infradimensionado.

3. **M3 — Red de Tuberías PVC PN10**:
   * Dimensionamiento tramo por tramo:
     * *Aspiración principal* ($0,8\text{ – }1,5\text{ m/s}$)
     * *Aspiración unitaria por bomba*
     * *Impulsión principal* ($1,5\text{ – }2,5\text{ m/s}$)
     * *Colector / batería de válvulas de filtros* ($1,2\text{ – }2,5\text{ m/s}$)
     * *Recogida superficial / rebosadero* ($0,3\text{ – }1,0\text{ m/s}$)
     * *Lavado de filtros a contracorriente* ($2,0\text{ – }3,0\text{ m/s}$)
     * *Sumidero de fondo de seguridad* ($\le 0,5\text{ m/s}$)
   * **Selección y personalización de DN comercial**: El software propone el diámetro comercial PVC PN10 recomendado por cálculo automático (**DN25 a DN500**), pero **permite al técnico modificar manualmente el DN de cualquier tramo**, recalculando al instante el diámetro interior ($DI$), la velocidad real ($v_{\text{real}}$) y el estado de cumplimiento (**OK / Revisar / Fuera de rango**).
   * **Sumideros de fondo por normativa**: Se dimensionan siempre con un **mínimo de 2 unidades** separadas **como mínimo 2 metros entre ejes/centros** según RD 742/2013 y UNE-EN 13451, con velocidad límite $\le 0,5\text{ m/s}$ contra riesgo de atrapamiento. Boquillas de impulsión calculadas según caudal unitario de diseño.

4. **M4 — Grupo de Bombeo en Paralelo**:
   * Reparto de caudal entre bombas en paralelo ($Q_{\text{bomba}} = Q_{\text{total}} / N$).
   * Potencia hidráulica ($P_{\text{hid}}$) y potencia absorbida del motor en **kW** y **CV** considerando rendimientos de bomba ($\eta_b$) y motor ($\eta_m$).
   * Comprobación de compatibilidad hidráulica bomba-filtro ($Q_{\text{bomba}} \le Q_{\text{máx\_filtro}}$).

5. **Cuadro General Comparativo Multi-Vaso**:
   * Vista consolidada con el volumen total de la instalación, caudal conjunto de recirculación, superficie total de lecho filtrante y potencia eléctrica instalada.

---

## 📁 Modos de Uso y Organización de Proyectos

Tienes **3 formas de trabajar**, según cómo organice los archivos tu oficina técnica:

### MODO 1: Archivo Autónomo Todo en Uno (`.html`) por Carpeta de Proyecto
*Es la opción más directa: no necesita instalar Node.js, no necesita servidores y funciona sin internet.*

1. **Obtener la plantilla base**:
   * En la barra superior, pulsa **Exportar** $\rightarrow$ **Archivo HTML Autónomo (`.html`)**.
   * Guarda ese archivo en una carpeta de plantillas como `CalcPisc_Plantilla_Base.html`.

2. **Organización en carpetas**:
   Copia el archivo dentro de la carpeta del proyecto correspondiente:
   ```text
   📁 Proyectos_MEP/
   └── 📁 2026_EXP-042_Piscina_Residencial/
       ├── 📄 CalcPisc_EXP-042.html    <-- Doble clic directo
       ├── 📄 EXP-042_Piscina.json     <-- Copia de seguridad JSON
       ├── 📄 Planos.dwg
       └── 📄 Memoria.pdf
   ```

3. **Aislamiento total entre proyectos**:
   * Cada archivo HTML autónomo genera su propio **Identificador Único (`projectId`)**.
   * Esto garantiza que abrir y modificar `Proyecto_A.html` **NUNCA modifique ni sobreescriba** los datos de `Proyecto_B.html`, aunque ambos estén abiertos a la vez en el mismo navegador.

4. **Guardar cambios en el archivo físico**:
   * Al pulsar **`💾 Guardar este HTML`**:
     * En navegadores modernos (Chrome, Edge) con soporte de la *File System Access API*, se abre el diálogo para seleccionar el archivo original y **sobrescribirlo directamente** con 1 clic.
     * En otros navegadores, descarga una copia actualizada con los datos incrustados.

---

### MODO 2: Flujo con Archivo de Datos JSON por Proyecto (`.json`)
*Flujo recomendado para oficinas técnicas con servidores compartidos, copias de seguridad o control de versiones.*

1. En la carpeta de cada proyecto se guarda únicamente el archivo de datos:
   ```text
   📁 2026_Polideportivo/
   └── 📄 EXP-2026-01_Polideportivo_San_Fernando.json
   ```

2. **Carga y guardado**:
   * Pulsa **`📂 Cargar JSON`** para abrir los datos de cualquier proyecto guardado en esa carpeta.
   * Al terminar, pulsa **`💾 Guardar JSON [REF]`** y la aplicación descargará el archivo nombrado automáticamente con la referencia y nombre del proyecto:
     `[CODIGO]_[Nombre_Proyecto].json` (ejemplo: `EXP-2026-01_Piscina_Privada.json`).

---

### MODO 3: Ejecución en Local con Node.js en tu PC
*Ideal si deseas disponer de la aplicación completa con React, TypeScript y Tailwind CSS en tu entorno local.*

#### Requisitos Previos
* **Node.js** (versión 20 LTS o superior) instalado desde [nodejs.org](https://nodejs.org).

#### Instalación y Puesta en Marcha
1. Abre un terminal en la raíz de la carpeta del proyecto y ejecuta:
   ```bash
   npm install
   ```

2. Arranca el servidor de desarrollo:
   ```bash
   npm run dev
   ```

3. Abre en tu navegador:
   ```text
   http://localhost:3000
   ```

4. Para compilar una versión optimizada para producción:
   ```bash
   npm run build
   npm run preview
   ```

---

## 📊 Formatos de Exportación Disponibles

| Formato | Botón | Uso Principal |
| :--- | :--- | :--- |
| **JSON (`.json`)** | `⬇ JSON` / `Guardar JSON` | Portabilidad total de datos, copias de seguridad y reapertura instantánea. Nombrado automático `[CODIGO]_[NOMBRE].json`. |
| **HTML Autónomo (`.html`)** | `Archivo HTML Autónomo` | Aplicación ejecutable autocontenida para mover de carpeta en carpeta con doble clic. |
| **CSV (`.csv`)** | `⬇ CSV` | Tabla estructurada con codificación UTF-8 BOM para importar directamente a **Excel** o **LibreOffice Calc** (mediciones, presupuestos). |
| **PDF / Impresión** | `🖨 Imprimir / PDF` | Memoria técnica con diseño oficial de ingeniería para visado colegial o entrega al cliente. Permite elegir entre imprimir el vaso activo o el informe consolidado de todos los vasos. |

---

## ❓ Preguntas Frecuentes (FAQ)

### ¿Por qué antes se mezclaban los datos de dos archivos HTML distintos?
En los navegadores web, todos los archivos abiertos mediante `file:///` comparten el mismo dominio local. Si dos archivos no tenían código de proyecto o usaban la misma clave de almacenamiento, compartían el `localStorage`. **Esto ha quedado completamente resuelto**: ahora cada archivo HTML autónomo genera y conserva un identificador único aleatorio e independiente (`projectId`), aislando los datos de cada proyecto de forma definitiva.

### ¿Se puede usar sin conexión a internet?
**Sí, al 100%.** Toda la lógica matemática, fórmulas hidráulicas, tablas de diámetros de PVC PN10 y filtros normalizados están programadas de forma nativa en JavaScript sin dependencias externas.

### ¿Puedo tener un proyecto con 4 o 5 piscinas diferentes?
**Sí.** Puedes añadir tantos vasos como requiera la instalación mediante el botón **`+ Añadir Vaso`** (por ejemplo: Vaso Polivalente, Vaso de Chapoteo Infantil, Vaso de Hidromasaje y Piscina de Saltos). La pestaña **Comparativa Global** y el **Informe PDF** consolidarán automáticamente toda la instalación.
