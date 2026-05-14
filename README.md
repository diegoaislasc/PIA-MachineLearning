# PIA: Introducción al Aprendizaje Automático
**Modelo Predictivo de Goles Esperados (xG) en la Premier League**

## Sobre el Conjunto de Datos
Este proyecto utiliza el dataset `epl_2024_2025_match_shots.csv`. 

### Origen y Extracción
Los datos corresponden a todos los tiros (shots) realizados en los partidos de la **English Premier League (EPL) en la temporada 2024-2025**. La extracción original de estos datos fue realizada a través de procesos de **Scraping y consultas en BigQuery** orientadas a repositorios abiertos de analítica deportiva.

### Justificación de Uso (Reutilización Académica)
Este dataset fue originado y limpiado inicialmente para la materia de **Minería de Datos** (donde se estructuraron las columnas JSON como `playerCoordinates`). Dado que el conjunto de datos cuenta con variables altamente relevantes (coordenadas X/Y, partes del cuerpo usadas, situación del tiro, etc.) y un tamaño de muestra excelente (casi 9,000 registros), cumple perfectamente con los requisitos de la rúbrica de **Machine Learning**, permitiéndonos saltar la fase básica de limpieza y enfocarnos completamente en la implementación de modelos predictivos complejos (KNN, Random Forest, SVM).

### Variables Clave
* `shotType`: Resultado final del tiro (Miss, Block, Save, Goal). Binarizado a `is_goal` para el modelo.
* `playerCoordinates`: Coordenadas extraídas de JSON a variables independientes (`shot_x`, `shot_y`).
* `distance_to_goal`: Variable de ingeniería calculada matemáticamente usando las coordenadas.
* `situation` / `bodyPart`: Variables categóricas procesadas mediante One-Hot Encoding.
