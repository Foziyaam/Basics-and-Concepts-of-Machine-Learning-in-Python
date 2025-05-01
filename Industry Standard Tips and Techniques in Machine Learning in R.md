---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---

### **Lecture Slides: Industry Standard Tips and Techniques in Machine Learning in R**

---

#### **Slide 1: Title Slide**

- **Title**: Industry Standard Tips and Techniques in Machine Learning in R
- **Subtitle**: Practical Advice for Real-World Machine Learning Projects
- **Course**: Machine Learning in R
- **Instructor**: [Your Name]

---

### **Part 1: Best Practices in Data Preparation and Preprocessing**

---

#### **Slide 2: Introduction to Industry Standard Best Practices**

- **Overview**:
  - Why best practices are essential in machine learning.
  - Practical, real-world challenges: data quality, overfitting, model maintenance.
  - Objective: Equip students with actionable techniques to enhance model robustness and efficiency.

---

#### **Slide 3: Data Cleaning – Handling Missing Values**

- **Techniques**:
  - **Remove Missing Data**: Useful for large datasets with few missing values.
  - **Impute Missing Values**: Use `na.omit()` or `impute()` functions in R.
- **Example**:
  ```r
  df <- data.frame(A = c(1, 2, NA, 4), B = c(NA, 2, 3, 4))
  df_clean <- na.omit(df)
  ```

---

#### **Slide 4: Advanced Imputation Techniques**

- **Techniques**:
  - **Mean/Median Imputation**: For numerical data.
  - **K-Nearest Neighbors (KNN)**: More advanced, data-driven approach.
- **Example**:
  ```r
  library(DMwR)
  df_imputed <- knnImputation(df)
  ```

---

#### **Slide 5: Scaling and Normalization**

- **Concepts**:
  - **Scaling**: Rescale data to a range, typically 0-1.
  - **Normalization**: Transform data to a standard normal distribution.
- **R Code**:
  ```r
  df_scaled <- scale(df)
  ```

---

#### **Slide 6: Feature Engineering**

- **Definition**: Transform raw data into meaningful features to improve model performance.
- **Techniques**:
  - **Binning**: Group continuous variables into discrete bins.
  - **Interaction Terms**: Combine features to capture relationships.
- **Example**:
  ```r
  df$interaction <- df$feature1 * df$feature2
  ```

---

#### **Slide 7: Handling Categorical Data**

- **Techniques**:
  - **One-Hot Encoding**: Convert categories to binary indicators.
  - **Label Encoding**: Convert categories to numeric values.
- **R Code**:
  ```r
  df$category <- as.factor(df$category)
  ```

---

#### **Slide 8: Exercise 1 – Data Preprocessing on Customer Dataset**

- **Task**:
  - Load a sample customer dataset.
  - Impute missing values, scale features, and encode categorical variables.
- **Solution**:
  ```r
  customer_data <- read.csv("customer.csv")
  customer_data <- na.omit(customer_data)
  customer_data$category <- as.factor(customer_data$category)
  ```

---

### **Part 2: Model Selection and Training Tips**

---

#### **Slide 9: Model Selection Basics**

- **Criteria**:
  - **Data Type**: Structured (tabular) or unstructured (images/text).
  - **Model Complexity**: Balance complexity and interpretability.
  - **Project Requirements**: Choose based on accuracy, speed, interpretability.
- **Examples**:
  - Random Forest for tabular data.
  - Neural Networks for image data.

---

#### **Slide 10: Cross-Validation Techniques**

- **Overview**:
  - **K-Fold Cross-Validation**: Standard for model validation.
  - **Stratified K-Fold**: Ensures balanced class distribution in folds.
- **Example Code**:
  ```r
  library(caret)
  control <- trainControl(method = "cv", number = 5)
  ```

---

#### **Slide 11: Tips for Model Tuning and Hyperparameter Optimization**

- **Grid Search**:
  - Use `caret` for tuning hyperparameters.
- **Example Code**:
  ```r
  tune_grid <- expand.grid(mtry = c(2, 3, 4))
  model <- train(Species ~ ., data = iris, method = "rf", tuneGrid = tune_grid, trControl = control)
  ```
- **Discussion**: Discuss impact of each parameter on model performance.

---

#### **Slide 12: Early Stopping in Neural Networks**

- **Concept**: Stop training when performance stops improving.
- **Implementation in Keras**:
  ```r
  model %>% fit(..., callbacks = list(callback_early_stopping(patience = 5)))
  ```

---

#### **Slide 13: Exercise 2 – Hyperparameter Tuning on Iris Dataset**

- **Task**:
  - Use grid search to optimize `mtry` in a Random Forest model on `iris`.
- **Solution**:
  ```r
  tune_grid <- expand.grid(mtry = c(2, 3, 4))
  model <- train(Species ~ ., data = iris, method = "rf", tuneGrid = tune_grid)
  ```

---

### **Part 3: Improving Model Robustness and Generalization**

---

#### **Slide 14: Avoiding Overfitting – Regularization**

- **Concept**: Regularization adds a penalty to prevent overfitting.
- **Types**:
  - **L2 (Ridge)**: Penalizes large coefficients.
  - **L1 (Lasso)**: Can zero out irrelevant features.
- **Example**:
  ```r
  glmnet_model <- glmnet(x, y, alpha = 1)
  ```

---

#### **Slide 15: Using Dropout in Neural Networks**

- **Concept**: Dropout randomly ignores neurons during training to reduce overfitting.
- **Example Code in Keras**:
  ```r
  model <- keras_model_sequential() %>%
    layer_dense(units = 128, activation = 'relu') %>%
    layer_dropout(rate = 0.5)
  ```

---

#### **Slide 16: Data Augmentation for Image Data**

- **Concept**: Generate new images by modifying existing ones.
- **Examples**: Rotate, crop, adjust brightness.
- **R Code**:
  ```r
  datagen <- image_data_generator(rotation_range = 40, width_shift_range = 0.2)
  ```

---

#### **Slide 17: Exercise 3 – Implementing Regularization on Boston Housing Data**

- **Task**:
  - Use Ridge and Lasso regularization to fit a model on the `BostonHousing` dataset.
- **Solution**:
  ```r
  library(glmnet)
  ridge_model <- glmnet(x, y, alpha = 0)
  lasso_model <- glmnet(x, y, alpha = 1)
  ```

---

### **Part 4: Best Practices in Model Evaluation**

---

#### **Slide 18: Evaluation Metrics for Classification Models**

- **Metrics**:
  - **Accuracy**: Percentage of correct predictions.
  - **Precision/Recall**: Important for imbalanced datasets.
  - **F1 Score**: Harmonic mean of precision and recall.
- **Example Code**:
  ```r
  confusionMatrix(predictions, actual)
  ```

---

#### **Slide 19: Evaluation Metrics for Regression Models**

- **Metrics**:
  - **Mean Absolute Error (MAE)**: Average absolute error.
  - **Mean Squared Error (MSE)**: Average squared error.
  - **R-squared**: Proportion of variance explained.
- **Example Code**:
  ```r
  rmse <- sqrt(mean((predictions - actual)^2))
  ```

---

#### **Slide 20: Confusion Matrix Analysis**

- **Function**: `confusionMatrix()` in `caret`
- **Interpretation**: Analyze false positives/negatives to improve model.
- **Example Code**:
  ```r
  confusionMatrix(data = predictions, reference = actual)
  ```

---

#### **Slide 21: ROC and AUC for Binary Classification**

- **Function**: `roc()` in `pROC`
- **R Code**:
  ```r
  library(pROC)
  roc_curve <- roc(actual, predictions)
  plot(roc_curve)
  ```

---

#### **Slide 22: Exercise 4 – Evaluating Model on Loan Default Data**

- **Task**:
  - Train a model on loan data.
  - Use confusion matrix, ROC, and AUC to evaluate.
- **Solution**:
  ```r
  model <- randomForest(default ~ ., data = loan_data)
  predictions <- predict(model, loan_data)
  confusionMatrix(predictions, loan_data$default)
  ```

---

### **Part 5: Model Deployment and Maintenance**

---

#### **Slide 23: Introduction to Model Deployment**

- **Concept**: Model deployment is the process of making a model available for real-time predictions.
- **Methods**:
  - **Batch Predictions**: Run predictions on large datasets periodically.
  - **Real-Time Predictions**: Make predictions as new data arrives.

---

#### **Slide 24: Model Deployment Options in R**

- **Deployment Options**:
  - **Plumber API**: Create REST APIs in R for model predictions.
  - **Shiny Applications**: Interactive web applications to showcase models.
  - **Batch Processing**: Use R scripts scheduled via cron jobs or services like RStudio Connect.
- **Example: Deploying with Plumber**:
  ```r
  library(plumber)
  
  # Define endpoint
  #* @post /predict
  function(feature1, feature2) {
    predict(model, newdata = data.frame(feature1 = as.numeric(feature1), feature2 = as.numeric(feature2)))
  }
  
  # Start API
  r <- plumb("api.R")
  r$run(port = 8000)
  ```

---

#### **Slide 25: Model Deployment with Shiny**

- **Overview**: Use Shiny for deploying interactive models with visualizations.
- **Example Code**:
  ```r
  library(shiny)
  
  ui <- fluidPage(
    titlePanel("Model Prediction"),
    sidebarLayout(
      sidebarPanel(
        numericInput("feature1", "Feature 1", value = 0),
        numericInput("feature2", "Feature 2", value = 0)
      ),
      mainPanel(
        textOutput("prediction")
      )
    )
  )
  
  server <- function(input, output) {
    output$prediction <- renderText({
      prediction <- predict(model, newdata = data.frame(feature1 = input$feature1, feature2 = input$feature2))
      paste("Prediction:", prediction)
    })
  }
  
  shinyApp(ui = ui, server = server)
  ```

---

#### **Slide 26: Tips for Model Maintenance**

- **Challenges**:
  - Data drift (when new data changes over time).
  - Model degradation as data changes.
- **Solutions**:
  - **Retrain regularly**: Update the model with new data.
  - **Monitor Metrics**: Track accuracy, recall, and other metrics over time.
- **Example**:
  - Set up scheduled retraining and validation to catch model degradation early.

---

#### **Slide 27: Version Control for Models**

- **Concept**: Track model changes using version control (e.g., Git).
- **Benefits**:
  - Improves collaboration.
  - Keeps records of model updates and improvements.
- **Practice**:
  - Version code, data processing scripts, and trained model objects.

---

#### **Slide 28: Monitoring Model Performance After Deployment**

- **Monitoring Metrics**:
  - Track metrics like precision, recall, and accuracy on live data.
  - Set up alerts for significant changes in performance.
- **Example**:
  - Use dashboards (e.g., Shiny or R Markdown) to visualize model performance trends.

---

#### **Slide 29: Automating Model Retraining**

- **Overview**: Automate retraining when performance drops below a threshold.
- **Example Workflow**:
  - Monitor performance, retrain when necessary, validate, then redeploy.
- **Example Code for Batch Retraining**:
  ```r
  if (model_accuracy < threshold) {
    model <- retrain_model(data)
    saveRDS(model, "model.rds")
  }
  ```

---

#### **Slide 30: Security Considerations in Model Deployment**

- **Key Areas**:
  - **Data Privacy**: Ensure that sensitive data is handled securely.
  - **API Security**: Use authentication for APIs to protect against unauthorized access.
- **Example**:
  - Secure Plumber API with basic authentication.
  ```r
  #* @filter auth
  function(req, res) {
    auth_header <- req$HTTP_AUTHORIZATION
    if (is.null(auth_header) || auth_header != "Basic [encoded_credentials]") {
      res$status <- 401
      return(list(error = "Unauthorized"))
    }
    forward()
  }
  ```

---

### **Part 6: Practical Tips for Real-World Machine Learning Projects**

---

#### **Slide 31: Understanding the Business Problem**

- **Importance**:
  - Understand the business goals and data requirements.
- **Example**:
  - Align model objectives with key performance indicators (KPIs) in business.
  - Clearly define the success metrics for the model.

---

#### **Slide 32: Balancing Model Accuracy and Interpretability**

- **Trade-Off**:
  - Complex models (e.g., deep learning) offer high accuracy but are less interpretable.
  - Simpler models (e.g., linear regression) may provide business insights.
- **Tip**: Choose a model that balances accuracy and interpretability based on business needs.

---

#### **Slide 33: Ensuring Model Explainability**

- **Tools for Explainability**:
  - **LIME (Local Interpretable Model-agnostic Explanations)**: Explains individual predictions.
  - **SHAP (SHapley Additive exPlanations)**: Global feature importance.
- **Example**:
  ```r
  library(iml)
  explainer <- lime::lime(training_data, model)
  explanation <- lime::explain(new_data, explainer)
  ```

---

#### **Slide 34: Handling Imbalanced Data**

- **Techniques**:
  - **Resampling**: Oversample minority class or undersample majority class.
  - **Synthetic Data Generation**: Use SMOTE (Synthetic Minority Over-sampling Technique).
- **R Code**:
  ```r
  library(DMwR)
  balanced_data <- SMOTE(target ~ ., data = imbalanced_data)
  ```

---

#### **Slide 35: Exercise 5 – Applying SMOTE on Imbalanced Dataset**

- **Task**:
  - Use SMOTE to balance classes in a loan default dataset.
- **Solution**:
  ```r
  loan_data <- read.csv("loan_data.csv")
  balanced_data <- SMOTE(default ~ ., data = loan_data)
  ```

---

#### **Slide 36: Importance of Feature Selection**

- **Benefits**:
  - Reduces overfitting.
  - Increases interpretability and reduces computational cost.
- **Methods**:
  - **Filter Methods**: Correlation, Chi-square.
  - **Wrapper Methods**: Recursive Feature Elimination.
- **Example**:
  ```r
  library(caret)
  control <- rfeControl(functions = rfFuncs, method = "cv", number = 10)
  results <- rfe(x, y, sizes = c(1:5), rfeControl = control)
  ```

---

#### **Slide 37: Logging and Documentation**

- **Best Practice**:
  - Document data sources, model versions, parameter settings.
  - Log model performance metrics and changes.
- **Tools**:
  - Use logging packages (e.g., `logger` in R) to capture model actions and results.

---

#### **Slide 38: Managing Data Quality**

- **Data Quality Checks**:
  - Check for missing values, outliers, duplicates.
- **Tip**: Run data validation scripts periodically on new data.
- **Example Code**:
  ```r
  sum(is.na(data))  # Check for missing values
  ```

---

#### **Slide 39: Communicating Results with Stakeholders**

- **Tips**:
  - Present results in a non-technical manner.
  - Use visualizations to show model performance and predictions.
- **Example**:
  - Create a Shiny dashboard to visualize model performance metrics.

---

### **Wrap-Up and Summary**

---

#### **Slide 40: Key Takeaways from Industry Standards**

- **Summary of Key Points**:
  - Data quality and preprocessing are crucial for robust models.
  - Regular model retraining and monitoring are necessary to maintain performance.
  - Security, version control, and documentation are essential for deployed models.

---

#### **Slide 41: Best Practices Checklist**

- **Checklist**:
  - Data preprocessing and cleaning.
  - Cross-validation and hyperparameter tuning.
  - Model deployment, monitoring, and retraining.
  - Documentation and version control.

---

#### **Slide 42: Exercise 6 – Real-World ML Project Setup**

- **Task**:
  - Design a small end-to-end ML project for a hypothetical business problem.
  - Include preprocessing, model selection, training, deployment, and monitoring steps.
- **Solution Guide**:
  - Choose a simple dataset (e.g., customer churn).
  - Document each step, from feature engineering to deployment options.

---

#### **Slide 43: Resources for Further Learning**

- **Books**:
  - "Machine Learning with R" by Brett Lantz.
  - "R for Data Science" by Hadley Wickham.
- **Online Resources**:
  - RStudio resources, machine learning blogs, and forums.

---

#### **Slide 44: Questions and Discussion**

- **Q&A**:
  - Open the floor for questions and real-world scenarios.
- **Discussion Points**:
  - Challenges faced by students in their own ML projects.
  - Discussion on handling specific industry use cases.

---


