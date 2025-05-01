---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---

#### **Introduction to Visualization with `ggplot2` and Other R Packages**

---

**Slide 1: Title Slide**  
   - **Title**: Visualization of Training Outputs in Machine Learning  
   - **Subtitle**: Using `ggplot2` and Other R Packages  
   - **Objective**: Learn how to visualize model training results and evaluation metrics to gain insights into model performance.  

---

**Slide 2: Importance of Visualization in Model Training**  
   - **Explanation**: Visualizations help identify trends, potential overfitting, feature importance, and model performance.
   - **Objective**: Show how visualizations make model evaluation more intuitive and actionable.

---

**Slide 3: Overview of Visualization Packages in R**
   - **Packages**:
     - `ggplot2`: General-purpose plotting.
     - `caret`: Provides visualizations for resampling and cross-validation.
     - `pROC`: ROC and AUC visualization.
     - `plotly`: Interactive plots for dynamic data exploration.
   - **Usage**: Each package has a unique focus, enhancing different aspects of model training and evaluation visualization.

---

### **Visualizing Model Training with ggplot2**

---

**Slide 4: Introduction to `ggplot2` Syntax for Model Plots**
   - **Explanation**: Basic syntax for creating a plot with `ggplot2`.
   - **Code Example**:
     ```r
     library(ggplot2)
     ggplot(data, aes(x = feature, y = outcome)) + geom_point()
     ```

---

**Slide 5: Plotting Training and Validation Accuracy Over Epochs**
   - **Concept**: Track accuracy over epochs to identify overfitting or underfitting.
   - **R Code**:
     ```r
     epochs <- 1:50
     accuracy <- runif(50, 0.7, 1)
     val_accuracy <- runif(50, 0.6, 0.9)
     data <- data.frame(epochs, accuracy, val_accuracy)
     ggplot(data) +
       geom_line(aes(x = epochs, y = accuracy, color = "Train")) +
       geom_line(aes(x = epochs, y = val_accuracy, color = "Validation")) +
       labs(title = "Training vs. Validation Accuracy", x = "Epoch", y = "Accuracy")
     ```

---

**Slide 6: Visualizing Confusion Matrix with `ggplot2`**
   - **Concept**: Confusion matrix shows true vs. predicted values.
   - **Example**:
     ```r
     library(caret)
     predictions <- factor(c("Yes", "No", "Yes", "Yes", "No"))
     actual <- factor(c("Yes", "No", "No", "Yes", "Yes"))
     confusionMatrix(predictions, actual)
     ```
   - **Visualization**: Plot confusion matrix as a heatmap for better visual clarity.

---

**Slide 7: Plotting Precision and Recall for Each Class**
   - **Concept**: Precision and recall by class using bar plots.
   - **R Code**:
     ```r
     metrics <- data.frame(
       Class = c("Class 1", "Class 2", "Class 3"),
       Precision = c(0.8, 0.75, 0.9),
       Recall = c(0.85, 0.7, 0.88)
     )
     ggplot(metrics, aes(x = Class, y = Precision, fill = "Precision")) + 
       geom_bar(stat = "identity") +
       geom_bar(aes(y = Recall, fill = "Recall"), stat = "identity", alpha = 0.5)
     ```

---

### **Visualizing Model Performance with ROC and Precision-Recall Curves**

---

**Slide 8: ROC Curve with `pROC` Package**
   - **Concept**: ROC curve evaluates classification model performance.
   - **R Code**:
     ```r
     library(pROC)
     actual <- factor(c(1, 0, 1, 1, 0, 1, 0, 0, 1, 0))
     predicted_probs <- c(0.9, 0.4, 0.8, 0.7, 0.3, 0.9, 0.1, 0.4, 0.8, 0.5)
     roc_curve <- roc(actual, predicted_probs)
     plot(roc_curve, main = "ROC Curve")
     auc(roc_curve)
     ```

**Slide 9: Precision-Recall Curve**
   - **Concept**: Useful for imbalanced datasets, especially in high-precision applications.
   - **Example Code**:
     ```r
     library(precrec)
     pr <- evalmod(scores = predicted_probs, labels = actual, mode = "PRC")
     autoplot(pr, main = "Precision-Recall Curve")
     ```

---

### **Feature Importance Visualization in Model Training**

---

**Slide 10: Visualizing Feature Importance with `randomForest`**
   - **Concept**: Feature importance in decision trees and random forests shows which variables contribute most to predictions.
   - **R Code**:
     ```r
     library(randomForest)
     rf_model <- randomForest(Species ~ ., data = iris)
     importance(rf_model)
     varImpPlot(rf_model)
     ```

**Slide 11: Feature Importance with `caret` Package**
   - **Explanation**: `caret` offers model-agnostic feature importance.
   - **R Code**:
     ```r
     library(caret)
     model <- train(Species ~ ., data = iris, method = "rf")
     varImp(model)
     ```

---

### **Comparing Model Performance Using `ggplot2`**

---

**Slide 12: Bar Plot for Model Comparison**
   - **Concept**: Visualize metrics (accuracy, precision, recall) for multiple models.
   - **Example Code**:
     ```r
     model_metrics <- data.frame(
       Model = c("Logistic Regression", "Random Forest", "SVM"),
       Accuracy = c(0.8, 0.85, 0.82),
       F1_Score = c(0.75, 0.82, 0.78)
     )
     ggplot(model_metrics, aes(x = Model, y = Accuracy)) +
       geom_bar(stat = "identity", fill = "skyblue") +
       labs(title = "Model Accuracy Comparison")
     ```

**Slide 13: Visualizing Cross-Validation Results**
   - **Concept**: Cross-validation metrics like accuracy and AUC over folds.
   - **R Code**:
     ```r
     library(caret)
     control <- trainControl(method = "cv", number = 5, savePredictions = TRUE)
     model <- train(Species ~ ., data = iris, method = "rf", trControl = control)
     res <- model$resample
     ggplot(res, aes(x = Resample, y = Accuracy)) + 
       geom_point() + 
       geom_line() +
       labs(title = "Cross-Validation Accuracy Across Folds")
     ```

---

### **Interactive Visualization with `plotly`**

---

**Slide 14: Introduction to `plotly` for Interactive Visualizations**
   - **Explanation**: `plotly` allows adding interactivity to `ggplot2` plots.
   - **Example**:
     ```r
     library(plotly)
     p <- ggplot(data, aes(x = feature, y = outcome)) + geom_point()
     ggplotly(p)
     ```

**Slide 15: Visualizing Decision Boundaries in 2D**
   - **Concept**: Visualize decision boundaries for two features.
   - **Example Code**:
     ```r
     library(ggplot2)
     model <- glm(Species ~ Petal.Length + Petal.Width, data = iris, family = "binomial")
     grid <- expand.grid(Petal.Length = seq(min(iris$Petal.Length), max(iris$Petal.Length), length.out = 100),
                         Petal.Width = seq(min(iris$Petal.Width), max(iris$Petal.Width), length.out = 100))
     grid$prob <- predict(model, newdata = grid, type = "response")
     ggplot(iris, aes(Petal.Length, Petal.Width)) +
       geom_point(aes(color = Species)) +
       geom_contour(data = grid, aes(z = prob, color = ..level..))
     ```

---

#### **Slide 16: Exercise 1 – Plotting Training vs. Validation Loss**

   - **Scenario**: You are training a neural network model and want to visualize training and validation loss over epochs to diagnose potential overfitting.
   - **Task**: Plot training and validation loss over 50 epochs.
   - **Solution Code**:
     ```r
     epochs <- 1:50
     train_loss <- runif(50, 0.1, 0.5)
     val_loss <- runif(50, 0.2, 0.6)
     data <- data.frame(epochs, train_loss, val_loss)
     ggplot(data) +
       geom_line(aes(x = epochs, y = train_loss, color = "Train Loss")) +
       geom_line(aes(x = epochs, y = val_loss, color = "Validation Loss")) +
       labs(title = "Training vs. Validation Loss", x = "Epoch", y = "Loss")
     ```

---

#### **Slide 17: Exercise 2 – Confusion Matrix Heatmap**

   - **Scenario**: After training a classification model, visualize the confusion matrix to evaluate how well the model performed on each class.
   - **Task**: Create a heatmap to represent the confusion matrix visually.
   - **Solution Code**:
     ```r
     library(ggplot2)
     confusion_data <- data.frame(
       Actual = rep(c("Class 1", "Class 2"), each = 2),
       Predicted = rep(c("Class 1", "Class 2"), 2),
       Frequency = c(50, 10, 5, 35)
     )
     ggplot(confusion_data, aes(x = Actual, y = Predicted, fill = Frequency)) +
       geom_tile() +
       geom_text(aes(label = Frequency)) +
       labs(title = "Confusion Matrix Heatmap")
     ```

---

#### **Slide 18: Exercise 3 – ROC Curve with ggplot2**

   - **Scenario**: Use `ggplot2` to create a custom ROC curve for a binary classification model.
   - **Task**: Plot an ROC curve to evaluate the model’s performance.
   - **Solution Code**:
     ```r
     library(pROC)
     actual <- factor(c(1, 0, 1, 1, 0, 1, 0, 0, 1, 0))
     predicted_probs <- c(0.9, 0.4, 0.8, 0.7, 0.3, 0.9, 0.1, 0.4, 0.8, 0.5)
     roc_curve <- roc(actual, predicted_probs)
     ggroc(roc_curve) + ggtitle("ROC Curve")
     ```

---

#### **Slide 19: Exercise 4 – Precision-Recall Curve for Imbalanced Data**

   - **Scenario**: For a highly imbalanced dataset, plot a precision-recall curve to evaluate the model.
   - **Task**: Use `precrec` package to create a precision-recall curve.
   - **Solution Code**:
     ```r
     library(precrec)
     actual <- factor(c(1, 1, 0, 0, 1, 0, 1, 0, 0, 1))
     predicted_probs <- c(0.9, 0.85, 0.1, 0.4, 0.7, 0.2, 0.6, 0.1, 0.3, 0.9)
     pr <- evalmod(scores = predicted_probs, labels = actual, mode = "PRC")
     autoplot(pr) + ggtitle("Precision-Recall Curve")
     ```

---

#### **Slide 20: Exercise 5 – Feature Importance Plot for Random Forest**

   - **Scenario**: After training a random forest model, create a plot to visualize feature importance.
   - **Task**: Plot feature importance scores for each feature.
   - **Solution Code**:
     ```r
     library(randomForest)
     rf_model <- randomForest(Species ~ ., data = iris)
     varImpPlot(rf_model, main = "Feature Importance in Random Forest")
     ```

---

#### **Slide 21: Exercise 6 – Bar Plot of Model Accuracy Across Folds**

   - **Scenario**: After using cross-validation, visualize accuracy across each fold.
   - **Task**: Create a bar plot to show accuracy for each fold.
   - **Solution Code**:
     ```r
     library(caret)
     control <- trainControl(method = "cv", number = 5, savePredictions = TRUE)
     model <- train(Species ~ ., data = iris, method = "rf", trControl = control)
     res <- model$resample
     ggplot(res, aes(x = Resample, y = Accuracy)) + 
       geom_bar(stat = "identity") +
       labs(title = "Accuracy Across Cross-Validation Folds")
     ```

---

#### **Slide 22: Exercise 7 – Box Plot of Cross-Validation Results**

   - **Scenario**: Compare the performance variability of multiple models across cross-validation folds.
   - **Task**: Use a box plot to show the distribution of accuracy for each model.
   - **Solution Code**:
     ```r
     model1 <- train(Species ~ ., data = iris, method = "rf", trControl = control)
     model2 <- train(Species ~ ., data = iris, method = "svmRadial", trControl = control)
     res <- resamples(list(RF = model1, SVM = model2))
     bwplot(res, metric = "Accuracy", main = "Model Accuracy Comparison Across Folds")
     ```

---

#### **Slide 23: Exercise 8 – Line Plot of Loss Over Time**

   - **Scenario**: Track model training loss over time to identify trends.
   - **Task**: Plot a line graph showing training and validation loss over epochs.
   - **Solution Code**:
     ```r
     epochs <- 1:50
     train_loss <- runif(50, 0.1, 0.5)
     val_loss <- runif(50, 0.15, 0.55)
     data <- data.frame(epochs, train_loss, val_loss)
     ggplot(data) +
       geom_line(aes(x = epochs, y = train_loss, color = "Train Loss")) +
       geom_line(aes(x = epochs, y = val_loss, color = "Validation Loss")) +
       labs(title = "Training vs Validation Loss")
     ```

---

#### **Slide 24: Exercise 9 – Interactive Plot of Prediction Probabilities**

   - **Scenario**: Visualize prediction probabilities interactively using `plotly`.
   - **Task**: Create an interactive scatter plot of prediction probabilities.
   - **Solution Code**:
     ```r
     library(plotly)
     data <- data.frame(Actual = actual, Predicted_Prob = predicted_probs)
     ggplot_obj <- ggplot(data, aes(x = Actual, y = Predicted_Prob)) + geom_point()
     ggplotly(ggplot_obj)
     ```

---

#### **Slide 25: Exercise 10 – Model Comparison Plot for Precision and Recall**

   - **Scenario**: Compare precision and recall for multiple models in a single plot.
   - **Task**: Create a grouped bar plot for precision and recall.
   - **Solution Code**:
     ```r
     model_performance <- data.frame(
       Model = rep(c("Logistic", "Random Forest", "SVM"), each = 2),
       Metric = rep(c("Precision", "Recall"), times = 3),
       Value = c(0.8, 0.75, 0.85, 0.8, 0.83, 0.78)
     )
     ggplot(model_performance, aes(x = Model, y = Value, fill = Metric)) +
       geom_bar(stat = "identity", position = "dodge") +
       labs(title = "Model Comparison for Precision and Recall")
     ```


---

#### **Slide 26: Exercise 11 – Overlaying ROC Curves from Multiple Models**

   - **Scenario**: Compare the ROC curves of three different models on the same dataset to determine which has better performance.
   - **Task**: Overlay the ROC curves of each model on a single plot.
   - **Solution Code**:
     ```r
     library(pROC)
     actual <- factor(c(1, 0, 1, 1, 0, 1, 0, 0, 1, 0))
     model1_probs <- c(0.9, 0.3, 0.8, 0.7, 0.4, 0.8, 0.2, 0.5, 0.7, 0.3)
     model2_probs <- c(0.85, 0.35, 0.78, 0.65, 0.45, 0.83, 0.25, 0.48, 0.72, 0.37)
     model3_probs <- c(0.8, 0.4, 0.75, 0.6, 0.5, 0.9, 0.3, 0.6, 0.75, 0.4)

     roc1 <- roc(actual, model1_probs)
     roc2 <- roc(actual, model2_probs)
     roc3 <- roc(actual, model3_probs)

     plot(roc1, col = "blue", main = "ROC Curves Comparison")
     lines(roc2, col = "red")
     lines(roc3, col = "green")
     legend("bottomright", legend = c("Model 1", "Model 2", "Model 3"),
            col = c("blue", "red", "green"), lty = 1)
     ```

   - **Discussion**: Interpret the curves, noting which model has the largest AUC, and discuss how this indicates better performance in distinguishing between classes.

---

#### **Slide 27: Exercise 12 – Customizing Feature Importance Plot with ggplot2**

   - **Scenario**: After training a model, visualize feature importance using a custom color scheme and ordering features by importance.
   - **Task**: Create a bar plot for feature importance with custom colors.
   - **Solution Code**:
     ```r
     library(randomForest)
     rf_model <- randomForest(Species ~ ., data = iris)
     importance_data <- as.data.frame(importance(rf_model))
     importance_data$Feature <- rownames(importance_data)

     ggplot(importance_data, aes(x = reorder(Feature, MeanDecreaseGini), y = MeanDecreaseGini)) +
       geom_bar(stat = "identity", fill = "steelblue") +
       coord_flip() +
       labs(title = "Feature Importance", x = "Feature", y = "Importance") +
       theme_minimal()
     ```

   - **Discussion**: Emphasize the significance of ranking features by importance and explain how this can help in feature selection.

---

#### **Slide 28: Exercise 13 – Visualizing k-Means Clustering Results**

   - **Scenario**: Apply k-means clustering on the `iris` dataset and visualize the clusters.
   - **Task**: Create a scatter plot showing clusters with color-coded data points for each cluster.
   - **Solution Code**:
     ```r
     set.seed(123)
     kmeans_model <- kmeans(iris[, -5], centers = 3)
     iris$Cluster <- as.factor(kmeans_model$cluster)

     ggplot(iris, aes(x = Petal.Length, y = Petal.Width, color = Cluster)) +
       geom_point(size = 3) +
       labs(title = "k-Means Clustering on Iris Data") +
       theme_minimal()
     ```

   - **Explanation**: Interpret the cluster plot, explaining how k-means grouping works and what the centroids represent in clustering.

---

#### **Slide 29: Exercise 14 – Facet Grid of Model Performance Metrics Across Datasets**

   - **Scenario**: Evaluate a model’s performance on different subsets of data, such as subsets based on demographic groups or regions, and compare performance metrics using a facet grid.
   - **Task**: Use facets to visualize accuracy, precision, and recall across each subset.
   - **Solution Code**:
     ```r
     performance_data <- data.frame(
       Group = rep(c("Group 1", "Group 2", "Group 3"), each = 3),
       Metric = rep(c("Accuracy", "Precision", "Recall"), times = 3),
       Value = c(0.85, 0.82, 0.88, 0.9, 0.85, 0.87, 0.88, 0.83, 0.9)
     )

     ggplot(performance_data, aes(x = Metric, y = Value, fill = Group)) +
       geom_bar(stat = "identity", position = "dodge") +
       facet_wrap(~ Group) +
       labs(title = "Model Performance by Group") +
       theme_minimal()
     ```

   - **Explanation**: Use facets to interpret differences in model performance across data subsets, illustrating the importance of group-based evaluation.

---

#### **Slide 30: Exercise 15 – Interactive Visualization of Decision Boundaries Using plotly**

   - **Scenario**: Visualize the decision boundaries of a classifier on two features, making the plot interactive to explore different regions in the feature space.
   - **Task**: Use `plotly` to create an interactive plot showing decision boundaries with data points.
   - **Solution Code**:
     ```r
     library(plotly)
     library(caret)

     # Train a simple classifier
     model <- train(Species ~ Petal.Length + Petal.Width, data = iris, method = "rpart")

     # Create a grid for plotting decision boundaries
     grid <- expand.grid(
       Petal.Length = seq(min(iris$Petal.Length), max(iris$Petal.Length), length.out = 100),
       Petal.Width = seq(min(iris$Petal.Width), max(iris$Petal.Width), length.out = 100)
     )
     grid$Predicted <- predict(model, newdata = grid)

     # Plotly plot with decision boundary
     plot <- ggplot(grid, aes(x = Petal.Length, y = Petal.Width, color = Predicted)) +
       geom_point(data = iris, aes(x = Petal.Length, y = Petal.Width, color = Species), alpha = 0.5) +
       geom_tile(aes(fill = Predicted), alpha = 0.3) +
       labs(title = "Interactive Decision Boundaries")

     ggplotly(plot)
     ```

   - **Explanation**: Discuss how decision boundary visualizations help in understanding the model’s decision logic and the role of interactive plots for more dynamic data exploration.

---

### **Wrap-Up and Summary Slides**

---

#### **Slide 31: Key Takeaways from Visualization Exercises**

   - **Summary of Visualization Types**:
     - **Training and Validation Curves**: Track loss and accuracy over time, diagnosing overfitting and underfitting.
     - **Confusion Matrix Heatmap**: Visualize classification performance, showing true positives, false positives, etc.
     - **ROC and Precision-Recall Curves**: Evaluate model discrimination power, especially in binary and imbalanced datasets.
   - **Purpose**: Each type provides unique insights into model strengths and weaknesses.

---

#### **Slide 32: Importance of Feature Importance Plots**

   - **Review**: Feature importance plots allow us to identify which variables contribute most to model predictions.
   - **Insights**:
     - Helps in **feature selection** by identifying top predictors.
     - Simplifies models by focusing on essential features.
   - **Example**: In decision trees or random forests, removing low-importance features can speed up computation without sacrificing accuracy.

---

#### **Slide 33: Comparison Plots for Model Selection**

   - **Review**: Visualization of performance metrics across models enables comparison of different algorithms.
   - **Benefits**:
     - Identify the model that best balances metrics like accuracy, F1, or AUC.
     - Visual comparisons (e.g., bar plots, ROC curves) clarify model trade-offs.
   - **Key Takeaway**: Visualization provides a straightforward way to determine the best model for specific tasks or data conditions.

---

#### **Slide 34: Advantages of Interactive Visualizations with `plotly`**

   - **Concept**: Interactive plots offer a more dynamic way to explore data and model behavior.
   - **Use Cases**:
     - **Exploring decision boundaries**: Interactive decision boundary plots help examine how models make predictions in different feature spaces.
     - **Improving understanding**: Enables students and practitioners to investigate how slight changes in input features might impact predictions.
   - **Example**: Interactive ROC or Precision-Recall curves for investigating threshold changes.

---

#### **Slide 35: Final Thoughts and Next Steps**

   - **Final Thoughts**:
     - Visualization is not just a supplementary tool—it’s integral to machine learning interpretation and model tuning.
     - The choice of visualization should align with the model type, data distribution, and business goals.
   - **Next Steps**:
     - Apply these visualization techniques to your own machine learning projects.
     - Experiment with different R packages (`ggplot2`, `plotly`, `pROC`, etc.) for more advanced and customized visualizations.
   - **Concluding Note**: Effective visualizations can drive better decision-making and model improvements, turning complex metrics into actionable insights.

---


