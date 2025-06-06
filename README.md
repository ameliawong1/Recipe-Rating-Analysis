# Recipe Ratings and Health: Does Nutrition Affect How We Rate?

By Amelia Wong

## Introduction

We all have different ideas about what makes a recipe great—taste, convenience, healthiness, or a mix of all three. But when people leave reviews online, what factors most influence those ratings? For my DSC 80 final project at UC San Diego, I explored whether the nutritional content of a recipe, like its protein, sugar, or calories, influences how well it's rated by users. I used two datasets from Food.com containing thousands of recipes and user ratings. My goal was to investigate the relationship between a recipe's healthiness and its average rating, and later, to build a model that could predict whether a recipe will be rated highly based on its nutritional profile.

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

