# PIA: Introducción al Aprendizaje Automático
**Modelo Predictivo de Goles Esperados (xG) en la Premier League**

## Sobre el Conjunto de Datos
Este proyecto utiliza el dataset `epl_2024_2025_match_shots.csv` (10MB) que contiene aproximadamente 9,000 registros correspondientes a **todos los tiros realizados en los partidos de la English Premier League (EPL)** durante la actual temporada.

### Arquitectura de Extracción de Datos (Scraping a BigQuery)
El dataset no fue simplemente descargado de Kaggle; es el resultado de un pipeline de **Data Engineering (MLOps)** desarrollado previamente. La lógica de extracción funciona de la siguiente manera:

1. **Scraping automatizado:** Utilizando la librería de Python `ScraperFC` y utilidades personalizadas, se implementó un script (`ingest_match_shots.py`) que primero recupera todos los IDs de los partidos de la temporada 24/25 de la API oculta de **Sofascore**.
2. **Extracción iterativa:** El script itera sobre cada `match_id` y extrae un JSON anidado con la telemetría de cada tiro (coordenadas X/Y, parte del cuerpo, tipo de jugada, tiempo exacto y xG base).
3. **Carga a Data Warehouse:** Una vez limpios y estructurados en un DataFrame de Pandas, los datos son cargados automáticamente a **Google BigQuery** (GCP) como una tabla centralizada para la creación de reportes y consumo en modelos.
4. **Descarga en CSV:** Para los propósitos académicos locales de este proyecto, se extrajo una instantánea de la tabla de BigQuery en formato CSV.

### Justificación de Reutilización (De Minería de Datos a Machine Learning)
Toda esta arquitectura y limpieza inicial de la data cruda se llevó a cabo para acreditar la materia de **Minería de Datos**. 

Sin embargo, debido a la inmensa riqueza matemática del dataset (especialmente por la alta dimensionalidad geométrica de las variables espaciales `playerCoordinates`), resulta el escenario perfecto para aplicar las técnicas exigidas por el PIA de **Machine Learning**. Reutilizar este recurso nos permite dar por superada la etapa básica de recolección de datos y concentrarnos enteramente en la construcción de **Modelos Predictivos Avanzados (KNN, Random Forest, SVM)** y lidiar con retos complejos como el **desbalance de clases** (los goles representan apenas el 10% de los tiros totales).

### Variables Clave para el Modelado Predictivo
* `shotType`: Nuestra variable a predecir. Fue binarizada a `is_goal` (1 para Gol, 0 para fallos, atajadas o bloqueos).
* `playerCoordinates`: Las coordenadas brutas (JSON) fueron extraídas mediante expresiones regulares en `shot_x` y `shot_y`.
* `distance_to_goal`: Una característica (feature) de ingeniería calculada euclidianamente usando la posición estándar de las porterías.
* `situation` / `bodyPart`: Transformadas vía One-Hot Encoding para uso en algoritmos basados en distancias (como SVM).
