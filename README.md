# Explainable-AI

# Team Name: mos

## Contributors
Matthew Fischer, Ogechukwu Ezenwa, Supriya Nannapaneni

## Dataset

The Telco Customer Churn dataset contains customer-level data and the task is to predict if a customer will stay or leave based on demographic information, services that each customer has signed up for, and customer account information.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| **Linear regression** | Linearity, multicollinearity, independence, homoscedasticity, normality, and influential observations. | Correlation matrix and VIF were used to assess multicollinearity. VIF(`tenure`) = **6.33** and VIF(`TotalCharges`) = **8.09**. Observations were treated as independent. Homoscedasticity, normality of residuals, and influential observations were assessed after model fitting. | **Moderate/high multicollinearity** between `tenure` and `TotalCharges`. Additionally, the binary churn outcome is a limitation of OLS. |
| **Logistic regression** | Binary outcome, sufficient sample size, independence of observations, multicollinearity. | Values of outcome variable, value counts of each variable, Durbin-Watson test, correlation matrix. | Total charges has high multicollinearity with tenure (0.825880), also linear log-odds is not checked. |
| **GAM** | Binary outcome, independence of observations, additive structure, appropriate smooth functions for continuous predictors, multicollinearity. | Churn is binary; customer IDs are unique; EDA identified tenure, MonthlyCharges, and TotalCharges as candidates for smooth terms; GAM explicitly models these using s() terms. | Concern: tenure and TotalCharges remain highly correlated, making their individual effects harder to interpret. GAM also assumes additive effects unless interactions are explicitly included. |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| **Linear regression** | R² = **0.276**, Adjusted R² = **0.274**, F-statistic = **157.0**, p < **0.001** | Coefficients are straightforward to interpret as changes in predicted churn probability, making individual predictor effects easy to communicate. | OLS is not ideal for a binary outcome and may produce predictions outside the [0, 1] probability range. Multicollinearity between `tenure` and `TotalCharges` also complicates coefficient interpretation. |
| **Logistic regression** | Accuracy: 0.788, Precision: 0.623, Recall: 0.516, F1 Score: 0.564, ROC-AUC: 0.832 | The outcome is binary and logistic regression outputs the probability of a binary outcome, so the formatting of the problem fits well with a logistic model. | The model has a negative R-squared value, includes many variables, and uses one-hot encoding, which can reduce interpretability. |
| **GAM** | Accuracy = 0.799, Precision = 0.646, Recall = 0.537, F1 = 0.587, ROC-AUC = 0.842 | Strong interpretability through partial-dependence/smooth-effect plots; can show how churn risk changes across tenure, MonthlyCharges, and TotalCharges | Smooth effects are less straightforward than individual logistic coefficients; correlated predictors make individual effects harder to isolate; additive structure does not automatically capture interactions |



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

The **GAM (Generalized Additive Model)** is the recommended model for predicting customer churn because it achieved the strongest performance, with **accuracy = 0.799, F1 = 0.587, and ROC-AUC = 0.842**.

Why this model:

GAM provides a good balance between **performance and interpretability**. It can capture nonlinear relationships between predictors such as `tenure`, `MonthlyCharges`, and `TotalCharges` while allowing these relationships to be visualized through smooth-effect plots.


What the company can responsibly conclude:

The results suggest that factors such as **contract type, tenure, payment method, and customer services** are associated with churn risk. These patterns can help identify higher-risk customer segments and inform retention strategies.

What the company should not conclude yet:
The results show **associations, not causation**. The model should not be used to conclude that changing a specific customer characteristic will directly prevent churn. Further validation is also needed before making automated customer-level decisions.

One next analysis we would run:
We would perform **cross-validation and fairness analysis** to evaluate model reliability and determine whether performance differs across customer groups.


## AI and External Resources Disclosure

Chatgpt was used to assist with portions of the Python code, and debugging which were all reviewed, tested, and adapted by the project team.

The team also referenced the Duke AIPI-590-XAI regression interpretability and eneralized-models interpretability example notebook for guidance on regression modeling and interpretability:

[AIPI-590-XAI Regression Interpretability Notebook](https://github.com/AIPI-590-XAI/Duke-AI-XAI/blob/main/interpretable-ml-example-notebooks/regression-interpretability.ipynb)

[AIPI-590-XAI Generalized Linear Model Notebook](https://github.com/AIPI-590-XAI/Duke-AI-XAI/blob/main/interpretable-ml-example-notebooks/generalized-models-interpretability.ipynb)