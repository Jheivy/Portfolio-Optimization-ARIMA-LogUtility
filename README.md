# Quantitative Portfolio Optimization & Time Series Forecasting with AutoARIMA

![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)
![Finance](https://img.shields.io/badge/Finance-Gold?style=for-the-badge&logo=bitcoin&logoColor=black)

Este repositorio contiene un ecosistema completo para el análisis, predicción y optimización de carteras de inversión utilizando modelos avanzados de series temporales y teoría moderna de carteras. 

**Logro Académico:** Este proyecto fue galardonado con la **máxima calificación** del curso, destacando por su robustez numérica y la precisión en la implementación de las funciones de utilidad.

## 🚀 Características Principales

* **Predicción con AutoARIMA:** Implementación de modelos autorregresivos integrados de media móvil con selección automática de parámetros.
* **Backtesting One-Step-Ahead:** Marco de simulación realista que actualiza el estado del modelo paso a paso.
* **Optimización Multiobjetivo:**
    * Teoría de Media-Varianza (MV) con y sin restricciones de posiciones cortas.
    * Maximización de Utilidad Logarítmica para crecimiento compuesto.
* **Análisis de Sensibilidad:** Barrido de parámetros de aversión al riesgo ($\gamma$) para identificar la frontera eficiente.

## 📈 Metodología de Predicción: One-Step-Ahead

El núcleo predictivo del proyecto utiliza un enfoque de **ventana deslizante recursiva**. A diferencia de las predicciones estáticas, la función `getPred_ts` simula un entorno real de trading:

1.  **Entrenamiento Inicial:** El modelo se ajusta con datos históricos (`Xtrain`).
2.  **Actualización Constante:** En cada paso temporal del periodo de prueba, el modelo incorpora la información más reciente observada en $t-1$ para predecir $t$.
3.  **Sin Sesgo de Anticipación:** Se garantiza que el modelo solo utiliza información disponible en el momento de la decisión.

```r
# Estructura del Backtesting Recursivo
for (h in seq_len(H)) {
  for (i in 1:5){
    # Actualización del estado del modelo con datos hasta t-1
    pred = do.call(getPredFunc, list(Xtrain[,i], X_test_past[,i]))
    mu_hat[h,i] <- pred$mu_hat
    se_hat[h,i] <- pred$se_hat
  }
  X_test_past <- Xtest[1:h,,drop=FALSE] # Información actualizada para el siguiente paso
}

```
## ⚖️ Optimización de Cartera

Se implementaron y compararon tres filosofías de inversión basadas en funciones de utilidad:1. Media-Varianza (Long/Short)Optimización cuadrática clásica que permite posiciones cortas para maximizar el ratio Sharpe:

$$U(\alpha) = \alpha^T \mu - \frac{\gamma}{2} \alpha^T \Sigma \alpha$$

**2. Media-Varianza (Long-Only)**

Restricción de no negatividad ($\alpha_i \ge 0$) para carteras tradicionales, gestionada mediante programación no lineal (nloptr).

**3. Utilidad Logarítmica**

Diseñada para inversores a largo plazo, buscando maximizar el crecimiento geométrico y protegiendo la cartera contra la ruina mediante penalizaciones numéricas de estabilidad.

## 🛠️ Requisitos
-TécnicosLenguaje: R v4.0+
- Librerías Clave: * forecast: Para la implementación de auto.arima.nloptr:
-  Para la optimización no lineal con restricciones.
-  ggplot2: Para la generación de gráficos técnicos.
- tseries & zoo: Gestión de objetos de series temporales.
