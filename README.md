# Aprendizaje No Supervisado
## 📑 Índice
1. [📂 Descripción y Estructura del Proyecto](#1---descripción-y-estructura-del-proyecto)
2. [💻 Entorno de Trabajo](#2---entorno-de-trabajo)
3. [🔍 Análisis Exploratorio de Datos (EDA)](#3---análisis-exploratorio-de-datos-eda)


## 1.- 📂 Descripción y estructura del proyecto
El equipo de Marketing va a realizar una campaña publicitaria para productos que involucran IA, y nos pidieron que les facilitaramos palabras que mejoren sus búsquedas en Google.

Para esto hemos utilizado el dataset: https://www.kaggle.com/datasets/devraai/ai-google-search-keyword-performance
proporciona información sobre el rendimiento de palabras clave relacionadas con la inteligencia artificial en las búsquedas de Google. Este dataset es útil para analizar tendencias de búsqueda y evaluar la popularidad de términos específicos en el ámbito de la IA.

### Objetivo General
Agrupar términos de búsqueda (Search terms) en clusters según su rendimiento publicitario, para ayudar al equipo de marketing a:
- Detectar segmenos de alto rendimiento
- Identificar términos costosos
- Optimizar la asignación de presupuesto

## 2.- 💻 Entorno de Trabajo
Nombrar herramientas y librerías.

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
