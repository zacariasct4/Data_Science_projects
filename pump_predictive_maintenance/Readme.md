# Pump Predictive Maintenance

This repository presents a predictive maintenance workflow for early machine failure detection using minute-level multivariate sensor data.

The objective is not to classify the machine after it has already entered a `BROKEN` state. Instead, the goal is to identify pre-failure behaviour while the machine is still operating under `NORMAL` status and to generate operationally useful alerts before real breakdown events occur.

The project is structured as a report-style notebook. The main results are already computed and shown in the notebook, while trained model files are not included in the repository.

---

## Project Objective

The main objective is to detect whether a machine is approaching a real breakdown within the next 60 minutes.

The binary target is defined as:

```text
y_60m = 1  -> the machine is still in NORMAL status, but a breakdown will occur within the next 60 minutes
y_60m = 0  -> the machine is in NORMAL status and no breakdown occurs within the next 60 minutes
```

Rows where the machine is already in `BROKEN` or `RECOVERING` status are excluded from the modelling dataset. This makes the task closer to a real early-warning problem: the model must detect pre-failure behaviour before the breakdown is officially observed.

The project evaluates models not only with standard row-level classification metrics, but also with operational alert metrics:

* real breakdown events detected;
* false alerts per day;
* event-alert precision;
* first useful alert time;
* lead time before failure;
* event recall with minimum lead-time constraints.

This framing is more realistic for predictive maintenance than a conventional timestamp-level binary classification benchmark.

---

## Repository Structure

```text
PUMP_PREDICTIVE_MAINTENANCE/
│
├── data/
│   ├── sensor.csv
│   ├── all_model_results_validation.csv
│   ├── best_final_configs_validation.csv
│   ├── final_comparison_with_sequence_nn.csv
│   ├── final_event_alert_summary.csv
│   ├── final_event_timing_summary.csv
│   ├── final_full_alert_summary.csv
│   ├── final_model_configs.csv
│   ├── final_test_results.csv
│   ├── logistic_best_operational_validation.csv
│   ├── logistic_tuning_results_validation.csv
│   ├── logistic_undersampling_best_validation.csv
│   ├── model_selection_results_validation.csv
│   ├── models_no_undersampling_best_validation.csv
│   ├── models_undersampling_best_validation.csv
│   ├── nn_no_undersampling_best_validation.csv
│   ├── nn_undersampling_best_validation.csv
│   ├── pca_logistic_best_operational_validation.csv
│   ├── pca_logistic_tuning_results_validation.csv
│   ├── sequence_conv1d_best_validation.csv
│   ├── sequence_conv1d_test_results.csv
│   └── sequence_conv1d_thresholds_validation.csv
│
├── models/
│   └── [local trained models are not tracked]
│
├── src/
│   └── main.ipynb
│
├── requirements.txt
└── README.md
```

The `models/` directory is used locally to store trained estimators and neural-network artifacts. These files are intentionally not included in the repository because they are generated outputs and can be large.

The `data/` directory may include selected result CSV files so that the notebook can be inspected as a report without retraining all experiments.

---

## Dataset

The project uses `sensor.csv`, a time-series dataset containing:

* a `timestamp` column;
* a `machine_status` column;
* multiple sensor variables named as `sensor_*`.

The raw dataset should be placed in:

```text
data/sensor.csv
```

If the dataset license does not allow redistribution, the raw file should not be committed to GitHub. In that case, users must download it separately and place it in the `data/` directory before running the notebook.

---

## Methodology

### 1. Exploratory Data Analysis

The initial EDA examines:

* time coverage and timestamp regularity;
* duplicated timestamps and temporal gaps;
* missing values by sensor;
* machine status distribution;
* real breakdown events;
* pre-failure windows;
* sensor behaviour before failures;
* redundant or low-information variables.

This analysis guides the cleaning decisions, target definition and feature-set construction.

### 2. Target Definition

The target is created from real `BROKEN` timestamps. For each breakdown, the previous 60 minutes of `NORMAL` operation are labelled as positive.

This converts the problem into an extremely imbalanced binary classification task, where the positive class represents short-term pre-failure behaviour.

### 3. Data Preparation and Feature Engineering

The preprocessing pipeline is designed to preserve temporal causality and avoid leakage.

Main decisions:

* `sensor_15` is removed because it does not provide useful information.
* `sensor_50` is removed as a continuous variable due to excessive missingness.
* A binary `sensor_50_missing` indicator is retained.
* Missing values are imputed using past rolling medians within continuous `NORMAL` segments.
* Temporal segments are created so rolling features do not cross gaps caused by removed `BROKEN` and `RECOVERING` periods.
* The data are split chronologically into train, validation and test.
* Feature selection is performed using only the training period.

The project initially constructs several candidate feature sets:

| Feature set               | Description                                                                |
| ------------------------- | -------------------------------------------------------------------------- |
| `A_raw_sensors`           | Raw cleaned sensor variables.                                              |
| `B_raw_plus_missing`      | Raw sensors plus the `sensor_50_missing` indicator.                        |
| `C_raw_missing_rolling`   | Raw sensors, missingness indicator and rolling-window features.            |
| `D_priority_rolling`      | Priority sensors and their rolling-window features.                        |
| `E_low_corr`              | Compact low-correlation representation selected using the training period. |
| `F_low_corr_plus_rolling` | Low-correlation representation including temporal engineered features.     |

A screening stage is then used to reduce the search space. The later operational experiments focus mainly on:

* `B_raw_plus_missing`, as the raw-feature baseline;
* `E_low_corr`, as the compact low-redundancy representation.

This keeps the notebook computationally manageable and makes the comparison easier to interpret.

### 4. Modelling Strategy

The modelling strategy follows a staged validation-based workflow.

First, lightweight model and feature-set screening is performed using validation metrics. Then, selected configurations are evaluated with operational alert metrics.

The evaluated models include:

* Logistic Regression;
* PCA + Logistic Regression;
* Extra Trees;
* Random Forest;
* HistGradientBoosting;
* XGBoost;
* Linear SVC;
* Balanced Random Forest;
* Easy Ensemble;
* regularized MLP;
* focal-loss MLP.

The project evaluates:

* models trained on the original class distribution;
* models trained with external temporal undersampling;
* internally balanced ensemble models.

For undersampling experiments, different negative-to-positive ratios are tested. The validation period is used to select the best combination of:

* model family;
* feature set;
* undersampling strategy;
* undersampling ratio;
* decision threshold.

The final test period is not used for model or threshold selection.

### 5. Evaluation Approach

The evaluation combines row-level metrics with event-level operational metrics.

Standard classification metrics:

* precision;
* recall;
* F1-score;
* PR-AUC;
* ROC-AUC;
* confusion matrix values.

Operational metrics:

* total breakdown events;
* detected events;
* missed events;
* event recall;
* total alerts;
* false alerts;
* false alerts per day;
* event-alert precision;
* first correct alert time;
* lead time before failure;
* event recall with at least 15, 30 and 60 minutes of lead time.

A 60-minute suppression window is applied to avoid counting repeated alerts generated too close in time.

This is important because, in predictive maintenance, a model is not useful simply because it classifies individual timestamps well. It must generate alerts that are actionable, timely and not excessively frequent.

---

## Final Test Results

The final test evaluation shows a clear operational trade-off.

Some validation-selected tabular configurations detect both test failures, but they generate many false alerts. Other configurations produce a much cleaner alert profile, but miss one of the two failures.

The strongest practical representation is `E_low_corr`, which appears consistently across the best-performing tabular configurations. This suggests that a compact low-redundancy feature representation generalizes better than larger rolling-feature sets in this dataset.

The final comparison also includes an exploratory Conv1D sequence model using raw 240-minute temporal windows. This sequence model detects one of the two test failures, but generates a very high number of false alerts. Therefore, it is not operationally competitive with the best tabular configurations in its current form.

The main conclusion is that the dataset contains detectable pre-failure signal, but the trade-off between event recall and false-alert frequency remains unresolved.

---

## Sequence-Based Neural Network Baseline

A Conv1D neural network is evaluated as an exploratory sequence-based baseline.

Unlike the tabular models, which use engineered features, the Conv1D receives raw temporal windows directly:

```text
input shape = (window_size, number_of_sensor_channels)
window_size = 240 minutes
```

The model is trained with capped class weighting and its threshold is selected on the validation period.

The Conv1D experiment is useful because it tests whether raw temporal trajectories contain additional signal. However, the results show that the current Conv1D baseline is too sensitive and produces excessive false alerts.

It should therefore be interpreted as a proof of concept, not as the recommended final model.

---

## Main Findings

The main findings are:

1. Early failure signal is present in the sensor data, but it is difficult to isolate cleanly.
2. Compact low-correlation features generalize better than simply increasing the number of engineered features.
3. Full event detection is possible, but usually at the cost of many false alerts.
4. Conservative models can reduce false alerts substantially, but may miss failures.
5. The Conv1D sequence model captures some temporal signal but is not yet operationally viable.
6. The current system is best interpreted as a short-term warning prototype, not a deployment-ready predictive-maintenance solution.

---

## How to Run the Project

### 1. Clone the repository

```bash
git clone <repository-url>
cd PUMP_PREDICTIVE_MAINTENANCE
```

### 2. Create and activate a virtual environment

On Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\activate
```

On macOS or Linux:

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Place the raw dataset here:

```text
data/sensor.csv
```

If the raw dataset is not included in the repository, it must be downloaded separately and copied into the `data/` directory.

### 5. Open the notebook

```bash
jupyter notebook src/main.ipynb
```

or open `src/main.ipynb` directly in VS Code.

---

## Report-Style Notebook and Cached Results

This repository is intended to be published as a report-style project. The notebook contains the final outputs and interpretation of the experiments.

Trained model files are not included in the repository. This means that the `models/` directory is local-only and should not be tracked by GitHub.

Some CSV files in `data/` may be included if they are needed to display the final result tables without retraining all models. These CSV files are cached outputs from previous runs and are useful because several experiments are computationally expensive.

If you want to fully regenerate all results from scratch, you may need to rerun the corresponding training cells in the notebook. If a cell attempts to load a model artifact that is not available, either rerun the training cell that generates it or skip that section if you only want to inspect the report outputs.

Recommended GitHub policy:

```text
Track:
- src/main.ipynb
- README.md
- requirements.txt
- selected CSV result files needed by the report notebook

Do not track:
- .venv/
- models/*.pkl
- models/*.keras
- __pycache__/
- .ipynb_checkpoints/
```

---

## Recommended `.gitignore`

```gitignore
# Python
__pycache__/
*.py[cod]
*.pyo

# Virtual environments
.venv/
venv/
env/

# Jupyter
.ipynb_checkpoints/

# Local trained model artifacts
models/*.pkl
models/*.keras

# Optional raw data
# Uncomment if the dataset cannot be redistributed
# data/sensor.csv

# OS files
.DS_Store
Thumbs.db
```

---

## Limitations

The main limitations are:

* only seven real breakdown events are available;
* validation contains one breakdown event and test contains two;
* positive samples are highly correlated because they come from short pre-failure windows;
* thresholds are sensitive to the desired false-alert tolerance;
* full event recall currently requires too many false alerts;
* the sequence-based neural baseline is not yet selective enough;
* results may not generalize to other machines, operating conditions or failure types.

---

## Future Work

The most relevant future improvements are:

* evaluate wider horizons such as `y_120m` and `y_240m`;
* apply event-based cross-validation, such as leave-one-failure-out;
* test Temporal Convolutional Networks and CNN-GRU models;
* reduce sequence-model input channels to the most informative sensors;
* evaluate temporal autoencoders trained on stable normal behaviour;
* add cost-based evaluation for missed failures and false alerts;
* validate the approach on additional machines or datasets with more failure events;
* design an event-level alert aggregation layer to reduce false alarms.

---

## Final Recommendation

The current pipeline should be considered an analytical prototype for short-term failure warning.

The recommended baseline for further work is the tabular pipeline using the compact `E_low_corr` feature representation, combined with validation-based threshold selection and operational event-level evaluation.

The Conv1D sequence model should not be used as the final model in its current form, but it motivates further sequence-based experiments focused on reducing false alerts and improving lead time.

Before any deployment, the system should be validated on more breakdown events and the acceptable trade-off between missed failures and false alerts should be defined with domain experts.
