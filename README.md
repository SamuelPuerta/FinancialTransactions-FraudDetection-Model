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
├── data/        # Dataset original (CSV)
├── notebook/    # Notebooks reproducibles (código del proyecto)
├── report/      # Reporte final (PDF), fuentes LaTeX y figuras
├── requirements.txt
└── README.md
```

## Cómo reproducir los resultados

Requiere Python 3.14 o superior.

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
```

Los notebooks usan rutas relativas (`../data`, `../report/figures`), por lo que deben ejecutarse **desde la carpeta `notebook/`**.

## Metodología (resumen)

- **Paradigma:** aprendizaje supervisado, clasificación binaria con clases muy desbalanceadas.
- **Validación:** validación cruzada estratificada con conjunto de test reservado; los preprocesadores se ajustan solo con datos de entrenamiento (sin fuga de información).
- **Modelos:** regresión logística, KNN, Random Forest, red neuronal (MLP) y SVM.
- **Métricas:** PR-AUC (principal), recall, precisión, F1, MCC, ROC-AUC y exactitud balanceada, con intervalos de confianza.
- **Reducción de dimensión:** PCA y UMAP, con reajuste de hiperparámetros en cada espacio.
