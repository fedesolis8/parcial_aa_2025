# parcial_aa_2025

# Proyecto Parcial: Predicción de Demanda de Transporte Público (SUBE)

**Alumno:** Federico Solis
**Materia:** Aprendizaje Automático
**Tecnicatura:** Ciencia de Datos e Inteligencia Artificial

---

## 1. Descripción Detallada del Proyecto

Este proyecto de Aprendizaje Automático se centrará en el desarrollo de un **modelo predictivo de regresión** para estimar la demanda futura del transporte público en Argentina, utilizando datos históricos del sistema SUBE.

El objetivo principal es construir un modelo capaz de predecir la **cantidad total de transacciones (usos)** registradas por la tarjeta SUBE para un día específico en el futuro.

La metodología implicará un fuerte componente de **ingeniería de características (feature engineering)** a partir de los datos temporales. Se procesará el dataset oficial para extraer variables predictoras relevantes (día de la semana, mes, año, indicadores de feriados, etc.). La columna `CANTIDAD` del dataset original servirá como base para definir la **variable objetivo** (probablemente la suma total de transacciones diarias a nivel nacional o regional).

El resultado final será un modelo entrenado capaz de tomar características temporales futuras y realizar una predicción cuantitativa sobre el volumen esperado de viajes, sirviendo como una herramienta potencial para la planificación y gestión del transporte.

---

## 2. Contexto y Relevancia del Problema

El sistema SUBE es la principal herramienta de pago y registro del transporte público en gran parte de Argentina. Los datos generados diariamente representan un volumen masivo de información sobre los patrones de movilidad de la población. Poder anticipar la demanda diaria de viajes es fundamental para:

* **Optimizar la Operación:** Ajustar la frecuencia de servicios (colectivos, trenes) según la demanda esperada para evitar saturación o capacidad ociosa.
* **Planificación de Infraestructura:** Identificar tendencias a largo plazo que informen sobre la necesidad de nuevas rutas, ampliaciones o mejoras en el sistema.
* **Gestión de Recursos:** Estimar los ingresos por recaudación y los costos asociados a la operación del sistema.
* **Análisis de Impacto:** Evaluar cómo eventos externos (feriados, paros, vacaciones, pandemias) afectan el uso del transporte público.

Este proyecto busca aplicar técnicas de Machine Learning para transformar datos históricos en una herramienta predictiva útil para la toma de decisiones en el sector del transporte.

---

## 3. Objetivos del Proyecto

### Objetivo General
* Desarrollar y evaluar un modelo de regresión supervisada capaz de predecir la cantidad total de transacciones diarias del sistema SUBE.

### Objetivos Específicos
1.  Realizar un análisis exploratorio del dataset de transacciones SUBE para entender patrones temporales (diarios, semanales, anuales) y la distribución de los viajes.
2.  Preprocesar el dataset, agregando los datos a nivel diario y manejando posibles inconsistencias o valores faltantes.
3.  Realizar ingeniería de características para crear variables predictoras a partir de la fecha (día de la semana, mes, año, es_feriado, etc.) y variables de retraso (cantidad del día anterior, semana anterior).
4.  Entrenar y comparar el rendimiento de al menos dos algoritmos de regresión (ej. Regresión Lineal, SVR, Árbol de Decisión para Regresión).
5.  Dividir los datos cronológicamente en conjuntos de entrenamiento y prueba para simular un escenario de predicción real.
6.  Evaluar los modelos utilizando métricas apropiadas para regresión y series temporales, como el **Error Absoluto Medio (MAE)** y el **Error Cuadrático Medio Raíz (RMSE)**.
7.  Interpretar los resultados del mejor modelo y analizar la importancia de las características creadas.

---

## 4. Definición del Problema (Clasificación vs. Regresión)

El problema a abordar es un problema de **Regresión Supervisada** enfocado en el **pronóstico de series temporales**.

La variable objetivo, la cantidad total de transacciones diarias, es un **valor numérico continuo**. El objetivo es estimar esta cantidad, no asignar una categoría.

---

### 5. Descripción del Dataset y Origen (Entrega 2)

#### Fuente y Origen
* **Origen:** Los datos provienen del **Ministerio de Transporte de la Nación Argentina** y son publicados a través del **Portal de Datos Abiertos oficial (`datos.gob.ar`)**.
* **Dataset Específico:** "Cantidad de transacciones (usos) por fecha, jurisdicción, línea, tipo de transporte y modo". (Corresponde al archivo `dat-ab-usos-2025.csv` para el año 2025).
* **Link de Acceso:** [https://datos.transporte.gob.ar/dataset/sube-cantidad-de-transacciones-usos-por-fecha/archivo/c7dad6d8-8fe4-449e-82c9-18ed8574eae8](https://datos.transporte.gob.ar/dataset/sube-cantidad-de-transacciones-usos-por-fecha/archivo/c7dad6d8-8fe4-449e-82c9-18ed8574eae8)
* **Fecha de Adquisición:** Se utilizará la versión más reciente disponible en el portal al momento de iniciar el proyecto (Descargado el 19 de Octubre de 2025).

#### Formato y Acceso
* El dataset se encuentra en formato **CSV (Comma-Separated Values)**.
* El archivo CSV (`dat-ab-usos-2025.csv`) se incluye en el directorio `data/raw` del repositorio GIT.

#### Estructura y Tipos de Datos
El dataset original (`df`) cargado en `pandas` tiene la siguiente estructura:

* **Dimensiones:**
    * Número de Instancias (Filas): **424,347**
    * Número de Características (Columnas): **10**
* **Columnas y Tipos de Datos:**
    1.  `DIA_TRANSPORTE` (object): Fecha de la transacción. Se requerirá conversión a tipo `datetime`.
    2.  `NOMBRE_EMPRESA` (object): Nombre de la empresa de transporte (Categórico).
    3.  `LINEA` (object): Identificador de la línea (Categórico).
    4.  `AMBA` (object): Indica si pertenece al Área Metropolitana de Buenos Aires ('SI'/'NO') (Categórico/Binario).
    5.  `TIPO_TRANSPORTE` (object): Tipo de transporte (ej. 'COLECTIVO') (Categórico).
    6.  `JURISDICCION` (object): Jurisdicción del servicio (ej. 'MUNICIPAL', 'PROVINCIAL') (Categórico). *Nota: Contiene valores nulos.*
    7.  `PROVINCIA` (object): Provincia donde opera el servicio (Categórico). *Nota: Contiene valores nulos.*
    8.  `MUNICIPIO` (object): Municipio donde opera el servicio (Categórico). *Nota: Contiene valores nulos.*
    9.  `CANTIDAD` (int64): Número de transacciones/usos registrados para esa combinación de día/línea/etc. (Numérico/Entero). **Esta es la base para la variable objetivo.**
    10. `DATO_PRELIMINAR` (object): Indicador ('SI'/'NO') (Categórico/Binario).


#### Variable Objetivo Preliminar
* La variable objetivo final para el modelo de regresión será la **suma total de `CANTIDAD` agrupada por `DIA_TRANSPORTE`**. Esto se calculará durante el preprocesamiento para obtener una serie temporal del total de transacciones diarias.

#### Proceso de Recopilación / Preprocesamiento Inicial
1.  **Descarga:** Obtención del archivo CSV del portal oficial.
2.  **Carga:** Uso de `pandas.read_csv()` para cargar los datos en un DataFrame.
3.  **Exploración Inicial:** Verificación de tipos de datos (`df.info()`), revisión de valores nulos (`df.isnull().sum()`), análisis del rango de fechas y confirmación de dimensiones (`df.shape`). Se identificaron valores nulos en `JURISDICCION`, `PROVINCIA` y `MUNICIPIO`.
4.  **Agregación:** Agrupar el DataFrame por `DIA_TRANSPORTE` y sumar la columna `CANTIDAD` para crear la serie temporal de transacciones diarias totales.
5.  **Ingeniería de Características:** Crear nuevas columnas a partir de la fecha (día de semana, mes, año, etc.) para usar como variables predictoras.

#### Información Relevante
* **Granularidad:** El dataset original es muy detallado (día/línea). El paso esencial será agregarlo a nivel diario total para el pronóstico.
* **Valores Nulos:** Se deberá definir una estrategia para manejar los valores faltantes en las columnas geográficas si se decide utilizarlas como características. Para el objetivo inicial de predecir el total nacional, podrían no ser necesarias.
* **Temporalidad:** Es crucial tratar los datos como una serie temporal, dividiendo los conjuntos de entrenamiento y prueba cronológicamente (ej. usar datos hasta cierta fecha para entrenar y datos posteriores para probar).

## 6. Modelos de Aprendizaje Automático a Utilizar

Para este problema de regresión de series temporales, se implementarán y evaluarán los siguientes algoritmos de la librería **scikit-learn**:

* **Regresión Lineal (`LinearRegression`):** Como modelo base, incluyendo características temporales como variables predictoras.
* **Regresión de Vectores de Soporte (`SVR`):** Para explorar si relaciones no lineales mejoran la predicción.
* **Árboles de Decisión para Regresión (`DecisionTreeRegressor`):** Para capturar posibles interacciones complejas entre las características temporales.
* **(Opcional) Modelos Ensemble como `RandomForestRegressor`:** Si el tiempo lo permite, para evaluar modelos más avanzados.

La selección final se basará en el rendimiento predictivo (MAE, RMSE) en el conjunto de prueba.

---
