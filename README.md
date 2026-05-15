# Practica 2.1 - Modelización en Ingeniería de Datos 

## Curso 2025-26 <img src="https://images.griddo.cunef.edu/logo-cunef-universidad-1272515f-17b3-4169-8bf6-ef63bfffe920" width="190" valign="middle"> 


## Descripción del Proyecto

Este repositorio contiene la primera fase (Modelado Analítico) del sistema *end-to-end* de Machine Learning para la detección de impago de créditos. Partiendo de los artefactos de preprocesamiento de la Práctica 1, este proyecto evoluciona el modelado base hacia un **sistema probabilístico robusto**, optimizando no solo la capacidad de discriminación, sino la calibración real de las probabilidades y la cuantificación de la incertidumbre epistémica.

## Cumplimiento de Objetivos (Rúbrica)

* **1.1 Optimización Bayesiana:** Uso de `Optuna` con `HyperbandPruner` y búsqueda multivariante para optimizar `LightGBM` y `XGBoost`. Se utiliza el **Log Loss** como métrica objetivo (*proper scoring rule*) para equilibrar *Resolution* y *Reliability*. Se incluye comparativa exhaustiva del impacto negativo del balanceo de clases artificial sobre la probabilidad.
* **1.2 Diagnóstico de Calibración:** Evaluación formal del modelo ganador mediante *Reliability Diagrams*, Descomposición de Brier y el **Test Z de Spiegelhalter**. Se justifica matemáticamente la decisión de *no* aplicar calibradores post-hoc dado el excelente rendimiento natural del algoritmo.
* **1.3 Cuantificación de Incertidumbre:** Implementación de **Cross Venn-Abers Predictors (CVAP)** para generar intervalos de probabilidad garantizados `[p_low, p_high]`. Se evalúa la regla estricta de derivación al agente (`> 0.2`) y se amplía el análisis con un umbral de negocio adaptado (`> 0.05`) para demostrar el *trade-off* de cobertura vs. precisión.
* **1.4 Persistencia:** Exportación del artefacto del modelo (`practica2_model.pkl`) encapsulando XGBoost y Venn-Abers, junto con su esquema de variables (`feature_schema.json`), listos para ser consumidos por la API (Repo 2).

## Estructura del Repositorio

```text
├── data/
│   └── filtered/
│       ├── X_train_filtered.pkl     # Artefactos de datos (heredados de Práctica 1)
│       ├── y_train_filtered.pkl
│       ├── X_test_filtered.pkl
│       └── y_test_filtered.pkl
├── models/
│   ├── practica2_model.pkl          # Pipeline fitteado (XGBoost Unbalanced + Venn-Abers)
│   └── feature_schema.json          # Esquema de columnas esperado por la API
├── practica2_notebook.ipynb         # Notebook principal ejecutado (salidas visibles)
├── pyproject.toml                   # Definición de dependencias
├── uv.lock                          # Congelación estricta de versiones
└── README.md                        # Este documento
```

## Reproducibilidad y Ejecución Local

Este proyecto utiliza el stack moderno de Python gestionado a través de [uv](https://github.com/astral-sh/uv) para asegurar tiempos de instalación ultrarrápidos y una **reproducibilidad estricta** (dependencias congeladas en `uv.lock` y semillas `random_state=42` fijadas en todo el código).

### 1. Prerrequisitos
Asegúrate de tener instalado `uv` en tu sistema. Si no lo tienes, puedes instalarlo rápidamente:

**En macOS y Linux:**
```bash
curl -LsSf [https://astral.sh/uv/install.sh](https://astral.sh/uv/install.sh) | sh
```

**En Windows:**
```bash
powershell -ExecutionPolicy ByPass -c "irm [https://astral.sh/uv/install.ps1](https://astral.sh/uv/install.ps1) | iex"
```

### 2. Sincronización del entorno
```bash
uv sync
```

### 3. Ejecución del notebook practica2_notebook.ipynb
```bash
uv run jupyter notebook
```
