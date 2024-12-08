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

## Visualization

Univariate Analysis
<iframe 
    src="image1.html" 
    width="1000" 
    height="600" 
    style="border: none;">
</iframe>

The most common tags, such as "preparation," "time-to-make," and "course," are general descriptors commonly used to categorize recipes. These tags likely reflect structural organization rather than specific culinary attributes.
Tags like "main-ingredient" and "dietary" are prominent, indicating that recipes are often classified based on their primary components or dietary considerations. This aligns with the importance of catering to specific dietary needs (e.g., vegetarian, gluten-free) in recipe datasets.
The tags "easy" and "occasion" highlight that simplicity and context (e.g., festive or special events) are important aspects of recipe selection. These attributes may reflect users' preferences for quick and contextually appropriate dishes.

Bivariate Analysis

<iframe 
    src="image2.html" 
    width="1000" 
    height="600" 
    style="border: none;">
</iframe>

<iframe 
    src="img3.html" 
    width="1000" 
    height="600" 
    style="border: none;">
</iframe>

Specifically , here are what our cleaned data look like
among those, relevant data only select the relevant cols in this analysis, and merged_data include all cols we have cleaned up.v
```py
relevant_data_head = relevant_data.head().to_markdown(index=False)
merged_data_head = merged_data.head().to_markdown(index=False)
```

Below is the head of the cleaned relevant data:



# Relevant Data Sample

Below is a sample of the merged dataset:

|   recipe_id | tags                                                                                                                                                                                                                        |   rating_average | review                                                                                                                                                                                                                                                                                                                                           |   minutes |   n_steps |   calories |   total_fat_pdv |   sugar_pdv |   protein_pdv |   num_tags |   review_length |
|------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|----------:|-----------:|----------------:|------------:|--------------:|-----------:|----------------:|
|      333281 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] |                4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |        40 |        10 |      138.4 |              10 |          50 |             3 |         14 |              50 |
|      453467 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               |                5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |        45 |        12 |      595.1 |              46 |         211 |            13 |          9 |              65 |



# Merged Data Sample

Below is a sample of the merged dataset:

| name                                 |   recipe_id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                        | nutrition                                    |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                          | ingredients                                                                                                                                                                    |   n_ingredients |   user_id | date       |   rating | review                                                                                                                                                                                                                                                                                                                                           |   rating_average |   calories |   total_fat_pdv |   sugar_pdv |   protein_pdv |
|:-------------------------------------|------------:|----------:|-----------------:|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|----------:|:-----------|---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------:|-----------:|----------------:|------------:|--------------:|
| 1 brownies in the world    best ever |      333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven! | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 |    386585 | 2008-11-19 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |                4 |      138.4 |              10 |          50 |             3 |
| 1 in canada chocolate chip cookies   |      453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                               | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 |    424680 | 2012-01-26 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |                5 |      595.1 |              46 |         211 |            13 |




# Create the Markdown content
markdown_content = f"""
# Data Cleaning

## Relevant Data Head

Below is the head of the cleaned relevant data:

```markdown
{relevant_data_head}



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
| **RMSE**            | 0.74          | 0.70        |


---



---

## About

**Authors**:  
- Xiulin Chen (xiulin@umich.edu)  
- Hanlu Wu (hanluwu@umich.edu)
