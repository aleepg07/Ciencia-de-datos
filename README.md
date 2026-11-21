# Ciencia de Datos — Preparación de Datos para Predicción y Optimización del Consumo Energético

Repositorio correspondiente al **Taller Práctico II: Preparación de datos para la implementación de modelos de predicción y Optimización del Consumo Energético** de la Especialización en Inteligencia Artificial de la Universidad Pedagógica y Tecnológica de Colombia (UPTC).

---

## 📌 Contexto del Problema

La **UPTC** desarrolla sus actividades académicas y administrativas en cuatro sedes principales en el departamento de Boyacá:
* **Tunja**
* **Duitama**
* **Sogamoso**
* **Chiquinquirá**

Cada sede presenta dinámicas energéticas diferenciadas distribuidas en cinco sectores clave:
1. Comedores
2. Salones
3. Laboratorios
4. Auditorios
5. Oficinas

El objetivo central es diseñar y ejecutar un pipeline integral de preparación, limpieza, análisis exploratorio y transformación de datos para estructurar un dataset limpio, consistente y libre de sesgos, óptimo para el entrenamiento de modelos de Machine Learning orientados a la predicción y optimización del consumo eléctrico.

---

## 📁 Estructura del Repositorio

```text
├── Archivo_clase.ipynb             # Cuaderno principal con el pipeline completo (Fases I, II, III y IV)
├── Archivo_clase_backup.ipynb      # Copia de respaldo previa a las actualizaciones
├── consumos_uptc.csv               # Dataset crudo multi-sede (275,387 registros horarios × 26 columnas)
├── consumos_duitama_preparado.csv  # Dataset final limpio y transformado para modelado (68,487 filas × 20 columnas)
├── intrucciones.md                 # Especificaciones técnicas y requerimientos metodológicos del taller
├── .gitignore                      # Configuración de exclusiones para Git
└── README.md                       # Documentación general del repositorio
```

---

## 🔬 Metodología Implementada

El proyecto se desarrolla en cuatro fases metodológicas estructuradas:

### Fase I: Carga y Exploración Inicial de Datos
* Verificación de dimensiones globales (275,387 registros × 26 variables).
* Inspección de tipos de datos y balance multi-sede (~25% de registros por campus).
* Análisis descriptivo global y detección inicial de anomalías físicas.

### Fase II: Limpieza y Depuración Inicial
* Filtrado y aislamiento de la sede **Duitama** (68,862 observaciones).
* Eliminación de 155 registros físicamente anómalos (valores negativos en `energia_total_kwh` y `co2_kg`).
* Depuración de 220 registros duplicados temporales por `timestamp` (reteniendo el 99.46% de los datos originales, 68,487 horas).

### Fase III: Análisis Exploratorio de Datos (EDA)
* Análisis estadístico y distribucional de los sectores de consumo eléctrico (laboratorios como mayor consumidor con mediana de 3.14 kWh y picos de 39 kWh).
* Visualización de asimetrías mediante histogramas y curvas KDE.
* Imputación de valores faltantes mediante estimadores robustos: mediana para variables numéricas y moda para variables categóricas (0 nulos restantes).
* Matriz de correlación lineal y diagnóstico de colinealidad.

### Fase IV: Preparación para Modelado Predictivo
* **Selección Técnica de Variables:** Definición de `energia_total_kwh` como target ($y$) y selección de 11 variables predictoras exógenas ($X$). Se excluyeron deliberadamente `co2_kg`, `potencia_total_kw` y submedidores para evitar fuga de información (*data leakage*) y colinealidad.
* **Estandarización Categórica:** Corrección de 8 variantes tipográficas en `periodo_academico` a las 4 categorías institucionales canónicas (`semestre_1`, `semestre_2`, `vacaciones_mitad`, `vacaciones_fin`).
* **Codificación:** Conversión binaria de indicadores booleanos, One-Hot Encoding (`drop_first=True`) y codificación cíclica mediante funciones sinusoidales ($\sin$ y $\cos$) para `hora` (24h) y `mes` (12m).
* **Escalamiento:** Normalización z-score (`StandardScaler`) sobre variables continuas (`temperatura_exterior_c`, `ocupacion_pct`).
* **Partición Train / Test:** División cronológica secuencial (80% entrenamiento = 54,789 registros, 20% prueba = 13,698 registros) para prevenir sesgo de anticipación (*lookahead bias*).
* **Exportación:** Generación del archivo consolidado `consumos_duitama_preparado.csv`.

---

## 🚀 Requisitos y Ejecución

El cuaderno [Archivo_clase.ipynb](Archivo_clase.ipynb) está diseñado para ejecutarse tanto en **Google Colab** como en un entorno local de Python.

### Dependencias Principales:
* `python >= 3.9`
* `pandas`
* `numpy`
* `matplotlib`
* `seaborn`
* `scikit-learn`
* `scipy`

---

## 👤 Autor
* **aleepg07** — Universidad Pedagógica y Tecnológica de Colombia (UPTC)
