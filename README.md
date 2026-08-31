# Explainable-AI

# Team Name: mos

## Contributors
Matthew Fischer, Ogechukwu Ezenwa, Supriya Nannapaneni

## Dataset

The Telco Customer Churn dataset contains customer-level data and the task is to predict if a customer will stay or leave based on demographic information, services that each customer has signed up for, and customer account information.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| **Linear regression** | Linearity, multicollinearity, independence, homoscedasticity, normality, and influential observations | Correlation matrix and VIF were used to assess multicollinearity. VIF(`tenure`) = **6.33** and VIF(`TotalCharges`) = **8.09**. Observations were treated as independent. Homoscedasticity, normality of residuals, and influential observations were assessed after model fitting. | **Moderate/high multicollinearity** between `tenure` and `TotalCharges`. Additionally, the binary churn outcome is a limitation of OLS. |
| Logistic regression | Binary outcome, sufficient sample size, independence of observations, multicollinearity | Values of outcome variable, value counts of each variable, Durbin-Watson test, correlation matrix | Total charges has high multicollinearity with tenure (0.825880), also linear log-odds is not checked. |
| GAM |  |  |  |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| **Linear regression** | R² = **0.276**, Adjusted R² = **0.274**, F-statistic = **157.0**, p < **0.001** | Coefficients are straightforward to interpret as changes in predicted churn probability, making individual predictor effects easy to communicate. | OLS is not ideal for a binary outcome and may produce predictions outside the [0, 1] probability range. Multicollinearity between `tenure` and `TotalCharges` also complicates coefficient interpretation. |
| Logistic regression |  |  |  |
| GAM |  |  |  |



## Note

The following columns were dropped from the final linear regression model:

- `customerID` — unique identifier with no predictive meaning.
- `tenure_bin` — binned version of `tenure`; the original continuous `tenure` variable was retained.
- `MonthlyCharges_bin` — binned version of `MonthlyCharges`; the original continuous `MonthlyCharges` variable was retained.
- `OnlineSecurity`
- `OnlineBackup`
- `DeviceProtection`
- `TechSupport`
- `StreamingTV`
- `StreamingMovies`
- `PhoneService`

The service-related variables were excluded to reduce structural redundancy and multicollinearity among predictors. In particular, several internet-service variables contain `"No internet service"`, which overlaps with `InternetService`, while `MultipleLines` contains `"No phone service"`, which overlaps with `PhoneService`.

**Caveat:** These variables were excluded from the final OLS model for modeling stability and to avoid redundant predictors. Their exclusion does **not** mean that they are unimportant predictors of churn. Their effects may still be captured indirectly by other correlated variables in the model. Therefore, the coefficient estimates should be interpreted as associations conditional on the predictors retained in this specific model specification.

## Recommendation

Recommended model:

Why this model:

What the company can responsibly conclude:

What the company should not conclude yet:

One next analysis we would run:


