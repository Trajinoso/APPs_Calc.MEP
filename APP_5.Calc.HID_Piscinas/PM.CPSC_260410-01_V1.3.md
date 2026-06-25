# MASTER PROMPT — CalcPisc v1.3
**Archivo:** PM.CPSC_260410-01_V1.3.md
**Vinculado a:** ITR.CPSC_260410-01 v1.3
**Modelo IA utilizado:** Claude Sonnet 4.6 (claude.ai)
**Fecha:** 2026-04-10

---

## ESTRUCTURA DE LA APLICACIÓN v1.3

3 pestañas:
1. Calculadora — M1 Geometría | M2 Filtración | M3 Tuberías | M4 Bomba | Panel resultados
2. Tablas de Referencia — Velocidades | Filtros normalizados | PVC PN10
3. Informe Resumen — 6 secciones, botón PDF (window.print + @media print)

## CAMBIOS v1.3 RESPECTO A v1.2

1. M4: Eliminado campo modelo comercial bomba y sus referencias JS.
2. M3: Descripción .vel-desc bajo cada input con rango normativo y criterio técnico.
3. Pestaña Tablas: Tres tablas estáticas pobladss por JS al cargar (poblarTablasFiltros, poblarTablasPVC).
4. Pestaña Informe: generarInforme(d) genera HTML dinámico en #rpt-body. @media print oculta todo excepto #tab-informe.

## ALGORITMOS (sin cambios desde v1.2)

Q_real = S_com × v_filtr × N_filtros  [m³/h]
Q_max  = S_com × v_filtr_max × N_filtros  [m³/h]
Q_bomba = Q_total / N_bombas  [m³/h]
P_motor = (rho·g·Q_bomba·HMT) / (3600·1000·eta_bomba·eta_motor)  [kW]

## CHANGELOG COMPLETO

- v1.0 (2026-04-10): Version inicial. ACEPTADO.
- v1.1 (2026-04-10): Sin perdidas de carga. HMT input. Paralelo. ACEPTADO.
- v1.2 (2026-04-10): Correccion Q_real. Tabla filtros. Selector dinamico. Q_max. ACEPTADO.
- v1.3 (2026-04-10): Sin modelo comercial. Descripciones velocidad. Pestana tablas. Pestana informe PDF. ACEPTADO.

## NORMATIVA

RD 742/2013 | CTE DB-HS4 | UNE-EN 13451 | UNE-EN 16713-1 | PVC PN10 (DN25-DN500)
