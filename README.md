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

### Data Cleaning: Preparing the Ingredients


### Overview of Cleaning Steps

To prepare the dataset for analysis, the following data cleaning steps were performed:

1. **Merging Datasets**:
   - Combined two datasets using "recipe ID" as the key.
   - Calculated the average rating for each recipe to create a new `rating_average` feature.

2. **Handling Missing Values**:
   - Replaced zero values in the `rating` column with `NaN` to indicate missing ratings.
   - Removed rows with missing values in critical columns, such as `review`, `tags`, and `calories`, to ensure data completeness for analysis.

3. **Data Type Transformation**:
   - Converted the `nutrition` column (originally a string) into a Python list to allow easy access to individual nutritional components (e.g., calories, fat).
   - Transformed the `tags` column from a string format into a list of tags for better usability.

4. **Feature Engineering**:
   - Created additional features to enhance the dataset:
     - `review_length`: Measured the word count of each review.
     - `num_tags`: Counted the number of tags associated with each recipe.

5. **Splitting Nutritional Data**:
   - Extracted specific nutrients (e.g., calories, protein, fat, sugar) into separate columns to analyze their influence on recipe ratings.


## Visualization

Univariate Analysis: Individual Recipe Insights
<iframe 
    src="image1.html" 
    width="1000" 
    height="600" 
    style="border: none;">
</iframe>

The most common tags, such as "preparation," "time-to-make," and "course," are general descriptors commonly used to categorize recipes. These tags likely reflect structural organization rather than specific culinary attributes.
Tags like "main-ingredient" and "dietary" are prominent, indicating that recipes are often classified based on their primary components or dietary considerations. This aligns with the importance of catering to specific dietary needs (e.g., vegetarian, gluten-free) in recipe datasets.
The tags "easy" and "occasion" highlight that simplicity and context (e.g., festive or special events) are important aspects of recipe selection. These attributes may reflect users' preferences for quick and contextually appropriate dishes.

Bivariate Analysis: Relationships Between Recipe Attributes


<iframe 
    src="image2.html" 
    width="1000" 
    height="600" 
    style="border: none;">
</iframe>

The majority of recipes have high ratings (4-5), indicating they are generally well-received.
Low ratings (1-2) are rare, suggesting either fewer poor-quality recipes or under-reporting of negative experiences.
The distribution is positively skewed, with a strong bias toward higher ratings.
Variability is observed in the mid-range (3-4 ratings), indicating mixed feedback for average recipes.

<iframe 
    src="img3.html" 
    width="1000" 
    height="600" 
    style="border: none;">
</iframe>

Across all rating categories, the median preparation time (log-transformed) is relatively consistent, suggesting that preparation time might not strongly influence a recipe's rating.
Lower-rated recipes (categories 0-1 and 1-2) exhibit slightly more variability in preparation time, with some recipes requiring notably longer times.
Higher-rated recipes (categories 4-5) show a tighter distribution, implying that recipes with more predictable preparation times tend to receive better ratings.
Recipes that are too time-consuming may deter users, especially for lower-rated recipes.
Focusing on optimizing preparation time for low-rated recipes could improve their reception.


Specifically , here are what our cleaned data look like
among those, relevant data only select the relevant cols in this analysis, and merged_data include all cols we have cleaned up.v
```py
relevant_data_head = relevant_data.head().to_markdown(index=False)
merged_data_head = merged_data.head().to_markdown(index=False)
```

Below is the head of the cleaned relevant data:



# Relevant Data Sample

Below is a sample of the merged dataset:

<div style="max-width: 800px; overflow-x: auto;">

|   recipe_id | tags                                  |   rating_average | review                                    |   minutes |   n_steps |   calories |   total_fat_pdv |   sugar_pdv |   protein_pdv |   num_tags |   review_length |
|------------:|:-------------------------------------|-----------------:|:-----------------------------------------|----------:|----------:|-----------:|----------------:|------------:|--------------:|-----------:|----------------:|
|      333281 | ['60-minutes-or-less', 'time-to...'] |                4 | These were pretty good, but took...      |        40 |        10 |      138.4 |              10 |          50 |             3 |         14 |              50 |
|      453467 | ['60-minutes-or-less', 'cuisine...'] |                5 | Originally I was gonna cut the rec...    |        45 |        12 |      595.1 |              46 |         211 |            13 |          9 |              65 |

</div>




# Merged Data Sample

Below is a sample of the merged dataset:


<div style="max-width: 800px; overflow-x: auto;">
| name                                  | recipe_id | minutes | contributor_id | submitted   | tags                                                                                             | nutrition                   | n_steps | steps                                           | description                        | ingredients                              | n_ingredients | user_id  | date       | rating | review                                     | rating_average | calories | total_fat_pdv | sugar_pdv | protein_pdv |
|:--------------------------------------|----------:|--------:|---------------:|:------------|:------------------------------------------------------------------------------------------------|:----------------------------|--------:|:------------------------------------------------|:-----------------------------------|:-----------------------------------------|--------------:|---------:|:-----------|-------:|:------------------------------------------|---------------:|---------:|-------------:|----------:|------------:|
| 1 brownies in the world best ever     |    333281 |      40 |         985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', ...]                                                    | [138.4, 10.0, 50.0, ...]    |      10 | ['heat the oven...', 'line...', ...]          | These are the most chocolatey...   | ['bittersweet chocolate', 'unsalted ... |             9 |   386585 | 2008-11-19 |      4 | These were pretty good, but...             |              4 |    138.4 |           10  |       50 |           3 |
| 1 in canada chocolate chip cookies    |    453467 |      45 |        1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', ...]                                                    | [595.1, 46.0, 211.0, ...]   |      12 | ['pre-heat oven...', 'in a mixing...', ...]  | This is the recipe we use...       | ['white sugar', 'brown sugar', ...]      |            11 |   424680 | 2012-01-26 |      5 | Originally I was gonna...                  |              5 |    595.1 |           46  |      211 |          13 |
</div>
                                                                                                                                                                                                                                                    

### Cleaned Data display

## Relevant Data Head

Below is the head of the cleaned relevant data:




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

### choose the right model

# Model Selection and Evaluation

## Objective
The goal was to identify the best model for predicting the average rating of recipes (`rating_average`) using the cleaned and preprocessed dataset. To achieve this, we tested a variety of regression models and evaluated their performance using cross-validation and test set evaluation.

---

## Models Tested
We compared the following models to determine the best-performing one:
1. **Random Forest Regressor**: A robust ensemble-based model that reduces overfitting using bagging and feature randomness.
2. **Gradient Boosting Regressor**: An ensemble technique that builds models sequentially to correct errors of prior models.
3. **Support Vector Regressor (SVR)**: A kernel-based regression technique designed for smaller datasets with high complexity.
4. **Ridge Regression**: A linear regression model with L2 regularization to prevent overfitting.
5. **Lasso Regression**: A linear regression model with L1 regularization for feature selection.
6. **Linear Regression**: A simple regression model without regularization.

---

## Feature Selection
The following features were used as predictors:
- **Numerical Features**:
  - `minutes`: Time required to prepare the recipe.
  - `n_steps`: Number of steps in the recipe.
  - `num_tags`: Number of tags associated with the recipe.
  - `review_length`: Length of user reviews (in words).
  - Nutritional details: `calories`, `total_fat_pdv`, `sugar_pdv`, `protein_pdv`.
- **Categorical Features**:
  - `tags`: A list of tags describing the recipe.

Numerical features were standardized using `StandardScaler`.

---

## Methodology
1. **Data Sampling**:
   - To speed up computation, we subsampled **20% of the dataset** while retaining the overall distribution.

2. **Data Splitting**:
   - The data was split into **80% training** and **20% test** sets using `train_test_split`.

3. **Cross-Validation**:
   - Each model was evaluated using **3-fold cross-validation** on the training set. 
   - Cross-validation scores were calculated using **Root Mean Squared Error (RMSE)**, which penalizes larger errors more heavily than Mean Absolute Error (MAE).
   - For each model, we computed the **mean RMSE** and the **standard deviation** of RMSE across the folds.

4. **Model Selection**:
   - The model with the **lowest mean RMSE** across the cross-validation folds was selected as the best model.

---

## Results of Cross-Validation
| Model                 | Mean RMSE ± Std Dev |
|------------------------|---------------------|
| Random Forest          | 0.71 ± 0.01        |
| Gradient Boosting      | 0.72 ± 0.02        |
| Support Vector Regressor (SVR) | 0.74 ± 0.02 |
| Ridge Regression       | 0.72 ± 0.02        |
| Lasso Regression       | 0.72 ± 0.02        |
| Linear Regression      | 0.72 ± 0.02        |

The **best-performing model** was:
- **Random Forest**
- **Mean RMSE**: 0.71
- **Standard Deviation of RMSE**: 0.01



The ** Random Forest ** was selected as the best model for predicting recipe ratings based on its superior performance in cross-validation and test set evaluation. The low RMSE and high R² score indicate that the model captures the variability in the data effectively. This model can now be used for further predictions or integrated into a recommendation system.



# Baseline Model: A Foundation to Build On

## Objective
The baseline model aims to establish a reference performance using the cleaned dataset. This step serves as a foundation for comparing and improving upon the model in subsequent iterations.

---

## Features Used
For simplicity and interpretability, only the following **numerical features** were used:
- **`minutes`**: Time required to prepare the recipe.
- **`n_steps`**: Number of steps in the recipe.
- **`num_tags`**: Number of tags associated with the recipe.
- **`review_length`**: Length of user reviews (in words).

These features were standardized using `StandardScaler`.

---

## Methodology

1. **Data Splitting**:
   - The dataset was divided into **60% training data** and **40% test data** using `train_test_split`.

2. **Subsampling**:
   - To reduce computation time, **20% of the training data** was randomly sampled to train the model. This allows the model to learn efficiently while retaining representativeness.

3. **Model Selection**:
   - A **RandomForest Regressor** was used as the baseline model due to its robustness and ability to handle feature interactions automatically.

4. **Pipeline Construction**:
   - A `Pipeline` was created to combine the preprocessing steps (scaling numerical features) with the model. This ensures consistent preprocessing during training and testing.

5. **Evaluation Metric**:
   - The model's performance was evaluated using **Root Mean Squared Error (RMSE)**, which penalizes larger errors more heavily, making it a suitable metric for regression tasks.

---

## Results

- **Baseline RMSE (RandomForest Regression)**: `0.74`

---


The baseline model provides a benchmark RMSE of `0.74`. This indicates that the model has reasonable predictive capability but leaves room for optimization and improvement. Future steps will explore additional features, fine-tune hyperparameters, and test more advanced models to improve performance.


# Final Model: Elevating Predictions

## Objective
The final model incorporates additional features and hyperparameter tuning to improve performance over the baseline model. The goal is to minimize the prediction error (measured by RMSE) on the test dataset.

---

## Features Added
To enhance the predictive power of the model, the following features were included:
- **`review_length`**: The number of words in the review, reflecting user engagement.
- **`minutes`**: Recipe preparation time.
- **Nutritional Features**:
  - **`calories`**: The calorie content of the recipe.
  - **`total_fat_pdv`**: Percentage daily value of fat.
  - **`sugar_pdv`**: Percentage daily value of sugar.
  - **`protein_pdv`**: Percentage daily value of protein.
- **`tags`**: Recipe tags, filtered to retain the top 10 most frequent tags and group others under "Other".

---

## Methodology

### 1. **Data Preprocessing**
- **Numerical Features**:
  - Standardized using `StandardScaler` to normalize their ranges.
- **Categorical Features**:
  - Encoded using `OneHotEncoder` for the top 10 tags, while infrequent tags were replaced with "Other".

### 2. **Hyperparameter Tuning**
- The model was fine-tuned using **RandomizedSearchCV** on the following hyperparameters:
  - Number of estimators (`n_estimators`): [50, 100, 200]
  - Maximum tree depth (`max_depth`): [10, 20, 30]
  - Minimum samples for a split (`min_samples_split`): [2, 5, 10]
  - Minimum samples per leaf (`min_samples_leaf`): [1, 2, 4]
- A 3-fold cross-validation was used to select the best parameters based on negative mean squared error (MSE).

### 3. **Model Training**
- The best model identified through hyperparameter tuning was a **RandomForest Regressor**.
- The final model was trained on the full training dataset and evaluated on the test dataset.

---

## Results

| Metric              | Baseline Model | Final Model |
|---------------------|----------------|-------------|
| **RMSE**            | 0.74          | 0.70        |

- **Best Hyperparameters**:
  - `n_estimators`: 200
  - `max_depth`: 20
  - `min_samples_split`: 5
  - `min_samples_leaf`: 2

---

## Insights and Improvements
- The **final model** achieved a **4-point improvement in RMSE** over the baseline model, reducing the error to 0.70. This demonstrates the importance of including additional relevant features and fine-tuning hyperparameters.
- The inclusion of nutritional features (e.g., calories, protein) and categorical tags significantly contributed to the improved performance, as these features provide valuable context for predicting recipe ratings.

---

## Conclusion
The final model, a **RandomForest Regressor** with optimized hyperparameters, demonstrates robust performance in predicting recipe ratings. Future improvements could explore:
- Incorporating NLP features from review text.
- Leveraging deeper feature engineering for categorical variables.
- Testing additional ensemble models like Gradient Boosting or XGBoost.




---

## About

**Authors**:  
- Xiulin Chen (xiulin@umich.edu)  
- Hanlu Wu (hanluwu@umich.edu)
