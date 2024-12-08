# Recipe Rating Analysis

## Introduction

### Dataset Overview

This project analyzes two datasets: a recipe dataset and an interaction dataset, containing information about recipes, user reviews, and ratings. The combined dataset offers rich insights into user preferences, cooking behaviors, and recipe characteristics. Specifically, the dataset contains:

- **Recipe Dataset**: Includes information about the recipes, such as cooking time, tags, and nutritional values.
- **Interaction Dataset**: Includes user ratings and reviews for various recipes.

The recipe dataset contains **83,782 rows**, and the interaction dataset contains **731,927 rows**. These datasets are merged on the `recipe_id` column to create a unified dataset for analysis.

### Research Question

**What factors influence a recipe's average rating, and can we predict it using its characteristics (e.g., cooking time, nutritional values, and tags)?**

Understanding the factors that drive user satisfaction with recipes can help chefs and content creators design recipes that better match user preferences. It also provides insight into how various features of a recipe (like preparation time or healthiness) impact its success.

### Relevant Columns

- **`recipe_id`**: The unique identifier for the recipe being analyzed.
- **`tags`**: A list of tags that describe the recipe, such as cuisine type, dietary restrictions, or cooking methods.
- **`rating_average`**: The average user rating for the recipe, calculated from the interaction dataset.
- **`review`**: Textual feedback from users describing their experience with the recipe.
- **`minutes`**: The total time required to prepare and cook the recipe, measured in minutes.
- **`n_steps`**: The number of steps required to prepare the recipe.
- **`calories`**: The total calorie count for the recipe, extracted from its nutritional data.
- **`total_fat_pdv`**: The percentage of the daily value of total fat provided by the recipe.
- **`sugar_pdv`**: The percentage of the daily value of sugar provided by the recipe.
- **`protein_pdv`**: The percentage of the daily value of protein provided by the recipe.

---

## Data Cleaning

### Overview of Cleaning Steps

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

---

## Prediction Problem

### Problem Type

Our prediction problem is to forecast the **average rating** of a recipe based on its features, such as the number of steps, review length, tags, and nutritional content. This is a **regression problem** because the response variable, `rating_average`, is continuous and ranges between 0 and 5.

---

### Response Variable

The response variable is `rating_average`:
- **Why we chose it:** Ratings are critical to understanding the popularity and quality of a recipe. By predicting the rating, we can provide insights into what makes a recipe well-received and help users or chefs improve their recipes.

---

### Evaluation Metric

The primary evaluation metric for this regression problem is **Root Mean Squared Error (RMSE)**:
- **Why RMSE:** RMSE penalizes large errors more than metrics like Mean Absolute Error (MAE), which is suitable since large prediction errors (e.g., a predicted rating of 4 when the true rating is 2) are particularly problematic in this context.
- RMSE provides an intuitive measure in the same unit as the target variable (`rating_average`), making it easy to interpret.

---

### Features Used

**Numerical Features**:
- `minutes`
- `n_steps`
- `num_tags`
- `review_length`
- Nutritional values: `total_fat_pdv`, `sugar_pdv`, `protein_pdv`, `calories`

**Categorical Features**:
- `tags`

**Rationale for Feature Selection**:
- These features are inherent properties of the recipe and are available **before prediction**.

---

## Results

### Step 4: Baseline Model

#### Features Used

- **`n_steps`** (quantitative)
- **`tags`** (nominal, one-hot encoded)

#### Baseline Model Results

- **Training RMSE**: `0.74`
- **Test RMSE**: `0.78`

---

### Step 5: Final Model

#### Features Added

- **`review_length`**
- **`minutes`**
- Nutritional features: `total_fat_pdv`, `sugar_pdv`, `protein_pdv`, `calories`

#### Final Model Results

| Metric              | Baseline Model | Final Model |
|---------------------|----------------|-------------|
| **Training RMSE**   | 0.74           | 0.61        |
| **Test RMSE**       | 0.78           | 0.65        |

---

## Visualization

![Residuals Plot](/images/03.jpg)

---

## About

**Authors**:  
- Xiulin Chen (xiulin@umich.edu)  
- Hanlu Wu (hanluwu@umich.edu)
