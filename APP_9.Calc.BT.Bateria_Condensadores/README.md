# ⚡ Calculadora de Batería de Condensadores (Web App)

Aplicación web sencilla para el cálculo de baterías de condensadores en instalaciones eléctricas de baja tensión.

Permite introducir parámetros básicos de una instalación y obtener automáticamente:

- Potencia reactiva necesaria (Qc)
- Intensidad nominal
- Intensidad de protección
- Dimensionado básico de bancos escalonados (1:2 y 1:2:4)

---

## 🚀 Características

✅ Cálculo automático en tiempo real  
✅ Interfaz intuitiva y simple  
✅ Persistencia de datos (localStorage)  
✅ Resultados instantáneos sin recargar  
✅ Incluye teoría y normativa básica  
✅ Preparada para embeber en Notion  

---

## 🧮 Variables de entrada

| Parámetro | Descripción | Unidad |
|----------|------------|--------|
| P | Potencia activa | kW |
| cos φ1 | Factor de potencia inicial | - |
| cos φ2 | Factor de potencia objetivo | - |
| U | Tensión de línea | V |
| Coef | Coeficiente de seguridad | - |

---

## 📊 Resultados calculados

| Resultado | Descripción | Unidad |
|----------|------------|--------|
| Qc | Potencia reactiva necesaria | kVAr |
| I | Intensidad nominal batería | A |
| Iprot | Intensidad de protección | A |
| 1:2:4 | Escalonamiento clásico | kVAr |
| 1:2 | Escalonamiento simple | kVAr |

---

## ⚙️ Fórmulas utilizadas

### 1. Cálculo de ángulos
```
φ = arccos(cos φ)
```

### 2. Tangente del ángulo
```
tan φ = tan(arccos(cos φ))
```

### 3. Potencia reactiva necesaria
```
Qc = P × (tan φ1 − tan φ2)
```

### 4. Intensidad de la batería
```
I = Qc × 1000 / (√3 × U)
```

### 5. Intensidad de protección
```
Iprot = I × coeficiente de seguridad
```

### 6. Escalonamiento

- 1:2:4 → división en 7 partes:
```
Qc / 7
```

- 1:2 → división en 3 partes:
```
Qc / 3
```

---

## 📘 Fundamento teórico

La corrección del factor de potencia se realiza mediante baterías de condensadores que compensan la potencia reactiva inductiva de la instalación.

### Objetivos:

- Reducir pérdidas eléctricas
- Evitar penalizaciones en facturación
- Mejorar eficiencia energética
- Liberar capacidad en líneas y transformadores

---

## 📜 Normativa aplicable (España)

- Reglamento Electrotécnico de Baja Tensión (REBT)
- ITC-BT-47: Instalaciones receptoras
- Tarifas eléctricas: penalización por energía reactiva (cos φ < 0.95)

---

## 💾 Persistencia de datos

La aplicación guarda automáticamente los valores introducidos mediante:

```
localStorage
```

Esto permite:

✅ Mantener los últimos inputs  
✅ No perder datos al recargar  
✅ Simulación continua  

---

## 🔄 Funcionamiento interno

1. El usuario introduce valores
2. Se dispara evento `input`
3. Se guardan datos en localStorage
4. Se recalculan resultados automáticamente
5. Se actualiza la interfaz

---

## 🖥️ Uso

### Opción 1: Local
1. Descargar archivo `.html`
2. Abrir en navegador

### Opción 2: Notion

1. Subir a:
   - GitHub Pages  
   - Netlify  

2. En Notion:
```
/embed
```

3. Pegar URL

---

## 📦 Estructura del proyecto

```
/calculadora-condensadores
│
├── index.html
├── README.md
```

---

## 🧪 Validaciones implementadas

- cos φ entre 0 y 1
- Conversión automática kVAr → VAr
- Control básico de errores

---

## 🛠️ Posibles mejoras futuras

- Selección de bancos comerciales
- Gráficas dinámicas (cos φ)
- Exportación a PDF
- Cálculo trifásico avanzado
- Simulación de cargas variables
- Integración con Notion API

---

## 📝 Bitácora de versiones

### v1.0 – Base
- Inputs manuales
- Botón de cálculo

### v1.1 – Automatización
- Cálculo en tiempo real
- Eliminación de botón

### v1.2 – Persistencia
- Implementación localStorage

### v1.3 – Mejora UX
- Validaciones
- Diseño mejorado

### v1.4 – Ingeniería
- Escalonamiento 1:2 y 1:2:4
- Ajuste de fórmulas reales

---

## ⚠️ Limitaciones

- No sustituye software profesional
- Simplificación del dimensionado real
- No incluye armónicos ni filtros
- No contempla normativa detallada de protección

---

## ✅ Conclusión

Herramienta ligera y rápida para:

✔ comprobación técnica  
✔ cálculos preliminares  
✔ uso en campo o ingeniería básica  
✔ integración con Notion  

---

## 👨‍💻 Autor

Desarrollado como herramienta práctica para cálculo rápido de compensación de energía reactiva.
