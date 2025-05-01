---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---
### **Slide 1: Title Slide**
- **Course Title**: Machine Learning in R
- **Topic**: Data Cleaning and Preprocessing

---

### **Slide 2: Learning Objectives**
- Understand the critical role of data cleaning in machine learning.
- Explore different data normalization techniques.
- Learn how to apply batch correction to datasets.
- Implement practical examples and solve exercises with provided solutions.

---

### **Slide 3: Importance of Data Cleaning**
- **Definition**: The process of detecting and correcting errors or inconsistencies in data to enhance its quality.
- **Impact**: Clean data leads to more reliable and accurate model results.
- **Example**: Missing values, outliers, duplicate records.

---

### **Slide 4: Common Data Quality Issues**
- **Missing Data**: Can skew model accuracy.
- **Outliers**: Can heavily impact statistical metrics.
- **Noise**: Random errors or variances in data.
- **Inconsistent Formats**: Dates, numeric scales, text encoding.

---

### **Slide 5: Handling Missing Data**
- **Techniques**:
  - Remove rows with missing data.
  - Impute missing values with mean, median, or mode.
  - Use predictive modeling for complex imputations.
- **R Code Example**:
```r
# Replace NA values with column mean
data[is.na(data)] <- mean(data, na.rm = TRUE)
```

---

### **Slide 6: Exercise – Handling Missing Data**
- **Task**: Load a dataset with missing values and apply different imputation techniques.
- **Solution**:
```r
data <- read.csv('dataset.csv')
data$column[is.na(data$column)] <- median(data$column, na.rm = TRUE)
```

---

### **Slide 7: Outlier Detection and Treatment**
- **Methods**:
  - Boxplot visualization.
  - Interquartile range (IQR) method.
- **R Code Example**:
```r
outliers <- boxplot.stats(data$column)$out
boxplot(data$column, main='Outlier Detection')
```

---

### **Slide 8: Exercise – Outlier Treatment**
- **Task**: Identify and remove outliers using IQR.
- **Solution**:
```r
Q1 <- quantile(data$column, 0.25)
Q3 <- quantile(data$column, 0.75)
IQR <- Q3 - Q1
data <- data[data$column >= (Q1 - 1.5 * IQR) & data$column <= (Q3 + 1.5 * IQR), ]
```

---

### **Slide 9: Data Normalization**
- **Definition**: Scaling data to ensure features contribute equally to the model.
- **Common Techniques**:
  - **Min-Max Scaling**: Rescales features to [0, 1].
  - **Z-Score Normalization**: Centers data at zero with a standard deviation of one.
- **Mathematical Formulas**:
  - **Min-Max**: \( X' = \\frac{X - X_{min}}{X_{max} - X_{min}} \)
  - **Z-Score**: \( Z = \\frac{X - \\mu}{\\sigma} \)

---

### **Slide 10: Code Example – Min-Max Scaling**
```r
scaled_data <- (data - min(data)) / (max(data) - min(data))
```

---

### **Slide 11: Code Example – Z-Score Normalization**
```r
z_scaled_data <- scale(data)
```

---

### **Slide 12: Exercise – Apply Normalization**
- **Task**: Normalize a dataset using both min-max scaling and z-score normalization.
- **Solution**:
```r
min_max_scaled <- (data$column - min(data$column)) / (max(data$column) - min(data$column))
z_score_scaled <- scale(data$column)
```

---

### **Slide 13: Batch Effects in Data**
- **Definition**: Unwanted variations in data introduced by technical differences across batches.
- **Impact**: Reduces comparability and introduces bias.
- **Examples**: Differences in processing times, machine calibrations.

---

### **Slide 14: Visualizing Batch Effects**
- **Methods**:
  - Boxplots and density plots for visual comparison.
  - PCA plots to observe clustering by batches.
- **Code Example**:
```r
library(ggplot2)
ggplot(data, aes(x=batch, y=value)) + geom_boxplot()
```

---

### **Slide 15: Correcting Batch Effects**
- **Techniques**:
  - **ComBat Method**: Popular for large datasets with batch effects.
  - **RUV (Remove Unwanted Variation)**.
- **R Code for ComBat**:
```r
library(sva)
corrected_data <- ComBat(dat = expr_matrix, batch = batch_vector)
```

---

### **Slide 16: Exercise – Batch Correction with ComBat**
- **Task**: Apply ComBat to a dataset with batch effects and evaluate results.
- **Solution**:
```r
library(sva)
corrected_data <- ComBat(dat = expr_data, batch = batch_info)
```

---

### **Slide 17: Case Study – Data Preprocessing Pipeline**
- **Steps**:
  1. Load the dataset.
  2. Handle missing values.
  3. Detect and correct outliers.
  4. Normalize features.
  5. Correct batch effects.

---

### **Slide 18: Complete Example – Data Preprocessing in R**
```r
# Load dataset
data <- read.csv('dataset.csv')

# Impute missing values with mean
data[is.na(data)] <- mean(data, na.rm = TRUE)

# Remove outliers
Q1 <- quantile(data$column, 0.25)
Q3 <- quantile(data$column, 0.75)
IQR <- Q3 - Q1
data <- data[data$column >= (Q1 - 1.5 * IQR) & data$column <= (Q3 + 1.5 * IQR), ]

# Normalize using Z-score
data_normalized <- scale(data)

# Apply batch correction
library(sva)
batch_corrected <- ComBat(dat = data_normalized, batch = batch_vector)
```

---

### **Slide 19: Exercise 1 – Handling Missing Data**
- **Objective**: Load a dataset, identify missing data, and apply imputation.
- **Task**: Use the `airquality` dataset in R.
- **R Code**:
```r
# Load dataset
data("airquality")

# Check for missing data
summary(airquality)

# Impute missing values with column mean
airquality$Ozone[is.na(airquality$Ozone)] <- mean(airquality$Ozone, na.rm = TRUE)
airquality$Solar.R[is.na(airquality$Solar.R)] <- mean(airquality$Solar.R, na.rm = TRUE)

# Verify imputation
summary(airquality)
```

**Explanation**:
- The `summary()` function helps identify columns with missing data.
- The `mean()` function with `na.rm = TRUE` ensures missing values are excluded during mean calculation.

---

### **Slide 20: Analysis of Exercise 1**
- **Outcome**: Missing values in `Ozone` and `Solar.R` were replaced with their respective column means.
- **Discussion**: Evaluate if mean imputation was appropriate or if other imputation techniques would be better.

**Question for Students**:
- What other imputation techniques could you use for different types of data (e.g., median, KNN imputation)?

---

### **Slide 21: Exercise 2 – Detecting and Removing Outliers**
- **Objective**: Identify and remove outliers in a numeric column using the IQR method.
- **Task**: Use the `mtcars` dataset and focus on the `mpg` column.
- **R Code**:
```r
# Boxplot to visualize outliers
boxplot(mtcars$mpg, main = 'MPG Outliers Detection')

# Calculate IQR and identify outliers
Q1 <- quantile(mtcars$mpg, 0.25)
Q3 <- quantile(mtcars$mpg, 0.75)
IQR <- Q3 - Q1

# Remove outliers
mtcars_clean <- mtcars[mtcars$mpg >= (Q1 - 1.5 * IQR) & mtcars$mpg <= (Q3 + 1.5 * IQR), ]

# Plot cleaned data
boxplot(mtcars_clean$mpg, main = 'Cleaned MPG Data')
```

**Explanation**:
- The `boxplot()` function visually identifies outliers.
- The IQR method is used to filter out data points outside 1.5 times the IQR range.

---

### **Slide 22: Analysis of Exercise 2**
- **Outcome**: Outliers removed from the `mpg` column.
- **Discussion**:
  - How does removing outliers affect data distribution?
  - Should outliers always be removed or only in specific contexts?

**Answer**:
- Removing outliers can reduce skewness but might exclude valid data points in certain scenarios.

---

### **Slide 23: Exercise 3 – Normalizing Data with Min-Max Scaling**
- **Objective**: Apply Min-Max scaling to a dataset.
- **Task**: Normalize the `hp` (horsepower) column in `mtcars`.
- **R Code**:
```r
# Min-Max scaling function
min_max_scaled_hp <- (mtcars$hp - min(mtcars$hp)) / (max(mtcars$hp) - min(mtcars$hp))

# Check scaled data range
range(min_max_scaled_hp)
```

**Explanation**:
- Min-Max scaling transforms data to a [0, 1] range, ensuring comparability between features.

---

### **Slide 24: Analysis of Exercise 3**
- **Outcome**: The `hp` column is now scaled between 0 and 1.
- **Discussion**:
  - Why is Min-Max scaling important for algorithms like k-NN and SVM?
  - How would this method be affected by outliers?

---

### **Slide 25: Exercise 4 – Applying Z-Score Normalization**
- **Objective**: Normalize a numeric column using Z-score normalization.
- **Task**: Normalize the `disp` (displacement) column in `mtcars`.
- **R Code**:
```r
# Z-score normalization
z_score_scaled_disp <- scale(mtcars$disp)

# Check summary of scaled data
summary(z_score_scaled_disp)
```

**Explanation**:
- Z-score normalization centers the data at zero with a standard deviation of one.
- Useful for algorithms sensitive to feature scale, such as linear regression.

---

### **Slide 26: Analysis of Exercise 4**
- **Outcome**: The `disp` column now has a mean of 0 and a standard deviation of 1.
- **Discussion**:
  - Compare the use cases of Min-Max scaling vs. Z-score normalization.
  - When is Z-score normalization preferred over Min-Max?

---

### **Slide 27: Visualizing Data Before and After Normalization**
- **Objective**: Use plots to visualize how data changes after normalization.
- **R Code**:
```r
par(mfrow = c(1, 2))
hist(mtcars$hp, main = 'Original HP', xlab = 'HP', col = 'lightblue')
hist(min_max_scaled_hp, main = 'Min-Max Scaled HP', xlab = 'Scaled HP', col = 'lightgreen')
```

**Explanation**:
- Comparing histograms helps understand the effect of scaling on data distribution.

---

### **Slide 28: Introduction to Batch Effects**
- **Definition**: Unwanted variations introduced during different data processing batches.
- **Impact**: Can lead to biased results and poor model performance.
- **Example**: Differences in processing times or machine calibrations.

---

### **Slide 29: Visualizing Batch Effects with PCA**
- **Objective**: Use PCA plots to detect batch effects.
- **R Code**:
```r
pca_result <- prcomp(data, scale. = TRUE)
plot(pca_result$x[, 1:2], col = batch_vector, main = 'PCA Plot Colored by Batch')
```

**Explanation**:
- PCA visualization helps to identify clustering of data points by batch, indicating potential batch effects.

---

### **Slide 30: Batch Correction Techniques**
- **Methods**:
  - **ComBat Method**: Adjusts data based on batch information.
  - **RUV (Remove Unwanted Variation)**: Targets unwanted variation in data.
- **When to Apply**: In large datasets with visible batch effects impacting model outcomes.

---

### **Slide 31: Code Example – Applying ComBat for Batch Correction**
```r
library(sva)
corrected_data <- ComBat(dat = expr_matrix, batch = batch_vector)
```

**Explanation**:
- The `ComBat()` function from the `sva` package adjusts for batch effects using empirical Bayes methods.

---

### **Slide 32: Exercise – Applying Batch Correction**
- **Task**: Use the provided dataset and batch information to apply ComBat correction.
- **R Code**:
```r
library(sva)
batch_corrected_data <- ComBat(dat = your_data_matrix, batch = your_batch_vector)
```

**Solution**:
- Verify the correction by visualizing the data before and after using PCA plots.

---

### **Slide 33: Analyzing Batch-Corrected Data**
- **Visualization**:
  - Create PCA plots before and after correction.
- **Discussion**:
  - How did the batch correction change data distribution?

---

### **Slide 34: Advanced Exercise 1 – Full Data Normalization Workflow**
**Objective**: Implement a complete normalization workflow using Z-score and Min-Max scaling on a dataset and visualize the results.

**Full R Code**:
```r
# Load dataset (e.g., iris)
data <- iris[, 1:4]

# Z-score normalization
z_score_scaled <- scale(data)

# Min-Max scaling function
min_max_scaled <- as.data.frame(lapply(data, function(x) (x - min(x)) / (max(x) - min(x))))

# Visualize results
par(mfrow = c(1, 2))
hist(z_score_scaled[, 1], main = 'Z-Score Normalized - Sepal.Length', xlab = 'Z-Score', col = 'lightblue')
hist(min_max_scaled$Sepal.Length, main = 'Min-Max Scaled - Sepal.Length', xlab = 'Min-Max', col = 'lightgreen')
```

**Explanation**:
- Z-score standardizes the dataset so the mean is 0 and the standard deviation is 1.
- Min-Max scaling normalizes data between 0 and 1.
- Histograms show how each method affects the data distribution.

---

### **Slide 35: Analysis of Exercise 1**
- **Outcome**: Visualize how normalization changes the range and distribution of the `Sepal.Length` feature.
- **Discussion Points**:
  - When is it more appropriate to use Z-score normalization over Min-Max scaling?
  - How does normalization impact algorithms like k-NN and linear regression?

---

### **Slide 36: Advanced Exercise 2 – Batch Correction on Gene Expression Data**
**Objective**: Apply batch correction using ComBat on a gene expression dataset with known batch effects.

**Full R Code**:
```r
# Load necessary library
library(sva)

# Simulate gene expression data matrix (e.g., expr_data) and batch vector (e.g., batch_info)
# Apply ComBat correction
corrected_data <- ComBat(dat = expr_data, batch = batch_info)

# Visualize using PCA
pca_before <- prcomp(expr_data, scale. = TRUE)
pca_after <- prcomp(corrected_data, scale. = TRUE)

par(mfrow = c(1, 2))
plot(pca_before$x[, 1:2], col = batch_info, main = 'PCA Before Correction')
plot(pca_after$x[, 1:2], col = batch_info, main = 'PCA After Correction')
```

**Explanation**:
- ComBat uses an empirical Bayes framework for batch correction.
- PCA plots before and after correction help visualize the impact of batch correction.

---

### **Slide 37: Analysis of Exercise 2**
- **Outcome**: Notice changes in clustering by batch before and after applying ComBat.
- **Discussion Points**:
  - How did the correction affect data variability?
  - What is the significance of seeing more cohesive clusters post-correction?

---

### **Slide 38: Case Study – Complete Preprocessing Pipeline**
**Objective**: Integrate all preprocessing steps from missing data handling to normalization and batch correction in a single pipeline.

**R Code**:
```r
# Step 1: Load dataset (e.g., expression data with missing values and batch effects)
data <- read.csv('gene_expression.csv')

# Step 2: Handle missing values with median imputation
data[is.na(data)] <- apply(data, 2, function(x) median(x, na.rm = TRUE))

# Step 3: Z-score normalization
normalized_data <- scale(data)

# Step 4: Batch correction using ComBat
corrected_data <- ComBat(dat = normalized_data, batch = batch_info)
```

**Explanation**:
- This comprehensive example shows how to apply all discussed preprocessing steps in sequence.
- The code addresses common data quality issues before machine learning modeling.

---

### **Slide 39: Exercise 3 – Practice Pipeline**
**Task**: Apply the full data preprocessing pipeline to the `mtcars` dataset, including handling missing data, normalization, and outlier removal.

**Full R Code**:
```r
# Step 1: Introduce missing values artificially
mtcars$hp[sample(1:nrow(mtcars), 5)] <- NA

# Step 2: Handle missing data with mean imputation
mtcars$hp[is.na(mtcars$hp)] <- mean(mtcars$hp, na.rm = TRUE)

# Step 3: Z-score normalization
mtcars_normalized <- scale(mtcars)

# Step 4: Visualize normalized data
summary(mtcars_normalized)
```

**Solution Analysis**:
- Discuss how each step impacts data quality and model performance.
- Visualize summary statistics before and after normalization.

---

### **Slide 40: Discussion – Challenges in Data Preprocessing**
- **Key Challenges**:
  - Choosing the right normalization method.
  - Deciding whether to remove outliers or keep them.
  - Correcting batch effects without losing important biological signals.
- **Questions for Reflection**:
  - What challenges have you faced in your own data cleaning efforts?
  - How would you adapt this pipeline to different types of datasets (e.g., time series, image data)?

---

### **Slide 41: Exercise 4 – Interactive Data Cleaning**
**Objective**: Use an interactive tool or R Shiny app for data cleaning and preprocessing.
- **Task**: Create an R Shiny app that allows users to upload data, visualize missing values, and apply imputation.
- **Code Snippet**:
```r
library(shiny)

ui <- fluidPage(
  fileInput("file", "Upload CSV File"),
  tableOutput("data_table"),
  actionButton("impute", "Impute Missing Data")
)

server <- function(input, output) {
  data <- reactive({
    req(input$file)
    read.csv(input$file$datapath)
  })
  
  output$data_table <- renderTable({
    if (input$impute == 0) return(data())
    data()[is.na(data())] <- apply(data(), 2, function(x) mean(x, na.rm = TRUE))
    data()
  })
}

shinyApp(ui, server)
```

---

### **Slide 42: Exercise 5 – Batch Correction on Real Dataset**
**Task**: Use real microarray data and apply ComBat for batch correction.
- **Guidance**:
  - Load and explore a publicly available gene expression dataset.
  - Visualize using PCA and apply batch correction.
- **Code**:
```r
# Load microarray data (assume preloaded expr_data and batch_info)
library(sva)
corrected_expr_data <- ComBat(dat = expr_data, batch = batch_info)

# Visualize corrected data
plot(prcomp(corrected_expr_data)$x[, 1:2], col = batch_info, main = 'PCA After Batch Correction')
```

---

### **Slide 43: Student Challenge – Customize Your Preprocessing**
- **Task**: Customize the provided pipeline to include feature scaling, outlier detection, and data transformation.
- **Discussion Points**:
  - What new challenges did you face when customizing the pipeline?
  - How did your modifications impact the final data quality?

---

### **Slide 44: Common Student Questions and Best Practices**
**Common Questions**:
- **Q1**: How do you decide between Z-score normalization and Min-Max scaling?
  - **A1**: Z-score normalization is used when you need a dataset centered around 0 with equal variance, which is essential for algorithms sensitive to outliers. Min-Max scaling is ideal when you need to maintain the relationships in data but bring it into a [0, 1] range.
- **Q2**: When should batch correction be avoided?
  - **A2**: Avoid batch correction when batch information overlaps with biological signals or when it’s unclear if batches introduce significant variation.

**Best Practices**:
- Always visualize your data before and after preprocessing.
- Validate imputation and scaling methods by observing their impact on downstream model performance.
- Document each step of the preprocessing pipeline for reproducibility.

---

### **Slide 45: Dataset Showcases – Preprocessing Impact**
**Case Studies**:
1. **Dataset 1**: Customer demographics with missing age data.
   - **Preprocessing Step**: Imputed missing age data with median values and normalized income.
   - **Impact**: Enhanced clustering performance with clearer group separations.
2. **Dataset 2**: Clinical trial data with batch effects due to different lab testing locations.
   - **Preprocessing Step**: Applied ComBat for batch correction.
   - **Impact**: Reduced variability between batches, leading to more reliable statistical tests.

**Discussion**:
- Showcase histograms and PCA plots comparing raw vs. preprocessed data to illustrate the importance of each step.

---

### **Slide 46: Preprocessing Techniques for Multi-Modal Data**
**Challenge**: Multi-modal data (e.g., numerical and categorical data) requires special attention during preprocessing.
- **Techniques**:
  - **Numerical Data**: Apply Z-score or Min-Max scaling.
  - **Categorical Data**: Encode using one-hot encoding or label encoding.
- **R Code Example**:
```r
# One-hot encoding using model.matrix()
encoded_data <- model.matrix(~ factor_column - 1, data = df)

# Combine encoded data with numerical features
final_data <- cbind(df[, c('num_col1', 'num_col2')], encoded_data)
```

**Key Insight**: Ensure that normalization or scaling is only applied to numerical columns.

---

### **Slide 47: Time Series and Categorical Data Preprocessing**
**Time Series Data**:
- **Considerations**:
  - Ensure data is stationary before applying algorithms.
  - Use techniques like differencing or log transformation to stabilize variance.
- **Example R Code**:
```r
# Log transformation for variance stabilization
time_series_data$log_value <- log(time_series_data$value)
```

**Categorical Data**:
- **Challenge**: Encoding high cardinality categorical features.
- **Solution**: Use target encoding or frequency encoding for categorical variables with many levels.

**Discussion**: 
- Discuss when these techniques are most beneficial and their potential pitfalls (e.g., overfitting in target encoding).

---

### **Slide 48: Addressing Common Misconceptions**
**Misconceptions**:
- **Normalization Is Always Required**: Not true for decision tree-based models that do not depend on feature scales.
- **Batch Correction Removes All Batch Effects**: Batch correction reduces, but may not completely remove, batch effects without losing biological variance.
- **Outliers Must Be Removed**: Not always—sometimes outliers carry critical information, especially in anomaly detection.

**Examples**:
- Use a decision tree model before and after normalization to demonstrate minimal impact on its performance.
- Show PCA plots with partial batch correction to illustrate how residual batch effects might remain.

**Key Takeaway**: Understand the specific needs of your dataset and model before applying these techniques blindly.

---

### **Slide 49: Before vs. After Preprocessing – A Visual Summary**
**Visualization**:
- **Side-by-Side Comparison**:
  - Histograms, scatter plots, and PCA plots of raw vs. preprocessed data.
- **Example**:
  - Display a dataset with missing values, outliers, and batch effects before cleaning.
  - Show the same dataset post-preprocessing with normalized features and corrected batches.

**Insights**:
- Highlight how preprocessing impacts model input and overall performance.
- Emphasize the importance of iterative preprocessing: checking intermediate steps to ensure data integrity.

**Questions for Reflection**:
- How does the choice of preprocessing technique influence different types of models?
- What additional preprocessing steps would you consider for specific machine learning models (e.g., neural networks)?

---

### **Slide 50: Summary and Next Steps**
**Recap**:
- **Key Learnings**:
  - **Data Cleaning**: Essential for handling missing data and outliers.
  - **Normalization**: Z-score and Min-Max scaling and their specific use cases.
  - **Batch Correction**: Techniques like ComBat for reducing unwanted variation.
- **Exercises Covered**:
  - Implementing a full preprocessing pipeline.
  - Applying batch correction and observing its impact using PCA.
- **Next Steps**:
  - Practice applying these techniques to new datasets.
  - Explore additional R packages for advanced preprocessing (`tidyverse`, `data.table`).
- **Q&A Session**:
  - Open the floor for any final questions or clarifications.

---

