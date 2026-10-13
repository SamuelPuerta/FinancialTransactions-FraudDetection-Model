# Detección de fraude en transacciones financieras

Proyecto final del curso **Modelos y Simulación II**, Universidad de Antioquia, Facultad de Ingeniería.

· **Equipo:** _[Samuel Puerta Patiño]_, _[Simón Berrio Gaviria]_, _[Juan Estiven Carmona Muñoz]_

· **Grupo:** _[5]_

· **Semestre:** _[2026-2]_

## Descripción

Este proyecto construye y compara modelos de aprendizaje de máquina para predecir si una transacción financiera es fraudulenta (`is_fraud`), siguiendo la guía oficial del curso. Incluye análisis exploratorio, revisión del estado del arte, comparación de modelos con ajuste de hiperparámetros, y evaluación de técnicas de reducción de dimensión.

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
│   └── synthetic_financial_transactions.csv
├── notebooks/
│   ├── 01_descripcion_problema.ipynb   # Sección 2: EDA, caracterización, preprocesamiento
│   └── 02_estado_del_arte.ipynb        # Sección 3: revisión de literatura
├── report/
│   ├── main.tex                        # Reporte en plantilla IEEE Transactions
│   ├── referencias.bib
│   └── figures/                        # Figuras generadas por los notebooks (PDF y PNG)
├── requirements.txt
└── README.md
```

| Notebook | Sección de la guía | Contenido |
|---|---|---|
| `01_descripcion_problema.ipynb` | 2. Descripción del problema | Composición del dataset, desbalance, análisis de variables, fase de caracterización (variables de comportamiento por usuario), preprocesamiento y paradigma |
| `02_estado_del_arte.ipynb` | 3. Estado del arte | Revisión de 5 artículos y definición de métricas |

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

Los notebooks usan rutas relativas (`../data`, `../report/figures`), por lo que deben ejecutarse **desde la carpeta `notebooks/`**. Al ejecutar `01_descripcion_problema.ipynb` se regeneran las figuras de `report/figures/`.

## Reporte

El reporte está en `report/main.tex` (plantilla IEEE Transactions, clase `IEEEtran`). Para compilarlo, se sube la carpeta `report/` a Overleaf (*New Project → Upload Project*) o se compila localmente con `pdflatex` + `bibtex`. El PDF compilado se guarda como `report/reporte.pdf`.

## Metodología (resumen)

- **Paradigma:** aprendizaje supervisado, clasificación binaria con clases muy desbalanceadas.
- **Caracterización:** variables temporales (hora cíclica, fin de semana) y de comportamiento por usuario calculadas solo con transacciones previas; 35 columnas tras la codificación.
- **Validación:** validación cruzada estratificada con conjunto de test reservado; los preprocesadores se ajustan solo con datos de entrenamiento (sin fuga de información).
- **Modelos:** regresión logística, KNN, Random Forest, red neuronal (MLP) y SVM.
- **Métricas:** PR-AUC (principal), recall, precisión, F1, MCC, ROC-AUC y exactitud balanceada, con intervalos de confianza.
- **Reducción de dimensión:** PCA y UMAP, con reajuste de hiperparámetros en cada espacio.
