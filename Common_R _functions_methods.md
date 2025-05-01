---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---
Here is a list of commonly used methods and functions in R, covering basic operations, statistical analysis, and graphics with `ggplot2`:

---

### **1. Basic Operations and Data Handling**

- **Arithmetic Functions**:
  - `sum(x)`: Sum of elements.
  - `prod(x)`: Product of elements.
  - `min(x)`, `max(x)`: Minimum and maximum of elements.
  - `mean(x)`: Mean of elements.
  - `median(x)`: Median value.
  - `range(x)`: Range (min and max).

- **Vector and Matrix Operations**:
  - `c()`: Create a vector.
  - `rep(x, times)`: Repeat elements.
  - `seq(from, to, by)`: Generate sequences.
  - `matrix(data, nrow, ncol)`: Create a matrix.
  - `t(x)`: Transpose of a matrix.
  - `diag(x)`: Create or extract diagonals.

- **Logical and Conditional Functions**:
  - `all(x)`, `any(x)`: Check if all or any elements are `TRUE`.
  - `which(x)`: Returns the indices of `TRUE` elements.
  - `ifelse(condition, true_value, false_value)`: Vectorized conditional.

- **Subsetting**:
  - `subset(data, condition)`: Return a subset of data.
  - `filter()` (from `dplyr`): Filter rows by condition.
  - `select()` (from `dplyr`): Select columns by name.

- **Apply Family**:
  - `apply(X, MARGIN, FUN)`: Apply a function over matrix margins.
  - `lapply(X, FUN)`: Apply a function over list elements.
  - `sapply(X, FUN)`: Simplified `lapply`.
  - `tapply(X, INDEX, FUN)`: Apply a function over subsets.
  - `mapply(FUN, ...)`: Multivariate `sapply`.

---

### **2. Statistical Analysis Functions**

- **Descriptive Statistics**:
  - `summary(x)`: Summary statistics (min, median, mean, etc.).
  - `quantile(x, probs)`: Quantiles of the data.
  - `sd(x)`: Standard deviation.
  - `var(x)`: Variance.
  - `cor(x, y)`: Correlation coefficient.
  - `cov(x, y)`: Covariance.

- **Hypothesis Testing**:
  - `t.test(x, y)`: Perform a t-test.
  - `wilcox.test(x, y)`: Wilcoxon test.
  - `chisq.test(x)`: Chi-squared test.
  - `anova(model)`: Analysis of variance.

- **Regression and Modeling**:
  - `lm(formula, data)`: Fit a linear model.
  - `glm(formula, family, data)`: Fit a generalized linear model.
  - `summary(model)`: Summary of model fit.
  - `predict(model, newdata)`: Generate predictions.
  - `residuals(model)`: Extract residuals.
  - `coef(model)`: Extract coefficients.
  - `confint(model)`: Confidence intervals for model parameters.

- **Distribution Functions**:
  - `rnorm(n, mean, sd)`: Generate random normal data.
  - `dnorm(x, mean, sd)`: Density of normal distribution.
  - `pnorm(q, mean, sd)`: Cumulative probability.
  - `qnorm(p, mean, sd)`: Quantile function.
  - (Similar functions for `runif()`, `rbinom()`, `rpois()`, etc.)

---

### **3. Data Manipulation Functions**

- **`dplyr` Package**:
  - `mutate(data, new_column = expression)`: Add/modify columns.
  - `arrange(data, column)`: Sort data by columns.
  - `summarise(data, new_summary = expression)`: Summarize data.
  - `group_by(data, column)`: Group data by columns.
  - `join()` family (e.g., `inner_join()`, `left_join()`): Combine dataframes.

- **`tidyr` Package**:
  - `pivot_longer()`: Transform wide data to long format.
  - `pivot_wider()`: Transform long data to wide format.
  - `separate()` and `unite()`: Split and combine columns.

---

### **4. File I/O Functions**

- `read.csv(file)`, `write.csv(data, file)`: Read/write CSV files.
- `read.table(file)`, `write.table(data, file)`: Read/write table data.
- `readRDS(file)`, `saveRDS(data, file)`: Read/write R serialized data.
- `load(file)`, `save(data, file)`: Load/save workspace variables.

---

### **5. Graphics with `ggplot2`**

- **Basic Plotting**:
  - `ggplot(data, aes(x, y))`: Create a base plot object.
  - `geom_point()`: Scatter plot.
  - `geom_line()`: Line plot.
  - `geom_histogram()`: Histogram.
  - `geom_bar()`: Bar plot.
  - `geom_boxplot()`: Boxplot.

- **Aesthetic Mappings**:
  - `aes(color = variable)`: Map color to a variable.
  - `aes(size = variable)`: Map size to a variable.
  - `aes(shape = variable)`: Map shape to a variable.

- **Faceting**:
  - `facet_wrap(~ variable)`: Create multiple panels.
  - `facet_grid(rows ~ cols)`: Create a grid layout.

- **Customizing Plots**:
  - `labs(title, x, y)`: Add labels and titles.
  - `theme()`: Customize overall appearance.
  - `scale_x_continuous()`, `scale_y_continuous()`: Customize axis scales.
  - `scale_color_manual(values = c("color1", "color2"))`: Manual color scales.

- **Adding Layers**:
  - `geom_smooth(method = "lm")`: Add trend lines.
  - `geom_text(aes(label = variable))`: Add text labels.
  - `annotate()`: Add annotations.

---

These functions and methods are important for data analysis, manipulation, and visualization in R.

