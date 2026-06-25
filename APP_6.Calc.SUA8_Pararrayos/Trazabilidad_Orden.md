# Registro de Auditoría de Entrada (Trazabilidad de la Orden)
**ID_PROMPT:** PM.SUA8_260624-01 | **MODELO DE IA:** Gemini Architecture (2026)

## Parámetros Críticos de Configuración Solicitados
* **Alcance del Software**: Calculadora reactiva web en archivo único HTML/JS/CSS bajo el estándar normativo del Código Técnico de la Edificación (CTE) España, Documento Básico SUA 8.
* **Lógica de Interfaz**: Configuración de entradas geométricas para obtención automática de superficies de captura equivalentes ($A_e = L \cdot W + 6H(L+W) + 9\pi H^2$), selectores dinámicos para los factores correctores $C_1$ a $C_5$ y almacenamiento automatizado en el búfer local (`localStorage`).
* **Motor Gráfico e Informe**: Integración estricta de la guía de colores corporativa provista (Elecnor Identity: `--elecnor-blue: #004687;`, `--elecnor-orange: #FF8200;`), soporte matemático con MathJax mediante CDN y estructuración limpia de CSS para forzar maquetación DIN A4 en la impresión nativa a PDF.
* **Módulo de Auto-Documentación**: Requisito ineludible de persistencia del ITR original visible en código y pantalla.