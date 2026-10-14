# Práctica 2. EDA y preparación de datasets



Análisis exploratorio, imputación, winsorización y tratamiento de desbalance
sobre dos datasets de clasificación, midiendo el impacto de cada decisión
sobre un clasificador (regresión logística).



## Datasets (carpeta `Data/`)



* **Titanic** (`train.csv`): predicción de supervivencia. Fuente: Kaggle (competencia "Titanic - Machine Learning from Disaster").
* **Telco Customer Churn** (`WA\_Fn-UseC\_-Telco-Customer-Churn.csv`): predicción de abandono de clientes. Fuente: Kaggle (dataset de IBM).



## Contenido



`Practica2\_IA.ipynb`: descripción, profiling, EDA, separación train/test, imputación, winsorización, balanceo, modelado, evaluación y conclusiones.



## Cómo ejecutarlo



1. `pip install -r requirements.txt`
2. Abrir el cuaderno en Jupyter o Google Colab (en Colab, subir también los CSV).
3. Ejecutar todas las celdas en orden (Entorno de ejecución > Reiniciar sesión y ejecutar todo).

El cuaderno busca los datos en `Data/` y, si no existe esa carpeta, en la raíz.



## Decisiones principales



* **Imputación:** mediana para variables numéricas y categoría "Desconocido" para categóricas.
* **Winsorización:** percentiles 1 y 99 sobre `Fare`.
* **Desbalance:** ponderación de clases (Telco, IR = 2.77).
* Todo el preprocesamiento se ajusta solo con datos de entrenamiento (dentro de pipelines) para evitar fuga de información.



## Declaración de uso de IA generativa



Se utilizó Claude (Anthropic) como asistente para una lluvia de ideas.El equipo
ejecutó el código, revisó las salidas y puede justificar cada decisión
(método de imputación, límites de winsorización y técnica de balanceo).



## Integrantes

* Dávila Martínez Brayan,
  Hernández Alonso Diego Raymundo,
  Martínez Rios Edson Jared, 
  Pallares Hernández Oscar. 

