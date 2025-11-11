# Informe Final del Modelo

Este informe detalla el modelo de Aprendizaje Automático desarrollado y sus resultados, en cumplimiento con la Entrega 3.

### 1. Desarrollo del Modelo
Se siguió un proceso iterativo en dos etapas:

1.  **Modelo Base (Sin Feriados):** Se entrenó un modelo de `Regresion Lineal` solo con características temporales y de retraso. Este modelo obtuvo un MAE de **1.02M** pero el análisis de su gráfico de predicción demostró que fallaba sistemáticamente en predecir los feriados.

2.  **Modelo Mejorado (Con Feriados):** Se agregó la característica `es_feriado` (validada en el EDA). Se compararon tres algoritmos de `scikit-learn`:
    * `Regresion Lineal` 
    * `Arbol de decisión` 
    * `SVR`

### 2. Métricas de Evaluación del Modelo Final
La evaluación se realizó sobre el conjunto de prueba (el 20% final de los datos). Las métricas relevantes para regresión (MAE y RMSE) arrojaron los siguientes resultados:

| Modelo (Con Feriados) | MAE (Error Absoluto Medio) | RMSE (Raíz Error Cuadrático) |
| :--- | :--- | :--- |
| Regresión Lineal | `1,344,915` | `2,176,503` |
| **Árbol de decisión** | **`1,281,233`** | **`2,545,707`** |
| SVR | `3,452,608` | `4,053,427` |

El **`Árbol de decisión`** fue seleccionado como el modelo final por tener el Error Absoluto Medio (MAE) más bajo.

### 3. Interpretación de Resultados del Modelo Final

#### Gráfico de Predicción
El gráfico de predicción del modelo ganador (`Árbol de decisión`) muestra un seguimiento cercano de los datos reales, capturando tanto los patrones semanales como las caídas abruptas de los feriados.

![alt text](<descarga (3).png>)
``

#### Importancia de Características
El análisis de `importancia de características` del Árbol de decisión revela qué variables usó el modelo para tomar sus decisiones:

![alt text](<descarga (4).png>)
``

* **`dia_semana` (58.3%)**: Fue la característica más decisiva.
* **`es_feriado` (18.8%)**: La segunda más importante, validando la mejora del modelo.
* **`lag_7_dias` (8.8%)**: El modelo priorizó el patrón semanal sobre el del día anterior.
* **`es_finde` (0.0%)**: Fue ignorada por ser redundante (información ya contenida en `dia_semana`).

### 4. Conclusión
El objetivo general se cumplió. Se desarrolló un modelo de `Árbol de decisión` que aborda el problema de predicción de demanda. El análisis demostró que una correcta **ingeniería de características** (identificar y validar `es_feriado`) fue el paso más crítico para el éxito del proyecto.