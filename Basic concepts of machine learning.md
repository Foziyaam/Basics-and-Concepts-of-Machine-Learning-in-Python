---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---

### **Basic Concepts of Machine Learning in R**

#### **Slide 1: Title Slide**
- **Course Title**: Machine Learning in R
- **Topic**: Basic Concepts of Machine Learning – Supervised and Unsupervised Learning, Model Training and Performance Evaluation

---

### **Slide 2: Learning Objectives**
- Understand machine learning and its applications.
- Distinguish between supervised and unsupervised learning.
- Comprehend model training and performance evaluation processes.

---

### **Slide 3: What is Machine Learning?**
- **Definition**: A subset of AI that focuses on building algorithms that can learn from data to make predictions.
- **Key Concepts**: Algorithms, data, models, learning process.

---

### **Slide 4: Real-World Applications of ML**
- **Examples**: Voice recognition, recommendation systems, autonomous driving.
- **Exercise**: List three examples of machine learning in your daily life.

**Answer**: Predictive text, product recommendations, fraud detection.

---

### **Slide 5: Types of Machine Learning**
- **Supervised Learning**: Learning from labeled datasets.
- **Unsupervised Learning**: Discovering hidden patterns in unlabeled data.
- **Reinforcement Learning**: (Mention briefly).

---

### **Slide 6: Supervised Learning Explained**
- **Definition**: A model learns to map inputs to outputs based on example input-output pairs.
- **Examples**: Regression (predicting continuous outcomes), classification (labeling data).

---

### **Slide 7: Code Example – Linear Regression in R**
```r
# Linear model example
model <- lm(mpg ~ hp + wt, data = mtcars)
summary(model)
```
**Explanation**: This code fits a linear model to predict `mpg` using `hp` and `wt`.

---

### **Slide 8: Model Training Process**
1. **Data Collection**
2. **Data Preprocessing**
3. **Model Training**
4. **Model Evaluation**

---

### **Slide 9: Data Preprocessing Steps**
- **Handling Missing Values**: Imputation techniques.
- **Feature Scaling**: Normalization and standardization.
- **Splitting Data**: Train-test split (e.g., 70% training, 30% testing).

---

### **Slide 10: Example – Splitting Data in R**
```r
set.seed(123)
train_index <- sample(1:nrow(mtcars), 0.7 * nrow(mtcars))
train_data <- mtcars[train_index, ]
test_data <- mtcars[-train_index, ]
```

---

### **Slide 11: Supervised Learning Algorithms**
- **Regression**: Linear regression, decision trees.
- **Classification**: Logistic regression, k-Nearest Neighbors (k-NN), SVM.

---

### **Slide 12: Unsupervised Learning Explained**
- **Definition**: Identifies patterns in data without labeled outputs.
- **Examples**: Clustering, dimensionality reduction.

---

### **Slide 13: Common Unsupervised Learning Algorithms**
- **k-Means Clustering**
- **Hierarchical Clustering**
- **Principal Component Analysis (PCA)**

---

### **Slide 14: Code Example – k-Means Clustering**
```r
set.seed(42)
kmeans_result <- kmeans(iris[, 1:4], centers = 3)
plot(iris$Sepal.Length, iris$Sepal.Width, col = kmeans_result$cluster)
```
**Explanation**: Clustering `iris` dataset into 3 groups.

---

### **Slide 15: Visualizing Clusters with ggplot2**
```r
library(ggplot2)
ggplot(iris, aes(Sepal.Length, Sepal.Width, color = factor(kmeans_result$cluster))) +
  geom_point()
```

---

### **Slide 16: Model Evaluation Metrics (Classification)**
- **Accuracy**: The proportion of correctly predicted instances.
- **Precision, Recall, F1 Score**: Metrics to evaluate model quality.

---

### **Slide 17: Example – Confusion Matrix in R**
```r
library(caret)
predictions <- predict(model, newdata = test_data)
confusionMatrix(predictions, test_data$target)
```

---

### **Slide 18: Model Evaluation Metrics (Regression)**
- **Mean Absolute Error (MAE)**: Average of absolute errors.
- **Root Mean Squared Error (RMSE)**: Square root of MSE.

---

### **Slide 19: Example – Regression Metrics Code**
```r
rmse <- sqrt(mean((predictions - test_data$mpg)^2))
print(rmse)
```

---

### **Slide 20: Cross-Validation**
- **Definition**: A technique to assess how a model performs across multiple data splits.
- **k-Fold Cross-Validation**: Data is split into k parts, and the model is trained and tested k times.

---

### **Slide 21: Code Example – Cross-Validation in R**
```r
library(caret)
train_control <- trainControl(method = "cv", number = 5)
model_cv <- train(mpg ~ ., data = mtcars, method = "lm", trControl = train_control)
```

---

### **Slide 22: Exercise 1 – Classification Model on the `iris` Dataset**
- **Objective**: Train a classification model and evaluate its accuracy.
- **Dataset**: `iris` dataset.
- **Task**: Train a logistic regression model to predict `Species`.
- **R Code**:
```r
set.seed(123)
train_index <- sample(1:nrow(iris), 0.7 * nrow(iris))
train_data <- iris[train_index, ]
test_data <- iris[-train_index, ]

# Train the model
model <- glm(Species ~ ., data = train_data, family = 'binomial')

# Make predictions
predictions <- predict(model, test_data, type = 'response')
predicted_classes <- ifelse(predictions > 0.5, 'virginica', 'versicolor')

# Evaluate model performance
library(caret)
confusionMatrix(as.factor(predicted_classes), as.factor(test_data$Species))
```

---

### **Slide 23: Explanation of Exercise 1**
- **Steps**:
  1. Data split into training (70%) and testing (30%).
  2. Logistic regression model is trained.
  3. Predictions are made and converted to binary classes.
  4. Model evaluation using a confusion matrix.
- **Key Points**:
  - The `glm()` function trains the model.
  - The `confusionMatrix()` function from the `caret` package evaluates accuracy.

---

### **Slide 24: Answer and Analysis for Exercise 1**
- **Output**:
  - Confusion matrix with metrics such as accuracy, precision, and recall.
- **Discussion**:
  - Analyze which classes were misclassified and possible reasons (e.g., data imbalance).

---

### **Slide 25: Exercise 2 – Implementing k-Means Clustering**
- **Objective**: Perform k-Means clustering on the `mtcars` dataset and visualize the results.
- **R Code**:
```r
set.seed(42)
kmeans_result <- kmeans(mtcars[, c('mpg', 'hp')], centers = 3)

# Plotting the clusters
plot(mtcars$mpg, mtcars$hp, col = kmeans_result$cluster, pch = 19, main = 'k-Means Clustering')
points(kmeans_result$centers[,1], kmeans_result$centers[,2], col = 1:3, pch = 8, cex = 2)
```

---

### **Slide 26: Explanation of k-Means Clustering Code**
- **Details**:
  - The `set.seed()` function ensures reproducibility.
  - `kmeans()` partitions the data into 3 clusters.
  - Cluster centers are plotted for better visualization.
- **Key Insight**: Understanding how data points are grouped and the cluster centroids.

---

### **Slide 27: Exercise 2 – Analysis and Interpretation**
- **Output**: Visual scatter plot showing clusters.
- **Discussion**:
  - Identify clusters and interpret their characteristics (e.g., high `mpg` with low `hp`).
- **Question**: What do these clusters tell us about the cars?

---

### **Slide 28: Exercise 3 – Applying Cross-Validation**
- **Objective**: Apply 5-fold cross-validation to a regression model on the `mtcars` dataset.
- **R Code**:
```r
library(caret)
train_control <- trainControl(method = 'cv', number = 5)

# Train the model with cross-validation
cv_model <- train(mpg ~ ., data = mtcars, method = 'lm', trControl = train_control)
print(cv_model)
```

---

### **Slide 29: Explanation of Cross-Validation Code**
- **Details**:
  - `trainControl()` specifies cross-validation parameters.
  - `train()` fits the model using specified training controls.
- **Benefits**: Provides a more reliable estimate of model performance by reducing overfitting.

---

### **Slide 30: Cross-Validation Results**
- **Output**: Summary of model performance across folds.
- **Interpretation**:
  - Analyze mean and standard deviation of performance metrics (e.g., RMSE).
- **Exercise**: What would happen if we increased the number of folds?

---

### **Slide 31: Exercise 4 – Dimensionality Reduction with PCA**
- **Objective**: Conduct PCA on the `iris` dataset and interpret the findings.
- **R Code**:
```r
pca_result <- prcomp(iris[, -5], scale. = TRUE)
summary(pca_result)

# Visualize PCA
biplot(pca_result, scale = 0)
```

---

### **Slide 32: Explanation of PCA Code**
- **Steps**:
  - `prcomp()` computes PCA with scaling.
  - `summary()` provides the proportion of variance explained by each component.
  - `biplot()` visualizes principal components.
- **Interpretation**: How much variance do the first two components capture?

---

### **Slide 33: Analyzing PCA Output**
- **Discussion**:
  - Identify which features contribute most to PC1 and PC2.
  - **Key Question**: How could PCA help simplify modeling?

---

### **Slide 34: Exercise 5 – Hierarchical Clustering on `mtcars`**
- **Objective**: Perform hierarchical clustering and create a dendrogram for visualization.
- **R Code**:
```r
# Calculate distance matrix
dist_matrix <- dist(mtcars)

# Perform hierarchical clustering
hclust_result <- hclust(dist_matrix)

# Plot the dendrogram
plot(hclust_result, main = 'Dendrogram for Hierarchical Clustering', xlab = '', sub = '')
```

---

### **Slide 35: Explanation of Hierarchical Clustering Code**
- **Steps**:
  - `dist()` computes the distance matrix.
  - `hclust()` performs clustering based on the distance matrix.
  - `plot()` visualizes the dendrogram.
- **Key Insight**: Identifying which groups of data points cluster together at different levels.

---

### **Slide 36: Analysis of the Dendrogram**
- **Interpretation**:
  - Observe clusters by cutting the dendrogram at different heights.
  - Discuss the natural groupings in `mtcars` and potential insights.
- **Exercise**: Identify how many clusters you would choose based on the dendrogram.

---

### **Slide 37: Exercise 6 – Feature Scaling and SVM on `iris`**
- **Objective**: Perform feature scaling and train a Support Vector Machine (SVM) for classification.
- **R Code**:
```r
library(e1071)

# Scale features
iris_scaled <- scale(iris[, -5])

# Train SVM model
svm_model <- svm(Species ~ ., data = data.frame(iris_scaled, Species = iris$Species))

# Predict on scaled data
predictions <- predict(svm_model, iris_scaled)

# Model evaluation
library(caret)
confusionMatrix(predictions, iris$Species)
```

---

### **Slide 38: Explanation of SVM and Scaling**
- **Steps**:
  - `scale()` normalizes the feature values.
  - `svm()` trains the SVM model on scaled features.
  - `predict()` makes predictions on the scaled dataset.
- **Key Insight**: Why scaling is crucial for SVM performance.

---

### **Slide 39: Analysis of SVM Results**
- **Output**:
  - View the confusion matrix to evaluate model accuracy.
- **Discussion**:
  - Analyze misclassified samples and potential reasons (e.g., overlapping classes).
- **Exercise**: Try tuning SVM parameters to improve accuracy.

---

### **Slide 40: Exercise 7 – Hyperparameter Tuning with `caret`**
- **Objective**: Use `caret` for hyperparameter tuning of an SVM model.
- **R Code**:
```r
train_control <- trainControl(method = 'cv', number = 5)
svm_tuned <- train(Species ~ ., data = iris, method = 'svmRadial', trControl = train_control, tuneLength = 5)
print(svm_tuned)
```

---

### **Slide 41: Explanation of Hyperparameter Tuning Code**
- **Details**:
  - `trainControl()` sets up cross-validation.
  - `train()` performs hyperparameter tuning using grid search.
- **Key Insight**: The importance of tuning hyperparameters for optimal performance.

---

### **Slide 42: Results and Interpretation of Tuning**
- **Output**: Best hyperparameters chosen and their effect on model accuracy.
- **Exercise**: Discuss how changing `tuneLength` affects the tuning process and results.

---

### **Slide 43: Exercise 8 – Combining PCA and Clustering**
- **Objective**: Perform PCA for dimensionality reduction and then apply k-Means clustering.
- **R Code**:
```r
# PCA for dimensionality reduction
pca_result <- prcomp(iris[, -5], scale. = TRUE)

# Use first 2 principal components for clustering
pca_data <- data.frame(pca_result$x[, 1:2])
kmeans_pca <- kmeans(pca_data, centers = 3)

# Plot results
plot(pca_data, col = kmeans_pca$cluster, pch = 19, main = 'PCA and k-Means Clustering')
```

---

### **Slide 44: Explanation of PCA and Clustering Code**
- **Steps**:
  - `prcomp()` computes PCA.
  - `kmeans()` clusters the transformed data.
- **Key Insight**: How PCA simplifies data for clustering tasks.

---

### **Slide 45: Analyzing the Combined Results**
- **Output**: Clustering visualization using principal components.
- **Discussion**:
  - Evaluate cluster quality and how PCA affected the clustering process.
- **Exercise**: Compare results with and without PCA.

---

### **Slide 46: Exercise 9 – Creating an End-to-End ML Pipeline**
- **Objective**: Develop a complete pipeline from data preprocessing to model training and evaluation.
- **R Code Skeleton**:
```r
# Load data and split into train/test
set.seed(123)
data_split <- sample(1:nrow(mtcars), 0.7 * nrow(mtcars))
train_set <- mtcars[data_split, ]
test_set <- mtcars[-data_split, ]

# Data preprocessing: scaling
train_set_scaled <- scale(train_set[, -1])
test_set_scaled <- scale(test_set[, -1])

# Train linear model
lm_model <- lm(mpg ~ ., data = data.frame(train_set_scaled, mpg = train_set$mpg))

# Evaluate on test set
predictions <- predict(lm_model, newdata = data.frame(test_set_scaled))
```

---

### **Slide 47: Explanation of the ML Pipeline**
- **Steps**:
  - Split data, scale features, train model, and evaluate predictions.
- **Key Insight**: The importance of maintaining consistent preprocessing for both training and test sets.

---

### **Slide 48: Exercise 10 – Visualizing Model Performance**
- **Objective**: Plot predicted vs. actual values for regression.
- **R Code**:
```r
plot(test_set$mpg, predictions, main = 'Predicted vs Actual MPG', xlab = 'Actual MPG', ylab = 'Predicted MPG')
abline(0, 1, col = 'red')
```

---

### **Slide 49: Interpretation of Model Performance Plot**
- **Analysis**:
  - Evaluate the alignment of points along the `y = x` line for goodness of fit.
- **Discussion**: How close are the predictions to the actual values?

---

### **Slide 50: Recap and Q&A**
- **Summary**:
  - Review of supervised and unsupervised learning concepts.
  - Discussion of key exercises and takeaways.
- **Q&A Session**: Open floor for questions and clarifications.

