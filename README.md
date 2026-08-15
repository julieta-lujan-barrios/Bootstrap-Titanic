# Bootstrap y sus Aplicaciones en Ingeniería en IA

Proyecto de investigación desarrollado para la asignatura **Probabilidad y Estadística** (2do año, Ingeniería en Inteligencia Artificial, UNSTA). El trabajo aplica el método de **remuestreo Bootstrap** para estimar la incertidumbre de distintos estadísticos y evaluar la robustez de un modelo de clasificación entrenado sobre el dataset *Titanic - Machine Learning from Disaster* (Kaggle).

> 📄 Informe completo: [`INFORME_-_PyE_-_Barrios_Chalin_Granito_Sappia.pdf`](INFORME - PyE - Barrios, Chalin, Granito, Sappia.pdf)
> 
> 📓 Notebook: [`PROYECTO_DE_INVESTIGACION_PyE_Barrios_Chalin_Granito_Sappia.ipynb`](PROYECTO_DE_INVESTIGACION_PyE_Barrios_Chalin_Granito_Sappia.ipynb)


## Equipo

- Barrios, Julieta Luján
- Chalín, Matías Alejandro
- Granito, Leandro Elio
- Sappia, Lucio Agustín

**Docentes a cargo:** MSc. M. Isabel Giannini — Lic. M. Florencia Mignone

## Descripción del proyecto

El objetivo fue aplicar la técnica de Bootstrap tanto a estadísticos descriptivos como a la evaluación de un modelo de IA, usando el dataset de pasajeros del Titanic (891 registros) para resolver un problema de **clasificación binaria**: predecir si un pasajero sobrevivió o no.

El flujo de trabajo incluyó:

1. **Exploración y preprocesamiento de datos**: análisis de las 12 variables del dataset, imputación de valores faltantes (mediana para `Age`, moda para `Embarked`, exclusión de `Cabin` por exceso de nulos), codificación de variables categóricas (`Sex` con mapeo binario, `Embarked` con OneHotEncoder) y estandarización con `ColumnTransformer`.
2. **Implementación de estimadores iniciales** (sin bootstrap) para validar la relevancia de las variables seleccionadas:
   - Diferencia de tarifas (`Fare`) entre sobrevivientes y no sobrevivientes.
   - Correlación de Pearson entre `Age` y `Fare`.
3. **Construcción de un modelo base** de **Regresión Logística** (split 70/30), evaluado con Accuracy y F1-Score.
4. **Aplicación del método Bootstrap** (1000 remuestras, método de percentiles) sobre los tres estimadores anteriores, para construir intervalos de confianza del 95% y analizar la estabilidad del modelo.
5. **Discusión** sobre la variabilidad del modelo, el Teorema Central del Límite y las limitaciones del método.

## Resultados principales

| Estimador | Media Bootstrap | IC 95% | Interpretación |
|---|---|---|---|
| Diferencia de tarifas | 26.08 | (19.03, 33.75) | Diferencia significativa a favor de los sobrevivientes |
| Correlación (Edad vs. Tarifa) | 0.0968 | (0.0444, 0.1556) | Correlación positiva pero débil/marginal |
| Accuracy del modelo (Regresión Logística) | 0.7963 | (0.7799, 0.8098) | Modelo estable, sin sesgo relevante por la partición de datos |

El intervalo de confianza del Accuracy tiene una amplitud de apenas ~3 puntos porcentuales, lo que indica un modelo robusto y con baja variabilidad ante distintas remuestras de entrenamiento.

## Metodología técnica

- **Entorno:** Google Colab
- **Librerías:** Scikit-learn (Regresión Logística, `ColumnTransformer`, `StandardScaler`, `OneHotEncoder`), Pandas, Matplotlib/Seaborn
- **Método de intervalo de confianza:** percentiles (2.5 y 97.5), sin asumir normalidad teórica
- **Iteraciones de remuestreo:** 1000

## Limitaciones discutidas

- El costo computacional del Bootstrap escala con el tamaño de la muestra y el número de remuestras, pudiendo ser inviable en contextos de Big Data o con modelos de entrenamiento lento.
- El método asume independencia entre observaciones, por lo que no es directamente aplicable a series temporales o datos espaciales sin adaptaciones.
- Si la muestra original no es representativa, el Bootstrap amplifica ese sesgo en lugar de corregirlo.

## Referencias

Ver bibliografía completa en el [informe](INFORME_-_PyE_-_Barrios_Chalin_Granito_Sappia.pdf), incluyendo Efron (1979) *Bootstrap Methods: Another Look at the Jackknife* y Chan (2021) *Introduction to Probability for Data Science*.
