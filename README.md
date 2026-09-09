# E-Commerce Data Analysis & Customer Intelligence

An end-to-end data science project analyzing e-commerce transactions to uncover sales trends, customer behavior, profitability patterns, customer segments, and future customer value.

## Project Overview

![E-Commerce Analysis Dashboard](E-Commerce%20Data%20Analysis_page.jpg)

This project analyzes 51,290 e-commerce transactions to understand business performance and customer purchasing behavior.

The analysis combines exploratory data analysis, customer-level feature engineering, RFM-based customer segmentation, and supervised machine learning to generate actionable business insights.

## Machine Learning Results

### RFM Customer Segmentation

![RFM Customer Segmentation](images/rfm_segmentation.png)

### Future Customer Value - Confusion Matrix

![Future Customer Value Confusion Matrix](images/future_value_confusion_matrix.png)

### Future Customer Value - Feature Importance

![Future Customer Value Feature Importance](images/future_value_feature_importance.png)

## Objectives

- Analyze overall sales, profit, and quantity performance.
- Identify trends across product categories and time.
- Analyze customer purchasing behavior and profitability.
- Perform RFM-based customer segmentation using K-Means.
- Evaluate clustering quality using the Silhouette Score.
- Predict future customer value using historical purchasing behavior.
- Evaluate future customer value predictions using F1 Score and ROC-AUC.
- Identify important historical behavioral factors associated with future customer value.
- Translate analytical results into business recommendations.

## Dataset

The project uses an e-commerce transaction dataset containing:

- **51,290 transactions**
- **1,590 unique customers**
- **10,292 unique products**
- **3 product categories**
- **13 regions**
- Sales, profit, quantity, discount, shipping cost, and order information

### Dataset Source

The dataset is included in the repository as `ECOMM DATA.xlsx`.

It was inherited from the original open-source e-commerce analysis project and is used here as the underlying dataset for an independent extension focused on customer analytics and machine learning.

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook
- Excel
- Power BI

## Analysis Workflow

### 1. Data Preparation

- Loaded and inspected the dataset.
- Checked missing values.
- Checked duplicate records.
- Converted date columns to datetime format.
- Prepared customer-level analytical features.

### 2. Exploratory Data Analysis

Analyzed:

- Overall sales and profit.
- Sales and profit by category.
- Monthly sales and profit trends.
- Top-performing products.
- Customer sales and profitability.
- Order frequency and purchase quantity.

Order-level metrics such as Average Order Value and Average Profit per Order were calculated after aggregating transaction lines by `Order ID`.

### 3. RFM Customer Segmentation

Customer behavior from **2011–2013** was used to create RFM-style features:

- **Recency:** Days since the customer's last purchase.
- **Frequency:** Number of orders placed.
- **Monetary:** Historical customer sales.

The features were standardized before applying K-Means clustering.

K-Means clustering was evaluated for K=2 through K=6 using the Silhouette Score.

The best configuration was:

- **K = 3**
- **Silhouette Score = 0.5366**

The resulting segments were:

| Segment | Customers | Behavioral Profile |
|---|---:|---|
| High-Value Loyal | 726 | Recent, frequent purchases and high spending |
| Mid-Value / Developing | 626 | Moderate purchase frequency and spending |
| Inactive / At-Risk | 144 | Low purchase frequency and long time since last purchase |

The segment labels are assigned based on the actual behavioral characteristics of each cluster rather than relying on arbitrary K-Means cluster IDs.

### 4. Future Customer Value Prediction

A Random Forest classifier was developed to predict whether a customer would become high-value in the future based on historical purchasing behavior.

To avoid same-period target leakage, customer behavior from **2011–2013** was used as the model input, while customer sales during **2014** were used to define the future target.

The target was defined using the **median customer sales in 2014 ($1,976.07)**. Customers with sales at or above this value were classified as high-value.

Historical features used:

- Past Sales
- Past Profit
- Past Quantity
- Past Orders
- Recency Days

The analysis included **1,496 customers** who appeared in both the historical and future periods.

### 5. Model Performance

The Random Forest model was evaluated on a held-out 2014 test set.

| Metric | Result |
|---|---:|
| Accuracy | **86.33%** |
| Precision | **86.58%** |
| Recall | **86.00%** |
| F1 Score | **86.29%** |
| ROC-AUC | **0.8669** |

Stratified 5-fold cross-validation was performed on the training data only:

- **Mean Cross-Validation F1:** 85.50%
- **Standard Deviation:** 2.40%

The similar cross-validation and held-out test performance indicates reasonable generalization to unseen customer outcomes.

### 6. Feature Importance

The most influential historical features were:

1. **Past Quantity:** 33.71%
2. **Past Orders:** 23.22%
3. **Past Sales:** 19.83%
4. **Past Profit:** 13.68%
5. **Recency Days:** 9.56%

The results suggest that historical purchase volume and order frequency are important signals for identifying customers who are likely to generate higher sales in the future.

## Business Impact of Prediction Errors

Prediction errors can have different business consequences:

- **False Negative:** A customer who becomes high-value in 2014 is predicted as low-value, potentially causing the business to miss retention or loyalty opportunities.
- **False Positive:** A customer who does not become high-value is predicted as high-value, potentially allocating marketing resources to a lower-value customer.

For customer retention campaigns, false negatives may be particularly important because failing to identify a valuable customer can result in a missed engagement opportunity.

The appropriate prediction threshold would ultimately depend on the relative business cost of false positives and false negatives.

## Key Business Insights

- Technology generated the highest overall sales and profit among the three product categories.
- Customer purchasing behavior varies significantly across the customer base.
- RFM analysis identified distinct groups based on recency, purchase frequency, and historical spending.
- High-Value Loyal customers showed substantially higher purchase frequency and spending than other segments.
- Inactive / At-Risk customers had significantly longer periods since their last purchase.
- Historical purchase quantity and order frequency were the strongest features in the future customer value model.
- Customer segmentation and future value prediction can support targeted marketing and retention strategies.

## Business Recommendations

- Provide loyalty rewards and personalized offers to High-Value Loyal customers.
- Use targeted re-engagement campaigns for Inactive / At-Risk customers.
- Encourage Mid-Value / Developing customers to increase purchase frequency and basket size.
- Prioritize customers predicted to become high-value for retention and personalized marketing initiatives.
- Use customer analytics to allocate marketing resources more efficiently.

## Modeling Limitation

The future customer value model is evaluated among customers who appear in both the historical and future periods.

Therefore, the model predicts **future customer value among returning customers** rather than predicting whether a customer will return in the first place.

## Project Structure

```text
E-Commerce-Data-Science/
│
├── analysis/
│   └── ecommerce_analysis.ipynb
│
├── images/
│   ├── rfm_segmentation.png
│   ├── future_value_confusion_matrix.png
│   └── future_value_feature_importance.png
│
├── Data & Resources/
│   └── ECOMM DATA.xlsx
│
├── README.md
├── requirements.txt
└── .gitignore