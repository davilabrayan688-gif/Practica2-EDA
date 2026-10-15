# Práctica 2. Análisis Exploratorio de Datos (EDA) y preparación de datasets

**Inteligencia Artificial · ESCOM-IPN**

**Equipo:**

* Dávila Martínez Brayan
* Hernández Alonso Diego Raymundo
* Martínez Rios Edson Jared
* Pallares Hernández Oscar

## Descripción

En esta práctica auditamos la calidad de dos datasets de clasificación, Titanic y Telco Customer Churn, y preparamos los datos antes de modelar. Comparamos estrategias de imputación, de tratamiento de outliers (winsorización) y de balanceo de clases, y medimos el efecto de cada decisión sobre un clasificador de regresión logística.

## Estructura del repositorio

```
Practica2-EDA/
├── Data/
│   ├── train.csv                               # Titanic
│   └── WA_Fn-UseC_-Telco-Customer-Churn.csv    # Telco Customer Churn
├── Practica2_IA.ipynb                          # Cuaderno con todo el análisis
├── Ficha_tecnica_datasets.pdf                  # Ficha técnica de los datasets
├── Informe_ejecutivo_Practica2.pdf             # Informe ejecutivo
├── Presentacion_Practica2.pptx                 # Presentación (máx. 10 minutos)
├── EDA y preparación.pdf                       # Material de referencia del curso
├── requirements.txt
└── README.md
```

## Datasets

| Dataset | Variable objetivo | Dimensión | Fuente | Condiciones de uso |
|---|---|---|---|---|
| Titanic | `Survived` (0 = no sobrevivió, 1 = sobrevivió) | 891 x 12 | [Kaggle: Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) | Sujeto a las reglas de la competencia: uso no comercial, académico y educativo |
| Telco Customer Churn | `Churn` (Yes = se dio de baja, No = sigue como cliente) | 7,043 x 21 | [Kaggle: Telco Customer Churn (IBM Sample Data Sets)](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) | "Data files © Original Authors", sin licencia abierta declarada |

## Instalación y ejecución

1. Instalar las dependencias:
   ```
   pip install -r requirements.txt
   ```
2. Abrir el cuaderno:
   ```
   jupyter notebook Practica2_IA.ipynb
   ```
   También se puede abrir en Google Colab; en ese caso hay que subir los dos archivos CSV.
3. Ejecutar todas las celdas en orden (**Kernel > Restart & Run All**).

El cuaderno busca los datos en la carpeta `Data/` y, si no existe, en la raíz. Todos los procesos aleatorios usan `random_state=42`, así que los resultados son reproducibles. Se probó con Python 3.13, pandas 3.0, scikit-learn 1.9 e imbalanced-learn 0.14.

## Metodología

1. **Comprensión de los datos:** fuente, licencia, unidad de análisis, variable objetivo, identificadores, posibles fugas y riesgos éticos.
2. **Data profiling:** tipos, valores ausentes, estadísticos descriptivos, frecuencias, duplicados, columnas constantes, valores fuera de dominio y correlaciones.
3. **Análisis exploratorio:** distribuciones, boxplots, gráficas categóricas, matriz de correlación, distribución de clases y relación de los atributos con el objetivo.
4. **Separación:** 80 % entrenamiento y 20 % prueba, estratificada.
5. **Imputación:** estrategia A (media + moda) contra estrategia B (mediana + "Desconocido"), más un indicador binario de ausencia.
6. **Outliers:** detección con IQR y winsorización con dos criterios, 1.5 x IQR y percentiles 1-99.
7. **Desbalance:** razón de desbalance (IR) y comparación de cinco estrategias: sin balanceo, ponderación de clases, sobremuestreo, submuestreo y SMOTE.
8. **Evaluación:** validación cruzada estratificada de 5 folds para comparar estrategias y evaluación final en el conjunto de prueba, con accuracy, precision, recall, F1 y ROC-AUC.

Todo el preprocesamiento y el remuestreo se ajustan solo con datos de entrenamiento, dentro de pipelines, para evitar fuga de información.

## Resultados principales

| Tema | Resultado | Decisión |
|---|---|---|
| Calidad de datos | Titanic: `Age` 19.9 % nulos, `Cabin` 77.1 %, `Fare` muy asimétrica. Telco: `TotalCharges` como texto con 11 vacíos (clientes con `tenure = 0`) | Conversión numérica, exclusión de identificadores y de `Cabin` |
| Imputación | A y B rinden igual (F1 0.729 y 0.730); ambas crean un pico artificial y reducen ~10 % la desviación de `Age` | Mediana + "Desconocido" |
| Winsorización (`Fare`) | IQR recorta 12.8 % de los datos y baja el F1 a 0.713; percentiles 1-99 recortan 1 % y mantienen el F1 en 0.730 | Percentiles 1 y 99 (no mejora el modelo, pero reduce extremos sin perder información) |
| Desbalance | IR Titanic = 1.61, IR Telco = 2.77 (ambos > 1.5). En Telco, balancear sube el recall de 0.54 a ~0.80 y el F1 de 0.59 a ~0.63 | Ponderación de clases en Telco; sin balanceo en Titanic |
| Prueba final (Telco) | Ponderación de clases: recall 0.78, precision 0.50, F1 0.61. Detecta 86 abandonos más que el modelo base, con 180 falsas alarmas más | Recomendada si retener a un cliente es más barato que perderlo |

Los detalles, gráficas e interpretaciones están en el cuaderno y en el informe ejecutivo.

## Declaración de uso de IA generativa

Se utilizó Claude (Anthropic) como herramienta de apoyo durante el desarrollo de la práctica para generar ideas, apoyar con ideas en la estructuración de algunas partes del código y facilitar la interpretación de determinados resultados. También se usó para revisar el trabajo frente a los criterios de evaluación y sugerir correcciones y mejoras de redacción, que el equipo revisó antes de incorporarlas. 
