---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---
### **Lecture Slides: Advanced Techniques – Ensemble Models and Neural Networks in R**

---

#### **Slide 1: Title Slide**

- **Title**: Advanced Techniques in Machine Learning with R
- **Subtitle**: Ensemble Models and Neural Networks
- **Course**: Machine Learning in R
- **Instructor**: [Your Name]

---

### **Part 1: Ensemble Models**

---

#### **Slide 2: Introduction to Ensemble Models**

- **Concept**: Ensemble learning combines multiple individual models to produce a stronger model.
- **Types of Ensembles**:
  - **Bagging**: Reduces variance by training on random subsets.
  - **Boosting**: Reduces bias by sequentially improving model errors.
  - **Stacking**: Combines diverse models for robust predictions.
- **Applications**: Used widely in competitions and real-world applications like spam detection, fraud detection, and medical diagnosis.

---

#### **Slide 3: Advantages of Ensemble Models**

- **Benefits**:
  - Higher accuracy than individual models.
  - Reduced risk of overfitting by combining predictions.
  - Improved robustness and generalization.
- **Limitations**:
  - Can be computationally expensive.
  - Increased model complexity and interpretability challenges.

---

#### **Slide 4: Overview of Bagging**

- **Concept**: Bagging (Bootstrap Aggregating) trains multiple instances of the same algorithm on different random subsets of the data.
- **Key Features**:
  - Each model is trained independently.
  - Final prediction is typically an average (for regression) or majority vote (for classification).
- **Popular Algorithm**: Random Forest.

---

#### **Slide 5: Bagging in R with Random Forest**

- **Function**: `randomForest()`
- **Example**: Train a random forest model on the `iris` dataset.
- **R Code**:
  ```r
  library(randomForest)
  set.seed(123)
  rf_model <- randomForest(Species ~ ., data = iris, ntree = 100)
  print(rf_model)
  ```
- **Output**: Displays model accuracy and feature importance.

---

#### **Slide 6: Visualizing Feature Importance in Random Forest**

- **Concept**: Feature importance ranks variables based on their influence on predictions.
- **R Code**:
  ```r
  varImpPlot(rf_model, main = "Feature Importance in Random Forest")
  ```
- **Interpretation**: Helps identify which features are most relevant for prediction.

---

#### **Slide 7: Random Forest Hyperparameters**

- **Common Hyperparameters**:
  - `ntree`: Number of trees in the forest.
  - `mtry`: Number of features considered at each split.
- **Tuning in R**:
  - **`caret` package** with `trainControl()` for hyperparameter tuning.
- **Example Code**:
  ```r
  library(caret)
  control <- trainControl(method = "cv", number = 5)
  tune_grid <- expand.grid(mtry = c(1, 2, 3))
  model <- train(Species ~ ., data = iris, method = "rf", tuneGrid = tune_grid, trControl = control)
  model
  ```

---

#### **Slide 8: Exercise 1 – Random Forest on Titanic Dataset**

- **Task**:
  - Load and clean the Titanic dataset.
  - Train a random forest model to predict survival.
  - Tune `ntree` and `mtry` parameters.
- **Solution**:
  ```r
  titanic <- read.csv("titanic.csv")
  # Data cleaning and preprocessing steps here
  model <- randomForest(Survived ~ ., data = titanic, ntree = 100, mtry = 3)
  ```

---

#### **Slide 9: Introduction to Boosting**

- **Concept**: Boosting sequentially trains models, giving more weight to observations with high errors in previous models.
- **Common Algorithms**:
  - **AdaBoost**: Focuses on misclassified samples, adjusting their weights.
  - **Gradient Boosting**: Optimizes a loss function iteratively by adding models that correct previous errors.

---

#### **Slide 10: Boosting in R with `xgboost`**

- **Function**: `xgboost()`
- **R Code Example**:
  ```r
  library(xgboost)
  data_matrix <- as.matrix(iris[, -5])
  label <- as.numeric(iris$Species) - 1
  dtrain <- xgb.DMatrix(data = data_matrix, label = label)
  params <- list(objective = "multi:softmax", num_class = 3)
  model <- xgboost(params = params, data = dtrain, nrounds = 100)
  ```

---

#### **Slide 11: Key Parameters in XGBoost**

- **Important Parameters**:
  - `nrounds`: Number of boosting rounds.
  - `eta`: Learning rate.
  - `max_depth`: Depth of each tree.
- **Hyperparameter Tuning**: Use `caret` for cross-validation and tuning in R.

---

#### **Slide 12: Visualizing XGBoost Decision Trees**

- **Concept**: Visualizing individual trees in XGBoost to understand decision-making.
- **Example Code**:
  ```r
  xgb.plot.tree(model = model, trees = 1)
  ```
- **Interpretation**: View tree splits and decision nodes to understand how each tree contributes to predictions.

---

#### **Slide 13: Exercise 2 – Tuning XGBoost on Wine Dataset**

- **Task**:
  - Load the `wine` dataset and preprocess it.
  - Train an XGBoost model.
  - Tune `eta` and `max_depth`.
- **Solution**:
  ```r
  wine <- read.csv("wine.csv")
  wine_matrix <- as.matrix(wine[, -1])
  label <- as.numeric(wine$Type) - 1
  dtrain <- xgb.DMatrix(data = wine_matrix, label = label)
  params <- list(objective = "multi:softmax", num_class = 3, eta = 0.1, max_depth = 4)
  model <- xgboost(params = params, data = dtrain, nrounds = 150)
  ```

---

#### **Slide 14: Stacking Overview**

- **Concept**: Combines predictions from different model types to improve accuracy.
- **Approach**:
  - Train base models independently.
  - Use their predictions as inputs to a meta-model (e.g., linear regression or decision tree).
- **Application**: Works well in complex problems requiring diverse model perspectives.

---

#### **Slide 15: Implementing Stacking in R**

- **Package**: `caretEnsemble`
- **Example Code**:
  ```r
  library(caretEnsemble)
  models <- caretList(Species ~ ., data = iris, trControl = trainControl(method = "cv"))
  ensemble <- caretEnsemble(models, metric = "Accuracy")
  ```

---

#### **Slide 16: Stacking Example on the `iris` Dataset**

- **Task**:
  - Stack models like logistic regression and decision trees on `iris`.
- **Solution Code**:
  ```r
  models <- caretList(Species ~ ., data = iris, trControl = trainControl(method = "cv"), methodList = c("glm", "rpart"))
  ensemble <- caretEnsemble(models)
  summary(ensemble)
  ```

---

#### **Slide 17: Evaluating Ensemble Models**

- **Metrics**:
  - **Accuracy**: Percentage of correct predictions.
  - **Precision/Recall**: For imbalanced datasets.
- **Visualization**:
  - Compare ensemble model performance with individual models.
- **R Code**:
  ```r
  resamples(models)
  ```

---

### **Part 2: Neural Networks**

---

#### **Slide 18: Introduction to Neural Networks**

- **Concept**: Neural networks simulate the human brain with interconnected nodes (neurons).
- **Structure**:
  - **Input Layer**: Takes in features.
  - **Hidden Layers**: Extract patterns.
  - **Output Layer**: Generates predictions.
- **Activation Functions**: Define how signals are passed through neurons (e.g., sigmoid, ReLU).

---

#### **Slide 19: Single-Layer Perceptron**

- **Concept**: Basic neural network with a single layer that maps inputs to outputs.
- **Function**: Linear model that can classify linearly separable data.
- **Limitations**: Cannot solve non-linear problems.

---

#### **Slide 20: Multilayer Perceptron (MLP)**

- **Concept**: MLP has one or more hidden layers to capture non-linear patterns.
- **Features**:
  - **Backpropagation**: Adjusts weights based on errors.
  - **Non-linear activation functions**: ReLU, sigmoid.
- **Application**: Suitable for image recognition, speech recognition, and classification tasks.

---

#### **Slide 21: Neural Network Training Process**

1. **Forward Propagation**: Calculate outputs based on current weights.
2. **Compute Loss**: Measure the error between predicted and actual values.
3. **Backpropagation**: Update weights to minimize loss.
4. **Repeat** until convergence.

---


### **Neural Networks in R**

---

#### **Slide 22: Neural Networks in R with `nnet`**

- **Package**: `nnet`
- **Function**: `nnet()` trains a single-layer neural network.
- **Example**: Train a neural network on the `iris` dataset.
- **R Code**:
  ```r
  library(nnet)
  set.seed(123)
  nn_model <- nnet(Species ~ ., data = iris, size = 5, maxit = 200)
  summary(nn_model)
  ```
- **Explanation**:
  - `size`: Number of units in the hidden layer.
  - `maxit`: Maximum number of iterations for training.

---

#### **Slide 23: Neural Network Parameters**

- **Parameters in `nnet()`**:
  - `size`: Controls the complexity of the model by defining hidden units.
  - `decay`: Regularization parameter to prevent overfitting.
  - `maxit`: Limits the number of iterations for training.
- **Hyperparameter Tuning**:
  - Tune `size` and `decay` to balance model complexity and generalization.

---

#### **Slide 24: Visualizing Neural Networks with `NeuralNetTools`**

- **Package**: `NeuralNetTools`
- **Function**: `plotnet()` to visualize network structure.
- **Example Code**:
  ```r
  library(NeuralNetTools)
  plotnet(nn_model)
  ```
- **Interpretation**: Visualize how input features connect to hidden and output layers, with weights shown for each connection.

---

#### **Slide 25: Exercise 3 – Neural Network on Wine Quality Dataset**

- **Task**:
  - Load the `wine` dataset.
  - Train a neural network to predict wine quality.
  - Tune `size` and `decay`.
- **Solution**:
  ```r
  wine <- read.csv("winequality-red.csv")
  set.seed(123)
  nn_model <- nnet(quality ~ ., data = wine, size = 5, decay = 0.1, maxit = 200)
  ```

---

#### **Slide 26: Multilayer Neural Networks with `keras`**

- **Concept**: `keras` allows for building deep neural networks with multiple hidden layers.
- **Installation**:
  ```r
  install.packages("keras")
  library(keras)
  ```
- **Advantages**: Can handle complex, high-dimensional data such as images and text.

---

#### **Slide 27: Building a Neural Network with `keras`**

- **Steps**:
  1. Define the model structure.
  2. Compile the model with a loss function and optimizer.
  3. Fit the model to data.
- **Example Code**:
  ```r
  model <- keras_model_sequential() %>%
    layer_dense(units = 64, activation = 'relu', input_shape = c(4)) %>%
    layer_dense(units = 3, activation = 'softmax')
  
  model %>% compile(
    optimizer = 'adam',
    loss = 'sparse_categorical_crossentropy',
    metrics = 'accuracy'
  )
  
  model %>% fit(as.matrix(iris[, -5]), as.numeric(iris$Species) - 1, epochs = 50, batch_size = 5)
  ```

---

#### **Slide 28: Key Parameters in `keras`**

- **Layers**:
  - **Dense Layer**: Fully connected layer.
  - **Activation Functions**: ReLU, sigmoid, softmax.
- **Compiling**:
  - **Optimizer**: Adjusts weights (e.g., `adam`, `sgd`).
  - **Loss Function**: Measures prediction error (e.g., `sparse_categorical_crossentropy`).
- **Training**:
  - **Epochs**: Number of complete passes through the data.
  - **Batch Size**: Number of samples per gradient update.

---

#### **Slide 29: Exercise 4 – Neural Network on MNIST Handwritten Digits Dataset**

- **Task**:
  - Load the MNIST dataset.
  - Build and train a neural network for digit classification.
- **Solution**:
  ```r
  mnist <- dataset_mnist()
  x_train <- mnist$train$x / 255
  y_train <- mnist$train$y
  x_test <- mnist$test$x / 255
  y_test <- mnist$test$y
  
  model <- keras_model_sequential() %>%
    layer_flatten(input_shape = c(28, 28)) %>%
    layer_dense(units = 128, activation = 'relu') %>%
    layer_dense(units = 10, activation = 'softmax')
  
  model %>% compile(
    optimizer = 'adam',
    loss = 'sparse_categorical_crossentropy',
    metrics = 'accuracy'
  )
  
  model %>% fit(x_train, y_train, epochs = 10, batch_size = 128, validation_split = 0.2)
  ```

---

#### **Slide 30: Convolutional Neural Networks (CNNs)**

- **Concept**: CNNs are designed for processing grid-like data, such as images.
- **Structure**:
  - **Convolutional Layers**: Extract features with filters.
  - **Pooling Layers**: Reduce spatial dimensions.
  - **Fully Connected Layers**: Make final predictions.
- **Application**: Image and video processing.

---

#### **Slide 31: Building a Simple CNN with `keras`**

- **Example Code**:
  ```r
  model <- keras_model_sequential() %>%
    layer_conv_2d(filters = 32, kernel_size = c(3,3), activation = 'relu', input_shape = c(28, 28, 1)) %>%
    layer_max_pooling_2d(pool_size = c(2, 2)) %>%
    layer_conv_2d(filters = 64, kernel_size = c(3,3), activation = 'relu') %>%
    layer_max_pooling_2d(pool_size = c(2, 2)) %>%
    layer_flatten() %>%
    layer_dense(units = 128, activation = 'relu') %>%
    layer_dense(units = 10, activation = 'softmax')
  
  model %>% compile(
    optimizer = 'adam',
    loss = 'sparse_categorical_crossentropy',
    metrics = 'accuracy'
  )
  ```

---

#### **Slide 32: Training the CNN on the MNIST Dataset**

- **R Code**:
  ```r
  model %>% fit(x_train, y_train, epochs = 10, batch_size = 128, validation_split = 0.2)
  ```
- **Evaluation**:
  ```r
  model %>% evaluate(x_test, y_test)
  ```

---

#### **Slide 33: Recurrent Neural Networks (RNNs)**

- **Concept**: RNNs are suitable for sequential data as they consider temporal dependencies.
- **Application**: Time series forecasting, natural language processing.
- **Layers**: Include `layer_lstm()` for long short-term memory networks.

---

#### **Slide 34: Building an RNN with `keras`**

- **Example Code**:
  ```r
  model <- keras_model_sequential() %>%
    layer_lstm(units = 50, input_shape = c(time_steps, features)) %>%
    layer_dense(units = 1)
  ```

---

#### **Slide 35: Hyperparameter Tuning for Neural Networks**

- **Key Hyperparameters**:
  - **Learning Rate**: Controls how much to adjust weights.
  - **Batch Size**: Number of samples processed before updating.
  - **Dropout**: Adds randomness to prevent overfitting.
- **Example**:
  - `model %>% layer_dropout(rate = 0.2)` reduces overfitting.

---

#### **Slide 36: Visualizing Training Progress**

- **Function**: Use `plot(history)` to see accuracy and loss.
- **R Code**:
  ```r
  history <- model %>% fit(...)
  plot(history)
  ```

---

#### **Slide 37: Evaluation Metrics for Neural Networks**

- **Metrics**:
  - **Accuracy**: Classification performance.
  - **Loss**: Measures error.
  - **Precision and Recall**: Important for imbalanced data.

---

#### **Slide 38: Regularization Techniques**

- **Methods**:
  - **Dropout**: Randomly ignores neurons during training.
  - **L2 Regularization**: Penalizes large weights.
  - **Early Stopping**: Stops training when improvement plateaus.
- **Implementation**:
  ```r
  model %>% layer_dropout(rate = 0.3)
  ```

---

#### **Slide 39: Exercise 5 – Neural Network with Dropout on CIFAR-10 Dataset**

- **Task**:
  - Build a Convolutional Neural Network (CNN) with dropout layers for image classification on the CIFAR-10 dataset.
  - Apply dropout to reduce overfitting and improve generalization.
- **Solution Code**:
  ```r
  cifar10 <- dataset_cifar10()
  x_train <- cifar10$train$x / 255
  y_train <- cifar10$train$y
  x_test <- cifar10$test$x / 255
  y_test <- cifar10$test$y

  model <- keras_model_sequential() %>%
    layer_conv_2d(filters = 32, kernel_size = c(3, 3), activation = 'relu', input_shape = c(32, 32, 3)) %>%
    layer_dropout(rate = 0.2) %>%
    layer_conv_2d(filters = 64, kernel_size = c(3, 3), activation = 'relu') %>%
    layer_max_pooling_2d(pool_size = c(2, 2)) %>%
    layer_dropout(rate = 0.3) %>%
    layer_flatten() %>%
    layer_dense(units = 128, activation = 'relu') %>%
    layer_dropout(rate = 0.4) %>%
    layer_dense(units = 10, activation = 'softmax')
  
  model %>% compile(
    optimizer = 'adam',
    loss = 'sparse_categorical_crossentropy',
    metrics = 'accuracy'
  )
  
  history <- model %>% fit(x_train, y_train, epochs = 25, batch_size = 64, validation_split = 0.2)
  ```

- **Explanation**:
  - Dropout layers are added after each major layer group to reduce overfitting.
  - `rate` specifies the proportion of nodes to drop.

---

#### **Slide 40: Evaluating Model Performance on CIFAR-10**

- **R Code**:
  ```r
  model %>% evaluate(x_test, y_test)
  ```
- **Visualize Training History**:
  - **R Code**:
    ```r
    plot(history)
    ```
  - **Interpretation**: Check if there’s a gap between training and validation accuracy, which could indicate overfitting.

---

#### **Slide 41: Transfer Learning with Pretrained Models**

- **Concept**: Transfer learning leverages pretrained models (e.g., VGG16, ResNet) for new tasks.
- **Advantages**:
  - Reduces training time.
  - Improves performance on smaller datasets.
- **Application**: Fine-tune a pretrained model on CIFAR-10.

---

#### **Slide 42: Implementing Transfer Learning with `keras`**

- **Example Code**:
  ```r
  base_model <- application_vgg16(weights = 'imagenet', include_top = FALSE, input_shape = c(32, 32, 3))
  model <- keras_model_sequential() %>%
    base_model %>%
    layer_flatten() %>%
    layer_dense(units = 256, activation = 'relu') %>%
    layer_dense(units = 10, activation = 'softmax')
  
  for (layer in base_model$layers) layer$trainable <- FALSE
  
  model %>% compile(
    optimizer = 'adam',
    loss = 'sparse_categorical_crossentropy',
    metrics = 'accuracy'
  )
  ```
- **Explanation**:
  - Freeze layers in `base_model` to retain learned features.
  - Add custom output layers for CIFAR-10 classification.

---

#### **Slide 43: Exercise 6 – Fine-Tuning with Transfer Learning**

- **Task**:
  - Unfreeze some layers of the pretrained model and retrain on CIFAR-10.
- **Solution**:
  ```r
  for (layer in base_model$layers[10:length(base_model$layers)]) layer$trainable <- TRUE
  
  history <- model %>% fit(x_train, y_train, epochs = 10, batch_size = 64, validation_split = 0.2)
  ```

---

#### **Slide 44: Hyperparameter Tuning in Neural Networks**

- **Approaches**:
  - **Manual Tuning**: Adjust parameters like learning rate, batch size, number of layers.
  - **Grid Search**: Automate tuning with specified ranges.
  - **Random Search**: Randomly sample parameter combinations.
- **Example Code for Grid Search**:
  ```r
  library(tfruns)
  flags <- flags(
    flag_numeric("learning_rate", c(0.001, 0.01, 0.1)),
    flag_integer("batch_size", c(32, 64))
  )
  ```

---

#### **Slide 45: Cross-Validation in Neural Networks**

- **Concept**: Cross-validation provides a more robust estimate of model performance.
- **Implementation**:
  - Use `k-fold` cross-validation by splitting the data and retraining on each fold.
- **Considerations**:
  - Neural networks are computationally expensive, so often limited cross-validation is performed.

---

#### **Slide 46: Comparing Ensemble Models and Neural Networks**

- **Ensemble Models**:
  - Strong in tabular, structured data.
  - Often interpretable (e.g., feature importance in Random Forest).
- **Neural Networks**:
  - Strong for unstructured data (e.g., images, text).
  - Highly flexible with deep learning architectures.
- **Choosing the Right Technique**: Depends on data type and problem complexity.

---

#### **Slide 47: Exercise 7 – Model Comparison on Structured Dataset**

- **Task**:
  - Use Random Forest and a simple neural network on the `wine` dataset.
  - Compare accuracy and F1 scores.
- **Solution Code**:
  ```r
  rf_model <- randomForest(quality ~ ., data = wine, ntree = 100)
  nn_model <- nnet(quality ~ ., data = wine, size = 5, maxit = 200)
  
  rf_pred <- predict(rf_model, wine)
  nn_pred <- predict(nn_model, wine)
  
  rf_accuracy <- mean(rf_pred == wine$quality)
  nn_accuracy <- mean(round(nn_pred) == wine$quality)
  ```

---

#### **Slide 48: Visualizing Model Performance Comparisons**

- **Visualization with ggplot2**:
  - Plot accuracy and F1 scores for each model type.
- **Example Code**:
  ```r
  library(ggplot2)
  data <- data.frame(
    Model = c("Random Forest", "Neural Network"),
    Accuracy = c(rf_accuracy, nn_accuracy)
  )
  ggplot(data, aes(x = Model, y = Accuracy)) +
    geom_bar(stat = "identity", fill = "skyblue") +
    labs(title = "Model Performance Comparison")
  ```

---

#### **Slide 49: Key Takeaways**

- **Ensemble Models**:
  - Effective on structured, tabular data.
  - Often easier to interpret.
- **Neural Networks**:
  - Powerful for high-dimensional, unstructured data.
  - Flexible architecture for complex data relationships.
- **Choosing the Right Model**:
  - Depends on data structure, computational resources, and project goals.

---

#### **Slide 50: Wrap-Up and Q&A**

- **Summary**:
  - Ensemble models like Random Forest and Boosting are powerful for structured data.
  - Neural networks, especially with deep learning architectures, excel in image and text data.
  - Advanced techniques like transfer learning and dropout enhance neural networks’ generalization capabilities.
- **Next Steps**:
  - Apply these techniques to a dataset of interest.
  - Explore hyperparameter tuning and regularization techniques for optimized performance.
- **Q&A**: Address any final questions and discuss potential projects or applications.

---



