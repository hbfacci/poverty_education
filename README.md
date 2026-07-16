# Predicting Secondary Educational Attainment from Global Poverty Rates
A cross-national regression study investigating whether poverty headcount ratio predicts secondary school completion rates, after controlling for GDP per capita.

**Team:** Juliana Cobb, Suzanne Crabtree, Hannah Facci, Addison Orndorff
 
## Overview
Poverty is widely cited as a major barrier to educational attainment, but most existing research either focuses on individual/household-level data, looks at enrollment rather than completion, or doesn't systematically control for national income. This project asks a narrower, cross-national question: how well does a country's poverty headcount ratio predict its secondary school completion rate, once GDP per capita is accounted for?
 
This project uses World Bank World Development Indicators data spanning 2002–2022 across 63 countries to build and compare several predictive models, with Ordinary Least Squares regression as the primary method.
 
## Data
- **Source:** World Bank Group, World Development Indicators
- **Initial pull:** 266 countries x 7 indicators (1,867 x 54 dataset)
- **Target variable:** secondary educational attainment (% of population 25+ who completed upper secondary), by total, male, and female population
- **Predictors:** poverty headcount ratio (at $8.30/day and at national poverty lines), GDP per capita (constant US$ and annual growth)
- **Final retained sample:** 63 countries, after dropping countries missing 60%+ of values in any indicator
  
## Methods
1. **Cleaning:** sorted by country/series via multi-index, standardized series names, replaced placeholder values with NaN, and established a 2002–2022 window based on data availability.
2. **Missing data:** addressed via linear interpolation within each country's time series, with forward/backward fill to handle edge gaps.
3. **Modeling:** primary model was OLS regression (`Educational_Attainment_Total ~ Poverty_Ratio_Wage + log_GDP_Constant`), with log-transformed GDP to normalize scale. Also tested Ridge regression, a Fixed Effects model, Random Forest, and XGBoost, using Group K-Fold cross-validation (by country) to check generalizability.
4. **Extended analysis:** a secondary OLS model added Gini index, political stability, urban population %, and education expenditure as additional predictors.

## Key Results
- Primary OLS model: **R² = 0.234**, F-statistic = 201.7 (p < .001) — poverty and log GDP explain roughly 23% of the variance in secondary completion rates.
- Scikit-learn OLS with an 80/20 train/test split: **R² = 0.267**, MSE = 330.018 on held-out data.
- Random Forest achieved a higher in-sample R² (0.635) but a negative cross-validated R² (-0.09), indicating overfitting — simpler linear models generalized better.
- Expanding the feature set to include Gini index, political stability, urban population %, and education expenditure improved R² to **0.433**, with Gini index emerging as the strongest individual predictor

## Limitations & Future Work
- Retained countries are geographically clustered in the Americas and Eurasia, with notable gaps in Africa, South Asia, Australia, and Greenland — limiting generalizability to underrepresented regions.
- Linear interpolation smooths over any sudden economic or political shocks between observed data points.
- Future work could prioritize Gini index as a primary predictor and examine how the poverty–education relationship differs by gender.
