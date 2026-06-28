# Customer Segmentation with Online Retail Data

## Project Overview

This project performs a customer segmentation analysis using the **Online Retail** dataset, a transactional dataset containing invoices, products, quantities, prices, customer identifiers, invoice dates and countries.

The main objective is to transform raw transaction-level data into meaningful customer-level segments that can support marketing, retention and customer relationship management decisions. The analysis follows a complete workflow: data cleaning, exploratory data analysis, feature engineering, RFM-based customer representation, clustering, model comparison, business interpretation, conclusions and limitations.

## Dataset

The dataset used in this project is available on Kaggle:

<https://www.kaggle.com/datasets/tunguz/online-retail>

The original dataset includes the following main variables:

- `InvoiceNo`: invoice identifier.
- `StockCode`: product identifier.
- `Description`: product description.
- `Quantity`: quantity purchased.
- `InvoiceDate`: transaction date and time.
- `UnitPrice`: unit price of the product.
- `CustomerID`: customer identifier.
- `Country`: customer country.

## Project Objectives

The main goals of the project are:

- Clean the raw transactional data and remove records that do not represent valid customer purchases.
- Build customer-level behavioural variables using an RFM-based approach.
- Identify customer segments using unsupervised learning techniques.
- Compare several clustering approaches and evaluate their interpretability.
- Translate the technical clustering results into business-oriented customer segments.
- Discuss the strengths and limitations of the final segmentation.

## Methodology

### 1. Data Cleaning

The dataset was cleaned to retain only valid purchase records. The following records were removed:

- Rows without a valid `customer_id`.
- Rows with `quantity <= 0`, which usually represent returns, cancellations or stock adjustments.
- Rows with `unit_price <= 0`, since they do not represent standard commercial transactions.
- Cancelled invoices, identified by invoice numbers starting with `C`.
- Non-product transaction codes, such as postage, bank charges or manual adjustments.

A new variable, `total_money`, was created as:

```python
total_money = quantity * unit_price
```

This variable represents the monetary value of each transaction line.

### 2. Customer-Level Feature Engineering

Since customer segmentation must be performed at customer level, the original transaction-level dataset was aggregated by `customer_id`.

The following variables were created:

- `recency`: number of days since the customer's last purchase.
- `frequency`: number of unique invoices associated with the customer.
- `monetary`: total amount spent by the customer.
- `total_quantity`: total number of units purchased.
- `unique_products`: number of different products purchased.

This RFM-based representation summarises each customer's purchasing behaviour and provides the basis for the clustering analysis.

### 3. Feature Transformation and Scaling

The customer-level variables showed strong right-skewness and the presence of outliers. Some customers spent or purchased significantly more than the majority of the customer base.

To reduce the impact of extreme values, a logarithmic transformation was applied:

```python
X_log = np.log1p(X)
```

After that, the variables were standardised using `StandardScaler`:

```python
X_scaled = StandardScaler().fit_transform(X_log)
```

This step ensures that all features contribute more equally to distance-based clustering algorithms.

### 4. Clustering Models Tested

Several unsupervised learning techniques were tested:

- **K-Means Clustering**
- **Agglomerative Clustering**
- **Gaussian Mixture Models**
- **DBSCAN**

K-Means was selected as the main model because it provided the clearest and most interpretable segmentation.

## Main Results

### K-Means with Two Clusters

The two-cluster K-Means solution achieved the best silhouette score among the tested K-Means configurations.

This solution separates the customer base into two broad groups:

- A larger low-value segment with lower frequency, lower monetary value and higher recency.
- A smaller high-value segment with more recent purchases, higher frequency, higher spending, greater quantity purchased and more product variety.

In this solution, the high-value segment represents a smaller share of the customer base but generates the majority of total revenue. This confirms that customer value is highly concentrated in a relatively small group of customers.

### K-Means with More Clusters

K-Means was also tested with three, four and five clusters.

Although the silhouette score decreased when increasing the number of clusters, the additional segmentations provided more business granularity. In particular, the four- and five-cluster solutions made it possible to distinguish between:

- Very high-value customers.
- Medium-value regular customers.
- Recent customers with potential.
- Inactive or low-value customers.

Therefore, the two-cluster solution is the most robust from a statistical point of view, while the four- and five-cluster solutions may be more useful for designing differentiated business actions.

### DBSCAN Analysis

DBSCAN was tested because it does not require predefining the number of clusters. However, it did not outperform K-Means for this dataset.

Some DBSCAN configurations produced high silhouette scores, but they classified a very large proportion of customers as noise. Other configurations reduced the amount of noise but produced one dominant cluster containing most of the customer base.

For this reason, DBSCAN was considered less suitable as the final segmentation model, although the noise group can still be useful for identifying atypical customers.

## Business Interpretation

The segmentation can support different business strategies:

### High-Value Customers

These customers should be prioritised because they generate a disproportionate share of total revenue.

Possible actions:

- Loyalty programmes.
- Personalised recommendations.
- Early access to new products.
- Dedicated retention campaigns.
- Premium customer service.

### Medium-Value Customers

These customers already show purchasing activity but have potential to increase their value.

Possible actions:

- Cross-selling campaigns.
- Product bundles.
- Personalised discounts.
- Recommendations based on previous purchases.

### Low-Value or Inactive Customers

These customers represent a large share of the customer base but contribute less to revenue.

Possible actions:

- Low-cost reactivation campaigns.
- Email marketing.
- Discount codes.
- Win-back campaigns.

### Atypical Customers

Some customers show unusual purchasing behaviour. These customers may correspond to wholesale buyers, one-off large purchases or isolated behaviour patterns.

Possible actions:

- Analyse separately from the general customer base.
- Review manually if they represent strategic accounts.
- Consider excluding them from general-purpose marketing segmentation.

## Conclusions

This project shows how customer segmentation can be built from transactional data using an RFM-based approach and unsupervised learning techniques.

The analysis confirms that the customer base is highly heterogeneous. Most customers purchase infrequently and generate relatively low revenue, while a smaller group of customers accounts for most of the business value.

K-Means provided the most interpretable and stable segmentation. The two-cluster solution offered the best statistical separation, while more granular solutions with four or five clusters provided richer business insights. Therefore, the final choice of segmentation depends on the objective: statistical robustness or business actionability.

Overall, the project demonstrates that customer segmentation can help identify valuable customers, prioritise retention efforts, design targeted campaigns and improve customer relationship management.

## Limitations

Several limitations should be considered:

- The analysis is based only on transactional data. Demographic, behavioural, marketing and satisfaction variables are not available.
- The dataset covers a limited historical period, so the segments should be interpreted as a snapshot of customer behaviour.
- The RFM variables summarise purchasing behaviour but do not fully capture customer preferences or future intentions.
- The dataset contains strong outliers, which may correspond to wholesale customers or atypical transactions.
- Clustering is an unsupervised technique, so there is no true label to validate the segments.
- Internal metrics such as silhouette score do not necessarily guarantee business usefulness.
- The analysis does not test whether applying different strategies to each segment improves business outcomes.

In a real business context, the next step would be to validate the segmentation through marketing experiments, campaign performance analysis or A/B testing.

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

## How to Run the Project

1. Clone the repository:

```bash
git clone <repository-url>
cd <repository-name>
```

2. Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # macOS/Linux
.venv\Scripts\activate     # Windows
```

3. Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl scipy jupyter
```

4. Place the dataset in the expected data folder, for example:

```text
data/Online Retail.xlsx
```

5. Open the notebook:

```bash
jupyter notebook
```

Then run the notebook cells sequentially.

## Suggested Repository Structure

```text
customer-segmentation-online-retail/
│
├── data/
│   └── Online Retail.xlsx
│
├── notebooks/
│   └── main.ipynb
│
├── README.md
└── requirements.txt
```

## Future Work

Possible extensions of this project include:

- Adding product-level analysis by segment.
- Analysing the geographical distribution of customer segments.
- Creating a dashboard to visualise customer segments.
- Testing additional clustering algorithms.
- Building a customer lifetime value model.
- Validating segments with marketing campaigns or A/B testing.
