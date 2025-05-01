---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---

### **Slide 1: Title Slide**
- **Course Title**: Machine Learning in R
- **Topic**: Linear and Logistic Regressions

---

### **Slide 2: Learning Objectives**
- Understand the theoretical background of linear and logistic regression.
- Learn how to implement these algorithms in R.
- Interpret the output and evaluate model performance.
- Apply concepts through hands-on exercises.

---

### **Slide 3: Introduction to Linear Regression**
- **Definition**: A method to model the linear relationship between a dependent variable and one or more independent variables.
- **Equation**: \\( y = \\beta_0 + \\beta_1 x_1 + \\epsilon \\)
- **Use Case**: Predicting continuous variables (e.g., housing prices).

---

### **Slide 4: Key Assumptions of Linear Regression**
- **Linearity**: The relationship between predictors and outcome is linear.
- **Independence**: Observations are independent.
- **Homoscedasticity**: Constant variance of errors.
- **Normality**: Errors should be normally distributed.

---

### **Slide 5: Implementing Linear Regression in R**
**R Code**:
```r
# Fit a linear regression model
model <- lm(mpg ~ hp + wt, data = mtcars)

# Summary of the model
summary(model)
```
**Explanation**: The `lm()` function fits a linear regression model; `summary()` provides coefficients, p-values, and R-squared.

---

### **Slide 6: Understanding Model Output**
- **Coefficients**: Indicate the change in the response variable for each unit change in the predictor.
- **R-squared**: Proportion of variance explained by the model.
- **P-values**: Test if coefficients are statistically significant.

---

### **Slide 7: Visualizing the Linear Model**
**R Code**:
```r
plot(mtcars$hp, mtcars$mpg, main = 'HP vs MPG')
abline(model, col = 'red')
```
**Explanation**: The plot shows the data points and the fitted regression line.

---

### **Slide 8: Residual Analysis**
- **Definition**: Residuals are the differences between observed and predicted values.
- **Objective**: Residuals should have no discernible pattern for the model to be valid.
**R Code**:
```r
plot(model$residuals, main = 'Residual Plot')
abline(h = 0, col = 'blue')
```

---

### **Slide 9: Exercise – Building a Linear Model**
**Task**: Fit a linear regression model using `mtcars` to predict `mpg` with `cyl` and `disp` as predictors.
**R Code**:
```r
exercise_model <- lm(mpg ~ cyl + disp, data = mtcars)
summary(exercise_model)
```
**Solution Analysis**: Interpret the significance of the coefficients and the R-squared value.

---

### **Slide 10: Evaluating Model Performance**
- **Metrics**:
  - **RMSE**: Root Mean Squared Error.
  - **MAE**: Mean Absolute Error.
**R Code**:
```r
library(Metrics)
rmse(mtcars$mpg, predict(model, mtcars))
mae(mtcars$mpg, predict(model, mtcars))
```

---

### **Slide 11: Introduction to Logistic Regression**
- **Definition**: A type of regression used when the dependent variable is binary (0 or 1).
- **Equation**: \\( P(Y=1|X) = \\frac{e^{\\beta_0 + \\beta_1 X}}{1 + e^{\\beta_0 + \\beta_1 X}} \\)
- **Use Case**: Predicting categorical outcomes (e.g., pass/fail).

---

### **Slide 12: Assumptions of Logistic Regression**
- **Binary Outcome**: Dependent variable should be binary.
- **Independence**: Observations should be independent.
- **Linearity of Logits**: Logit of the outcome should have a linear relationship with predictors.

---

### **Slide 13: Implementing Logistic Regression in R**
**R Code**:
```r
# Load dataset
data("mtcars")
mtcars$am <- as.factor(mtcars$am) # Convert to a binary factor

# Fit a logistic regression model
log_model <- glm(am ~ hp + wt, data = mtcars, family = binomial)
summary(log_model)
```
**Explanation**: The `glm()` function with `family = binomial` fits a logistic regression model.

---

### **Slide 14: Interpreting Logistic Regression Coefficients**
- **Coefficients**: Represent the log-odds change for a unit change in predictors.
- **Odds Ratio**: \\( e^{\\beta_1} \\) indicates the odds change for a one-unit increase in the predictor.

---

### **Slide 15: Calculating Odds Ratios**
**R Code**:
```r
exp(coef(log_model))
```
**Explanation**: Transform coefficients to odds ratios for better interpretability.

---

### **Slide 16: Predicting Outcomes with Logistic Regression**
**R Code**:
```r
predicted_probs <- predict(log_model, type = 'response')
predicted_classes <- ifelse(predicted_probs > 0.5, 1, 0)
```
**Explanation**: `type = 'response'` provides probabilities; `ifelse()` converts them to classes.

---

### **Slide 17: Evaluating Model Performance – Confusion Matrix**
**R Code**:
```r
library(caret)
confusionMatrix(as.factor(predicted_classes), mtcars$am)
```
**Metrics**:
- **Accuracy**: Proportion of correct predictions.
- **Sensitivity and Specificity**.

---

### **Slide 18: Visualizing ROC Curve**
**R Code**:
```r
library(pROC)
roc_curve <- roc(mtcars$am, predicted_probs)
plot(roc_curve, main = 'ROC Curve')
```
**Explanation**: ROC curve shows the trade-off between sensitivity and specificity.

---

### **Slide 19: Exercise – Building and Evaluating Logistic Model**
**Task**: Fit a logistic regression model using `mtcars` to predict `am` with `cyl` and `hp`.
**R Code**:
```r
exercise_log_model <- glm(am ~ cyl + hp, data = mtcars, family = binomial)
summary(exercise_log_model)
```
**Solution**: Interpret coefficients and calculate odds ratios.

---

### **Slide 20: Comparing Linear and Logistic Regressions**
- **Linear Regression**:
  - Predicts continuous outcomes.
  - Sensitive to outliers.
- **Logistic Regression**:
  - Predicts binary outcomes.
  - Provides probabilities and class predictions.

---

### **Slide 21: Practical Use Cases**
- **Linear Regression**: Predicting house prices, sales revenue.
- **Logistic Regression**: Predicting customer churn, disease diagnosis.

---

### **Slide 22: Handling Multicollinearity**
- **Definition**: When predictors are highly correlated, leading to unreliable coefficient estimates.
- **Solution**:
  - Use `vif()` (Variance Inflation Factor) from `car` package.
**R Code**:
```r
library(car)
vif(model)
```

---

### **Slide 23: Regularization in Regression (Overview)**
- **Lasso (L1)**: Shrinks coefficients; some become zero.
- **Ridge (L2)**: Shrinks coefficients but doesn’t zero them out.
- **Elastic Net**: Combines L1 and L2 penalties.

---

### **Slide 24: Implementing Regularized Regression in R**
**R Code**:
```r
library(glmnet)
x <- model.matrix(mpg ~ ., mtcars)[, -1]
y <- mtcars$mpg

# Fit Lasso regression
lasso_model <- cv.glmnet(x, y, alpha = 1)
plot(lasso_model)
```

---

### **Slide 25: Interpreting Regularized Model Results**
**Explanation**: The plot shows the cross-validated error for different lambda values. Optimal lambda minimizes error.

---

### **Slide 26: Advanced Exercise 1 – Linear Regression with Feature Engineering**
**Objective**: Build a linear regression model incorporating feature engineering.
**Task**: Create a new feature `hp_per_wt` (horsepower per weight) and use it in the model.
**R Code**:
```r
mtcars$hp_per_wt <- mtcars$hp / mtcars$wt
advanced_model <- lm(mpg ~ hp_per_wt + cyl + disp, data = mtcars)
summary(advanced_model)
```
**Solution Analysis**:
- **Interpret Coefficients**: Assess the impact of `hp_per_wt` on `mpg`.
- **R-squared Comparison**: Compare to a model without `hp_per_wt` to see improvements.

---

### **Slide 27: Exercise Solution Analysis**
- **Result Summary**: Display the change in R-squared and p-values after adding `hp_per_wt`.
- **Discussion**: Did adding the engineered feature improve model performance significantly?

**Key Insight**: Feature engineering can highlight hidden relationships in the data.

---

### **Slide 28: Case Study 1 – Linear Regression for Sales Prediction**
**Scenario**: A retail company wants to predict monthly sales based on advertising spend, promotions, and economic indicators.
**Steps**:
1. Load the dataset.
2. Fit a model using advertising spend and promotions.
**R Code**:
```r
sales_model <- lm(sales ~ advertising + promotions, data = retail_data)
summary(sales_model)
```
**Outcome**: Analyze R-squared and coefficients to see which factors are most impactful.

---

### **Slide 29: Model Diagnostics – Checking Assumptions**
**Key Checks**:
- **Residuals vs. Fitted**: Check for patterns.
- **Normal Q-Q Plot**: Check if residuals are normally distributed.
**R Code**:
```r
par(mfrow = c(2, 2))
plot(sales_model)
```

**Discussion**: Discuss any violations of assumptions and how to address them.

---

### **Slide 30: Advanced Exercise 2 – Logistic Regression with Interaction Terms**
**Objective**: Fit a logistic regression model with interaction terms to predict `am` using `hp`, `wt`, and their interaction.
**R Code**:
```r
log_model_interaction <- glm(am ~ hp * wt, data = mtcars, family = binomial)
summary(log_model_interaction)
```
**Solution**:
- **Interpretation**: How does the interaction between `hp` and `wt` affect the probability of `am`?

---

### **Slide 31: Exercise Solution Analysis – Interaction Effects**
- **Interaction Term Coefficient**: Explains how the effect of one predictor changes with the level of another.
- **Interpretation**: Does `wt` modify the impact of `hp` on the odds of `am` being 1?
**Graph**:
```r
library(ggplot2)
ggplot(mtcars, aes(hp, wt, color = as.factor(am))) + geom_point()
```

---

### **Slide 32: Evaluating Logistic Model with ROC Curve**
**Objective**: Assess the performance of the logistic regression model using ROC and AUC.
**R Code**:
```r
library(pROC)
roc_curve <- roc(mtcars$am, fitted(log_model_interaction))
plot(roc_curve, main = 'ROC Curve')
auc(roc_curve)
```
**Discussion**: Analyze the AUC to evaluate the model’s predictive power.

---

### **Slide 33: Exercise – Hyperparameter Tuning in Logistic Regression**
**Task**: Apply cross-validation to tune hyperparameters for a logistic regression model.
**R Code**:
```r
library(caret)
train_control <- trainControl(method = 'cv', number = 5)
log_cv_model <- train(am ~ hp + wt, data = mtcars, method = 'glm', family = 'binomial', trControl = train_control)
print(log_cv_model)
```
**Solution Analysis**: Interpret the cross-validated accuracy and AUC.

---

### **Slide 34: Regularization Techniques Overview**
- **Lasso (L1)**: Feature selection through coefficient shrinking.
- **Ridge (L2)**: Reduces coefficient magnitude without setting them to zero.
- **Elastic Net**: Combination of L1 and L2.
**Application**: Used to handle multicollinearity and prevent overfitting.

---

### **Slide 35: Implementing Lasso and Ridge in R**
**R Code**:
```r
library(glmnet)
x <- model.matrix(am ~ hp + wt, mtcars)[, -1]
y <- mtcars$am

# Lasso
lasso_model <- cv.glmnet(x, y, alpha = 1, family = 'binomial')
print(lasso_model)
```
**Discussion**: Analyze the optimal lambda value for minimal error.

---

### **Slide 36: Exercise – Using Regularization in Linear Regression**
**Task**: Implement Ridge regression to predict `mpg` using `mtcars`.
**R Code**:
```r
ridge_model <- cv.glmnet(x, mtcars$mpg, alpha = 0)
plot(ridge_model)
```
**Solution**: Interpret the coefficient shrinkage and R-squared.

---

### **Slide 37: Cross-Validation Techniques**
**Types**:
- **k-Fold**: Data split into k parts, each used as a validation set once.
- **LOOCV**: Leave-One-Out Cross-Validation for small datasets.
**R Code**:
```r
cv_model <- train(mpg ~ hp + wt, data = mtcars, method = 'lm', trControl = trainControl(method = 'cv', number = 10))
```
**Outcome**: Compare k-fold and LOOCV in terms of bias and variance.

---

### **Slide 38: Advanced Case Study – Predicting Diabetes Using Logistic Regression**
**Objective**: Use the `PimaIndiansDiabetes` dataset to predict diabetes status.
**R Code**:
```r
library(MASS)
data(PimaIndiansDiabetes)
log_diabetes_model <- glm(diabetes ~ ., data = PimaIndiansDiabetes, family = binomial)
summary(log_diabetes_model)
```
**Outcome Analysis**: Discuss significant predictors and model accuracy.

---

### **Slide 39: Improving Model Performance with Feature Scaling**
**Concept**: Scale features to improve the performance of regression algorithms.
**R Code**:
```r
mtcars_scaled <- scale(mtcars[, c('hp', 'wt')])
```
**Outcome**: Discuss the benefits of scaling in logistic regression.

---

### **Slide 40: Exercise – Cross-Validation with Feature Scaling**
**Task**: Implement a logistic regression model with scaled features and cross-validation.
**R Code**:
```r
scaled_model <- train(am ~ hp + wt, data = mtcars, method = 'glm', family = 'binomial', trControl = trainControl(method = 'cv', number = 5), preProcess = c('center', 'scale'))
```
**Solution Analysis**: Evaluate model accuracy with and without scaling.

---

### **Slide 41: Visualizing Coefficients in Logistic Regression**
**R Code**:
```r
library(coefplot)
coefplot(log_model_interaction)
```
**Explanation**: Visual representation helps understand the relative impact of predictors.

---

### **Slide 42: Handling Imbalanced Datasets in Logistic Regression**
**Techniques**:
- **Oversampling**: Use `ROSE` or `SMOTE`.
- **Undersampling**: Reduce majority class size.
**R Code**:
```r
library(ROSE)
balanced_data <- ROSE(am ~ ., data = mtcars)$data
```
**Outcome**: Discuss when to use each technique.

---

### **Slide 43: Case Study – Feature Selection in Logistic Regression**
**Objective**: Use stepwise selection to identify key features predicting `am`.
**R Code**:
```r
step_model <- step(log_model_interaction, direction = 'both')
summary(step_model)
```
**Solution**: Interpret which features were retained and why.

---

### **Slide 44: Implementing Cross-Validation for Hyperparameter Tuning**
**R Code**:
```r
tuned_model <- train(mpg ~ ., data = mtcars, method = 'glmnet', trControl = trainControl(method = 'cv', number = 10), tuneGrid = expand.grid(alpha = 1, lambda = seq(0.001, 0.1, by = 0.01)))
print(tuned_model)
```
**Outcome**: Analyze the best lambda value selected by cross-validation.

---

### **Slide 45: Comparing Model Performance Metrics**
- **Linear Regression**:
  - **RMSE**: Lower is better.
- **Logistic Regression**:
  - **AUC/ROC**: Higher is better.
- **Confusion Matrix**: Shows accuracy, sensitivity, and specificity.

**Key Insight**: Choosing the right metric based on problem type.

---

### **Slide 46: Exercise – Final Project on Regression Techniques**
**Objective**: Apply linear and logistic regression on a provided dataset.
**Guidelines**:
- Fit models.
- Perform feature engineering.
- Evaluate using cross-validation.
**R Code**:
```r
final_model <- train(outcome ~ ., data = new_data, method = 'lm', trControl = trainControl(method = 'cv', number = 5))
```

---

### **Slide 47: Troubleshooting Common Issues in Regression**
**Challenges**:
- **Multicollinearity**: When predictors are highly correlated, leading to unstable coefficient estimates.
- **Heteroscedasticity**: Variance of errors is not constant, which can affect model reliability.
- **Outliers and Influential Points**: These can disproportionately skew model coefficients and predictions.

**Solutions**:
- **Detecting Multicollinearity**:
  - Use the Variance Inflation Factor (VIF). A VIF > 10 indicates high multicollinearity.
  **R Code**:
  ```r
  library(car)
  vif(model)
  ```
- **Addressing Heteroscedasticity**:
  - Apply transformations (e.g., log or square root) to the dependent variable.
  - Use robust standard errors.
- **Identifying Outliers**:
  - Use Cook’s distance or leverage plots.
  **R Code**:
  ```r
  plot(model, which = 4)
  ```

---

### **Slide 48: Exercise – Identifying and Addressing Issues**
**Objective**: Diagnose and correct issues in a linear regression model.
**Task**: Fit a regression model and check for multicollinearity and outliers.
**R Code**:
```r
# Fit a regression model
model <- lm(mpg ~ hp + wt + disp, data = mtcars)

# Check for multicollinearity
library(car)
vif(model)

# Identify influential points using Cook's distance
plot(model, which = 4)
```
**Solution Analysis**:
- **Multicollinearity**: Review VIF output and remove or combine variables with high values.
- **Outliers**: Identify data points with high Cook’s distance and assess their impact on the model.

---

### **Slide 49: Best Practices for Regression Analysis**
**Guidelines**:
- **Data Preprocessing**:
  - Scale and center predictors, especially when using regularized models (e.g., Ridge, Lasso).
- **Feature Selection**:
  - Use stepwise selection or regularization to minimize overfitting.
- **Model Validation**:
  - Use k-fold cross-validation to ensure generalizability.
- **Interpretability**:
  - Select models that can be easily explained to stakeholders when necessary.

**Tips**:
- Validate assumptions with diagnostic plots (e.g., residual plots, Q-Q plots).
- Ensure your model generalizes by testing on unseen data.

---

### **Slide 50: Recap and Next Steps**
**Key Points Covered**:
- Theoretical and practical aspects of linear and logistic regression.
- Implemented regression models and interpreted outputs.
- Diagnosed and addressed common issues (e.g., multicollinearity, outliers).
- Applied regularization techniques and cross-validation.

**Next Steps**:
- Apply these techniques to real-world projects.
- Explore advanced models, such as generalized linear models (GLMs) and mixed-effects models.

**Q&A**:
- Open the floor for questions and further clarifications.

---
