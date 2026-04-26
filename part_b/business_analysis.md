# Part B: Business Case Analysis

## Promotion Effectiveness at a Fashion Retail Chain

 

---

 

## B1. Problem Formulation

 

### (a) Machine Learning Problem Definition

This problem can be framed as a supervised machine learning regression task.

 

- Target variable: `items_sold`

- Input features: promotion type, store size, store location type, competition density, calendar attributes (month, festival flag, weekend indicator), and historical sales patterns.

- ML problem type: Regression.

 

Regression is appropriate because the objective is to predict a continuous numeric value (items sold) based on historical labeled data.

 

---

 

### (b) Why Items Sold Is a Better Target Than Revenue

Using items sold is more reliable than total revenue because revenue can be influenced by pricing strategies, discount depth, or premium product mix. Sales volume directly reflects customer demand and promotion effectiveness.

 

This illustrates the broader principle that target variables should be stable, interpretable, and closely aligned with the true business objective.

 

---

 

### (c) Alternative to a Single Global Model

Instead of a single global model, stores can be segmented by location type (urban, semi‑urban, rural) or store size, with separate models trained for each segment. This captures heterogeneity in customer behavior and promotion response across different store contexts.

 

---

 

## B2. Data and EDA Strategy

 

### (a) Data Integration and Dataset Grain

The raw data from transactions, store attributes, promotion details, and calendar tables would be joined using store_id and date keys.

 

The final dataset grain would be one row per store per month. Monthly aggregations would include total items sold, promotion type active, number of weekends, festival counts, and average competition density.

 

---

 

### (b) Exploratory Data Analysis Strategy

The following analyses would be performed:

1. Box plots of items sold by promotion type to assess effectiveness.

2. Monthly trend analysis to identify seasonality.

3. Scatter plots of store size versus average sales.

4. Correlation heatmaps to detect multicollinearity.

 

These insights would guide feature engineering and model selection.

 

---

 

### (c) Handling Promotion Imbalance

If 80% of records involve no promotion, the model may under‑learn promotion effects. This can be handled using stratified sampling, promotion indicators, or weighting techniques to ensure balanced learning.

 

---

 

## B3. Model Evaluation and Deployment

 

### (a) Train‑Test Split and Metrics

A time‑based split should be used, training on earlier months and testing on the most recent months. Random splitting is inappropriate due to temporal dependency.

 

Evaluation metrics include RMSE and MAE, which are interpretable as average errors in predicted sales units.

 

---

 

### (b) Explaining Model Recommendations

Feature importance can explain why different promotions are recommended for the same store at different times. Seasonal indicators, competition levels, or customer behavior patterns can shift promotion effectiveness month‑to‑month.

 

This explanation can be communicated to marketing teams to build trust in the model’s decisions.

 

---

 

### (c) Deployment and Monitoring

The trained model would be saved and used monthly to generate store‑level promotion recommendations. New data would be preprocessed using the same pipeline.

 

Monitoring would include tracking prediction error over time and retraining the model when performance degrades due to seasonality or market changes.

``
