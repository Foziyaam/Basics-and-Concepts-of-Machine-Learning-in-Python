---
title: "R Notebook"
output:
  html_document:
    df_print: paged
---

#### **Slide 1: Title Slide**

- **Title**: Unsupervised Learning in R
- **Subtitle**: Clustering and Dimensionality Reduction Techniques
- **Course**: Machine Learning in R
- **Instructor**: [Your Name]

---

#### **Slide 2: Introduction to Unsupervised Learning**

- **Concept**: Unsupervised learning involves finding hidden patterns or intrinsic structures in data without pre-existing labels.
- **Principles**:
  - **Clustering**: Grouping similar data points together.
  - **Dimensionality Reduction**: Reducing the number of variables while retaining important information.
- **Applications**: Market segmentation, image compression, anomaly detection.

---

#### **Slide 3: Differences Between Supervised and Unsupervised Learning**

- **Supervised Learning**:
  - Data with labeled outcomes.
  - Goal: Predict labels for new data.
- **Unsupervised Learning**:
  - Data without labeled outcomes.
  - Goal: Discover structure or patterns in data.
- **Example**:
  - **Supervised**: Predicting house prices.
  - **Unsupervised**: Grouping customers based on purchasing behavior.

---

### **Part 1: Clustering Techniques**

---

#### **Slide 4: Introduction to Clustering**

- **Concept**: Clustering is the task of dividing the dataset into groups (clusters) such that data points in the same group are more similar to each other than to those in other groups.
- **Types of Clustering Algorithms**:
  - **Hierarchical Clustering**
  - **Partitioning Methods** (e.g., K-Means)
  - **Density-Based Clustering** (e.g., DBSCAN)
- **Use Cases**: Customer segmentation, gene sequence analysis, document classification.

---

#### **Slide 5: Hierarchical Clustering Overview**

- **Concept**: Builds a hierarchy of clusters either by merging or splitting existing clusters.
- **Types**:
  - **Agglomerative (Bottom-Up)**: Starts with individual data points and merges them into clusters.
  - **Divisive (Top-Down)**: Starts with one cluster and splits it into smaller clusters.
- **Visualization**: Dendrogram.

---

#### **Slide 6: Steps in Hierarchical Clustering**

1. **Compute the Distance Matrix**:
   - Measure the distance between each pair of observations.
   - Common metrics: Euclidean, Manhattan.
2. **Linkage Criteria**:
   - **Single Linkage**: Minimum distance between points.
   - **Complete Linkage**: Maximum distance between points.
   - **Average Linkage**: Average distance between points.
3. **Build the Dendrogram**:
   - Merge clusters based on the linkage criteria.
4. **Decide on the Number of Clusters**:
   - Cut the dendrogram at a specific height.

---

#### **Slide 7: Distance Metrics**

- **Euclidean Distance**:
  - Formula: \( d(p,q) = \sqrt{\sum_{i=1}^{n}(p_i - q_i)^2} \)
- **Manhattan Distance**:
  - Formula: \( d(p,q) = \sum_{i=1}^{n}|p_i - q_i| \)
- **Choice of Metric**:
  - Depends on the data and the problem context.

---

#### **Slide 8: Linkage Methods**

- **Single Linkage**:
  - Distance between two clusters is the minimum distance between any two points in the clusters.
- **Complete Linkage**:
  - Distance between clusters is the maximum distance between any two points.
- **Average Linkage**:
  - Distance is the average distance between all pairs of points.
- **Ward's Method**:
  - Minimizes the total within-cluster variance.

---

#### **Slide 9: Hierarchical Clustering in R**

- **Functions**:
  - `dist()`: Compute the distance matrix.
  - `hclust()`: Perform hierarchical clustering.
- **Syntax Example**:
  ```r
  # Compute distance matrix
  dist_matrix <- dist(data, method = "euclidean")
  
  # Perform hierarchical clustering
  hc <- hclust(dist_matrix, method = "complete")
  ```

---

#### **Slide 10: Visualizing Dendrograms**

- **Function**: `plot()`
- **Example**:
  ```r
  plot(hc, labels = FALSE, main = "Hierarchical Clustering Dendrogram")
  ```
- **Interpretation**:
  - The height at which clusters are merged indicates the distance between them.
  - Decide on the number of clusters by cutting the dendrogram at a specific height.

---

#### **Slide 11: Cutting the Dendrogram**

- **Function**: `cutree()`
- **Syntax**:
  ```r
  clusters <- cutree(hc, k = 3)  # Cut into 3 clusters
  ```
- **Result**:
  - Assigns each observation to one of the specified number of clusters.

---

#### **Slide 12: Hierarchical Clustering Example with Iris Dataset**

- **Dataset**: `iris` (excluding the species column)
- **Steps**:
  - Load the data and remove labels.
  - Compute distance matrix.
  - Perform clustering.
  - Visualize the dendrogram.
- **R Code**:
  ```r
  data(iris)
  iris_data <- iris[, -5]  # Remove species column
  dist_matrix <- dist(iris_data)
  hc <- hclust(dist_matrix)
  plot(hc, main = "Dendrogram for Iris Dataset")
  ```

---

#### **Slide 13: Exercise 1 – Hierarchical Clustering on Iris Dataset**

- **Task**:
  - Perform hierarchical clustering on the `iris` dataset.
  - Cut the dendrogram to form 3 clusters.
  - Compare the clusters with actual species.
- **Solution**:
  ```r
  # Cut the dendrogram
  clusters <- cutree(hc, k = 3)
  
  # Add clusters to the data
  iris$Cluster <- as.factor(clusters)
  
  # Compare with actual species
  table(iris$Species, iris$Cluster)
  ```
- **Interpretation**:
  - Examine how well the clusters correspond to actual species.

---

#### **Slide 14: K-Means Clustering Overview**

- **Concept**: Partitioning method that divides data into K non-overlapping clusters.
- **Algorithm Steps**:
  1. Initialize K centroids randomly.
  2. Assign each data point to the nearest centroid.
  3. Recalculate centroids as the mean of assigned points.
  4. Repeat steps 2 and 3 until convergence.

---

#### **Slide 15: Choosing the Number of Clusters (K)**

- **Elbow Method**:
  - Plot the total within-cluster sum of squares (WSS) against the number of clusters.
  - Look for an "elbow" point where the rate of decrease sharply changes.
- **Silhouette Method**:
  - Measures how similar an object is to its own cluster compared to other clusters.
  - Silhouette coefficient ranges from -1 to 1.

---

#### **Slide 16: K-Means Clustering in R**

- **Function**: `kmeans()`
- **Syntax Example**:
  ```r
  set.seed(123)
  kmeans_result <- kmeans(data, centers = 3, nstart = 20)
  ```
- **Parameters**:
  - `centers`: Number of clusters.
  - `nstart`: Number of random sets to choose the best.

---

#### **Slide 17: K-Means Clustering Example with Iris Dataset**

- **Steps**:
  - Load the data and remove labels.
  - Perform k-means clustering with K=3.
  - Analyze the cluster assignments.
- **R Code**:
  ```r
  set.seed(123)
  kmeans_result <- kmeans(iris_data, centers = 3, nstart = 20)
  iris$Cluster <- as.factor(kmeans_result$cluster)
  
  # Compare with actual species
  table(iris$Species, iris$Cluster)
  ```

---

#### **Slide 18: Visualizing K-Means Clusters**

- **Using ggplot2**:
  ```r
  library(ggplot2)
  ggplot(iris, aes(Petal.Length, Petal.Width, color = Cluster)) +
    geom_point(size = 3) +
    labs(title = "K-Means Clustering of Iris Dataset")
  ```
- **Interpretation**:
  - Visualize how data points are grouped into clusters.

---

#### **Slide 19: Exercise 2 – K-Means Clustering on Customer Data**

- **Task**:
  - Assume you have a dataset `customers` with spending habits.
  - Perform k-means clustering to segment customers.
  - Determine the optimal number of clusters using the elbow method.
- **Solution**:
  ```r
  # Assuming customers data is loaded
  wss <- vector()
  for (k in 1:10) {
    kmeans_model <- kmeans(customers, centers = k, nstart = 20)
    wss[k] <- kmeans_model$tot.withinss
  }
  plot(1:10, wss, type = "b", main = "Elbow Method", xlab = "Number of Clusters", ylab = "Within-Cluster Sum of Squares")
  ```
- **Interpretation**:
  - Choose K where the decrease in WSS begins to level off.

---

#### **Slide 20: Density-Based Clustering (DBSCAN)**

- **Concept**: Clusters are formed based on the density of data points in a region.
- **Advantages**:
  - Can find arbitrarily shaped clusters.
  - Handles noise and outliers effectively.
- **Parameters**:
  - `eps`: Maximum distance between two samples for them to be considered as in the same neighborhood.
  - `minPts`: Minimum number of points required to form a dense region.

---

#### **Slide 21: DBSCAN in R**

- **Package**: `dbscan`
- **Syntax Example**:
  ```r
  library(dbscan)
  dbscan_result <- dbscan(data, eps = 0.5, minPts = 5)
  ```
- **Visualization**:
  ```r
  plot(data, col = dbscan_result$cluster + 1L)
  ```
  
---

#### **Slide 22: Exercise 3 – Applying DBSCAN**

- **Task**:
  - Apply DBSCAN to the `iris` dataset.
  - Experiment with different `eps` and `minPts` values.
- **Solution**:
  ```r
  library(dbscan)
  dbscan_result <- dbscan(iris_data, eps = 0.4, minPts = 5)
  iris$Cluster <- as.factor(dbscan_result$cluster)
  plot(iris$Petal.Length, iris$Petal.Width, col = iris$Cluster, main = "DBSCAN Clustering")
  ```
- **Interpretation**:
  - Analyze how changing parameters affects clustering.

---

### **Part 2: Dimensionality Reduction Techniques**

---

#### **Slide 23: Introduction to Dimensionality Reduction**

- **Concept**: Reducing the number of input variables in a dataset while retaining essential information.
- **Benefits**:
  - Simplifies models.
  - Reduces computational cost.
  - Helps in visualizing high-dimensional data.
- **Techniques**:
  - **Principal Component Analysis (PCA)**
  - **t-Distributed Stochastic Neighbor Embedding (t-SNE)**
  - **Linear Discriminant Analysis (LDA)**

---

#### **Slide 24: Principal Component Analysis (PCA) Overview**

- **Concept**: PCA transforms the original variables into a new set of uncorrelated variables (principal components) that capture the maximum variance in the data.
- **Principles**:
  - Each principal component is a linear combination of the original variables.
  - The first principal component accounts for the most variance.
- **Applications**:
  - Data visualization.
  - Noise reduction.
  - Feature extraction.

---

#### **Slide 25: Steps in PCA**

1. **Standardize the Data**:
   - Ensure each variable contributes equally.
2. **Compute the Covariance Matrix**:
   - Measure how variables vary together.
3. **Compute Eigenvalues and Eigenvectors**:
   - Eigenvectors determine the directions of the new feature space.
   - Eigenvalues determine their magnitude.
4. **Select Principal Components**:
   - Choose components with the highest eigenvalues.
5. **Transform the Data**:
   - Project data onto the new feature space.

---

#### **Slide 26: PCA in R**

- **Function**: `prcomp()`
- **Syntax Example**:
  ```r
  pca_result <- prcomp(data, center = TRUE, scale. = TRUE)
  ```
- **Parameters**:
  - `center`: Centers the variables to have mean zero.
  - `scale.`: Scales variables to have unit variance.

---

#### **Slide 27: Interpreting PCA Output**

- **Components**:
  - **Rotation**: Loadings of variables on principal components.
  - **sdev**: Standard deviations of principal components.
  - **x**: Scores of the observations on the components.
- **Explained Variance**:
  - Proportion of variance explained by each principal component.
  - Use `summary(pca_result)`.

---

#### **Slide 28: PCA Example with Iris Dataset**

- **Steps**:
  - Standardize the data.
  - Apply PCA.
  - Analyze the proportion of variance.
  - Visualize the results.
- **R Code**:
  ```r
  iris_data <- iris[, -5]
  pca_result <- prcomp(iris_data, center = TRUE, scale. = TRUE)
  summary(pca_result)
  plot(pca_result, type = "l", main = "Scree Plot")
  ```

---

#### **Slide 29: Scree Plot**

- **Concept**: A line plot of the eigenvalues (variances) associated with each principal component.
- **Purpose**:
  - Helps decide how many principal components to retain.
  - Look for an "elbow" point where additional components contribute less variance.

---

#### **Slide 30: Biplot in PCA**

- **Function**: `biplot()`
- **Purpose**:
  - Visualizes both the principal components and the loadings of variables.
- **Example**:
  ```r
  biplot(pca_result, scale = 0)
  ```
- **Interpretation**:
  - Points represent observations.
  - Arrows represent variables.

---

#### **Slide 31: Exercise 4 – PCA on Wine Dataset**

- **Task**:
  - Use the `wine` dataset (available in UCI Machine Learning Repository).
  - Perform PCA and decide how many components to retain.
  - Visualize the first two principal components.
- **Solution**:
  ```r
  # Load the data
  wine <- read.csv("wine.csv")
  wine_data <- wine[, -1]  # Remove label column
  
  # Standardize and apply PCA
  pca_result <- prcomp(wine_data, center = TRUE, scale. = TRUE)
  summary(pca_result)
  
  # Scree Plot
  plot(pca_result, type = "l", main = "Scree Plot")
  
  # Biplot
  biplot(pca_result, scale = 0)
  ```
- **Interpretation**:
  - Examine the proportion of variance explained.
  - Visualize data in reduced dimensions.

---

#### **Slide 32: t-SNE Overview**

- **Concept**: Non-linear dimensionality reduction technique that embeds high-dimensional data in a low-dimensional space, maintaining local similarities.
- **Principles**:
  - Converts high-dimensional Euclidean distances into conditional probabilities.
  - Minimizes the divergence between the two distributions.
- **Applications**:
  - Visualizing complex datasets.

---

#### **Slide 33: t-SNE in R**

- **Package**: `Rtsne`
- **Syntax Example**:
  ```r
  library(Rtsne)
  tsne_result <- Rtsne(data, dims = 2, perplexity = 30)
  ```
- **Parameters**:
  - `dims`: Number of dimensions (usually 2 or 3).
  - `perplexity`: Balances attention between local and global aspects of data.

---

#### **Slide 34: t-SNE Example with MNIST Dataset**

- **Dataset**: MNIST handwritten digits.
- **Steps**:
  - Load and preprocess the data.
  - Apply t-SNE.
  - Visualize the results.
- **Note**: Due to computational intensity, t-SNE is better suited for smaller datasets or subsets.

---

#### **Slide 35: Exercise 5 – t-SNE on Iris Dataset**

- **Task**:
  - Apply t-SNE to the `iris` dataset.
  - Visualize the 2D embedding.
- **Solution**:
  ```r
  library(Rtsne)
  set.seed(123)
  tsne_result <- Rtsne(iris_data, dims = 2, perplexity = 30)
  tsne_data <- data.frame(tsne_result$Y, Species = iris$Species)
  
  # Plotting
  ggplot(tsne_data, aes(X1, X2, color = Species)) +
    geom_point() +
    labs(title = "t-SNE Visualization of Iris Dataset")
  ```
- **Interpretation**:
  - Examine how well t-SNE separates the species.

---

#### **Slide 36: Comparing PCA and t-SNE**

- **PCA**:
  - Linear technique.
  - Preserves global structure.
  - Good for data with linear relationships.
- **t-SNE**:
  - Non-linear technique.
  - Preserves local structure.
  - Better for complex, non-linear data.
- **Considerations**:
  - t-SNE is computationally intensive.
  - PCA is faster and interpretable.

---

#### **Slide 37: Combining Clustering and Dimensionality Reduction**

- **Approach**:
  - Use dimensionality reduction to preprocess data.
  - Apply clustering algorithms on reduced data.
- **Benefits**:
  - Reduces noise and computation time.
  - May improve clustering results.

---

#### **Slide 38: Exercise 6 – Clustering on PCA-Reduced Data**

- **Task**:
  - Apply PCA to the `wine` dataset and retain the top 2 components.
  - Perform k-means clustering on the reduced data.
  - Compare the clusters with actual wine types.
- **Solution**:
  ```r
  # PCA
  pca_result <- prcomp(wine_data, center = TRUE, scale. = TRUE)
  pca_data <- data.frame(pca_result$x[, 1:2])
  
  # K-Means Clustering
  set.seed(123)
  kmeans_result <- kmeans(pca_data, centers = 3, nstart = 20)
  wine$Cluster <- as.factor(kmeans_result$cluster)
  
  # Compare with actual types
  table(wine$Type, wine$Cluster)
  
  # Visualization
  ggplot(pca_data, aes(PC1, PC2, color = wine$Cluster)) +
    geom_point() +
    labs(title = "K-Means Clustering on PCA-Reduced Wine Data")
  ```
- **Interpretation**:
  - Assess how well clustering aligns with actual wine types.

---

#### **Slide 39: Practical Considerations in Unsupervised Learning**

- **Standardization**:
  - Important for algorithms sensitive to scale (e.g., K-Means, PCA).
- **Choosing Parameters**:
  - Use domain knowledge and methods like the elbow method.
- **Evaluation**:
  - No direct measures like accuracy.
  - Use silhouette scores, clustering validation indices.

---

#### **Slide 40: Limitations and Challenges**

- **Interpretability**:
  - Clusters may not have clear meanings.
- **Choosing the Right Technique**:
  - Depends on data characteristics.
- **Computational Complexity**:
  - Some methods are computationally intensive (e.g., t-SNE).

---

#### **Slide 41: Exercise 7 – Evaluating Clustering Performance**

- **Task**:
  - Use the silhouette method to evaluate the quality of clusters obtained from K-Means on the `iris` dataset.
- **Solution**:
  ```r
  library(cluster)
  set.seed(123)
  kmeans_result <- kmeans(iris_data, centers = 3, nstart = 20)
  silhouette_score <- silhouette(kmeans_result$cluster, dist(iris_data))
  plot(silhouette_score, main = "Silhouette Plot for K-Means Clustering")
  ```
- **Interpretation**:
  - Silhouette values close to 1 indicate better clustering.

---

#### **Slide 42: Hierarchical Clustering vs. K-Means**

- **Hierarchical Clustering**:
  - Does not require pre-specifying the number of clusters.
  - Can capture nested clusters.
- **K-Means Clustering**:
  - Efficient for large datasets.
  - Requires specifying the number of clusters.
- **When to Use**:
  - Use hierarchical for small datasets or when the number of clusters is unknown.
  - Use K-Means for larger datasets with a known number of clusters.

---

#### **Slide 43: Dimensionality Reduction for Visualization**

- **Purpose**:
  - Reducing data to 2 or 3 dimensions for plotting.
- **Techniques**:
  - PCA for linear data.
  - t-SNE or UMAP for complex data.
- **Example**:
  - Visualizing high-dimensional customer data to identify segments.

---

#### **Slide 44: Exercise 8 – Applying UMAP**

- **Task**:
  - Apply UMAP (Uniform Manifold Approximation and Projection) to the `iris` dataset.
- **Solution**:
  ```r
  library(umap)
  umap_result <- umap(iris_data)
  umap_data <- data.frame(umap_result$layout, Species = iris$Species)
  
  # Plotting
  ggplot(umap_data, aes(X1, X2, color = Species)) +
    geom_point() +
    labs(title = "UMAP Visualization of Iris Dataset")
  ```
- **Interpretation**:
  - UMAP may preserve more of the global structure compared to t-SNE.

---

#### **Slide 45: Real-World Applications**

- **Market Segmentation**:
  - Clustering customers based on behavior.
- **Image Compression**:
  - PCA to reduce image dimensions.
- **Anomaly Detection**:
  - Identify outliers using clustering algorithms.

---

#### **Slide 46: Summary of Key Concepts**

- **Clustering**:
  - Groups similar data points.
  - Techniques: Hierarchical, K-Means, DBSCAN.
- **Dimensionality Reduction**:
  - Reduces variables.
  - Techniques: PCA, t-SNE, UMAP.
- **Evaluation**:
  - Use methods like silhouette scores, visual assessment.

---

#### **Slide 47: Best Practices**

- **Data Preprocessing**:
  - Standardize or normalize data.
- **Parameter Tuning**:
  - Experiment with different parameters (e.g., number of clusters, perplexity).
- **Validation**:
  - Use multiple techniques to confirm findings.

---

#### **Slide 48: Exercise 9 – Combining Techniques**

- **Task**:
  - Perform hierarchical clustering on PCA-reduced `iris` data.
  - Compare results with original clustering.
- **Solution**:
  ```r
  # PCA
  pca_result <- prcomp(iris_data, center = TRUE, scale. = TRUE)
  pca_data <- data.frame(pca_result$x[, 1:2])
  
  # Hierarchical Clustering
  dist_matrix <- dist(pca_data)
  hc <- hclust(dist_matrix)
  clusters <- cutree(hc, k = 3)
  iris$Cluster_PCA_HC <- as.factor(clusters)
  
  # Comparison
  table(iris$Species, iris$Cluster_PCA_HC)
  ```
- **Interpretation**:
  - Assess if dimensionality reduction improves clustering.

---

#### **Slide 49: Final Exercise – Project**

- **Task**:
  - Choose a dataset of interest.
  - Apply both clustering and dimensionality reduction techniques.
  - Present your findings, including visualizations and interpretations.
- **Guidelines**:
  - Explain your choice of methods.
  - Evaluate the quality of clusters.
  - Discuss any challenges faced.

---

#### **Slide 50: Q&A and Further Reading**

- **Questions**:
  - Address any queries or clarifications.
- **Further Reading**:
  - **Books**:
    - "An Introduction to Statistical Learning" by James et al.
    - "R in Action" by Robert I. Kabacoff.
  - **Online Resources**:
    - R documentation and tutorials.
    - Machine learning forums and communities.

---
