---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---

### **Slide 1: Title Slide**
- **Course Title**: Machine Learning in R
- **Topic**: Feature Engineering and Data Partitioning

---

### **Slide 2: Learning Objectives**
- Understand the concept and importance of feature engineering.
- Learn data partitioning for training and testing sets.
- Apply cross-validation for reliable model evaluation.
- Practice with hands-on examples and exercises.

---

### **Slide 3: Introduction to Feature Engineering**
- **Definition**: The process of creating, selecting, and transforming features to improve model performance.
- **Key Concept**: Features should be relevant, informative, and increase predictive power.
- **Examples**: Extracting date components, creating interaction terms, and encoding categorical variables.

---

### **Slide 4: Why Feature Engineering is Important**
- **Improves Model Performance**: Helps algorithms identify patterns more effectively.
- **Reduces Overfitting**: More representative features can prevent overfitting.
- **Simplifies the Model**: Reduces complexity by creating more meaningful inputs.

---

### **Slide 5: Types of Feature Engineering**
- **Feature Creation**: Generate new features from existing data (e.g., ratios, sums).
- **Feature Transformation**: Apply mathematical transformations (e.g., log, square root).
- **Feature Selection**: Identify and retain features that contribute most to predictive power.

---

### **Slide 6: Feature Creation Example**
- **Example Task**: Create a feature from the `mtcars` dataset by combining `hp` and `wt` into a power-to-weight ratio.
**R Code**:
```r
mtcars$power_to_weight <- mtcars$hp / mtcars$wt
summary(mtcars$power_to_weight)
```

---

### **Slide 7: Feature Transformation Techniques**
- **Log Transformation**: Reduces skewness and stabilizes variance.
- **Square Root Transformation**: Useful for moderately skewed data.
**R Code**:
```r
mtcars$log_hp <- log(mtcars$hp)
hist(mtcars$log_hp, main = 'Log Transformed HP', col = 'lightblue')
```

---

### **Slide 8: Exercise – Feature Creation**
**Task**: Create an interaction term between `mpg` and `hp` in the `mtcars` dataset.
**Solution**:
```r
mtcars$mpg_hp_interaction <- mtcars$mpg * mtcars$hp
summary(mtcars$mpg_hp_interaction)
```
**Discussion**: How does this new feature potentially impact model performance?

---

### **Slide 9: Encoding Categorical Variables**
- **Label Encoding**: Assigns unique integer values.
- **One-Hot Encoding**: Creates binary columns for each category.
**R Code**:
```r
library(caret)
dummies <- dummyVars(~ cyl, data = mtcars)
encoded_data <- predict(dummies, mtcars)
head(encoded_data)
```

---

### **Slide 10: Handling Missing Values in Features**
- **Imputation Techniques**:
  - Mean/Median imputation for numerical variables.
  - Mode imputation for categorical variables.
**R Code**:
```r
mtcars$hp[is.na(mtcars$hp)] <- median(mtcars$hp, na.rm = TRUE)
```

---

### **Slide 11: Data Scaling and Normalization**
- **Standardization (Z-score)**:
  - Mean = 0, SD = 1.
- **Normalization (Min-Max)**:
  - Rescales to [0, 1] range.
**R Code**:
```r
mtcars$scaled_hp <- scale(mtcars$hp)
mtcars$normalized_hp <- (mtcars$hp - min(mtcars$hp)) / (max(mtcars$hp) - min(mtcars$hp))
```

---

### **Slide 12: Exercise – Data Transformation**
**Task**: Apply log transformation to `wt` in the `mtcars` dataset and plot a histogram.
**Solution**:
```r
mtcars$log_wt <- log(mtcars$wt)
hist(mtcars$log_wt, main = 'Log Transformed WT', col = 'lightgreen')
```

---

### **Slide 13: Introduction to Data Partitioning**
- **Definition**: Splitting data into training and testing sets to evaluate model performance.
- **Reason**: Ensures the model can generalize to unseen data.

---

### **Slide 14: Importance of Data Partitioning**
- **Training Set**: Used to train the model.
- **Testing Set**: Used to evaluate model performance.
- **Validation Set** (optional): Used for hyperparameter tuning.

---

### **Slide 15: Basic Data Splitting Techniques**
- **Random Splitting**: Common approach for general data partitioning.
- **Stratified Splitting**: Ensures distribution of target variable is consistent.
**R Code**:
```r
set.seed(123)
train_index <- sample(1:nrow(mtcars), 0.7 * nrow(mtcars))
train_data <- mtcars[train_index, ]
test_data <- mtcars[-train_index, ]
```

---

### **Slide 16: Exercise – Data Partitioning**
**Task**: Split the `iris` dataset into 70% training and 30% testing sets.
**Solution**:
```r
set.seed(42)
train_index <- sample(1:nrow(iris), 0.7 * nrow(iris))
train_data <- iris[train_index, ]
test_data <- iris[-train_index, ]
```

---

### **Slide 17: Cross-Validation Overview**
- **Definition**: A method for assessing model performance by splitting the data into multiple training and validation sets.
- **Types**:
  - k-Fold Cross-Validation
  - Leave-One-Out Cross-Validation

---

### **Slide 18: Benefits of Cross-Validation**
- **Reduces Overfitting**: Ensures the model generalizes well.
- **Reliable Performance Metrics**: Provides a better estimate of model accuracy.

---

### **Slide 19: k-Fold Cross-Validation Explained**
- **Process**:
  1. Data is split into `k` equally sized folds.
  2. Model is trained `k` times, each time using a different fold as validation.
- **Common Choice**: k = 5 or 10.

---

### **Slide 20: Code Example – k-Fold Cross-Validation in R**
**R Code**:
```r
library(caret)
train_control <- trainControl(method = 'cv', number = 5)
model <- train(mpg ~ ., data = mtcars, method = 'lm', trControl = train_control)
print(model)
```

**Explanation**: This code sets up 5-fold cross-validation for a linear regression model.

---

### **Slide 21: Leave-One-Out Cross-Validation (LOOCV)**
- **Definition**: A special case of k-fold where k equals the number of observations.
- **Advantage**: Uses most data for training; high variance in results.

---

### **Slide 22: Exercise – Applying Cross-Validation**
**Task**: Apply 10-fold cross-validation on the `iris` dataset for a decision tree model.
**Solution**:
```r
library(caret)
train_control <- trainControl(method = 'cv', number = 10)
model <- train(Species ~ ., data = iris, method = 'rpart', trControl = train_control)
print(model)
```

---

### **Slide 23: Visualizing Cross-Validation Results**
- **Plot Performance**:
```r
plot(model)
```
**Discussion**: Analyze the performance metrics across folds.

---

### **Slide 24: Tips for Effective Data Partitioning**
- **Set a Seed**: Ensures reproducibility.
- **Balance Data**: Stratified sampling for classification problems.
- **Avoid Data Leakage**: Keep training data separate from testing data.

---

### **Slide 25: Exercise – Full Workflow**
**Task**: Partition the `mtcars` dataset, apply cross-validation, and evaluate a model.
**Solution**:
```r
set.seed(101)
train_index <- sample(1:nrow(mtcars), 0.8 * nrow(mtcars))
train_data <- mtcars[train_index, ]
test_data <- mtcars[-train_index, ]

train_control <- trainControl(method = 'cv', number = 5)
lm_model <- train(mpg ~ ., data = train_data, method = 'lm', trControl = train_control)

# Predict on test set
predictions <- predict(lm_model, newdata = test_data)
```

---

### **Slide 26: Advanced Exercise – Feature Engineering for Regression**
**Objective**: Engineer new features in the `mtcars` dataset to improve model performance for predicting `mpg`.

**Task**: Create a new feature representing the `weight-to-horsepower` ratio and use it in the model.
**R Code**:
```r
mtcars$wt_hp_ratio <- mtcars$wt / mtcars$hp
train_index <- sample(1:nrow(mtcars), 0.8 * nrow(mtcars))
train_data <- mtcars[train_index, ]
test_data <- mtcars[-train_index, ]

lm_model <- lm(mpg ~ wt + hp + wt_hp_ratio, data = train_data)
summary(lm_model)
```

**Solution Analysis**:
- Examine the p-values and R-squared to assess the impact of the new feature.
- **Discussion**: How did the new feature affect the model’s predictive power?

---

### **Slide 27: Exercise – Feature Selection Techniques**
**Objective**: Use feature selection to identify the most relevant features in the `mtcars` dataset.
**Method**: Apply stepwise selection.

**R Code**:
```r
library(MASS)
step_model <- stepAIC(lm(mpg ~ ., data = mtcars), direction = "both")
summary(step_model)
```

**Explanation**:
- Stepwise selection helps automate the process of feature inclusion or exclusion based on AIC (Akaike Information Criterion).

**Question for Students**:
- Why might stepwise selection sometimes overfit, and how can cross-validation help prevent this?

---

### **Slide 28: Case Study – Impact of Feature Engineering on Model Performance**
**Scenario**: A regression model was initially built using raw features in the `mtcars` dataset. Feature engineering was applied, including interactions and ratios.

**Results**:
- **Initial Model R-squared**: 0.75
- **Model with Feature Engineering R-squared**: 0.88

**Key Insight**:
- Feature engineering significantly improved the model’s explanatory power.
- **Discussion**: What other features could we create to further improve the model?

---

### **Slide 29: Cross-Validation in Practice – Real Dataset Example**
**Objective**: Apply k-fold cross-validation to evaluate a random forest model on the `iris` dataset.

**R Code**:
```r
library(randomForest)
library(caret)

train_control <- trainControl(method = 'cv', number = 5)
rf_model <- train(Species ~ ., data = iris, method = 'rf', trControl = train_control)
print(rf_model)
```

**Explanation**:
- The `train()` function from `caret` with `method = 'rf'` runs cross-validation for the random forest model.

---

### **Slide 30: Analysis of Cross-Validation Results**
**Output**:
- Mean accuracy across folds.
- Visualization of the variance in performance.
**R Code for Visualization**:
```r
plot(rf_model)
```

**Discussion**:
- Identify the fold with the highest and lowest performance.
- What might cause variability across folds, and how can this inform model adjustments?

---

### **Slide 31: Advanced Exercise – Nested Cross-Validation**
**Objective**: Implement nested cross-validation for hyperparameter tuning and model assessment.

**R Code**:
```r
outer_control <- trainControl(method = 'cv', number = 5)
inner_control <- trainControl(method = 'cv', number = 3, search = 'grid')

tuned_model <- train(Species ~ ., data = iris, method = 'svmRadial', trControl = outer_control,
                     tuneGrid = expand.grid(C = c(0.1, 1, 10), sigma = c(0.01, 0.1)))
print(tuned_model)
```

**Explanation**:
- Nested cross-validation helps avoid data leakage during hyperparameter tuning.
- **Question for Reflection**: What are the computational trade-offs of nested cross-validation?

---

### **Slide 32: Exercise – Evaluating Cross-Validation Results**
**Task**: Compare k-fold cross-validation results with LOOCV on the `iris` dataset for a logistic regression model.
**R Code**:
```r
# K-fold CV
train_control_kfold <- trainControl(method = 'cv', number = 5)
kfold_model <- train(Species ~ ., data = iris, method = 'glm', trControl = train_control_kfold)

# LOOCV
train_control_loocv <- trainControl(method = 'LOOCV')
loocv_model <- train(Species ~ ., data = iris, method = 'glm', trControl = train_control_loocv)
```

**Discussion**:
- **Pros and Cons** of k-fold vs. LOOCV in terms of computation and reliability.

---

### **Slide 33: Case Study – Feature Selection Impact on Model**
**Scenario**: Applied recursive feature elimination (RFE) on the `mtcars` dataset to determine the most important predictors for `mpg`.

**R Code**:
```r
library(caret)
control <- rfeControl(functions = lmFuncs, method = "cv", number = 5)
rfe_model <- rfe(mtcars[, -1], mtcars$mpg, sizes = c(1:5), rfeControl = control)
print(rfe_model)
```

**Outcome**:
- Highlight the top features selected and how they influenced model performance.

---

### **Slide 34: Exercise – Create a Cross-Validation Pipeline**
**Objective**: Implement a complete data partitioning and cross-validation pipeline for a linear regression model.
**R Code**:
```r
set.seed(101)
train_index <- createDataPartition(mtcars$mpg, p = 0.8, list = FALSE)
train_data <- mtcars[train_index, ]
test_data <- mtcars[-train_index, ]

train_control <- trainControl(method = 'cv', number = 10)
lm_model <- train(mpg ~ ., data = train_data, method = 'lm', trControl = train_control)

# Predict and evaluate
predictions <- predict(lm_model, newdata = test_data)
postResample(predictions, test_data$mpg)
```

**Solution Analysis**:
- Use `postResample()` to evaluate model performance on the test set.

---

### **Slide 35: Discussion – Common Pitfalls in Feature Engineering**
**Key Points**:
- **Overfitting**: Creating too many features can lead to a model that does not generalize.
- **Feature Correlation**: Highly correlated features can lead to multicollinearity.
- **Data Leakage**: Ensure that information from test data does not influence training.

---

### **Slide 36: Best Practices for Data Partitioning and Cross-Validation**
**Guidelines**:
- Use stratified sampling when dealing with imbalanced datasets.
- Always set a seed for reproducibility.
- Ensure that test data remains unseen during training and validation.

---

### **Slide 37: Interactive Session – Apply Cross-Validation**
**Objective**: Work through a live coding session applying cross-validation to the `Boston` dataset from `MASS` package.
**R Code**:
```r
library(MASS)
library(caret)
set.seed(123)

train_control <- trainControl(method = 'cv', number = 5)
lm_boston <- train(medv ~ ., data = Boston, method = 'lm', trControl = train_control)
print(lm_boston)
```

**Discussion**:
- Analyze results and discuss potential next steps for feature engineering.

---

### **Slide 38: Exercise – Hyperparameter Tuning with Cross-Validation**
**Task**: Tune hyperparameters for a decision tree using `caret`.
**R Code**:
```r
tune_grid <- expand.grid(cp = seq(0.01, 0.1, by = 0.01))
dt_model <- train(mpg ~ ., data = mtcars, method = 'rpart', trControl = train_control, tuneGrid = tune_grid)
print(dt_model)
```

**Solution**:
- Discuss the best `cp` value and how it affects model complexity.

---

### **Slide 39: Case Study 1 – Feature Engineering in Financial Forecasting**
**Objective**: Understand how feature engineering can enhance financial model predictions.
- **Dataset**: Stock market data with features like `open`, `close`, `volume`.
- **Feature Engineering Techniques**:
  - **Lag Features**: Create lagged versions of `close` price to capture trends.
  - **Rolling Mean**: Smooth fluctuations over a defined window period.
**R Code**:
```r
stock_data$lag_close <- dplyr::lag(stock_data$close, n = 1)
stock_data$rolling_mean_5 <- zoo::rollmean(stock_data$close, 5, fill = NA)
```

**Outcome**: Improved predictive accuracy by using time-dependent features.

---

### **Slide 40: Case Study 2 – Feature Engineering in Medical Data Analysis**
**Objective**: Apply feature engineering for patient risk stratification in healthcare.
- **Dataset**: Patient records with features such as `age`, `blood_pressure`, `cholesterol`.
- **New Features**:
  - **Risk Score**: Composite score combining `age` and `blood_pressure`.
  - **BMI Calculation**: Feature derived from `weight` and `height`.
**R Code**:
```r
patient_data$risk_score <- patient_data$age * patient_data$blood_pressure
patient_data$bmi <- patient_data$weight / (patient_data$height/100)^2
```

**Outcome**: Enhanced model's ability to stratify patient risk more effectively.

---

### **Slide 41: Analyzing the Impact of Feature Engineering**
- **Before vs. After**:
  - **Initial Model Accuracy**: 78%
  - **Model Accuracy After Feature Engineering**: 85%
- **Visualization**:
  - Plot feature importance before and after engineering.
**R Code**:
```r
library(randomForest)
rf_model <- randomForest(risk_score ~ ., data = patient_data)
varImpPlot(rf_model)
```

**Key Insight**: Strategic feature engineering significantly boosts model performance.

---

### **Slide 42: Real-World Application – Cross-Validation in Large Datasets**
**Scenario**: Applying k-fold cross-validation to a large e-commerce dataset to predict customer churn.
- **Challenges**:
  - Computational time.
  - Data partitioning without data leakage.
**Solution**:
- Use parallel processing with `caret`’s `trainControl`.
**R Code**:
```r
library(doParallel)
cl <- makeCluster(detectCores() - 1)
registerDoParallel(cl)

train_control <- trainControl(method = 'cv', number = 5)
model <- train(churn ~ ., data = ecommerce_data, method = 'rf', trControl = train_control)
stopCluster(cl)
```

**Outcome**: Efficient cross-validation across large datasets.

---

### **Slide 43: Exercise – Implement Cross-Validation on a New Dataset**
**Task**: Apply 10-fold cross-validation on a customer segmentation dataset.
**Guidelines**:
- Use a random forest model.
- Evaluate the cross-validation results and interpret the model’s predictive power.
**R Code**:
```r
set.seed(456)
train_control <- trainControl(method = 'cv', number = 10)
rf_model <- train(segment ~ ., data = customer_data, method = 'rf', trControl = train_control)
print(rf_model)
```

**Solution Analysis**:
- Discuss mean accuracy and standard deviation across folds.

---

### **Slide 44: Group Project – Build a Feature Engineering and Validation Strategy**
**Objective**: Collaborate to create a feature engineering strategy and validate the model using cross-validation.
- **Instructions**:
  - Form groups and use a provided dataset.
  - Apply at least three feature engineering techniques.
  - Use 5-fold cross-validation to evaluate your model.

**Outcome**:
- Present your findings and discuss what worked well or needed improvement.

---

### **Slide 45: Tips for Effective Feature Engineering and Validation**
**Best Practices**:
- **Iterative Approach**: Build, test, and refine features step-by-step.
- **Avoid Data Leakage**: Ensure engineered features don’t use information from the test set.
- **Balance Features**: Avoid creating too many features that can lead to overfitting.
- **Feature Selection**: Use techniques like RFE (Recursive Feature Elimination) for optimal feature sets.

**Question for Reflection**:
- What is one feature engineering technique you found most effective, and why?

---

### **Slide 46: Common Mistakes in Feature Engineering and Cross-Validation**
**Mistakes to Avoid**:
- **Overfitting by Over-Engineering**: Adding too many specific features can lead to overfitting.
- **Data Leakage**: Using future data as part of feature creation.
- **Improper Scaling**: Not scaling data when necessary, leading to skewed model performance.
**Example**:
- Discuss an example where feature engineering led to overfitting and how cross-validation helped identify it.

---

### **Slide 47: Interactive Session – Live Feature Engineering**
**Objective**: Work with a sample dataset in a live coding session.
**Instructions**:
- Engineer new features based on guidance from the instructor.
- Apply cross-validation and interpret the results.

**Outcome**:
- Immediate feedback on feature effectiveness and model performance.

---

### **Slide 48: Key Takeaways from the Session**
- **Feature Engineering**:
  - Increases model interpretability and predictive power.
  - Must be done with a clear understanding of the problem domain.
- **Cross-Validation**:
  - Essential for assessing model performance and preventing overfitting.
  - Should be combined with feature engineering for robust evaluation.

---

### **Slide 49: Additional Tools and Resources**
- **Useful R Packages**:
  - `caret` for model training and cross-validation.
  - `randomForest` for feature importance.
  - `dplyr` for data manipulation.
- **Recommended Reading**:
  - Textbooks and provided course materials.
- **Online Courses**:
  - Explore R-based machine learning courses on platforms like Coursera and edX.

---

### **Slide 50: Final Summary and Q&A**
**Recap**:
- The importance of well-crafted feature engineering for improving model performance.
- The role of cross-validation in ensuring model generalizability.
**Next Steps**:
- Apply these techniques to your own projects.
- Experiment with different feature engineering strategies and validation methods.

**Open Floor**:
- Questions and discussions to clarify doubts and share insights.

---
