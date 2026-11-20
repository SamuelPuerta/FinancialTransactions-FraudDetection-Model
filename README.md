# Detección de fraude en transacciones financieras

Proyecto final del curso **Modelos y Simulación II**, Universidad de Antioquia, Facultad de Ingeniería.

· **Equipo:** _[Samuel Puerta Patiño]_, _[Simón Berrio Gaviria]_, _[Juan Estiven Carmona Muñoz]_

· **Grupo:** _[5]_

· **Semestre:** _[2026-2]_

## Descripción

Este proyecto construye y compara modelos de aprendizaje de máquina para predecir si una transacción financiera es fraudulenta (`is_fraud`), siguiendo la guía oficial del curso. Incluye análisis exploratorio, revisión del estado del arte, comparación de modelos con ajuste de hiperparámetros, evaluación de técnicas de reducción de dimensión y discusión del modelo final.

## Dataset

[Financial Transactions & Fraud Detection Dataset (Kaggle)](https://www.kaggle.com/datasets/greatcool/financial-transactions-and-fraud-detection-dataset)

- 10.000 transacciones **sintéticas** (agosto 2025 – agosto 2026), 11 variables, 2.142 usuarios.
- Variable objetivo: `is_fraud` (126 fraudes, ≈ 1,26 %).
- Archivo en `data/synthetic_financial_transactions.csv`.

| Variable         | Descripción                                         |
| ---------------- | --------------------------------------------------- |
| `transaction_id` | Identificador de la transacción                     |
| `timestamp`      | Fecha y hora                                        |
| `user_id`        | Identificador del cliente                           |
| `account_type`   | Checking, Savings, Credit Card                      |
| `category`       | Dominio del gasto (Groceries, Electronics, Travel…) |
| `amount`         | Monto en USD                                        |
| `currency`       | Código ISO (constante: USD)                         |
| `payment_method` | Debit Card, Credit Card, UPI, Bank Transfer         |
| `device_type`    | Desktop, Mobile, POS Terminal, ATM                  |
| `risk_score`     | Puntaje de riesgo calculado (0–100)                 |
| `is_fraud`       | 0 = normal, 1 = fraude                              |

## Estructura del repositorio

```
.
├── data/
│   ├── synthetic_financial_transactions.csv
│   └── caracteristicas.csv             # Matriz de características (generada por el notebook 01)
├── notebooks/
│   ├── 01_descripcion_problema.ipynb   # Sección 2: EDA, caracterización, preprocesamiento
│   ├── 02_estado_del_arte.ipynb        # Sección 3: revisión de literatura
│   ├── 03_modelos.ipynb                # Sección 4: entrenamiento y evaluación de modelos
│   ├── 04_controles_y_paradigmas.ipynb # Sección 4 (complemento): controles de desbalance, modelos de una clase, validación temporal y agrupada por usuario
│   ├── 05_reduccion_dimension.ipynb    # Sección 5: análisis individual de variables, PCA y UMAP
│   └── 06_discusion_y_modelo_final.ipynb # Ítem 12: discusión, modelo final y comparación con el estado del arte
├── report/
│   ├── main.tex                        # Reporte en plantilla IEEE Transactions (pendiente: aún no está en el repositorio)
│   ├── referencias.bib
│   └── figures/                        # Figuras generadas por los notebooks (PDF y PNG)
├── requirements.txt
└── README.md
```

| Notebook                            | Sección de la guía                | Contenido                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------------------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `01_descripcion_problema.ipynb`     | 2. Descripción del problema       | Composición del dataset, desbalance, análisis de variables, fase de caracterización (variables de comportamiento por usuario, puntaje de anomalía respecto al historial y búsqueda de otras fuentes de señal), preprocesamiento y paradigma. Genera `data/caracteristicas.csv`                                                                                               |
| `02_estado_del_arte.ipynb`          | 3. Estado del arte                | Revisión de 5 artículos y definición de métricas                                                                                                                                                                                                                                                                                                                             |
| `03_modelos.ipynb`                  | 4. Entrenamiento y evaluación     | Configuración experimental, 6 modelos (regresión logística, KNN, Random Forest balanceado, gradient boosting, red neuronal y SVM) con ajuste de hiperparámetros, ablación por grupos de variables (con y sin `risk_score` y con y sin las variables de historial), evaluación en prueba y validación cruzada anidada                                                         |
| `04_controles_y_paradigmas.ipynb`   | 4. (complemento)                  | Techo de desempeño, intervalos con corrección de Nadeau–Bengio, controles de desbalance (ponderación, submuestreo por árbol, SMOTENC), indicador de umbral aprendido, comparación con detección de anomalías y clasificación de una clase, historial de `risk_score`, validación temporal y agrupada por usuario, y análisis de errores. Requiere `data/caracteristicas.csv` |
| `05_reduccion_dimension.ipynb`      | 5. Reducción de dimensión         | Análisis individual de las 22 variables (AUC, Mann-Whitney, V de Cramér, corrección de Benjamini–Hochberg) e importancia por permutación; selección de variables, PCA y UMAP con reajuste de hiperparámetros para los dos mejores modelos de la sección 4; tabla final con porcentaje de reducción. Requiere `data/caracteristicas.csv` y `umap-learn`                       |
| `06_discusion_y_modelo_final.ipynb` | Ítem 12: discusión y modelo final | Estructura de la etiqueta (por qué existe un techo de desempeño), modelo final recomendado (regla de umbral), punto de operación (cuántas alertas revisar), ahorro económico, prueba de permutación, curva de aprendizaje y comparación con el estado del arte. Requiere `data/caracteristicas.csv`                                                                          |

## Cómo reproducir los resultados

Requiere Python 3.10 o superior.

```bash
# 1. Clonar el repositorio
git clone <URL-DEL-REPOSITORIO>
cd <NOMBRE-DEL-REPOSITORIO>

# 2. Crear y activar un entorno virtual
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Verificar que el dataset esté en data/synthetic_financial_transactions.csv
#    (descargarlo de Kaggle si no está)

# 5. Abrir Jupyter y ejecutar los notebooks en orden
jupyter lab

# Alternativa sin interfaz: ejecutar un notebook completo desde la terminal
cd notebooks
jupyter nbconvert --to notebook --execute --inplace 01_descripcion_problema.ipynb
```

**Orden de ejecución:** 01 → 03 → 04 → 05 → 06 (el notebook 01 genera `data/caracteristicas.csv`, que cargan el 03, el 04, el 05 y el 06; el 02 es solo texto). Los notebooks usan rutas relativas (`../data`, `../report/figures`), por lo que deben ejecutarse **desde la carpeta `notebooks/`**. Los notebooks que generan figuras las guardan en `report/figures/`.

**Tiempo de ejecución y núcleos.** Los notebooks 03, 05 y 06 trabajan en paralelo con `N_JOBS` procesos (por defecto 6), que se ajusta en su celda de configuración según la máquina. Como referencia, con 6 procesos el notebook 03 tarda unos 22 minutos, el 05 unos 40 minutos y el 06 unos 10 minutos. Para que el gradient boosting no use todos los núcleos dentro de cada proceso, conviene fijar `OMP_NUM_THREADS=1` antes de abrir Jupyter.

## Reporte

El reporte (en elaboración) estará en `report/main.tex` (plantilla IEEE Transactions, clase `IEEEtran`). Para compilarlo, se sube la carpeta `report/` a Overleaf (_New Project → Upload Project_) o se compila localmente con `pdflatex` + `bibtex`. El PDF compilado se guarda como `report/reporte.pdf`.

## Metodología (resumen)

- **Paradigma:** aprendizaje supervisado, clasificación binaria con clases muy desbalanceadas.
- **Caracterización:** variables temporales (hora cíclica, fin de semana) y de comportamiento por usuario calculadas solo con transacciones previas; entre 23 y 37 columnas tras la codificación según el conjunto de variables (A: originales; B: caracterizadas; C: originales + `risk_score`; D: caracterizadas + `risk_score`).
- **Validación:** partición de prueba del 20 %, validación cruzada estratificada de 5 particiones repetida 2 veces para ajustar hiperparámetros y validación cruzada anidada como estimación principal; los preprocesadores se ajustan solo con datos de entrenamiento (sin fuga de información). Los intervalos de confianza de la validación cruzada usan la corrección de Nadeau y Bengio.
- **Desbalance:** ponderación de clases y Random Forest balanceado (submuestreo por árbol), con controles (sin tratamiento y SMOTENC).
- **Modelos:** regresión logística, KNN, Random Forest balanceado, gradient boosting, red neuronal (MLP) y SVM; como contraste, modelos de una clase (Isolation Forest, LOF, One-Class SVM y Mahalanobis).
- **Métricas:** PR-AUC (principal) y precisión y recall al revisar el 1 % y el 2 % de las transacciones con mayor puntaje, además de ROC-AUC y otras, con intervalos de confianza.
- **Reducción de dimensión:** selección de variables, PCA y UMAP, con reajuste de hiperparámetros en cada espacio.
- **Discusión y modelo final:** análisis de por qué existe un techo de desempeño, punto de operación, ahorro económico (Correa Bahnsen et al.), prueba de permutación, curva de aprendizaje y comparación con el estado del arte.
