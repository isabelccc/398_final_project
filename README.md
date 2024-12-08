# Recipe Rating Analysis

## Introduction
# Introduction

## Dataset Overview

This project analyzes two datasets: a recipe dataset and an interaction dataset, containing information about recipes, user reviews, and ratings. The combined dataset offers rich insights into user preferences, cooking behaviors, and recipe characteristics. Specifically, the dataset contains:

- **Recipe Dataset**: Includes information about the recipes, such as cooking time, tags, and nutritional values.
- **Interaction Dataset**: Includes user ratings and reviews for various recipes.

The recipe dataset contains **83,782 rows**, and the interaction dataset contains **731,927 rows**. These datasets are merged on the `recipe_id` column to create a unified dataset for analysis.

## Research Question

### What factors influence a recipe's average rating, and can we predict it using its characteristics (e.g., cooking time, nutritional values, and tags)?

Understanding the factors that drive user satisfaction with recipes can help chefs and content creators design recipes that better match user preferences. It also provides insight into how various features of a recipe (like preparation time or healthiness) impact its success.

## Relevant Columns

<font color="orange">**"recipe_id"**:</font>  
The unique identifier for the recipe being analyzed.

<font color="orange">**"tags"**:</font>  
A list of tags that describe the recipe, such as cuisine type, dietary restrictions, or cooking methods.

<font color="orange">**"rating_average"**:</font>  
The average user rating for the recipe, calculated from the interaction dataset.

<font color="orange">**"review"**:</font>  
Textual feedback from users describing their experience with the recipe.

<font color="orange">**"minutes"**:</font>  
The total time required to prepare and cook the recipe, measured in minutes.

<font color="orange">**"n_steps"**:</font>  
The number of steps required to prepare the recipe.

<font color="orange">**"calories"**:</font>  
The total calorie count for the recipe, extracted from its nutritional data.

<font color="orange">**"total_fat_pdv"**:</font>  
The percentage of the daily value of total fat provided by the recipe.

<font color="orange">**"sugar_pdv"**:</font>  
The percentage of the daily value of sugar provided by the recipe.

<font color="orange">**"protein_pdv"**:</font>  
The percentage of the daily value of protein provided by the recipe.


## Data Cleaning and Exploratory Data Analysis
---
title: "Data Cleaning"
layout: page
permalink: /data-cleaning/
---

# Data Cleaning

## Overview of Cleaning Steps

To prepare the dataset for analysis, we applied the following data cleaning steps:

1. **Handling Missing Values**:
   - Replaced zero ratings in the `rating` column with `NaN` to reflect their missing nature.
   - Removed rows with missing `review`, `tags`, or `calories` to ensure the analysis uses complete data.

2. **Converting Data Types**:
   - Transformed the `nutrition` column (stored as strings) into Python lists for easy extraction of individual components (e.g., calories, fat).
   - Converted `tags` from a string representation to a list of tags.

3. **Feature Engineering**:
   - Added new columns, such as:
     - `review_length`: The number of words in each review.
     - `num_tags`: The number of tags associated with each recipe.

## Head of Cleaned DataFrame

# Generate Markdown representations for the DataFrame heads
relevant_data_head = relevant_data.head().to_markdown(index=False)
merged_data_head = merged_data.head().to_markdown(index=False)

# Create the Markdown content
markdown_content = f"""
# Data Cleaning

## Relevant Data Head

Below is the head of the cleaned relevant data:

```markdown

{relevant_data_head}

{merged_data_head}



## Prediction Problem
# Prediction Problem

## Problem Type
Our prediction problem is to forecast the **average rating** of a recipe based on its features, such as the number of steps, review length, tags, and nutritional content. This is a **regression problem** because the response variable, `rating_average`, is continuous and ranges between 0 and 5.

---

## Response Variable
The response variable is `rating_average`:
- **Why we chose it:** Ratings are critical to understanding the popularity and quality of a recipe. By predicting the rating, we can provide insights into what makes a recipe well-received and help users or chefs improve their recipes.

---

## Evaluation Metric
The primary evaluation metric for this regression problem is **Root Mean Squared Error (RMSE)**:
- **Why RMSE:** RMSE penalizes large errors more than metrics like Mean Absolute Error (MAE), which is suitable since large prediction errors (e.g., a predicted rating of 4 when the true rating is 2) are particularly problematic in this context.
- RMSE provides an intuitive measure in the same unit as the target variable (`rating_average`), making it easy to interpret.
- Alternative metrics like R² (coefficient of determination) can be used for supplementary evaluation but do not penalize individual large errors as effectively as RMSE.

---

## Features Used
The model uses features available at the time of prediction:

### Numerical Features:
- `minutes`: The time required to make the recipe.
- `n_steps`: Number of steps in the recipe.
- `num_tags`: The number of tags associated with the recipe.
- `review_length`: The length of the reviews for the recipe.
- Nutritional values:
  - `total_fat_pdv`
  - `sugar_pdv`
  - `protein_pdv`
  - `calories`

### Categorical Features:
- `tags`: Descriptive labels for the recipe (e.g., "dessert," "healthy").

### Rationale for Feature Selection:
- These features are known **before the prediction** because they are inherent properties of the recipe or its description. For example:
  - We can calculate `n_steps` and `num_tags` directly from the recipe instructions.
  - Nutritional information is available before the recipe is prepared or reviewed.

### What We Exclude:
- Features like user reviews or user behavior after the recipe is published, as they are not known at the time of prediction.

---

## Justification of "Time of Prediction"
The selected features represent attributes available when a recipe is created or described, ensuring no information leakage. This approach ensures that the model mimics real-world scenarios where we predict ratings before a recipe gains feedback or reviews.

---

## Classification or Regression
This is a **regression problem** because the target variable (`rating_average`) is continuous. If the task were to classify recipes into categories like "Highly Rated" or "Poorly Rated," it would become a **binary classification problem**.

## Results
# Step 4: Baseline Model

## Baseline Model Description

### **Features Used**
For our baseline model, we selected **two features** from the dataset:
1. **`n_steps`**: A quantitative feature representing the number of steps in the recipe.
2. **`tags`**: A nominal (categorical) feature representing the descriptive labels for each recipe (e.g., "healthy," "dessert").

### **Feature Encodings**
- **Numerical Features (`n_steps`)**: Left as-is since it is already numeric.
- **Categorical Features (`tags`)**: One-hot encoding was applied to transform the nominal data into binary columns, where each unique tag is represented as a separate column.

---

### **Model Used**
- **Model Type**: Linear Regression
- **Pipeline**: All preprocessing (one-hot encoding for `tags`) and model training steps were implemented in a single `sklearn.Pipeline` to ensure streamlined and consistent handling of data during training and evaluation.

---

## Generalization to Unseen Data
To evaluate the model's ability to generalize, we split the data into:
- **Training Set**: 60% of the data
- **Test Set**: 40% of the data

The model was trained on the training set and evaluated on the test set.

---

## Baseline Model Performance

### **Evaluation Metric**
We used **Root Mean Squared Error (RMSE)** to measure performance:
- **Reason for Choosing RMSE**: It penalizes large prediction errors more than Mean Absolute Error (MAE), which is crucial for understanding deviations in predicted ratings.

### **Baseline Model Results**
- **Training RMSE**: `0.74`
- **Test RMSE**: `0.78`

---

## Is the Baseline Model "Good"?

### **Performance Analysis**
The baseline model provides a relatively simple starting point. However:
1. **Pros**:
   - The model captures some relationship between the number of steps and tags with the recipe ratings.
   - The preprocessing pipeline ensures reproducibility and consistency.
2. **Cons**:
   - The RMSE value of `0.78` suggests the model's predictions are off by an average of 0.78 rating points, which is moderate given the target range (0–5).
   - The categorical encoding of `tags` may not fully capture the importance or relationships between different tags.

---

## Improvements Planned for Final Model
To improve the model in the next step, we plan to:
1. **Incorporate Additional Features**: Add more quantitative and nominal features, such as `review_length`, `minutes`, and nutritional content.
2. **Optimize the Model**: Explore more advanced models like Random Forest or Gradient Boosting, which can handle feature interactions more effectively.
3. **Feature Engineering**: Create new features or interactions (e.g., `steps_per_minute` or `tags_weighted`) to enhance predictive power.

---

## Summary
While the baseline model is a good starting point and demonstrates basic predictive power, its RMSE indicates room for improvement. The insights gained here will inform our approach in the final model.


# Step 5: Final Model

## Features Added and Their Relevance

For the final model, we added the following features to the baseline set:

1. **`review_length`**: This feature captures the length of user reviews. Recipes with longer reviews may indicate more user engagement, which can correlate with higher or lower ratings depending on the sentiment or quality of the recipe.
2. **`minutes`**: The total time required to prepare the recipe. Recipes with extreme preparation times (too short or too long) might have more polarizing ratings.
3. **Nutritional Features**:
   - **`total_fat_pdv`**: The percentage of the recommended daily value of fat.
   - **`sugar_pdv`**: The percentage of the recommended daily value of sugar.
   - **`protein_pdv`**: The percentage of the recommended daily value of protein.
   - **`calories`**: The total caloric value of the recipe.
   
### Why These Features Are Relevant
- **Behavioral Insights**: `review_length` and `minutes` are behavioral indicators of how users perceive and interact with recipes.
- **Health Metrics**: Nutritional features are central to users' decision-making, as healthier recipes might attract higher ratings in certain categories (e.g., "healthy" or "low-calorie").
- These features provide additional predictive power by capturing different aspects of the recipe that influence user satisfaction, thereby improving the model's ability to generalize to unseen data.

---

## Modeling Algorithm and Hyperparameter Tuning

### **Model Chosen**
The final model is a **Random Forest Regressor**, which was chosen for the following reasons:
1. **Handling Nonlinear Interactions**: Random Forests can automatically model complex interactions between features without requiring explicit feature engineering.
2. **Feature Importance**: The model provides insights into which features are most important for predicting ratings.
3. **Robustness**: It handles both numerical and categorical data well and is less prone to overfitting than other tree-based models.

### **Hyperparameters**
The following hyperparameters were selected using a **Randomized Search Cross-Validation** approach with 5 folds:
- **`n_estimators`**: 200 (number of trees in the forest)
- **`max_depth`**: 20 (maximum depth of each tree)
- **`min_samples_split`**: 5 (minimum number of samples required to split an internal node)
- **`min_samples_leaf`**: 4 (minimum number of samples required to be a leaf node)

### **Hyperparameter Tuning Method**
- **Randomized Search CV**:
  - We used a randomized search over a grid of hyperparameters to identify the best combination.
  - This approach is faster and computationally less expensive than a full grid search.
- **Evaluation Metric**:
  - RMSE was used during cross-validation to measure model performance on validation folds.

---

## Final Model Performance vs. Baseline Model

| Metric              | Baseline Model | Final Model |
|---------------------|----------------|-------------|
| **Training RMSE**   | 0.74           | 0.61        |
| **Test RMSE**       | 0.78           | 0.65        |

### **Performance Improvement**
- The final model reduced the **Test RMSE** from `0.78` to `0.65`, representing a significant improvement in predictive accuracy.
- The added features and Random Forest's ability to capture nonlinear relationships improved the model's generalizability to unseen data.

---

## Visualization of Model Performance

To better understand the performance of the final model, we include a residual plot to visualize prediction errors:

```python
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.metrics import mean_squared_error

# Calculate residuals
y_pred = final_model.predict(X_test)
residuals = y_test - y_pred

# Plot residuals
plt.figure(figsize=(10, 6))
sns.scatterplot(x=y_pred, y=residuals, alpha=0.6)
plt.axhline(0, color='red', linestyle='--', linewidth=1)
plt.title('Residuals Plot: Final Model', fontsize=16)
plt.xlabel('Predicted Ratings', fontsize=12)
plt.ylabel('Residuals (True - Predicted)', fontsize=12)
plt.grid(True, linestyle='--', alpha=0.6)
plt.tight_layout()
plt.show()
## About
Author: xiulin Chen, hanlu Wu
Email: xiulin@umich.edu , hanluwu@umich.edu

![About Image](/images/03.jpg)

