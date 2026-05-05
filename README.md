# Análisis del Estrés Crediticio y su Correlación con el Comportamiento de Gasto
### Usuarios de 25-34 años

---

## DESCRIPCIÓN

Este proyecto investiga cómo el **estrés crediticio** (experiencias de insolvencia, preocupación financiera, percepciones de sobreendeudamiento) está correlacionado con cambios en el **comportamiento de gasto** de adultos jóvenes mexicanos.

- Identificación temprana de usuarios en riesgo financiero
- Diseño de productos financieros adaptados a jóvenes (25-34)
- Comprensión de dinámicas de endeudamiento irresponsable
- Modelado predictivo para intervenciones preventivas

---

## OBJETIVOS

### Objetivo General
Analizar la correlación entre estrés crediticio y comportamiento de gasto, identificando mecanismos causales y predecir el riesgo de estrés crediticio alto en usuarios de 25-34 años.

### Objetivos Específicos

1. **SEM (Structural Equation Modeling)**
   - Modelar las rutas causales: Insuficiencia ingresos → Estrés → Acciones de coping → Cambios de gasto
   - Cuantificar efectos directos e indirectos
   - Validar ajuste del modelo teórico

2. **Random Forest**
   - Predecir probabilidad de estrés crediticio alto (clasificación multinomial)
   - Identificar variables más predictivas
   - Crear scoring operacional para identificar usuarios en riesgo

3. **Integración**
   - Comparar insights teóricos (SEM) vs predictivos (RF)
   - Generar recomendaciones

---

## DATOS

**Fuente:** ENSAFI 2023 (Encuesta Nacional de Salud Financiera) - INEGI y CONDUSEF

**Muestra:**
- Usuarios de 25-34 años
- N: 4070 casos

**Tablas utilizadas:**
- TMODULO: Características personales, deuda, estrés, gasto, coping behaviors
- TSDEM: Sociodemografía (edad, sexo)


---

## ESTRUCTURA DEL PROYECTO

```
credit-stress-analysis/
├── README.md                          # Este archivo
├── requirements.txt                   # Dependencias Python
├── config.yaml                        # Configuración (paths, params)
│
├── data/
│   ├── raw/
│   │   ├── TMODULO.csv                # Datos originales
│   │   └── TSDEM.csv                
│   ├── processed/
│   │   ├── df_csa.csv                 # Datos limpios filtrados
│   │   └── csa_features.csv           # Variables derivadas
│   └── data_dictionary.md             # Documentación de variables
│
├── src/
│   ├── __init__.py
│   ├── data_loader.py                # Cargar y filtrar datos
│   ├── feature_engineering.py        # Crear variables derivadas
│   ├── rf_model.py                   # Training Random Forest
│   ├── visualization.py              # Gráficas y diagramas
│   └── utils.py                      # Funciones auxiliares
│
├── notebooks/
│   ├── 01_data_preparation.ipynb     # Carga, limpieza, EDA
│   ├── 02_feature_engineering.ipynb  # Crear variables derivadas
│   ├── 03_random_forest_model.ipynb  # Entrenamiento y validación RF
│
├── outputs/
│   ├── figures/
│   │ 
│   ├── models/
│   │   
│   └── reports/
│
└── tests/               
```

---

## REQUISITOS

### Python

```bash
pip install -r requirements.txt
```

### Librerías:
- `pandas` - Manipulación de datos
- `numpy` - Operaciones numéricas
- `semopy` - Structural Equation Modeling
- `scikit-learn` - Random Forest + métricas
- `matplotlib`, `seaborn` - Visualización
- `pyyaml` - Configuración
- `networkx` - Grafos (diagrama causal)

---

## FLUJO DE TRABAJO

### 1. **Preparación de Datos** (Notebook 01)
```
TMODULO.csv → Filtrar edad → Crear variables → csa_features.csv
```

### 2. **Exploración** (Notebook 02)
```
Distribuciones → Correlaciones → Valores faltantes → Validaciones
```

### 3. **Modelado SEM** (Notebook 03) 
```
Definir rutas causales → Estimar parámetros → Validar ajuste → Diagnosticar
```

**Modelo teórico:**
```
Insuficiencia ingresos (P6_9) ─┐
Deuda percibida (P6_8) ───────┼──→ Estrés crediticio (latente)
Tarjeta de banco (P6_6_2) ────┘
                                  ↓
                            Acciones coping (P6_10_*)
                                  ↓
                            Cambios gasto (P6_11_*)
                                  ↓
                            Atrasos (P6_7)
```

### 4. **Modelado RF** (Notebook 04)
```
Preparar X, y → Train/Test split → Cross-validation → Tunear → Feature importance
```

### 5. **Análisis Integrado** (Notebook 05)
```
Comparar SEM vs RF → Identificar patrones comunes → Generar insights
```

### 6. **Reporte** (Notebook 06)
```
Recomendaciones operacionales → Scoring de riesgo 
```

---

## VARIABLES

### Variable Dependiente
**estres_crediticio**: Índice ordinal (0=Bajo, 1=Moderado, 2=Alto)

Construcción:
```
BAJO (0): P8_4 ∈ [0,3] AND P6_7=2 AND P6_8 ∈ [3,4,5]
MODERADO (1): P8_4 ∈ [4,6] OR (P6_7=1 AND P6_8 ∈ [2,3])
ALTO (2): P8_4 ∈ [7,10] OR (P6_7=1 AND P6_8=1)
```

### Predictores Principales
- **Situación crediticia:** P6_6_2, P6_7, P6_8, P6_9
- **Coping:** P6_10_1 a P6_10_8 (acciones tomadas)
- **Cambios gasto:** P6_11_1 a P6_11_4
- **Psicológico:** P7_11_1, P7_11_4
- **Capacidad pago:** P5_19, P5_21, ingresos
- **Demográficas:** EDAD, SEXO

---

## MÉTRICAS 

### SEM
- **GFI** (Goodness of Fit Index): 
- **CFI** (Comparative Fit Index): 
- **RMSEA** (Root Mean Square Error of Approximation): 
- **SRMR** (Standardized Root Mean Square Residual): 

### Random Forest
- **Accuracy**: 
- **Precision (estrés alto)**: 
- **Recall (estrés alto)**: 
- **F1-score**: 
- **AUC-ROC**: 

---

## NOTAS METODOLÓGICAS

1. **Variable objetivo multinomial (3 clases)** - No es binaria por diseño
2. **SEM es exploratorio** 
3. **RF es predictivo** 
4. **Complementariedad** - SEM da "por qué", RF da "quién"
5. **Segmento específico** - 25-34 años (acceso a crédito reciente)

---

## Autor

**Ana Flores** - Data Scientist | Física 

---

## Versión

v1.0 - 2026
