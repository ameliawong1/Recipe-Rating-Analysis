# Recipe Ratings and Health: Does Nutrition Affect How We Rate?

By Amelia Wong

## Introduction

We all have different ideas about what makes a recipe great—taste, convenience, healthiness, or a mix of all three. But when people leave reviews online, what factors most influence those ratings? For my DSC 80 final project at UC San Diego, I explored whether the **nutritional content of a recipe, like its protein, sugar, or calories, influences how well it's rated by users**. I used two datasets from [Food.com](https://food.com/) containing thousands of recipes and user ratings.

To answer my exploration question, I began by cleaning the data and engineering new features such as the proportion of sugar per calorie and an indicator for whether a recipe’s nutritional values exceed the average. I then conducted exploratory analysis, statistical testing, and even trained a model to predict whether a recipe would receive a high rating based solely on its nutritional profile and other features like prep time and ingredient length.

By understanding whether nutrition impacts online ratings, we can gain insights into public health trends, taste preferences, and what kinds of recipes might be worth promoting more heavily on recipe platforms like Food.com.


The following chart explains each of the 12 columns in the `RAW_recipes` dataset, which includes 83,782 rows that correspond to 83,782 recipes.

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |


The following chart explains each of the 5 columns in the `interactions` dataset, which includes 731,927 rows that correspond to 731,927 reviews left on recipes.

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

## Data Cleaning and Exploratory Data Analysis


## Assessment of Missingness


## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis

