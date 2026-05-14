# Plan de Implementación: PIA de Machine Learning (Modelo xG - Expected Goals)

**Objetivo del Proyecto:** Predecir si un tiro terminará en gol (Clasificación Binaria: Gol vs. No Gol) utilizando el dataset `epl_2024_2025_match_shots.csv` de la Premier League.
**Estrategia:** Reciclar el preprocesamiento de la clase de Minería de Datos e incorporar modelos de ML robustos (KNN, Random Forest, SVM) y Algoritmos Genéticos para cumplir con la excelencia esperada en el reporte.

---

## Fase 1: Configuración y Preprocesamiento de Datos (Reciclaje)
Esta fase aprovechará el código que ya escribiste en tus Prácticas 1 y 2.
1. **Carga del Dataset:** Leer `epl_2024_2025_match_shots.csv`.
2. **Definición del Target:** Crear la variable objetivo binaria (ej. `is_goal`). Si en tu dataset existe una columna indicando el resultado del tiro, la binarizamos (1 para Gol, 0 para lo demás).
3. **Limpieza de Datos:**
   - Eliminar columnas irrelevantes (ej. IDs, nombres de jugadores, minutos exactos si no aportan valor directo al modelo geométrico).
   - Manejo de valores nulos (Imputación por media/mediana o eliminación).
4. **Codificación de Variables Categóricas:** Usar `LabelEncoder` o `OneHotEncoder` para variables como la pierna del tiro (derecha/izquierda/cabeza) o situación de juego (tiro libre, jugada regular).
5. **Escalado de Datos:** Aplicar `StandardScaler` o `MinMaxScaler` a las variables numéricas (distancia a la portería, ángulo de tiro). *Paso crucial para que el modelo KNN y SVM funcionen correctamente.*

## Fase 2: Selección de Características (Feature Selection) con Algoritmos Genéticos
Dado que tienes el archivo `ga_feature_selection.py` abierto, integraremos esta técnica avanzada para darle un nivel superior a tu reporte.
1. **Definición de la Función de Aptitud (Fitness Function):** El algoritmo genético evaluará subconjuntos de variables. La aptitud será el rendimiento (ej. F1-score) de un modelo rápido (como Decision Tree o Logistic Regression) usando solo ese subconjunto de variables.
2. **Ejecución del GA:** Correr el algoritmo para encontrar las "N" mejores características (por ejemplo, descubrir que la distancia y el tipo de pase son más importantes que el clima).
3. **Reducción de Dimensionalidad:** Aplicar el mejor subconjunto de características al dataset final que se usará para entrenar los modelos principales.

## Fase 3: Modelo Base (Baseline) - KNN
Se usará el modelo KNN de tu Práctica 6 como punto de referencia.
1. **División de Datos:** Split del dataset en Entrenamiento (80%) y Prueba (20%).
2. **Entrenamiento KNN:** Entrenar el clasificador K-Nearest Neighbors.
3. **Evaluación Base:** Extraer métricas:
   - *Accuracy* (Exactitud).
   - *Matriz de Confusión*.
   - *F1-score* (Crítico, ya que los tiros que son gol suelen ser una clase minoritaria o desbalanceada respecto a los tiros fallados).

## Fase 4: Modelos Avanzados (Random Forest y SVM)
Implementaremos los modelos que estudiaste en tu NotebookLM ("Machine Learning").
1. **Random Forest Classifier:**
   - Entrenamiento con el dataset optimizado.
   - Es un algoritmo de ensamble ideal para capturar relaciones no lineales (ej. combinación de ángulo cerrado + pierna mala).
2. **Support Vector Machine (SVM):**
   - Entrenamiento utilizando un kernel adecuado (RBF o Polinómico).
   - *Opcional:* Si el dataset es muy grande, usar una muestra o calibrar bien los hiperparámetros.
3. **Ajuste de Hiperparámetros (Hyperparameter Tuning):** Si el tiempo lo permite, usar `GridSearchCV` (tema de tus apuntes) para encontrar los mejores parámetros para el Random Forest y SVM.

## Fase 5: Comparación de Resultados y Evaluación Final
1. **Generación de Tabla Comparativa:** Consolidar las métricas de KNN, Random Forest y SVM en una tabla clara.
2. **Análisis de Desempeño:** Identificar qué modelo logró identificar mejor los "Goles" (Recall de la clase positiva) y cuál tuvo mejor balance global (F1-score).
3. **Interpretabilidad (Opcional):** Para el Random Forest, generar el gráfico de "Importancia de Variables" (Feature Importance) y comparar si coincide con lo que eligió el Algoritmo Genético en la Fase 2.

## Fase 6: Documentación y Reporte (Formato PIA)
1. **Creación del Jupyter Notebook Final:** Consolidar el código limpio (Markdown y celdas de código) documentando el paso a paso ("Storytelling").
2. **Redacción de Conclusiones Técnicas:** Justificar el modelo ganador basándose en la métrica elegida y discutir las limitaciones (por ejemplo, si faltaba el dato de la "posición del portero" en el dataset).
3. **Preparación de Presentación/Video:** Elaborar las diapositivas con las gráficas resultantes y enfocarse en *los hallazgos* (ej. "El modelo SVM logró predecir goles con 85% de precisión, confirmando que la distancia y el ángulo son los determinantes principales del xG, superando por 15 puntos al modelo KNN").
