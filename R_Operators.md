---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---
Based on general R programming practices, here is an exhaustive list of operators, operations, and operands in the R programming language:

---

### **1. Arithmetic Operators**

These operators perform basic mathematical operations.

| Operator | Description            | Example        |
|----------|------------------------|----------------|
| `+`      | Addition               | `x + y`       |
| `-`      | Subtraction            | `x - y`       |
| `*`      | Multiplication         | `x * y`       |
| `/`      | Division               | `x / y`       |
| `^` or `**` | Exponentiation     | `x^y` or `x**y` |
| `%%`     | Modulus (remainder)    | `x %% y`      |
| `%/%`    | Integer division       | `x %/% y`     |

---

### **2. Relational Operators**

Relational operators return a Boolean value (TRUE or FALSE) and are commonly used in conditional statements.

| Operator | Description       | Example        |
|----------|-------------------|----------------|
| `==`     | Equal to          | `x == y`      |
| `!=`     | Not equal to      | `x != y`      |
| `>`      | Greater than      | `x > y`       |
| `<`      | Less than         | `x < y`       |
| `>=`     | Greater or equal  | `x >= y`      |
| `<=`     | Less or equal     | `x <= y`      |

---

### **3. Logical Operators**

Logical operators are used for Boolean logic.

| Operator | Description            | Example         |
|----------|------------------------|-----------------|
| `&`      | Element-wise AND       | `x & y`        |
| `|`      | Element-wise OR        | `x | y`        |
| `!`      | NOT                    | `!x`           |
| `&&`     | Short-circuit AND      | `x && y`       |
| `||`     | Short-circuit OR       | `x || y`       |

---

### **4. Assignment Operators**

Assignment operators are used to assign values to variables.

| Operator | Description                   | Example       |
|----------|-------------------------------|---------------|
| `<-`     | Left assignment               | `x <- 5`     |
| `->`     | Right assignment              | `5 -> x`     |
| `<<-`    | Global environment assignment | `x <<- 5`    |
| `=`      | Equal assignment              | `x = 5`      |

---

### **5. Miscellaneous Operators**

These operators include membership, sequence, and matrix operations.

| Operator | Description               | Example           |
|----------|---------------------------|-------------------|
| `%in%`   | Membership                | `x %in% y`       |
| `:`      | Sequence generation       | `1:10`           |
| `%*%`    | Matrix multiplication     | `A %*% B`        |
| `%o%`    | Outer product             | `A %o% B`        |
| `%x%`    | Kronecker product         | `A %x% B`        |

---

### **6. Indexing and Subsetting Operators**

Used for accessing elements in vectors, lists, and other data structures.

| Operator | Description                   | Example             |
|----------|-------------------------------|---------------------|
| `[]`     | Subset                         | `x[1]`             |
| `[[]]`   | Element selection from lists   | `list[[1]]`        |
| `$`      | Named list element selection   | `data$column`      |

---

### **7. Special Operators**

R provides special operators for certain specific operations.

| Operator | Description                    | Example               |
|----------|--------------------------------|-----------------------|
| `?`      | Help                           | `?mean`              |
| `@`      | Slot operator (for S4 objects) | `object@slot`        |
| `::`     | Access function in a package   | `package::function`  |
| `:::`    | Access internal function       | `package:::function` |

---

### **8. Comparison with `match()` and `%in%`**

Although `%in%` is an operator, it’s often paired with `match()` to check if an element exists in a vector, and `match()` is commonly used for finding matching indices.

---

### **Operands**

Operands in R are typically variables, constants, vectors, or expressions that these operators act on. Examples include:

- **Scalars** (like `5`, `TRUE`, `NA`),
- **Vectors** (e.g., `c(1, 2, 3)`),
- **Data Frames** (e.g., `mtcars`),
- **Matrices** (e.g., `matrix(c(1,2,3,4), nrow=2)`), and
- **Lists** (e.g., `list(a=1, b=2)`).

---

This comprehensive list outlines R’s primary operators, operations, and the typical operands you will encounter, providing a strong basis for effective coding and data manipulation in R. 

Here are examples for each operator and operand type in R, showcasing their usage:

---

### **1. Arithmetic Operators**

| Operator | Description | Example | Code                                      | Output |
|----------|-------------|---------|-------------------------------------------|--------|
| `+`      | Addition    | `3 + 4` | `3 + 4`                                   | `7`    |
| `-`      | Subtraction | `10 - 5` | `10 - 5`                                 | `5`    |
| `*`      | Multiplication | `4 * 5` | `4 * 5`                               | `20`   |
| `/`      | Division    | `20 / 4` | `20 / 4`                                 | `5`    |
| `^`      | Exponentiation | `2^3` | `2^3`                                   | `8`    |
| `%%`     | Modulus     | `10 %% 3` | `10 %% 3`                               | `1`    |
| `%/%`    | Integer Division | `10 %/% 3` | `10 %/% 3`                         | `3`    |

---

### **2. Relational Operators**

| Operator | Description | Example | Code                                      | Output |
|----------|-------------|---------|-------------------------------------------|--------|
| `==`     | Equal to    | `5 == 5` | `5 == 5`                                 | `TRUE` |
| `!=`     | Not equal to | `5 != 3` | `5 != 3`                                | `TRUE` |
| `>`      | Greater than | `5 > 3` | `5 > 3`                                 | `TRUE` |
| `<`      | Less than   | `3 < 5` | `3 < 5`                                  | `TRUE` |
| `>=`     | Greater or equal | `5 >= 5` | `5 >= 5`                           | `TRUE` |
| `<=`     | Less or equal | `3 <= 5` | `3 <= 5`                             | `TRUE` |

---

### **3. Logical Operators**

| Operator | Description | Example | Code                                      | Output |
|----------|-------------|---------|-------------------------------------------|--------|
| `&`      | Element-wise AND | `c(TRUE, FALSE) & c(TRUE, TRUE)` | `c(TRUE, FALSE) & c(TRUE, TRUE)` | `TRUE, FALSE` |
| `|`      | Element-wise OR | `c(TRUE, FALSE) | c(FALSE, TRUE)` | `c(TRUE, FALSE) | c(FALSE, TRUE)` | `TRUE, TRUE` |
| `!`      | NOT         | `!TRUE` | `!TRUE`                                  | `FALSE` |
| `&&`     | Short-circuit AND | `TRUE && FALSE` | `TRUE && FALSE`           | `FALSE` |
| `||`     | Short-circuit OR | `TRUE || FALSE` | `TRUE || FALSE`             | `TRUE`  |

---

### **4. Assignment Operators**

| Operator | Description | Example | Code                                      | Output |
|----------|-------------|---------|-------------------------------------------|--------|
| `<-`     | Left assignment | `x <- 10` | `x <- 10; x`                           | `10`   |
| `->`     | Right assignment | `10 -> x` | `10 -> x; x`                           | `10`   |
| `<<-`    | Global environment assignment | `y <<- 20` | `y <<- 20; y`           | `20`   |
| `=`      | Equal assignment | `z = 30` | `z = 30; z`                             | `30`   |

---

### **5. Miscellaneous Operators**

| Operator | Description | Example | Code                                      | Output |
|----------|-------------|---------|-------------------------------------------|--------|
| `%in%`   | Membership  | `5 %in% c(1, 2, 3, 5)` | `5 %in% c(1, 2, 3, 5)`           | `TRUE` |
| `:`      | Sequence generation | `1:5` | `1:5`                                 | `1, 2, 3, 4, 5` |
| `%*%`    | Matrix multiplication | `matrix(1:4, 2, 2) %*% matrix(4:1, 2, 2)` | `matrix(1:4, 2, 2) %*% matrix(4:1, 2, 2)` | `10, 7; 22, 15` |
| `%o%`    | Outer product | `c(1, 2) %o% c(3, 4)` | `c(1, 2) %o% c(3, 4)`            | `3, 4; 6, 8` |
| `%x%`    | Kronecker product | `c(1, 2) %x% c(3, 4)` | `c(1, 2) %x% c(3, 4)`        | `3, 4, 6, 8` |

---

### **6. Indexing and Subsetting Operators**

| Operator | Description | Example | Code                                      | Output |
|----------|-------------|---------|-------------------------------------------|--------|
| `[]`     | Subset      | `x <- c(10, 20, 30); x[1]` | `x <- c(10, 20, 30); x[1]`   | `10`   |
| `[[]]`   | Element selection from lists | `list(1, 2)[[1]]` | `list(1, 2)[[1]]` | `1`    |
| `$`      | Named list element selection | `data.frame(a=1:3)$a` | `data.frame(a=1:3)$a` | `1, 2, 3` |

---

### **7. Special Operators**

| Operator | Description | Example | Code                                      | Output |
|----------|-------------|---------|-------------------------------------------|--------|
| `?`      | Help        | `?mean` | `?mean`                                  | Opens help page for `mean` |
| `@`      | Slot operator (for S4 objects) | `obj@slot` | Only applicable in S4 objects |
| `::`     | Access function in a package | `stats::sd` | `stats::sd(c(1, 2, 3))`    | `1`    |
| `:::`    | Access internal function | `package:::function` | Accesses internal functions within packages |

---

### **8. Examples of Operands in R**

1. **Scalars**:
   ```r
   x <- 5           # Single numeric value
   y <- TRUE        # Boolean value
   z <- NA          # Missing value
   ```

2. **Vectors**:
   ```r
   vec <- c(1, 2, 3) # Numeric vector
   vec_logical <- c(TRUE, FALSE, TRUE) # Logical vector
   ```

3. **Data Frames**:
   ```r
   df <- data.frame(Name = c("Alice", "Bob"), Age = c(25, 30))
   ```

4. **Matrices**:
   ```r
   mat <- matrix(1:4, nrow=2)
   ```

5. **Lists**:
   ```r
   my_list <- list(name = "Alice", age = 25, scores = c(90, 80, 70))
   ```

---

These examples demonstrate practical uses for each operator and operand type in R, illustrating their roles and typical applications. 

---
Here are thoughtfully crafted class exercises with solutions on functions in R, designed to reinforce essential skills for machine learning.

---

### **Exercise 1: Writing Basic Functions**

**Objective**: Understand how to write and execute basic functions in R.

**Exercise**: 
1. Define a function called `square_num()` that takes a single numeric argument and returns its square.
2. Define a function called `greet_user()` that takes a name as an argument and prints "Hello, [name]!".

**Solution**:
```r
# Function to square a number
square_num <- function(x) {
  return(x^2)
}

# Testing square_num function
square_num(4) # Output: 16

# Function to greet user
greet_user <- function(name) {
  cat("Hello,", name, "!\n")
}

# Testing greet_user function
greet_user("Alice") # Output: Hello, Alice!
```

---

### **Exercise 2: Calculating Basic Statistics with a Function**

**Objective**: Learn to create functions that return multiple outputs.

**Exercise**: Write a function called `calc_stats()` that takes a numeric vector as input and returns a list with the mean, median, and standard deviation of the vector.

**Solution**:
```r
# Function to calculate basic statistics
calc_stats <- function(data) {
  stats <- list(
    mean = mean(data, na.rm = TRUE),
    median = median(data, na.rm = TRUE),
    sd = sd(data, na.rm = TRUE)
  )
  return(stats)
}

# Testing calc_stats function
calc_stats(c(2, 4, 6, 8, 10))
# Output: List with mean: 6, median: 6, sd: 3.162278
```

---

### **Exercise 3: Using Functions for Data Transformation**

**Objective**: Practice creating functions that can apply transformations to data.

**Exercise**:
1. Write a function called `normalize()` that takes a numeric vector and returns a normalized version where each element is scaled between 0 and 1.
2. Test your function on a vector of values between 10 and 20.

**Solution**:
```r
# Function to normalize a vector
normalize <- function(x) {
  return((x - min(x, na.rm = TRUE)) / (max(x, na.rm = TRUE) - min(x, na.rm = TRUE)))
}

# Testing normalize function
normalize(c(10, 12, 14, 16, 18, 20))
# Output: 0, 0.25, 0.5, 0.75, 1
```

---

### **Exercise 4: Writing a Function to Apply a Mathematical Formula**

**Objective**: Gain experience with mathematical functions in R.

**Exercise**: 
Create a function `euclidean_distance()` that takes two numeric vectors of the same length and calculates the Euclidean distance between them.

**Solution**:
```r
# Function to calculate Euclidean distance
euclidean_distance <- function(vec1, vec2) {
  return(sqrt(sum((vec1 - vec2)^2)))
}

# Testing euclidean_distance function
euclidean_distance(c(1, 2, 3), c(4, 5, 6)) # Output: 5.196152
```

---

### **Exercise 5: Higher-Order Functions and Apply Family**

**Objective**: Use functions that apply operations to entire datasets.

**Exercise**:
1. Create a function `mean_sd()` that calculates the mean and standard deviation for each column in a given dataframe of numeric values.
2. Use `sapply()` to apply this function to each column of the `mtcars` dataset.

**Solution**:
```r
# Function to calculate mean and sd for a column
mean_sd <- function(column) {
  return(c(mean = mean(column, na.rm = TRUE), sd = sd(column, na.rm = TRUE)))
}

# Applying mean_sd to each column in mtcars
sapply(mtcars, mean_sd)
```

---

### **Exercise 6: Custom Function with Default Arguments**

**Objective**: Learn to use default values in functions.

**Exercise**: 
Create a function `power()` that raises a base number to an exponent. Set the exponent’s default value to 2. Test the function with both one and two arguments.

**Solution**:
```r
# Function with default argument
power <- function(base, exponent = 2) {
  return(base ^ exponent)
}

# Testing power function
power(3)        # Output: 9 (3^2)
power(3, 3)     # Output: 27 (3^3)
```

---

These exercises cover foundational skills in R functions that are particularly valuable for machine learning, where functions can simplify code, facilitate data transformations, and promote reusability.
