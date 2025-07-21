+++
title = 'Mini Project 2: Clustering Movies Based on Characteristics'
date = 2025-07-20T14:00:00-04:00
weight = 10
description = 'Analyzing trends within similar movies based on different metrics'
tags = []
pageName = "miniproject2"
icon = 'movie'
draft = false
+++


## **Introduction**

The purpose of our study was to utilize machine learning techniques to effectively group movies with their characteristics in order to provide insights into applications such as **market segmentation and recommendation settings**.

---

## **Data**

We made use of a dataset that included information about various features of movies such as **genre flags, popularity scores, runtime durations, and average viewer ratings**. Furthermore, during our **exploratory data analysis (EDA)**, we were able to assess certain measurements and the general presence of any missing values.

---

## **Methodology**

### **Exploratory Data Analysis**
- Examined the dataset's **structure** (shape, number of rows and columns, missing values).
- Calculated the **number of unique clusters** identified during clustering and examined **statistical summaries** of quantitative features.
- Analyzed **distributions of key features** to visualize shape, potential outliers, center, and spread.

### **Feature Analysis**
- Visualized distributions using **histograms and boxplots**.

![Histograms and Boxplots](/distributions.svg "**Distributions** and **spreads** of **popularity**, **runtime**, and **vote average**")

- Created **scatter plot matrices** to illustrate relationships and correlations between quantitative features.

![Scatter Plot Matrix](/scatter_matrix.svg "**Scatter plot matrix** showing **relationships** between **popularity**, **runtime**, and **vote average**")

- Generated a **correlation heatmap** to identify possible linear relationships between genre flags, popularity scores, runtime durations, and vote average ratings.

![Correlation Heatmap](/correlation_heatmap.svg "**Heatmap** showing **correlations** between **movie features**")

### **Clustering Preparation**
- **Standardized features** using `StandardScaler` to ensure all features contributed equally to clustering, preventing features with larger scales from dominating distance calculations.
- Determined the **optimal number of clusters to be 5** using:
  - **Elbow Method**: Assessed within-cluster sum of squares (WCSS) for different cluster counts, with the "elbow" point indicating diminishing returns when adding more clusters.
  - **Silhouette Scores**: Measured how similar objects are to their own cluster compared to other clusters; higher silhouette scores indicate better-defined clusters.

![Elbow Method and Silhouette Scores](/elbow_silhouette.svg "**Elbow method** and **silhouette scores** for optimal **cluster determination**")

### **Dimensionality Reduction**
We used two techniques to visualize high-dimensional data in 2D:
- **Principal Component Analysis (PCA)**: Linear dimension reduction projecting data onto orthogonal axes maximizing variance to obtain uncorrelated principal components.
- **t-Distributed Stochastic Neighbor Embedding (t-SNE)**: Nonlinear probabilistic algorithm minimizing Kullback–Leibler divergence between joint probability distributions of high-dimensional and low-dimensional data to preserve local neighborhood structure for visualization.

![PCA and t-SNE Graphs](/PCA_t-SNE.svg "**2D projections** of **K-means clusters** using **PCA** and **t-SNE**")

### **K-means Clustering**
- Used **K-means** to group movies into the **five identified clusters**.
- Visualized resulting clusters in both PCA and t-SNE 2D projected spaces to observe separation and distribution.
- Analyzed **cluster characteristics**:
  - **Genre composition**: Percentage of each genre within each cluster to identify dominant genres.
  - **Average popularity, runtime, and vote averages** for each cluster.
  - **Sample movie titles** to provide concrete examples from each group.

![Genre Composition](/genre_composition_by_cluster.svg "**Genre composition percentages** within each **cluster**")

---

## **Discussion**

Our analysis found that **movie features like popularity, runtime, and vote averages are roughly normally distributed** with some outliers, and that **genres have low correlations with these features**. Using both elbow and silhouette methods, we determined that **five clusters were optimal**.

We then used **PCA and t-SNE to simplify the data into two dimensions**, making it easier to see how movies cluster together. **t-SNE did a better job of separating similar movies into distinct groups**.

**Here’s what we found:**
- **Cluster 0** mostly includes **Action and Drama movies**, with some **Sci-Fi** mixed in.
- **Cluster 1** is **almost entirely Comedy**.
- **Cluster 2** has a mix of **Action, Drama, and Comedy**.
- **Cluster 3** is made up **entirely of Horror movies**.
- **Cluster 4** is most diverse, including **Comedy, Drama, Romance, Action, and Sci-Fi**.

These insights show that our data **naturally groups movies by genre and performance**, which can be useful for things like **recommendation systems, marketing strategies, and understanding viewer preferences**.
