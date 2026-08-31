# Explainable-AI

# Team Name: mos

## Contributors
Matthew Fischer, Ogechukwu Ezenwa, Supriya Nannapaneni

## Dataset

The Telco Customer Churn dataset contains customer-level data and the task is to predict if a customer will stay or leave based on demographic information, services that each customer has signed up for, and customer account information.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression |  |  |  |
| Logistic regression | Binary outcome, sufficient sample size, independence of observations, multicollinearity | Values of outcome variable, value counts of each variable, Durbin-Watson test, correlation matrix | Total charges has high multicollinearity with tenure (0.825880), also linear log-odds is not checked. |
| GAM |  |  |  |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression |  |  |  |
| Logistic regression |  |  |  |
| GAM |  |  |  |

## Recommendation

Recommended model:

Why this model:

What the company can responsibly conclude:

What the company should not conclude yet:

One next analysis we would run:
