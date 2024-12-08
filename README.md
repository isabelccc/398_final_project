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
- Problem type: Classification/Regression
- Target variable: [Your target column]
- Features used in the model

## Results
- Baseline model performance
- Final model improvements

## About
Author: xiulin Chen, hanlu Wu
Email: xiulin@umich.edu , hanluwu@umich.edu

![About Image](/images/03.jpg)

