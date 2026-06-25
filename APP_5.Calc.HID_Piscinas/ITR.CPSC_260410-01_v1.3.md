# CALCULADORA HIDRÁULICA DE PISCINAS — CalcPisc
**ID_INFORME:** ITR.CPSC_260410-01 | **VERSIÓN:** v1.3
**ESTADO:** ACEPTADO
**FECHA ACEPTACIÓN:** 2026-04-10

---

### 1. ESPECIFICACIONES TÉCNICAS

Normativa: RD 742/2013 | CTE DB-HS4 | UNE-EN 13451 | UNE-EN 16713-1 | PVC PN10

---

### 2. LÓGICA DE INGENIERÍA

Sin cambios en algoritmos respecto a v1.2. Cambios exclusivamente de interfaz.

**Cambio 1 — M4:** Eliminado campo modelo comercial de bomba.
**Cambio 2 — M3:** Añadida descripción inline bajo cada input de velocidad con rango y criterio.
**Cambio 3 — Pestaña Tablas:** Velocidades recomendadas, filtros normalizados, PVC PN10 con caudales de referencia.
**Cambio 4 — Pestaña Informe:** Informe dinámico generado al calcular. 6 secciones. Exportación PDF por window.print() con CSS @media print.

---

### 3. BITÁCORA DE CAMBIOS

- **v1.0 — 2026-04-10:** Informe inicial. ACEPTADO.
- **v1.1 — 2026-04-10:** Sin pérdidas de carga. HMT input. Paralelo. ACEPTADO.
- **v1.2 — 2026-04-10:** Corrección Q_real filtros. Tabla filtros UNE-EN 16713-1. Selector dinámico. Q_máx. ACEPTADO.
- **v1.3 — 2026-04-10:** Eliminación modelo comercial bomba. Descripciones velocidad M3. Pestaña tablas de referencia. Pestaña informe resumen con PDF. ACEPTADO.
