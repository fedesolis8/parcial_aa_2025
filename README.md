# Proyecto Parcial: Predicción de Demanda de Transporte Público (SUBE)

**Alumno:** Federico Solis
**Materia:** Aprendizaje Automático
**Tecnicatura:** Ciencia de Datos e Inteligencia Artificial

**NOTA:** Este `README` principal contiene la descripción del proyecto (Entrega 1 y 2) y el resumen del modelo final (Entrega 3), como respuesta al feedback recibido.

- **Notebook Principal:** `/notebooks/Entrega 3 - Predicción transacciones del sistema SUBE.ipynb` 
- **Informes Detallados:** En la carpeta `/reports/`
- **Video Explicativo:** https://drive.google.com/file/d/18tq5Cu5Yp1hQ-Bxld-fWrbmbQMiy7C1u/view?usp=sharing

---

## 1. Descripción Detallada del Proyecto

Este proyecto de Aprendizaje Automático se centrará en el desarrollo de un **modelo predictivo de regresión** para estimar la demanda futura del transporte público en Argentina, utilizando datos históricos del sistema SUBE.

El objetivo principal es construir un modelo capaz de predecir la **cantidad total de transacciones (usos)** registradas por la tarjeta SUBE para un día específico en el futuro.

La metodología implicará un fuerte componente de **ingeniería de características (feature engineering)** a partir de los datos temporales. Se procesará el dataset oficial para extraer variables predictoras relevantes (día de la semana, mes, año, indicadores de feriados, etc.). La columna `CANTIDAD` del dataset original servirá como base para definir la **variable objetivo**.

## 2. Contexto y Relevancia del Problema

El sistema SUBE es la principal herramienta de pago y registro del transporte público en gran parte de Argentina. Poder anticipar la demanda diaria de viajes es fundamental para:

* **Optimizar la Operación:** Ajustar la frecuencia de servicios (colectivos, trenes).
* **Planificación de Infraestructura:** Identificar tendencias a largo plazo.
* **Gestión de Recursos:** Estimar los ingresos por recaudación.
* **Análisis de Impacto:** Evaluar cómo eventos externos (feriados, paros, etc.) afectan el uso del transporte.

## 3. Objetivos del Proyecto

### Objetivo General
* Desarrollar y evaluar un modelo de regresión supervisada capaz de predecir la cantidad total de transacciones diarias del sistema SUBE.

### Objetivos Específicos
1.  Realizar un análisis exploratorio del dataset de transacciones SUBE.
2.  Preprocesar el dataset, agregando los datos a nivel diario.
3.  Realizar ingeniería de características para crear variables predictoras (día de la semana, mes, es_feriado, etc.).
4.  Entrenar y comparar el rendimiento de al menos dos algoritmos de regresión (Regresión Lineal, SVR, Árbol de Decisión).
5.  Dividir los datos cronológicamente en conjuntos de entrenamiento y prueba.
6.  Evaluar los modelos utilizando métricas apropiadas (MAE y RMSE).
7.  Interpretar los resultados del mejor modelo.

---

## 4. Definición del Problema (Clasificación vs. Regresión)

El problema a abordar es un problema de **Regresión Supervisada** enfocado en el **pronóstico de series temporales**. La variable objetivo (`cantidad_total`) es un **valor numérico continuo**.

---

## 5. Estructura del Repositorio (Project Organization)

**Esta sección responde al feedback del profesor sobre la estructura del proyecto.**

Este repositorio utiliza la plantilla **Cookiecutter Data Science**. La organización de los archivos es la siguiente:

* `/data/raw/`: Contiene el dataset original `dat-ab-usos-2025.csv`.
* `/notebooks/`: Contiene el notebook principal con todo el análisis: `Entrega 3 - Predicción transacciones del sistema SUBE.ipynb`.
    * *(Nota: El archivo `notebook_parcial.ipynb` fue parte de la entrega 1 y 2, el mismo ha sido reemplazado por el notebook final).*
* `/reports/`: Contiene los informes de análisis y el video explicativo.
    * `EDA_Reporte.md`: Resumen de los hallazgos del Análisis Exploratorio de Datos.
    * `Reporte_Final.md`: Resumen de los resultados y métricas del modelo final.
    * `video_Explicativo_enlace.txt`.

---

## 6. Descripción del Dataset y Origen (Entrega 2)

#### Fuente y Origen
* **Origen:** Ministerio de Transporte de la Nación Argentina, publicado en **`datos.gob.ar`**.
* **Dataset Específico:** "Cantidad de transacciones (usos) por fecha..." (`dat-ab-usos-2025.csv`).
* **Acceso:** El archivo `.csv` está en la carpeta `/data/raw/`.

#### Estructura y Tipos de Datos
* **Dimensiones Originales:** 424,347 filas, 10 columnas.
* **Columnas Clave:** `DIA_TRANSPORTE` (object), `LINEA` (object), `CANTIDAD` (int64).
* **Valores Nulos:** Se identificaron en columnas geográficas (`PROVINCIA`, `MUNICIPIO`), pero no fueron usadas en el modelo final.

#### Proceso de Preprocesamiento Inicial
1.  **Carga:** `pandas.read_csv()`.
2.  **Agregación:** Los datos se agruparon por `DIA_TRANSPORTE` y se sumó `CANTIDAD` para crear la serie temporal diaria.
3.  **Ingeniería de Características:** Se crearon nuevas columnas a partir de la fecha (`dia_semana`, `mes`, `es_feriado`, `lag_1_dia`, `lag_7_dias`).

---

## 7. Modelos y Análisis de Resultados (Entrega 3)

**Esta sección responde al feedback del profesor sobre la falta de informes del EDA y del modelo final.**

El análisis completo se encuentra en el notebook, pero aquí se presenta el resumen:

### Análisis Exploratorio de Datos (EDA)
El EDA (detallado en `reports/EDA_Reporte.md`) reveló dos patrones clave:
1.  **Estacionalidad Semanal:** Un patrón de picos (días de semana) y valles (fines de semana) muy fuerte.
2.  **Anomalías (Feriados):** Valles atípicos y muy profundos que el modelo base no podía explicar. Un Boxplot confirmó que los días feriados tienen una media de viajes drásticamente menor.

### Desarrollo y Evaluación del Modelo
Se siguió un proceso iterativo:

1.  **Modelo Base (Sin Feriados):** Se entrenaron 3 modelos (Lineal, Árbol, SVR). El mejor fue `LinearRegression` con un **MAE de 1.02M**. El análisis de su gráfico de predicción mostró que fallaba en todos los feriados.
2.  **Modelo Mejorado (Con Feriados):** Se agregó la característica `es_feriado`. Al re-entrenar, los resultados cambiaron:
    * `LinearRegression` empeoró (MAE: 1.34M).
    * `DecisionTreeRegressor` mejoró drásticamente y se convirtió en el **modelo ganador**.

### Métricas del Modelo Final (Árbol de Decisión)
* **Algoritmo:** `DecisionTreeRegressor`
* **Métricas (MAE/RMSE):
    * **MAE (Error Absoluto Medio): 1,281,233 transacciones**
    * **RMSE (Raíz Error Cuadrático): 2,545,707 transacciones**

### Conclusión Final
El objetivo se cumplió. El modelo `DecisionTreeRegressor` fue el más efectivo. El gráfico de predicción muestra que captura con éxito tanto los patrones semanales como las caídas de los feriados.

El análisis de importancia de características confirmó que las variables más decisivas fueron `dia_semana` (58.3%) y `es_feriado` (18.8%), validando el proceso de ingeniería de características.
