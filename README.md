# Data Science Projects

This repository contains a collection of data science, machine learning and analytics projects developed as part of my learning path and personal portfolio.

The projects cover different types of problems, including classification, regression, clustering, exploratory data analysis, predictive maintenance, industrial monitoring, renewable energy analytics and customer segmentation.

Most projects are structured as self-contained folders, usually including:

* a `src/` folder with the main notebook or source code;
* a `data/` folder with input data, processed data or model results when applicable;
* a `requirements.txt` file with the Python environment used in the project;
* project-specific documentation when available.

## Repository structure

~~~text
Data_Science_projects/
│
├── CARE_SCADA_Wind_Turbine/
├── Conveyor_Fault_Prediction/
├── customer_segmentation/
├── predictive_maintenance/
├── pump_predictive_maintenance/
├── Solar_power_generation/
├── Turbofan_HPC_Efficiency/
├── Wind_Turbine_Scada/
├── .gitignore
└── README.md
~~~

## Projects

### `CARE_SCADA_Wind_Turbine`

Project related to wind turbine SCADA data analysis.

The goal is to work with operational data from wind energy systems and extract useful patterns for monitoring, performance assessment or anomaly detection.

This project belongs to the industrial data analysis and renewable energy domain.

Typical use case:

* wind turbine SCADA data analysis;
* renewable energy monitoring;
* anomaly detection;
* operational performance analysis.

---

### `Conveyor_Fault_Prediction`

Machine learning project focused on fault prediction and classification in a conveyor or rotating machinery system.

The dataset includes operational variables such as speed, load, temperature, vibration and current, together with a fault label. The project is oriented toward predictive maintenance, where the objective is to classify or anticipate different types of mechanical faults from sensor-like tabular data.

Typical use case:

* supervised classification;
* fault diagnosis;
* mechanical condition monitoring;
* industrial predictive maintenance.

---

### `customer_segmentation`

Customer segmentation project based on unsupervised learning techniques applied to transactional retail data.

The project uses the Online Retail dataset, which contains invoice-level transactional information such as invoice number, stock code, product description, quantity, invoice date, unit price, customer identifier and country. The main objective is to transform raw transaction-level data into a customer-level analytical dataset and identify meaningful customer groups according to purchasing behaviour.

The analysis is based on an extended RFM approach using the following customer-level variables:

* `recency`: number of days since the customer's last purchase;
* `frequency`: number of unique invoices associated with the customer;
* `monetary`: total amount spent by the customer;
* `total_quantity`: total number of purchased units;
* `unique_products`: number of different products purchased.

Before modelling, the dataset is cleaned to keep only valid purchase transactions. Rows with missing customer identifiers, cancelled invoices, negative or zero quantities, non-positive prices and non-product transaction codes are removed. A new variable, `total_money`, is created as the product of quantity and unit price.

Since customer purchasing behaviour is highly skewed, a logarithmic transformation is applied to reduce the effect of extreme values. The variables are then standardised before applying clustering algorithms, ensuring that all features contribute fairly to the distance-based models.

Several unsupervised learning techniques are tested and compared:

* K-Means Clustering;
* Agglomerative Clustering;
* Gaussian Mixture Models;
* DBSCAN.

The final analysis shows that K-Means provides the clearest and most interpretable segmentation. The two-cluster solution achieves the best silhouette score and separates customers into two broad behavioural groups:

* low-value customers, with higher recency, lower frequency, lower monetary value and lower product variety;
* high-value customers, with more recent purchases, higher frequency, greater product variety and a much larger contribution to total revenue.

Additional K-Means configurations with three, four and five clusters are also analysed. Although these more granular solutions obtain lower silhouette scores, they provide a more detailed business view by separating inactive low-value customers, medium-value regular customers, recent customers with potential and very high-value customers.

The project concludes that the two-cluster solution is the most robust from a statistical point of view, while four- or five-cluster alternatives may be more actionable for marketing, retention and customer relationship management strategies.

Current contents include:

* `src/main.ipynb`: main notebook with data cleaning, feature engineering, exploratory analysis, clustering and final conclusions;
* `requirements.txt`: Python environment used for the project.

Typical use case:

* customer segmentation;
* RFM analysis;
* clustering;
* unsupervised learning;
* customer profiling;
* marketing analytics;
* business decision-making.

Main techniques used:

* data cleaning;
* feature engineering;
* exploratory data analysis;
* logarithmic transformation;
* standardisation;
* K-Means;
* Agglomerative Clustering;
* Gaussian Mixture Models;
* DBSCAN;
* silhouette analysis;
* cluster profiling.

---

### `predictive_maintenance`

General predictive maintenance project.

This folder contains work related to failure prediction or equipment condition monitoring. The project is focused on applying machine learning techniques to industrial data in order to detect patterns associated with degradation, abnormal behaviour or potential failures.

Typical use case:

* binary or multiclass classification;
* anomaly or failure detection;
* evaluation of predictive models;
* industrial maintenance decision support.

---

### `pump_predictive_maintenance`

Predictive maintenance project focused on pump failure detection and event-based alerting.

The project includes model validation outputs, operational alert summaries and final test results. It is oriented toward evaluating machine learning models not only with standard classification metrics, but also from an operational perspective: whether failures are detected early enough and whether the number of false alerts remains acceptable.

Current contents include:

* `src/main.ipynb`: main notebook with the modelling and evaluation workflow;
* `requirements.txt`: Python environment used for the project;
* `data/*.csv`: validation results, model comparison files, alert summaries and final test outputs;
* `Readme.md`: project-specific documentation.

The project is especially relevant for industrial monitoring scenarios where the key objective is not only to classify individual samples correctly, but to generate useful alerts before failure events.

Typical use case:

* predictive maintenance;
* time-series and event-based failure detection;
* alert generation;
* model selection under class imbalance;
* operational evaluation of false alerts and detected events.

---

### `Solar_power_generation`

Project related to solar power generation data.

The objective is to analyse solar energy production data and potentially model or predict power generation from historical or environmental variables. This type of project is linked to renewable energy analytics and forecasting.

Typical use case:

* exploratory data analysis;
* regression or forecasting;
* renewable energy monitoring;
* solar generation performance analysis.

---

### `Turbofan_HPC_Efficiency`

Regression and exploratory analysis project focused on predicting the isentropic efficiency of a turbofan High-Pressure Compressor from operating and performance variables.

The current version is focused on:

* exploratory data analysis;
* distribution and outlier analysis;
* correlation analysis;
* multicollinearity diagnostics;
* VIF-based feature elimination.

The project uses a tabular dataset with 11,971 rows and 17 numerical variables. The target variable is:

* `Isentr.HPCEfficiency`

The current status is exploratory analysis and feature diagnostics. No final production-ready machine learning model is included yet.

Typical use case:

* regression;
* aerospace and turbomachinery data analysis;
* feature correlation analysis;
* multicollinearity assessment;
* preparation for predictive modelling.

---

### `Wind_Turbine_Scada`

Project related to wind turbine SCADA data.

This folder is focused on the analysis of operational wind turbine data, with potential applications in performance monitoring, fault detection, anomaly detection or predictive maintenance.

Typical use case:

* SCADA data analysis;
* renewable energy monitoring;
* time-series or tabular data exploration;
* anomaly and performance assessment.

## Technical stack

The repository is mainly based on the Python data science ecosystem:

* Python
* Jupyter Notebook
* pandas
* NumPy
* scikit-learn
* matplotlib
* seaborn
* machine learning models for classification, regression and clustering

Some projects may include additional libraries depending on the specific modelling approach used.

## How to use this repository

Clone the repository:

~~~bash
git clone https://github.com/zacariasct4/Data_Science_projects.git
cd Data_Science_projects
~~~

Each project is intended to be run independently. Enter the project folder you want to work with:

~~~bash
cd project_folder_name
~~~

For example, to run the customer segmentation project:

~~~bash
cd customer_segmentation
~~~

Create and activate a virtual environment:

~~~bash
python -m venv .venv
~~~

On Windows:

~~~bash
.venv\Scripts\activate
~~~

On macOS/Linux:

~~~bash
source .venv/bin/activate
~~~

Install the project dependencies:

~~~bash
pip install -r requirements.txt
~~~

Then open the corresponding notebook:

~~~bash
jupyter notebook
~~~

or use VS Code/JupyterLab.

## Notes on data and generated files

Some large or local files are intentionally excluded from version control through `.gitignore`, including:

* virtual environments;
* IDE configuration files;
* Python cache files;
* local environment variable files;
* compressed archives;
* trained model artifacts;
* large raw datasets when applicable.

This keeps the repository focused on source code, notebooks, documentation and relevant processed outputs.

## Current focus

The repository is currently oriented toward applied data science projects with a strong emphasis on industrial, engineering and business analytics use cases, especially:

* predictive maintenance;
* fault detection;
* renewable energy analytics;
* mechanical and industrial monitoring;
* regression and classification workflows;
* clustering and customer segmentation;
* exploratory data analysis and model validation.

## Author

**Zacarías Conde Teruel**
