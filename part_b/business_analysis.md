Promotion Effectiveness at a Fashion Retail Chain

B1. Problem Formulation

(a) Framing the Problem as a Machine Learning Task

This problem can be framed as a supervised machine learning regression problem.

The target variable is the number of items sold by a given store in a given month. This directly reflects customer demand and promotion effectiveness.

The candidate input features would include:

Promotion type (Flat Discount, BOGO, Free Gift, Category Offer, Loyalty Points),
Store characteristics (store size, location type, competition density),
Time‑related features (month, season, festival presence, proportion of weekends),
Historical sales patterns for the store.
This is a regression problem because the outcome is continuous and numeric, and there is historical labelled data mapping store conditions and promotions to observed sales outcomes. The goal is to estimate expected sales volume under different conditions rather than purely classify outcomes.

(b) Why “Items Sold” Is a Better Target than Revenue

While the company currently tracks performance using total revenue, items sold is a more appropriate target for this problem.

Revenue is influenced by factors such as pricing strategy, discount depth, premium versus budget product mix, and clearance sales. These factors can obscure the true impact of a promotion. For example, a luxury item sale may increase revenue significantly without reflecting broader customer engagement.

Items sold provides a cleaner and more stable signal of customer response to promotions. It directly measures how many products customers are willing to buy, which better aligns with the goal of evaluating promotion effectiveness.

This illustrates a broader principle in real‑world machine learning:
the best target variable is the one that most directly captures the decision you are trying to optimise, not necessarily the metric that looks best on financial reports.

(c) Rethinking a Single Global Model

Using a single global model across all 50 stores risks averaging away important local differences.

A better strategy would be to adopt a segmented or hierarchical modelling approach. For example:

Train separate models for urban, semi‑urban, and rural stores, or
Use a global model with interaction terms between promotion type and store attributes.
This allows the model to capture the fact that customer sensitivity to promotions differs significantly by location, store size, and competitive environment, leading to more realistic and actionable recommendations.

B2. Data and EDA Strategy

(a) Data Integration and Dataset Grain

The raw data arrives in four separate tables: transactions, store attributes, promotion details, and a calendar table.

These tables would be joined using store identifiers and date keys. Before modelling, the data would be aggregated so that the final grain of the dataset is:

One row per store per month.

Typical aggregations would include:

Total items sold per store per month,
Promotion type active during that month,
Number or proportion of festival and weekend days,
Monthly averages or summaries of competition density and footfall proxies.
This aggregation aligns the dataset with the monthly decision‑making cycle of the marketing team.

(b) Exploratory Data Analysis Plan

Before modelling, several EDA steps are essential:

Items sold by promotion type (box plots):
To understand variability and identify promotions that consistently outperform others.
Seasonality analysis (line charts by month):
To detect recurring peaks during festivals, sales seasons, or holidays.
Store segmentation analysis:
Scatter plots comparing store size or competition density against average sales levels.
Correlation analysis:
To identify strongly correlated features and potential multicollinearity issues.
Findings from EDA would directly influence feature engineering decisions, such as creating interaction terms or separating models by store category.

(c) Impact of Promotion Imbalance

If 80% of transactions occur without promotions, the model may become biased toward non‑promotion outcomes and under‑estimate the effect of promotions.

To address this:

Ensure balanced representation of promotion months during training,
Include explicit promotion indicator features,
Potentially apply weighting techniques so promotional periods are not drowned out.
This helps the model learn promotion effects more accurately despite skewed data.

B3. Model Evaluation and Deployment

(a) Train‑Test Split and Evaluation Metrics

Because the dataset spans three years of monthly store‑level data, a time‑based train‑test split should be used. The model would be trained on earlier periods and tested on the most recent months.

A random split would be inappropriate because it would leak future information into training, producing unrealistically optimistic results.

Appropriate evaluation metrics include:

MAE, which provides an intuitive average error in units of items sold,
RMSE, which penalises large forecasting errors that are particularly costly in retail planning.
(b) Explaining Promotion Recommendations

If the model recommends different promotions for the same store in different months, feature importance and feature contribution analysis can explain why.

For example:

A Loyalty Points Bonus in December may be driven by high seasonal demand, festivals, and regular customers,
A Flat Discount in March may be influenced by lower demand, higher competition, or weaker footfall.
Communicating these drivers to the marketing team builds trust by showing that recommendations are grounded in observable patterns rather than arbitrary model decisions.

(c) Deployment and Monitoring Strategy

Once trained, the model would be saved and reused to generate monthly recommendations without retraining each time.

Each month:

New store, promotion, and calendar data is prepared using the same preprocessing pipeline,
The model generates predicted items sold for each promotion option per store,
The best‑performing promotion is selected.
Ongoing monitoring would compare predicted versus actual sales. Sustained increases in error or systematic bias would trigger model retraining, ensuring long‑term reliability.
