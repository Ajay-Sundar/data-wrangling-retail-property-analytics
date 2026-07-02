# Data Wrangling and Preprocessing for Retail & Property Analytics

A data wrangling and preprocessing project focused on identifying, correcting, and documenting data-quality issues in retail transaction data, followed by exploratory reshaping of Melbourne property data for future linear regression modelling.

This repository contains the final notebooks, Python script, processed CSV outputs, and supporting report produced for a university group project. The original submitted filenames have been retained so the implementation remains consistent with the generated outputs.

---

## Project Overview

The project contains two main analytical components:

1. **Retail transaction data cleansing**
   - Detect and correct invalid or inconsistent values
   - Impute missing data
   - identify and remove delivery-charge outliers
   - Validate business rules using derived values and regression-based checks

2. **Property data reshaping**
   - Examine whether selected Melbourne suburb variables require scaling or transformation
   - Compare normalisation and transformation approaches
   - Prepare numerical features for future linear regression analysis

The retail dataset represents fictional online electronics orders fulfilled from three Melbourne warehouses. Each order contains customer, cart, location, delivery, discount, review, and pricing information. The property dataset contains suburb-level housing, population, income, and demographic variables. fileciteturn4file0

---

## Key Objectives

### Retail data cleansing

- Explore the structure and quality of the supplied datasets
- Detect errors in dirty transaction records
- Impute missing values using appropriate analytical methods
- Remove outliers in the `delivery_charges` field
- Preserve the original schema in all processed outputs
- Validate corrected records against business rules and derived relationships

### Property data preprocessing

- Examine feature distributions, scales, and skewness
- Assess relationships between predictor variables and `median_house_price`
- Compare standardisation, min-max normalisation, logarithmic transformation, Box-Cox, and Yeo-Johnson transformation
- Improve feature suitability for later linear regression modelling

---

## Dataset Context

### Retail transaction data

Each retail record represents one customer order and includes fields such as:

- `order_id`
- `customer_id`
- `date`
- `nearest_warehouse`
- `shopping_cart`
- `order_price`
- `customer_lat`
- `customer_long`
- `coupon_discount`
- `distance_to_nearest_warehouse`
- `delivery_charges`
- `order_total`
- `season`
- `is_expedited_delivery`
- `latest_customer_review`
- `is_happy_customer`

The data-cleaning workflow uses several known business relationships:

- Customer-to-warehouse distance is calculated using the Haversine formula
- Delivery charges follow season-specific linear relationships
- Delivery cost depends on warehouse distance, expedited delivery status, and customer satisfaction
- Review sentiment is used to infer whether a customer is happy
- Coupon discounts are applied before delivery charges are added
- Product unit prices can be inferred from order totals and shopping-cart combinations

### Property data

The suburb-level dataset includes:

- `suburb`
- `number_of_houses`
- `number_of_units`
- `municipality`
- `aus_born_perc`
- `median_income`
- `median_house_price`
- `population`

The preprocessing work focuses on preparing the numerical variables for later linear regression modelling rather than building the final predictive model.

---

## Methodology

## 1. Exploratory Data Analysis

The notebooks begin with structural and statistical checks, including:

- Dataset dimensions and data types
- Missing-value counts
- Duplicate inspection
- Unique-value and category validation
- Summary statistics
- Distribution analysis
- Relationship checks between dependent fields
- Graphical inspection of numerical variables and potential outliers

These checks are used to identify which fields require rule-based correction, imputation, transformation, or removal.

## 2. Dirty Data Correction

The dirty dataset is corrected by validating fields against expected formats and business logic.

Examples of validation logic include:

- Date and season consistency
- Nearest warehouse verification
- Customer coordinate and warehouse-distance validation
- Shopping-cart and order-price consistency
- Coupon and order-total reconciliation
- Sentiment-derived customer satisfaction checks
- Detection of records that violate known pricing or delivery relationships

Where appropriate, corrected values are derived from other reliable fields rather than manually replaced.

## 3. Geospatial Feature Validation

Customer-to-warehouse distances are calculated using the Haversine formula with the supplied warehouse coordinates.

This supports:

- Verification of `nearest_warehouse`
- Validation of `distance_to_nearest_warehouse`
- Detection of incorrect customer-coordinate or warehouse assignments
- Delivery-charge modelling

## 4. Sentiment Analysis

Customer review sentiment is analysed using NLTK VADER.

A review is classified using its compound sentiment score, allowing `is_happy_customer` to be validated or reconstructed from `latest_customer_review`.

## 5. Delivery-Charge Modelling

Season-specific linear regression models are used to estimate expected delivery charges based on:

- Distance to the nearest warehouse
- Expedited delivery status
- Customer satisfaction status

The models support:

- Validation of delivery-charge patterns
- Detection of abnormal values
- Missing-value imputation where suitable
- Outlier identification in the delivery-charge dataset

## 6. Missing-Value Imputation

Missing values are handled using methods selected according to the field type and available dependencies.

Possible approaches include:

- Rule-based reconstruction
- Cross-field derivation
- Regression-based imputation
- Category inference
- Distribution-aware numerical imputation

The chosen method aims to preserve consistency with known relationships in the data.

## 7. Outlier Detection

Outlier analysis is applied specifically to `delivery_charges`.

Rows are assessed against expected seasonal delivery-charge behaviour using the trained regression models and residual analysis. Records with implausible deviations are removed from the final outlier-cleaned dataset.

## 8. Property Data Transformation

The property notebook compares multiple preprocessing strategies:

- Standardisation
- Min-max normalisation
- Log transformation
- Box-Cox transformation
- Yeo-Johnson transformation

The evaluation considers:

- Feature scale
- Skewness
- Distribution shape
- Linearity with `median_house_price`
- Suitability for later linear regression

---

## Technologies Used

### Programming and analysis

- Python
- Jupyter Notebook
- Pandas
- NumPy
- SciPy
- Scikit-learn

### Natural language processing

- NLTK
- VADER SentimentIntensityAnalyzer

### Visualisation

- Matplotlib
- Seaborn

### Data formats

- CSV
- Excel
- PDF

---

## Repository Contents

```text
data-wrangling-retail-property-analytics/
│
├── README.md
├── Group044_ass2_task1.ipynb
├── Group044_ass2_task2.ipynb
├── group044_ass2_task1.py
├── Group044_ass2_task3.pdf
├── Group044_dirty_data_solution.csv
├── Group044_missing_data_solution.csv
└── Group044_outlier_data_solution.csv
```

### File descriptions

| File | Description |
|---|---|
| `Group044_ass2_task1.ipynb` | Main notebook containing EDA, data-cleaning methodology, validation logic, missing-value treatment, and outlier analysis |
| `Group044_ass2_task2.ipynb` | Exploratory notebook comparing scaling and transformation methods for property data |
| `group044_ass2_task1.py` | Python export of the task 1 notebook |
| `Group044_dirty_data_solution.csv` | Corrected version of the dirty retail dataset |
| `Group044_missing_data_solution.csv` | Retail dataset with missing values imputed |
| `Group044_outlier_data_solution.csv` | Retail dataset after delivery-charge outliers were removed |
| `Group044_ass2_task3.pdf` | Supporting project declaration document |

---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/data-wrangling-retail-property-analytics.git
cd data-wrangling-retail-property-analytics
```

### 2. Create a virtual environment

#### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

#### macOS/Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install pandas numpy scipy scikit-learn nltk matplotlib seaborn openpyxl jupyter
```

### 4. Download the VADER lexicon

Run the following once in Python:

```python
import nltk
nltk.download("vader_lexicon")
```

### 5. Open the notebooks

```bash
jupyter notebook
```

Then open:

- `Group044_ass2_task1.ipynb`
- `Group044_ass2_task2.ipynb`

---

## Important Data Availability Note

The original raw assessment datasets are not included in this public repository.

This repository retains the final notebooks, Python script, processed outputs, and report for portfolio demonstration. Re-running the full workflow requires the original source files supplied for the university assessment, including the retail input datasets, warehouse data, and property workbook.

The processed CSV files are included to demonstrate the final output structure and data-cleaning results.

---

## Main Skills Demonstrated

- End-to-end data wrangling
- Exploratory data analysis
- Data-quality assessment
- Missing-value imputation
- Outlier detection
- Feature engineering
- Geospatial calculations
- Sentiment analysis
- Regression-based validation
- Data transformation and normalisation
- Reproducible notebook documentation
- Business-rule validation
- Preparation of data for machine learning

---

## Project Outcomes

The project produced:

- A corrected retail transaction dataset
- A missing-value-imputed dataset
- A delivery-charge outlier-cleaned dataset
- A documented and reproducible Python workflow
- A comparative analysis of data scaling and transformation methods
- A property dataset assessment focused on linear-model readiness

The overall workflow demonstrates how domain rules, statistical methods, NLP, geospatial calculations, and regression analysis can be combined to improve data reliability before downstream analytics or machine learning.

---

## Academic Project Notice

This project was completed as part of **FIT5196 Data Wrangling** at Monash University.

It was originally completed as a group assessment. The repository is shared for portfolio and learning purposes and retains the original submission filenames for consistency.

Assessment instructions, raw restricted datasets, AI interaction records, and the original submission archive are not included in the public repository.

Anyone currently enrolled in the same or a similar unit should not copy or submit this work as their own.

---

## Author

**Ajay Sundar Ramanathan**

- LinkedIn: [linkedin.com/in/ajay-sundar2909](https://www.linkedin.com/in/ajay-sundar2909/)
- GitHub: [github.com/Ajay-Sundar](https://github.com/Ajay-Sundar)

---

## License

This repository is intended for portfolio and educational viewing.

The original datasets and assessment materials remain subject to their respective ownership and usage conditions. Reuse of the code should respect academic integrity requirements and any applicable university policies.
