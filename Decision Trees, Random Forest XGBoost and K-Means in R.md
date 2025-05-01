---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---
---

### **Slide 1: Title Slide**
- **Course Title**: Machine Learning in R
- **Topic**: Decision Trees, Random Forest, XGBoost, and K-Means

---

### **Slide 2: Learning Objectives**
- Understand key concepts and principles behind decision trees, random forest, XGBoost, and k-means clustering.
- Implement these algorithms in R using practical examples.
- Interpret and evaluate the model outputs.
- Engage in hands-on exercises with solutions.

---

### **Slide 3: Introduction to Decision Trees**
- **Definition**: A tree-based model for decision-making and prediction where nodes represent tests on features, branches represent decision rules, and leaves represent outcomes.
- **Key Concept**: Recursive binary splitting partitions the data.
- **Benefits**: Easy interpretation and visual representation.
- **Limitations**: Prone to overfitting.

---

### **Slide 4: Decision Tree Structure**
- **Root Node**: The topmost node representing the complete dataset.
- **Internal Nodes**: Intermediate nodes that test conditions.
- **Leaf Nodes**: Terminal nodes that provide the predicted result.
- **Splitting Criteria**: Gini impurity or entropy for classification.

---

### **Slide 5: Implementing Decision Trees in R**
**R Code Example**:
```r
# Load required package
library(rpart)
# Fit decision tree model
model_tree <- rpart(Species ~ ., data = iris, method = 'class')
# Visualize the tree
library(rpart.plot)
rpart.plot(model_tree)
```
**Explanation**: The `rpart()` function constructs the model, and `rpart.plot()` helps visualize it.

---

### **Slide 6: Interpreting a Decision Tree**
- **Splits**: Represent decisions made based on feature values.
- **Tree Depth**: Influences complexity and risk of overfitting.
- **Example**: If `Petal.Length <= 2.45`, classify as `setosa`.

**Visual Aid**: Diagram showing a decision path from the root to a leaf.

---

### **Slide 7: Exercise – Building and Visualizing a Decision Tree**
**Task**: Create a decision tree using the `mtcars` dataset to predict `am` (automatic/manual transmission).
**R Code**:
```r
model_tree_mtcars <- rpart(am ~ hp + wt + qsec, data = mtcars, method = 'class')
rpart.plot(model_tree_mtcars)
```
**Solution**: Analyze the decision nodes and understand the splits.

---

### **Slide 8: Evaluating Decision Tree Performance**
**Metrics**:
- **Accuracy**: Proportion of correctly classified instances.
- **Confusion Matrix**: Provides insights into true positives, false positives, etc.
**R Code**:
```r
predicted <- predict(model_tree_mtcars, type = 'class')
confusionMatrix(as.factor(predicted), as.factor(mtcars$am))
```

---

### **Slide 9: Introduction to Random Forest**
- **Definition**: Ensemble method building multiple decision trees to output the majority class (classification) or mean prediction (regression).
- **Concepts**: Bagging (bootstrap aggregating) and feature randomness to improve generalization.
- **Advantages**: Robust to overfitting, handles non-linear relationships well.
- **Limitations**: Less interpretable than individual trees.

---

### **Slide 10: How Random Forest Works**
- **Bagging**: Multiple trees are built using random samples from the training data.
- **Feature Randomness**: Random selection of features at each split ensures diversity among trees.
- **Voting Mechanism**: The final output is determined by majority voting (classification) or averaging (regression).

---

### **Slide 11: Implementing Random Forest in R**
**R Code Example**:
```r
library(randomForest)
# Fit a random forest model
model_rf <- randomForest(Species ~ ., data = iris, ntree = 100)
# View model summary
print(model_rf)
```
**Explanation**: The `randomForest()` function fits the model with `ntree` specifying the number of trees.

---

### **Slide 12: Tuning Random Forest Hyperparameters**
- **Key Parameters**:
  - `ntree`: Number of trees.
  - `mtry`: Number of features considered for splitting at each node.
**R Code**:
```r
model_rf_tuned <- randomForest(Species ~ ., data = iris, ntree = 150, mtry = 2)
```
**Impact**: Fine-tuning helps optimize model performance.

---

### **Slide 13: Exercise – Random Forest for Classification**
**Task**: Build a random forest model using `mtcars` to predict `am`.
**R Code**:
```r
model_rf_mtcars <- randomForest(am ~ hp + wt + qsec, data = mtcars, ntree = 100)
```
**Solution**: Evaluate feature importance and model accuracy.

---

### **Slide 14: Analyzing Feature Importance in Random Forest**
- **Concept**: Measures the contribution of each feature in predicting the target variable.
**R Code**:
```r
importance(model_rf)
varImpPlot(model_rf)
```
**Explanation**: Higher scores indicate more influential features.

---

### **Slide 15: Introduction to XGBoost**
- **Definition**: An efficient implementation of gradient boosting for supervised learning.
- **Core Idea**: Sequentially builds trees that correct errors made by previous models.
- **Advantages**: High predictive accuracy, supports regularization, handles missing data.
- **Disadvantages**: Complex tuning, potential overfitting without proper regulation.

---

### **Slide 16: How XGBoost Works**
- **Boosting Process**: Gradually improves model performance by adding weak learners.
- **Learning Rate (`eta`)**: Controls the contribution of each tree.
- **Regularization**: Helps prevent overfitting (L1/L2 penalties).

**Diagram**: Sequential boosting cycle with error correction.

---

### **Slide 17: Implementing XGBoost in R**
**R Code Example**:
```r
library(xgboost)
# Prepare data matrix
iris_matrix <- model.matrix(Species ~ . - 1, data = iris)
label <- as.numeric(iris$Species) - 1
dtrain <- xgb.DMatrix(data = iris_matrix, label = label)

# Train model
model_xgb <- xgboost(data = dtrain, max.depth = 3, eta = 0.1, nrounds = 100, objective = 'multi:softmax', num_class = 3)
```
**Explanation**: The `xgb.DMatrix()` function structures data, and `xgboost()` trains the model.

---

### **Slide 18: Tuning Hyperparameters in XGBoost**
- **Important Parameters**:
  - `max.depth`: Depth of trees.
  - `eta`: Learning rate.
  - `nrounds`: Number of boosting iterations.
  - `gamma`: Minimum loss reduction required for a split.
**R Code**:
```r
model_xgb_tuned <- xgboost(data = dtrain, max.depth = 5, eta = 0.05, nrounds = 150, objective = 'multi:softmax', num_class = 3)
```

---

### **Slide 19: Evaluating XGBoost Model Performance**
**Metrics**:
- **Accuracy** and **Confusion Matrix**.
**R Code**:
```r
preds <- predict(model_xgb, dtrain)
table(preds, label)
```

---

### **Slide 20: Exercise – Training an XGBoost Model on `mtcars`**
**Task**: Train and evaluate an XGBoost model for predicting `am`.
**R Code**:
```r
mtcars_matrix <- model.matrix(am ~ . - 1, data = mtcars)
label_mtcars <- mtcars$am
dtrain_mtcars <- xgb.DMatrix(data = mtcars_matrix, label = label_mtcars)
model_xgb_mtcars <- xgboost(data = dtrain_mtcars, max.depth = 3, eta = 0.1, nrounds = 100, objective = 'binary:logistic')
```

**Solution Analysis**: Interpret the model's confusion matrix and AUC score.

---

### **Slide 21: Introduction to K-Means Clustering**
- **Definition**: Unsupervised algorithm that partitions data into `k` clusters.
- **Algorithm**:
  - Initialize `k` centroids.
  - Assign points to the nearest centroid.
  - Recalculate centroids.
  - Repeat until convergence.

**Diagram**: K-means clustering example with `k` clusters.

---

### **Slide 22: Implementing K-Means in R**
**R Code Example**:
```r
set.seed(123)
kmeans_result <- kmeans(iris[, -5], centers = 3, nstart = 25)
```
**Explanation**: The `kmeans()` function partitions data into `k` clusters. The `nstart` parameter controls the number of initial configurations.

---


### **Slide 23: Visualizing K-Means Clusters**
**Concept**: Visualizing clusters helps understand how data points are grouped and assess the quality of clustering.
**R Code**:
```r
library(ggplot2)
iris$cluster <- as.factor(kmeans_result$cluster)

# Create scatter plot to visualize clusters
ggplot(iris, aes(Petal.Length, Petal.Width, color = cluster)) +
  geom_point(size = 3) +
  labs(title = "K-Means Clustering on Iris Dataset", x = "Petal Length", y = "Petal Width") +
  theme_minimal()
```
**Explanation**: This visualization provides insights into how well-separated the clusters are based on the chosen features.

---

### **Slide 24: Determining the Optimal Number of Clusters (Elbow Method)**
- **Concept**: The Elbow Method is used to find the optimal `k` by plotting the total within-cluster sum of squares (WSS) against the number of clusters and identifying the 'elbow' point.
**R Code**:
```r
wss <- sapply(1:10, function(k) {
  kmeans(iris[, -5], centers = k, nstart = 25)$tot.withinss
})
plot(1:10, wss, type = "b", pch = 19, frame = FALSE,
     xlab = "Number of Clusters K", ylab = "Total Within-Cluster Sum of Squares",
     main = "Elbow Method for Optimal K")
```
**Explanation**: The 'elbow' point in the plot suggests the optimal number of clusters where adding more clusters does not significantly improve the model.

---

### **Slide 25: Evaluating K-Means Clustering Performance**
**Metrics**:
- **Silhouette Score**: Measures how similar each data point is to its own cluster compared to other clusters.
- **Total WSS**: Indicates the compactness of the clusters.
**R Code**:
```r
library(cluster)
silhouette_score <- silhouette(kmeans_result$cluster, dist(iris[, -5]))
plot(silhouette_score, main = "Silhouette Plot for K-Means Clustering")
```
**Explanation**: A higher average silhouette width indicates better-defined clusters.

---

### **Slide 26: Exercise – Implementing K-Means Clustering**
**Task**: Perform K-means clustering on the `mtcars` dataset with `k = 3`.
**R Code**:
```r
set.seed(456)
kmeans_mtcars <- kmeans(mtcars, centers = 3, nstart = 20)
mtcars$cluster <- as.factor(kmeans_mtcars$cluster)

# Visualize results
pairs(mtcars[, c("mpg", "hp", "wt")], col = mtcars$cluster, pch = 19)
```
**Solution Analysis**: Visualize and evaluate the distinctiveness of the clusters formed.

---

### **Slide 27: Practical Applications of Decision Trees**
**Use Cases**:
- **Healthcare**: Predicting patient outcomes based on clinical data.
- **Finance**: Assessing credit risk and loan approvals.
- **Marketing**: Customer segmentation and targeting strategies.
**Discussion Prompt**: Share examples of how decision trees could be applied in your field.

---

### **Slide 28: Practical Applications of Random Forest**
**Use Cases**:
- **Fraud Detection**: Identifying fraudulent transactions in banking.
- **Predictive Maintenance**: Forecasting machine failure in manufacturing.
- **Environmental Science**: Classifying land cover using satellite imagery.
**Discussion Prompt**: Why is random forest preferred over individual decision trees for these applications?

---

### **Slide 29: Practical Applications of XGBoost**
**Use Cases**:
- **Data Science Competitions**: Often used for winning Kaggle competitions due to its high accuracy.
- **Customer Retention**: Predicting customer churn in telecom.
- **Finance**: Stock market trend prediction.
**Discussion Prompt**: Discuss scenarios where XGBoost outperforms other algorithms.

---

### **Slide 30: Practical Applications of K-Means Clustering**
**Use Cases**:
- **Market Segmentation**: Grouping customers based on buying behavior.
- **Image Compression**: Reducing the number of colors in an image.
- **Anomaly Detection**: Identifying unusual patterns or outliers.
**Discussion Prompt**: How can K-means clustering be used in exploratory data analysis?

---

### **Slide 31: Comparing Decision Trees, Random Forest, and XGBoost**
**Comparison Table**:
- **Decision Trees**:
  - **Pros**: Easy to interpret, simple visualization.
  - **Cons**: High variance and prone to overfitting.
- **Random Forest**:
  - **Pros**: Reduces overfitting, robust model.
  - **Cons**: Less interpretable.
- **XGBoost**:
  - **Pros**: High accuracy, handles complex data well.
  - **Cons**: Complex to tune and train.
**Visual Aid**: Table or chart comparing the algorithms.

---

### **Slide 32: Comparing Clustering Algorithms**
- **K-Means**:
  - **Strengths**: Simple, scalable for large datasets.
  - **Weaknesses**: Sensitive to initial centroid selection.
- **Hierarchical Clustering**:
  - **Strengths**: Provides a dendrogram showing cluster hierarchy.
  - **Weaknesses**: Computationally intensive for large datasets.
**Discussion**: Discuss use cases suitable for each clustering algorithm.

---

### **Slide 33: Exercise – Applying Ensemble Methods**
**Task**: Use bagging to combine decision trees into an ensemble model.
**R Code**:
```r
library(ipred)
bagged_model <- bagging(Species ~ ., data = iris, nbagg = 50)
predictions <- predict(bagged_model, iris)
confusionMatrix(predictions, iris$Species)
```
**Solution Analysis**: Assess how the ensemble model affects accuracy and robustness.

---

### **Slide 34: Tuning Parameters in Ensemble Models**
**Focus**:
- **Random Forest**: Tuning `ntree` and `mtry` to improve performance.
- **XGBoost**: Adjusting `eta`, `max.depth`, and `nrounds` for better accuracy.
**R Code**:
```r
model_rf_tuned <- randomForest(Species ~ ., data = iris, ntree = 200, mtry = 2)
```
**Discussion**: Best practices for tuning parameters for optimal model performance.

---

### **Slide 35: Visualizing Feature Importances**
**Concept**: Understanding which variables contribute most to the model's predictions.
**R Code**:
```r
varImpPlot(model_rf)
```
**Explanation**: This plot helps identify the most influential features for model decision-making.

---

### **Slide 36: Advanced Case Study – Customer Segmentation with K-Means**
**Scenario**: Segment a retail company's customer base using transaction data.
**Steps**:
1. Preprocess and scale the data.
2. Apply K-means clustering.
3. Visualize and interpret results.
**R Code**:
```r
scaled_data <- scale(customer_data)
kmeans_customer <- kmeans(scaled_data, centers = 4, nstart = 25)
```
**Outcome**: Insights into customer behavior patterns.

---

### **Slide 37: Handling Categorical Data in Decision Trees**
**Concept**: Decision trees can handle both continuous and categorical data.
**Technique**: Convert categorical variables using one-hot encoding or factor levels in R.
**Example**:
```r
data$category <- as.factor(data$category)
model_tree <- rpart(outcome ~ ., data = data)
```

---

### **Slide 38: Exercise – Applying XGBoost for Classification**
**Task**: Train an XGBoost model to classify outcomes using the `mtcars` dataset.
**R Code**:
```r
mtcars_matrix <- model.matrix(am ~ . - 1, data = mtcars)
label_mtcars <- mtcars$am
dtrain_mtcars <- xgb.DMatrix(data = mtcars_matrix, label = label_mtcars)
model_xgb_mtcars <- xgboost(data = dtrain_mtcars, max.depth = 3, eta = 0.1, nrounds = 100, objective = 'binary:logistic')
```
**Solution**: Evaluate the model with accuracy and confusion matrix analysis.

---

### **Slide 39: Visualizing Model Performance (ROC Curve)**
**R Code**:
```r
library(pROC)
roc_curve <- roc(label_mtcars, predict(model_xgb_mtcars, dtrain_mtcars))
plot(roc_curve, main = \"ROC Curve for XGBoost Model\")
```
**Explanation**: The ROC curve helps assess the trade-off between sensitivity and specificity.

---

### **Slide 40: Exercise – Visualizing Clusters in a New Dataset**
**Task**: Apply K-means clustering to a different dataset and visualize the clusters.
**R Code**:
```r
new_data_scaled <- scale(new_data)
kmeans_new <- kmeans(new_data_scaled, centers = 3, nstart = 20)
new_data$cluster <- as.factor(kmeans_new$cluster)
ggplot(new_data, aes(feature1, feature2, color = cluster)) + geom_point()
```
**Analysis**: Interpret the clustering results and assess the cluster quality.

--- 

### **Slide 41: Common Pitfalls in Decision Trees and How to Avoid Them**
**Challenges**:
- **Overfitting**: When the model fits the training data too closely and fails to generalize to new data.
- **High Variance**: Leads to poor model performance on unseen data.
- **Bias-Variance Trade-off**: Balancing complexity and accuracy is crucial.
**Solutions**:
- **Pruning**: Simplifies the tree by removing sections that provide little predictive power.
- **Cross-Validation**: Helps ensure the model performs well on unseen data.
**R Code Example**:
```r
pruned_tree <- prune(model_tree, cp = 0.02)
rpart.plot(pruned_tree)
```
**Explanation**: Pruning reduces the depth of the tree to avoid overfitting and enhance generalizability.

---

### **Slide 42: Using Cross-Validation in R for Model Evaluation**
**Concept**: Cross-validation splits the dataset into training and testing subsets multiple times to validate model performance reliably.
**Types**:
- **k-Fold Cross-Validation**: Data is split into `k` parts; each part is used as a validation set once.
- **Leave-One-Out Cross-Validation (LOOCV)**: Each data point acts as a validation set once.
**R Code**:
```r
library(caret)
train_control <- trainControl(method = "cv", number = 5)
model_cv <- train(Species ~ ., data = iris, method = "rpart", trControl = train_control)
print(model_cv)
```
**Explanation**: Cross-validation helps prevent overfitting and assesses how well the model generalizes to new data.

---

### **Slide 43: Advanced Exercise – Combining Models for Better Performance**
**Objective**: Create an ensemble model combining decision trees, random forest, and XGBoost for improved predictive accuracy.
**R Code**:
```r
library(caretEnsemble)
model_list <- caretList(Species ~ ., data = iris, trControl = trainControl(method = "cv", number = 5),
                        methodList = c("rpart", "rf", "xgbTree"))
ensemble_model <- caretEnsemble(model_list)
summary(ensemble_model)
```
**Solution Analysis**: Evaluate how the ensemble approach leverages the strengths of each model for robust predictions.

---

### **Slide 44: Handling Missing Data in Clustering**
**Concept**: K-means clustering cannot directly handle datasets with missing values.
**Techniques to Address Missing Data**:
- **Imputation**: Replace missing values with statistical measures (mean, median, mode).
- **Removing Missing Data**: Only applicable if the dataset size permits.
**R Code**:
```r
iris_with_na <- iris
iris_with_na[sample(1:nrow(iris_with_na), 5), 1] <- NA
preprocess_model <- preProcess(iris_with_na, method = "medianImpute")
iris_imputed <- predict(preprocess_model, iris_with_na)
```
**Explanation**: Imputation ensures the data is complete for clustering analysis.

---

### **Slide 45: Real-World Case Study – Predicting Customer Churn with Decision Trees**
**Scenario**: A telecom company wants to predict customer churn using decision trees.
**Approach**:
1. **Load and preprocess customer data**.
2. **Train a decision tree model**.
3. **Evaluate the model with a confusion matrix**.
**R Code**:
```r
customer_tree <- rpart(Churn ~ Age + Contract + MonthlyCharges, data = telecom_data, method = "class")
rpart.plot(customer_tree)
predicted_churn <- predict(customer_tree, type = "class")
confusionMatrix(predicted_churn, telecom_data$Churn)
```
**Outcome**: Gain insights into which features contribute most to customer churn and identify potential retention strategies.

---

### **Slide 46: Best Practices for Model Optimization**
**Tips for Optimization**:
- **Grid Search**: A method to exhaustively search for the optimal hyperparameters by testing all possible combinations.
- **Early Stopping (for XGBoost)**: Prevents overfitting by halting training when no significant improvement is detected in validation metrics.
**R Code Example**:
```r
library(caret)
tune_grid <- expand.grid(mtry = c(2, 3), ntree = c(100, 200))
rf_tuned <- train(Species ~ ., data = iris, method = "rf", trControl = trainControl(method = "cv", number = 5), tuneGrid = tune_grid)
print(rf_tuned)
```
**Explanation**: Using `train()` with `tuneGrid` in `caret` allows for systematic hyperparameter tuning.

---

### **Slide 47: Using Feature Engineering to Improve Model Performance**
**Concept**: Feature engineering involves creating new features or modifying existing ones to enhance model learning.
**Examples**:
- **Feature Interaction**: Combining two features to capture non-linear relationships.
- **Scaling**: Normalizing data to improve convergence in gradient-boosting models.
**R Code**:
```r
iris$PetalRatio <- iris$Petal.Length / iris$Petal.Width
model_rf_fe <- randomForest(Species ~ . + PetalRatio, data = iris)
```
**Outcome**: Enhanced model performance due to the inclusion of a relevant feature.

---

### **Slide 48: Final Exercise – Comprehensive Model Building**
**Task**: Create a comprehensive pipeline that includes data preprocessing, feature engineering, and model training.
**Steps**:
1. **Impute Missing Values**.
2. **Create New Features**.
3. **Train Decision Tree, Random Forest, and XGBoost Models**.
**R Code**:
```r
# Preprocessing and imputation
iris_na[sample(1:nrow(iris_na), 5), 1] <- NA
preprocess_model <- preProcess(iris_na, method = c("medianImpute", "center", "scale"))
iris_preprocessed <- predict(preprocess_model, iris_na)

# Feature engineering
iris_preprocessed$SepalPetalRatio <- iris_preprocessed$Sepal.Length / iris_preprocessed$Petal.Length

# Model training
set.seed(789)
model_tree_final <- rpart(Species ~ ., data = iris_preprocessed)
model_rf_final <- randomForest(Species ~ ., data = iris_preprocessed, ntree = 150)
model_xgb_final <- xgboost(data = model.matrix(Species ~ . - 1, data = iris_preprocessed), label = as.numeric(iris_preprocessed$Species) - 1, max.depth = 4, eta = 0.1, nrounds = 100, objective = 'multi:softmax', num_class = 3)
```
**Solution Analysis**: Compare models using accuracy metrics and choose the best-performing one.

---

### **Slide 49: Model Evaluation and Comparison**
**Metrics to Use**:
- **Accuracy**: The percentage of correct predictions.
- **Precision and Recall**: Evaluate model sensitivity and specificity.
- **AUC-ROC Curve**: Particularly useful for binary classification models.
**R Code**:
```r
library(pROC)
roc_rf <- roc(as.numeric(iris_preprocessed$Species) - 1, predict(model_rf_final, type = "prob")[, 2])
plot(roc_rf, main = "ROC Curve for Random Forest")
```
**Outcome**: Assess the overall performance and reliability of each model through visualization.

---

### **Slide 50: Conclusion and Next Steps**
**Summary**:
- Reviewed decision trees, random forest, XGBoost, and k-means clustering.
- Implemented models in R with detailed code examples and exercises.
- Practiced feature engineering and hyperparameter tuning to improve model outcomes.
**Next Steps**:
- Apply these algorithms and techniques to real-world datasets.
- Explore advanced topics such as ensemble learning and deep learning methods in R.
**Final Exercise**: Experiment with combining models using stacking for a final project.

---


