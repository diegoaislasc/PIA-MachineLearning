# PIA: Introducción al Aprendizaje Automático
**Modelo Predictivo de Goles Esperados (xG) en la Premier League**

## Sobre el Conjunto de Datos
Este proyecto utiliza el dataset `epl_2024_2025_match_shots.csv` (10MB) que contiene aproximadamente 9,000 registros correspondientes a **todos los tiros realizados en los partidos de la English Premier League (EPL)** durante la actual temporada.

### Origen de los Datos: Proyecto Personal (SportsAnalytics)
La obtención de estos datos no proviene de repositorios genéricos, sino de un esfuerzo de **Data Engineering (MLOps)** desarrollado inicialmente en el repositorio personal [SportsAnalytics](https://github.com/diegoaislasc/SportsAnalytics). 
La arquitectura técnica subyacente para construir este conjunto de datos consistió en:
1. **Scraping automatizado:** Uso de la librería de Python `ScraperFC` para extraer IDs de los partidos de la API oculta de **Sofascore**.
2. **Extracción y anidación:** Iteración sobre los IDs para extraer la telemetría masiva de cada tiro y jugada.
3. **Carga a Data Warehouse:** Consolidación de la información en una tabla centralizada en **Google BigQuery** (GCP).
De esa base de datos robusta de BigQuery, se extrajo una instantánea en CSV para trabajar localmente.

### Escalamiento Académico (Minería de Datos a Machine Learning)
Este dataset de telemetría deportiva pasó por distintas fases académicas para llegar a su madurez actual:

1. **Aportación de la materia de Minería de Datos:**
   Durante las prácticas de dicha materia, los datos brutos extraídos del proyecto personal fueron sometidos a procesos de transformación. Se utilizaron técnicas de *Data Cleaning* y manejo de calidad de datos (como el desempaquetado de las estructuras JSON de `playerCoordinates` para obtener columnas como `shot_x` y `shot_y`, y conversiones de variables de fecha/hora). Esto nos brindó un dataset estructurado.

2. **Aplicación para el PIA de Machine Learning (Propósito actual):**
   Gracias a que el dataset heredado cuenta con características tabulares de alta dimensionalidad espacial y categórica, resulta el recurso perfecto para aplicar las metodologías centrales de esta materia. Haber superado la etapa de recolección y limpieza nos permite enfocarnos al 100% en:
   * Construcción de **Modelos Predictivos Avanzados (KNN, Random Forest, SVM)**.
   * Manejo estratégico del **desbalance de clases** (los goles representan ~10% de los datos).
   * Análisis de la importancia geométrica de las variables para definir la frontera de decisión no lineal de un *Expected Goal* (xG).

### Variables Clave para el Modelado Predictivo
* `shotType`: Nuestra variable a predecir. Fue binarizada a `is_goal` (1 para Gol, 0 para fallos, atajadas o bloqueos).
* `playerCoordinates`: Convertidas desde JSON a variables independientes (`shot_x`, `shot_y`).
* `distance_to_goal`: Característica de ingeniería calculada de manera euclidiana.
* `situation` / `bodyPart`: Transformadas vía One-Hot Encoding para su asimilación en algoritmos clasificatorios.
