# OpenAQ PM2.5 Spatio-Temporal Forecasting

This repository contains the final artefacts for the MSc Data Science and Artificial Intelligence research project:

**Spatio-Temporal Air Quality Prediction Using Deep Learning and Satellite Data**

The project develops an end-to-end, ethics-compliant PM2.5 forecasting pipeline using secondary OpenAQ air-quality monitoring data. The workflow includes data loading, data audit, cleaning, exploratory data analysis, spatio-temporal feature engineering, machine-learning and deep-learning model comparison, technical evaluation, and explainability using feature importance / SHAP-style analysis.

---

## Project Overview

Air quality changes across both **time** and **location**. PM2.5 is an important pollutant because it represents fine particulate matter that can vary strongly between countries, cities, monitoring stations, seasons and pollution events.

The main problem addressed in this project is that short-term PM2.5 prediction is difficult when open monitoring data is uneven by country, city, station, pollutant coverage and timestamp availability. This project therefore builds a reproducible forecasting pipeline that tests whether historical readings, temporal features and spatial station metadata can predict the next PM2.5 reading.

---

## Research Question

**How effectively can a secondary-data, spatio-temporal machine-learning pipeline predict the next PM2.5 reading from OpenAQ monitoring records, and how interpretable are the model outputs?**

---

## Aim

To develop and evaluate an intelligent air-quality forecasting pipeline that uses public OpenAQ secondary data to support PM2.5 prediction across selected cities and countries.

---

## Objectives

1. Document the OpenAQ ethics, licensing and secondary-data position.
2. Clean and prepare timestamped air-quality monitoring records.
3. Explore spatial and temporal PM2.5 patterns across selected countries and cities.
4. Engineer lag, rolling, temporal and spatial features for forecasting.
5. Compare machine-learning models and a CNN-LSTM deep-learning model.
6. Evaluate model performance using MAE, RMSE, R², residual diagnostics, country/city error analysis and SHAP-style feature importance.

---

## Repository Contents

| File | Description |
|---|---|
| `README.md` | Project documentation and run instructions |
| `spatio-temporal-air-quality-prediction-using-d...ipynb` | Main Kaggle/Jupyter notebook for the complete pipeline |
| `best_pm25_ml_pipeline.joblib` | Saved best-performing machine-learning pipeline |
| `cnn_lstm_pm25_model.keras` | Saved CNN-LSTM comparison model |
| `model_metrics.csv` | Model comparison and evaluation metrics |
| `feature_importance.csv` | Feature-importance / SHAP-style explanation output |
| `test_predictions_best_model.csv` | Held-out test predictions from the selected model |
| `pm25_station_map.html` | Interactive station-level PM2.5 map |
| `project_summary.json` | Summary metadata for the project outputs |

---

## Dataset

The project uses **OpenAQ global air-quality monitoring data** accessed through Kaggle / BigQuery or a CSV fallback.

### Dataset fields used

Typical fields used in the notebook include:

- `location`
- `city`
- `country`
- `pollutant`
- `value`
- `timestamp`
- `unit`
- `source_name`
- `latitude`
- `longitude`
- `averaged_over_in_hours`

### Pollutants audited

The raw dataset was audited for several pollutants, including PM2.5, PM10, NO₂, SO₂, CO, O₃ and black carbon.

The final forecasting target is **PM2.5**, because it has strong coverage and clear relevance for air-quality forecasting.

### Kaggle dataset path used during development

```text
/kaggle/input/datasets/open-aq/openaq/global_air_quality
```

If the dataset path is different in your Kaggle environment, update the path in the notebook configuration cell before running the notebook.

---

## Ethics and Data Governance

This project uses **secondary environmental sensor data only**.

No human participants, surveys, interviews, observations, personal data or confidential organisational data were used. The project therefore remains aligned with the approved UREC1 ethics boundary.

Key controls:

- No primary data collection
- No personal data processing
- No user testing or participant feedback
- No fabricated data, metrics or feedback
- OpenAQ / Kaggle sources are acknowledged
- Provider-specific OpenAQ licensing and attribution are treated cautiously

---

## Methodology

The project follows an applied data-science and artificial-intelligence workflow.

| Layer | Project choice |
|---|---|
| Philosophy | Positivism |
| Approach | Deductive |
| Method choice | Quantitative mono-method |
| Strategy | Experimental machine-learning modelling |
| Time horizon | Longitudinal / time-series |
| Data collection | Secondary OpenAQ data only |
| Analysis | EDA, preprocessing, feature engineering, model training, validation, testing and explainability |

---

## Workflow

The end-to-end pipeline is:

1. Load OpenAQ data.
2. Audit schema and data quality.
3. Clean timestamps, missing values and coordinates.
4. Filter PM2.5 records.
5. Perform exploratory data analysis.
6. Engineer spatio-temporal features.
7. Create a chronological train/validation/test split.
8. Train baseline, machine-learning and CNN-LSTM models.
9. Evaluate using regression metrics and diagnostic plots.
10. Explain the final model using feature importance / SHAP-style analysis.
11. Save model artefacts, metrics, predictions and visual outputs.

---

## Preprocessing

The main preprocessing steps were:

- Parsed timestamps into datetime format.
- Removed rows with missing essential fields.
- Checked latitude and longitude bounds.
- Filtered valid pollutant and unit combinations.
- Grouped sparse city/source categories.
- Selected PM2.5 as the final forecasting target.
- Created station-level identifiers.
- Sorted records chronologically per station.
- Created next-reading target values.
- Generated lag and rolling-window features.
- Used chronological train/validation/test splitting to reduce time leakage.

---

## Feature Engineering

The notebook creates several groups of features:

### Temporal features

- hour
- weekday
- month
- day of year
- weekend indicator
- sine/cosine cyclic hour features

### Lag features

- PM2.5 lag 1
- PM2.5 lag 2
- PM2.5 lag 3
- PM2.5 lag 6
- PM2.5 lag 12
- PM2.5 lag 24

### Rolling features

- rolling mean
- rolling standard deviation
- 3, 6, 12 and 24-reading windows

### Spatial and source features

- country
- city
- latitude
- longitude
- source/provider grouping
- averaging period

---

## Models Implemented

| Model | Purpose |
|---|---|
| Naive Current-Value Baseline | Strong persistence benchmark: next PM2.5 equals current PM2.5 |
| Ridge Regression | Linear baseline for simple comparison |
| Random Forest Regressor | Nonlinear ensemble model selected as the final best model |
| Histogram Gradient Boosting | Boosted-tree comparison model |
| XGBoost | Scalable boosted-tree comparison model |
| CNN-LSTM | Deep-learning sequence comparison model |

---

## Evaluation Metrics

The project uses regression metrics because PM2.5 forecasting is a continuous-value prediction problem.

| Metric | Meaning |
|---|---|
| MAE | Mean Absolute Error; average absolute prediction error |
| RMSE | Root Mean Squared Error; penalises larger errors more strongly |
| R² | Explains how much variance the model captures |
| SMAPE | Symmetric percentage error, useful for relative forecasting comparison |
| Residual analysis | Shows actual minus predicted error patterns |
| Country/city MAE | Shows whether model error differs by location |

Classification metrics such as accuracy, precision, recall, F1-score, ROC-AUC and confusion matrix were not used because the final task is regression, not classification.

---

## Main Results

The selected final model was the **Random Forest Regressor**.

| Metric | Held-out test result |
|---|---:|
| MAE | 4.100 |
| RMSE | 11.798 |
| R² | 0.850 |

The Random Forest model outperformed the naive current-value baseline on the held-out test set. It performed well because recent PM2.5 values, lag features and rolling averages captured strong short-term persistence in the data.

The CNN-LSTM model learned sequence patterns but did not outperform the feature-engineered Random Forest on the common subset. This is likely because the open sensor data was uneven across countries, cities and monitoring stations.

---

## Explainability

Feature importance / SHAP-style analysis showed that the strongest predictors were:

- current PM2.5 value
- recent lag values
- rolling PM2.5 averages
- temporal features
- selected spatial features

This improves transparency because the final model is not treated as a black box. The explanation confirms that the model relies mainly on meaningful short-term pollution history.

---

## Visual Outputs

The notebook produces visual evidence such as:

- missing-value percentage by column
- rows by country
- rows by pollutant
- pollutant coverage by country
- PM2.5 distribution after cleaning
- average PM2.5 by country
- top cities by average PM2.5
- monthly PM2.5 trends
- station-level spatial PM2.5 intensity
- correlation heatmap
- target distribution across chronological splits
- validation RMSE and MAE by model
- held-out test RMSE comparison
- actual vs predicted PM2.5
- residual distribution
- absolute error distribution
- country and city prediction error analysis
- SHAP-style feature importance

---

## How to Run

### Option 1: Run in Kaggle

1. Open the main notebook in Kaggle.
2. Attach the OpenAQ dataset.
3. Check that the dataset path is correct.
4. Run all cells from top to bottom.
5. Download the saved output files from the Kaggle working directory.

### Option 2: Run locally

Clone the repository:

```bash
git clone https://github.com/35041152/openaq-pm25-spatiotemporal-forecasting.git
cd openaq-pm25-spatiotemporal-forecasting
```

Create and activate a virtual environment:

```bash
python -m venv .venv
```

For Windows:

```bash
.venv\Scripts\activate
```

For macOS/Linux:

```bash
source .venv/bin/activate
```

Install required packages:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost shap tensorflow joblib folium jupyter
```

Open the notebook:

```bash
jupyter notebook
```

Run the main notebook cell by cell.

---

## Reproducibility Notes

The repository includes saved model and output artefacts so that the results can be reviewed without rerunning the full pipeline.

Important reproducibility controls:

- chronological splitting rather than random splitting
- saved model metrics
- saved predictions
- saved trained pipeline
- saved CNN-LSTM model
- saved feature-importance outputs
- saved project summary file

---

## Limitations

The current project has some limitations:

- OpenAQ coverage is uneven across countries, cities, stations and pollutants.
- Extreme high-pollution spikes remain harder to predict than normal readings.
- The final implementation mainly uses OpenAQ ground-monitoring data.
- Direct satellite variables were treated as a future extension where they were not fully integrated in the final notebook.
- Results are based on the selected countries and available monitoring records, so they should not be overgeneralised to every city globally.

---

## Future Work

Future improvements could include:

- adding direct satellite-derived features from Sentinel-5P or MODIS
- adding weather variables such as wind, humidity and temperature
- training station-specific or country-specific models
- testing Transformer-based sequence models
- adding uncertainty intervals around predictions
- improving high-pollution spike detection
- building an interactive dashboard for non-participant demonstration

---

## AI Transparency Statement

AI support was used for project explanation, report structuring, language refinement, presentation preparation and debugging explanation.

The dataset selection, ethics boundary, notebook execution, model training, result interpretation, evaluation and final academic responsibility remained with the student.

AI was not used to fabricate data, model results, feedback or citations.

---

## Author

**Dodda Avinash Reddy**  
Student ID: **C5041152**  
MSc Data Science and Artificial Intelligence  
School of Computing and Digital Technologies  
Sheffield Hallam University

---

## Licence and Attribution

This repository is submitted for academic assessment. OpenAQ data and Kaggle-hosted resources should be used according to their original provider terms, licence conditions and attribution requirements.

Useful links:

- OpenAQ documentation: https://docs.openaq.org/
- OpenAQ licence information: https://docs.openaq.org/resources/licenses
- Kaggle OpenAQ dataset: https://www.kaggle.com/datasets/open-aq/openaq
