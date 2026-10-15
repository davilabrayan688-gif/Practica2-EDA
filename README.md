# Práctica 2. EDA y preparación de datasets

Análisis exploratorio, imputación, winsorización y tratamiento de desbalance
sobre dos datasets de clasificación, midiendo el impacto de cada decisión
sobre un clasificador (regresión logística).

## Datasets (carpeta `Data/`)

| Dataset | Archivo | Fuente | Condiciones de uso |
|---|---|---|---|
| **Titanic** (supervivencia) | `train.csv` | [Kaggle, competencia "Titanic - Machine Learning from Disaster"](https://www.kaggle.com/competitions/titanic/data) | "Subject to Competition Rules": uso no comercial, académico y educativo |
| **Telco Customer Churn** (abandono de clientes) | `WA_Fn-UseC_-Telco-Customer-Churn.csv` | [Kaggle, dataset de BlastChar basado en IBM Sample Data Sets](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) | "Data files © Original Authors" (sin licencia abierta declarada) |

Los detalles (dimensiones, variables, problemas de calidad, riesgos y limitaciones) están en la ficha técnica.

## Entregables

| Archivo | Contenido |
|---|---|
| `Practica2_IA.ipynb` | Cuaderno reproducible: comprensión de los datos, profiling, EDA, separación train/test, imputación, outliers, balanceo, modelado, evaluación, conclusiones y declaración de IA |
| `Data/` | Datasets originales (no se modifican) |
| `Ficha_tecnica_datasets.pdf` | Ficha técnica de ambos datasets (2 páginas) |
| `Informe_ejecutivo_Practica2.pdf` | Hallazgos, justificación, resultados, recomendación y limitaciones |
| `Presentacion_Practica2.pptx` | Presentación de ~8 minutos (7 diapositivas, con notas del orador) |
| `EDA y preparación.pdf` | Material de referencia del curso |

## Cómo ejecutarlo

1. Instalar dependencias: `pip install -r requirements.txt`
2. Abrir el cuaderno con `jupyter notebook Practica2_IA.ipynb` o en Google Colab (en Colab, subir también los dos CSV).
3. Ejecutar todas las celdas en orden (Kernel > Restart & Run All, o en Colab: Entorno de ejecución > Reiniciar sesión y ejecutar todo).

El cuaderno busca los datos en `Data/` y, si no existe esa carpeta, en la raíz. Todos los procesos aleatorios usan `random_state=42`.

**Versiones probadas** (ejecución completa sin errores):
- Python 3.13, pandas 3.0.5, numpy 2.5.3, scikit-learn 1.9.1, imbalanced-learn 0.14.2, matplotlib 3.11.2, seaborn 0.13.2.
- Python 3.13, pandas 2.2.3, numpy 2.1.3, scikit-learn 1.6.1, imbalanced-learn 0.13.0 (mismos resultados; solo la tercera decimal de SMOTE puede variar entre versiones).

## Decisiones principales

* **Imputación:** mediana para variables numéricas y categoría "Desconocido" para categóricas (se comparó contra media + moda y contra un indicador de ausente; rendimiento equivalente).
* **Winsorización:** percentiles 1 y 99 sobre `Fare` (se comparó contra 1.5 x IQR; ninguno mejoró el modelo, pero IQR recortaba 12.8 % de tarifas legítimas).
* **Desbalance:** ambos datasets superan IR > 1.5 (Titanic 1.61, Telco 2.77). En Telco se compararon las cinco estrategias y se eligió ponderación de clases; en Titanic el balanceo no mejoró el F1 y se dejó sin balancear.
* Todo el preprocesamiento y el remuestreo se ajustan solo con datos de entrenamiento (dentro de pipelines) para evitar fuga de información; el conjunto de prueba se usa una sola vez al final.

## Declaración de uso de IA generativa

Se utilizó Claude (Anthropic) como herramienta de apoyo durante el desarrollo de la práctica para generar ideas, apoyar con ideas en la estructuración de algunas partes del código y facilitar la interpretación de determinados resultados. En la revisión final también se usó para comparar el trabajo con la rúbrica y sugerir correcciones y ajustes de redacción, que el equipo revisó antes de incorporarlos. El equipo revisó y validó el trabajo, realizó los ajustes necesarios y es responsable de comprender y justificar los métodos, las decisiones de preprocesamiento y las conclusiones presentadas.

## Integrantes

* Dávila Martínez Brayan
* Hernández Alonso Diego Raymundo
* Martínez Rios Edson Jared
* Pallares Hernández Oscar
