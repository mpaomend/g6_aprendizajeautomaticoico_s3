# Aprendizaje No Supervisado 
## 📑 Índice
1. [📂 Descripción y Estructura del Proyecto](#1---descripción-y-estructura-del-proyecto)
2. [💻 Entorno de Trabajo](#2---entorno-de-trabajo-y-librerías-utilizadas)
3. [🔍 Análisis Exploratorio de Datos (EDA)](#3---análisis-exploratorio-de-datos-eda)
4. [⚙️ Preprocesamiento](#4--️-preprocesamiento)
5. [🤖 Implementación de Modelos](#5---implementación-de-modelos)
   1. [🛠️ Implementación de K means](#51--️-implementación-de-k-means)
   2. [🛠️ Implementación de DBSCAN](#52--️-implementación-de-dbscan)
6. []

## 1.- 📂 Descripción del proyecto
El equipo de Marketing está preparando una campaña publicitaria para productos relacionados con la inteligencia artificial y nos ha solicitado que les proporcionemos palabras clave que optimicen sus búsquedas en Google.

Para esto hemos utilizado el dataset: https://www.kaggle.com/datasets/devraai/ai-google-search-keyword-performance

Este dataset nos proporciona información sobre el rendimiento de palabras clave relacionadas con la inteligencia artificial en las búsquedas de Google. Este dataset es útil para analizar tendencias de búsqueda y evaluar la popularidad de términos específicos en el ámbito de la IA.

#### Objetivo General
Agrupar términos de búsqueda (Search terms) en clusters según su rendimiento publicitario, para ayudar al equipo de marketing a:
- Detectar segmenos de alto rendimiento
- Identificar términos costosos
- Optimizar la asignación de presupuesto

## 2.- 💻 Entorno de Trabajo y librerías utilizadas

Este proyecto fue desarrollado con Python en Google Colab

**Librerías utilizadas**

- **pandas**: para la carga, exploración y manipulación estructurada de los datos.
- **numpy**: soporte para operaciones matemáticas y manejo eficiente de arreglos numéricos.
- **matplotlib.pyplot** y **seaborn**: utilizadas para la visualización de datos mediante gráficos como histogramas, diagramas de dispersión y mapas de calor.
- **scikit-learn (sklearn)**: para el preprocesamiento de datos, reducción de dimensionalidad y aplicación de algoritmos de aprendizaje no supervisado:
  - `StandardScaler`: normaliza los datos para que todas las variables tengan igual importancia en los cálculos de distancia.
  - `KMeans` y `DBSCAN`: algoritmos de *clustering* utilizados para identificar patrones o grupos similares en el dataset.
  - `PCA (Principal Component Analysis)` y `t-SNE (t-distributed Stochastic Neighbor Embedding)`: técnicas de reducción de dimensionalidad empleadas para visualizar los datos y los resultados del clustering en dos dimensiones.

## 3.- 🔍 Análisis Exploratorio de Datos (EDA) 
El dataset contiene las siguientes variable o columnas

| Nombre de columna | Tipo de dato | Tipo de variable            | Descripción breve                                                           | Utilidad principal en el análisis                                        |
|-------------------|--------------|-----------------------------|-----------------------------------------------------------------------------|--------------------------------------------------------------------------|
| Search term       | Texto        | Categórica nominal          | Consulta real ingresada por el usuario en Google.                           | Detectar intención del usuario, descubrir nuevas palabras clave valiosas.|
| Match type        | Texto        | Categórica ordinal          | Tipo de coincidencia entre el search term y keyword (Exact, Phrase, Broad). | Evaluar el grado de control sobre la intención del usuario y refinar segmentación.|
| Impr.             | Numérico     | Cuantitativa discreta       | Número de veces que el anuncio fue mostrado.                                | Mide visibilidad, sirve para calcular CTR y evaluar alcance.             |
| Clicks            | Numérico     | Cuantitativa discreta       | Veces que el anuncio fue clicado.                                           | Mide interés y engagement del usuario, insumo para calcular CTR.         |
| Currency code     | Texto        | Categórica nominal          | Código de la moneda usada en los reportes (e.g., USD, EUR).                 | No sirve para nuestro análisis, se excluirá                              |
| Avg. CPC          | Numérico     | Cuantitativa continua       | Costo promedio por clic generado por el término de búsqueda.                | Mide eficiencia de inversión publicitaria.                               |
| Keyword           | Texto        | Categórica nominal          | Palabra clave objetivo que activó el anuncio.                               | Permite analizar relación entre keyword y término de búsqueda, y optimizar campañas. |

Todo lo realizado en esta sección se encuentra en el archivo: <a href="https://github.com/ancantos99/g6_aprendizajeautomaticoico_s3/blob/main/colab/g6_aprendizajeautomaticoico_s3_exploraciondatos.ipynb" target="_blank">colab/g6_aprendizajeautomaticoico_s3_exploraciondatos.ipynb</a>

#### 🟦 Análisis de distribuciones individuales
![Gráfico de resultados](imagenes/analisisdistribuciones.png)

#### 🟦 Boxplot y Matriz de Correlación
![Gráfico de resultados](imagenes/boxplotimpr.png)
<p align="center">
<img src="imagenes/matrizcorrelacion.png" width="500">
</p>

#### 🟦 Distribución de la variable no categórica Match Type
![Gráfico de resultados](imagenes/distribucionmatchtype.png)

### Después de realizar el análisis individual de cada variable, se obtuvieron las siguientes conclusiones:

**1. Alta concentración de datos en valores bajos**
*   La mayoría de los términos tienen bajo impacto (pocas impresiones, clics y bajo CPC)
  
**2. Pocos términos dominan la distribución**
*   Probablemente, unas pocas palabras clave son muy efectivas

**2. Tendencia general (Impr. vs Clicks)**
*   Las dos variables presentan una correlación positiva, esto se respalda por la matriz de correlación, en la que se reporta un coeficiente de 0.69.

**3. Se puede calcular valores adicionales como:**
*   **Tasa de Clics o CTR (Click-Through Rate)** =  (Clics / Impr.)
*   Un CTR alto → el anuncio o enlace es relevante y atractivo para los usuarios.
*   Un CTR bajo → podría indicar que el contenido no está llamando la atención o no está bien dirigido al público adecuado.
*   **Costo Total** =  Clicks * Avg. CPC , Aunque no resulta útil para nuestro análisis, es un valor que puede calcularse.

  
## 4.- ⚙️ Preprocesamiento


Para la Limpieza y Preprocesamiento de datos se realizó lo siguiente:

- Se eliminó la columna Currency code del DATASET_NAMES

- Se eliminó Duplicados si existieran

- Se eliminan filas con valores nulos en las columnas importantes

- Se agregó la Columna CTR (Tasa de Clics) = Clicks/ Impr.

- Renombrar Nombre de Columnas para mejor comprensión: Search term -> TerminoBusqueda, Impr. -> Impresiones, CTR -> TasaClicks, Avg. CPC->CostoxClick

Esto dió como resultado un nuevo dataset preprocesado que se encuentra en: <a href="https://github.com/ancantos99/g6_aprendizajeautomaticoico_s3/tree/main/dataset/dataset_procesado.csv" target="_blank">dataset/dataset_procesado.csv</a>

Para terminar el preprocesamiento, con el nuevo dataset se realizo la normalización de los datos para las variables cuantitativas.

Este paso es fundamental, ya que observamos que la distribución de las variables es muy dispersa. Por ejemplo, la variable Impr. presenta una desviación estándar de 20.9, lo que indica una alta variabilidad.

```python
columnas = ['Impr.', 'Clicks', 'Avg. CPC', 'CTR']
scaler = StandardScaler()
X_scaled = scaler.fit_transform(df[columnas])
```
Con esto, los datos quedarán preparados para ser utilizados en los modelos de clustering K-Means y DBSCAN.


## 5.- 🤖 Implementación de Modelos

Se aplicarán los modelos sobre los términos de búsqueda y no sobre los keywords, El objetivo es agrupar los términos según su comportamiento en Clicks, Impresiones, Tasa de Clicks y Costo promedio por Click

**¿Por qué analizamos los Términos de Búsqueda?**

En el dataset, los términos de búsqueda muestran cómo los usuarios piensan o formulan sus necesidades relacionadas con IA en los buscadores, mientras que las Keywords son palabras seleccionadas para activar el anuncio.

Al fijarnos en los Términos de búsqueda en lugar de los keywords podemos encontrar nuevos términos que sean más precisos y reales, por ejemplo:

> **Ejemplo:**  
> Una keyword configurada podría ser `inteligencia artificial`,  
> pero el término de búsqueda real podría ser `cómo usar inteligencia artificial en el aula`.

### 5.1.- 🛠️ Implementación de K means

Todo lo realizado en esta sección se encuentra en el archivo: <a href="https://github.com/ancantos99/g6_aprendizajeautomaticoico_s3/blob/main/colab/g6_aprendizajeautomaticoico_s3_KMEANS.ipynb" target="_blank">colab/g6_aprendizajeautomaticoico_s3_KMEANS.ipynb</a>

#### Elección de K

<p align="center">
<img src="imagenes/kmeanscodo.png" width="400">
</p>
Aunque el método del codo sugiere que k=4 es una buena opción, al entrenar el modelo con k=5 se obtuvo una segmentación más detallada. 

En ambos casos aparece un cluster con un solo elemento (posible outlier), pero con k=5 los demás clusters están mejor definidos, por esta razón se decidió entrenar el modelo con K = 5 obteniendo los siguiente resultados:

#### Resultados Promedios por Cluster (K=5)

| Cluster | Impresiones | Clicks  | CostoxClick | TasaClicks |  Cantidad de elementos |
|---------|-------------|---------|-------------|------------|------------------------|
| 0       | 1.92        | 0.03    | 0.0027      | 0.0039     |        13084           |
| 1       | 13.87       | 1.80    | **0.4804**  | 0.3669     |         535            |
| 2       | 2060.00     | 47.00   | 0.1000      | 0.0228     |          1             |
| 3       | 1.31        | 1.09    | 0.2238      | **0.9132** |         763            |
| 4       | **200.11**  |**18.11**| 0.2795      | 0.1321     |          19            |

**Interpretación**

- **CLUSTER 0:** Tiene muy pocas impresiones, casi sin clicks y baja tasa de clicks este contiene la mayor cantidad de palabras (PALABRAS O FRASES IRRELEVANTES)
- **CLUSTER 1:** Tiemen mayor costo promedio por click (PALABRAS MÁS CARAS PERO CON UNA TASA DE CLICKS ACEPTABLE)
- **CLUSTER 2:** Tiene más impresiones y Clicks, pero solo contiene un elemento por eso lo descarto.
- **CLUSTER 3:** Tiente más tasa de clicks promedio 91% (CONTIENE PALABRAS O FRASES QUE GENERAN ALTA EFICIENCIA)
- **CLUSTER 4:** Descartando al 2, este es el que tiene más impresiones y Clicks (CONTIENE PALABRAS O FRASES QUE GENERAN ALTA VISIBILIDAD)


### 5.2.- 🛠️ Implementación de DBSCAN
Agrupaciones densas de puntos sin necesidad de conocer cuantos cluster tienen.

Entrenar el modelo

Con eps=0.8 y min_samples=15, el modelo DBSCAN generó clústeres más definidos y menos ruido, mejorando la agrupación de datos relevantes. Comparado con eps=0.6 y min_samples=10, el segundo entrenamiento ofrece mejor estabilidad y segmentación más precisa.
