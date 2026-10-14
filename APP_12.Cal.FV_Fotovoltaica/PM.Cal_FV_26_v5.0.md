# PM.Cal_FV 26 v5.0
## Prompt Master & Registro de Auditoría

### REGISTRO DE AUDITORÍA
- **Nombre de la App:** App.Cal_FV 26
- **Versión de Código:** v5.0
- **ID ITR:** INF-20260410-08 v1.0
- **Fecha de Generación:** 2026-04-24 (Fecha contexto)
- **Tecnología:** HTML5 + CSS3 + JavaScript ES6 (Archivo único, Vanilla JS, LocalStorage).

### PROMPT MAESTRO PARA REPLICACIÓN / CONTINUACIÓN EN OTRA IA
Copia y pega el siguiente bloque en una nueva sesión de IA para continuar el desarrollo:

***

**Actúa como Ingeniero MEP Senior y Desarrollador Full-Stack.**
Te voy a adjuntar el código fuente HTML/JS/CSS de la herramienta "App.Cal_FV 26 v5.0". Es una aplicación de una sola página y sin dependencias externas para el cálculo termo-eléctrico de instalaciones fotovoltaicas multi-inversor.

**ESTADO ACTUAL DE LA LÓGICA (v5.0):**
1. Calcula tensiones en frío (T_amb_min) para Vmax_string, y tensiones en calor (T_amb_max + 25) para Vmin_string.
2. Derating Térmico y Clipping: El clipping se evalúa calculando primero la potencia real de la célula en caliente (`P_real_mod`), multiplicada por el número de módulos y por un factor de pérdidas DC (`f_loss_DC`). Esto da `P_DC_Treal`.
3. El indicador principal de diseño es el **Ratio Real DC/AC** (`P_DC_Treal / Pac_inversor`), evaluado en un semáforo de 7 tramos (desde "Sin clipping" en <= 1.00, hasta "Alarma Excesivo" en >= 1.25).
4. Cuenta con un módulo de secciones CC interactivo basado en la norma UNE-HD 60364-7-712 (I_diseño = 1.25 × Isc_max).
5. Tiene persistencia de datos implementada mediante `localStorage` con la clave `calcfv_v5_state`.

**INSTRUCCIÓN CRÍTICA:**
Acepta el código proporcionado como base inmutable. **No elimines, reescribas, ni simplifiques ninguna de las funcionalidades ya existentes** (Motor CC, Pestañas, Acordeones, Generación de Textos o PDF). Cualquier nuevo requerimiento que te pida a partir de ahora deberá integrarse de forma aditiva sobre este ecosistema, respetando estrictamente el protocolo de Informe Técnico de Requisitos (ITR) incremental previo a generar código.

Confirma que has indexado esta arquitectura y estás listo para recibir el código base y la siguiente orden.
***