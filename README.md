# Customer Renewal Prediction — Broughty Ferry Media

![R](https://img.shields.io/badge/R-276DC3?style=flat&logo=r&logoColor=white)
![Model](https://img.shields.io/badge/Model-Random%20Forest-2e7d32?style=flat)
![AUC](https://img.shields.io/badge/AUC-0.933-1565c0?style=flat)
![Module](https://img.shields.io/badge/Module-Predictive%20Analytics-7b1fa2?style=flat)

End-to-end predictive modelling pipeline built in R to support a £20,000 subscription renewal campaign. The model ranks 5,000 customers by predicted renewal probability, enabling targeted contact of the top 500 and generating an estimated profit of £32,920 versus a £13,000 loss under random selection.

## Overview

Broughty Ferry Media needed to identify which of their 8,000 customers were most likely to renew a magazine subscription, with a fixed budget allowing a maximum of 500 contacts at £40 each and renewal revenue of £140 per customer. The project covers the full modelling lifecycle from exploratory analysis through to actionable business recommendation.

## Methodology

- Stratified 70/30 train-validation split using `createDataPartition()` (seed: 2417549)
- Variable selection via point-biserial correlation across all numeric predictors; top 10 confirmed with VIF testing (all < 5)
- Three models trained: Logistic Regression, Decision Tree (CART, CP=0.001, depth=6), Random Forest (500 trees, mtry=3)
- Model selected on combined AUC and profit simulation on validation set (scaled to 150 contacts)
- Final model locked and re-trained on full training set before applying to held-out test data

## Results

| Set | AUC | Accuracy | Precision | Lift @ top 10% |
|---|---|---|---|---|
| Validation | 0.8903 | 0.9571 | 0.8310 | — |
| Test | 0.9325 | 0.9598 | 0.9308 | 7.56× |

## Targeting strategies

| Strategy | Contacts | Renewals | Profit |
|---|---|---|---|
| Random baseline | 500 | 50 | −£13,000 |
| Model top 500 | 500 | 378 | +£32,920 |

## Key findings

- `PoundsPerIssue` was the dominant predictor by both point-biserial correlation (r=0.20) and Random Forest Gini importance
- The optimal profit threshold aligned exactly with the campaign budget cap (n=500), eliminating any trade-off between strategies
- Test AUC exceeded validation AUC, attributable in part to a higher renewal rate in the test set (10% vs 6.25%)

## Tech stack

R · `caret` · `randomForest` · `rpart` · `pROC` · `car` (VIF) · `ggplot2`
