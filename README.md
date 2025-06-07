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
### Cleaning and Feature Engineering

I began with the `RAW_recipes` dataset, which contained 83,782 recipes and 12 columns. Several of these columns contained improperly formatted or missing data. Below are the major preprocessing and cleaning tasks I performed:

**1. Parsing the `nutrition` Column**

Each entry in the `nutrition` column is a string representation of a list of seven values: calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates. I used `ast.literal_eval` to convert this string into a Python list. From this list, I extracted the following:

- `calories` : total caloric content
- `sugar` : total grams of sugar
- `protein` : total grams of protein
- `sat_fat` : total grams of saturated fat

These features were selected to help construct a simplified health metric later in the project.

**2. Parsing the `steps` and `ingredients` Columns**

These columns also stored list-like data as strings. I converted them into actual Python lists and used them to derive the following features:

- `n_steps` : the number of steps in the cooking procedure
- `n_ingredients` : the number of ingredients in each recipe
- `ingredient_length` : the character length of the original ingredients string (used to approximate complexity)

**3. Handling the `minutes` Column**

I found that cooking durations ( `minutes` ) were heavily right-skewed. To address this, I logtransformed the values using `log_minutes = log(minutes + 1)` to reduce the effect of extreme outliers and improve interpretability.

### Finalizing the Cleaned Dataset
After creating the new features, I removed rows with malformed or missing values in critical columns like `nutrition`, `ingredients`, or `steps`. This left a clean dataset, `recipes_cleaned`, which I used for all subsequent steps. The table below shows the first few rows of `recipes_cleaned` with some relevant columns selected.

 
| name                                 | log_minutes | n_ingredients | ingredient_length | calories | avg_rating | has_description |
|--------------------------------------|-------------|----------------|-------------------|----------|-------------|-----------------|
| 1 brownies in the world best ever    | 3.71357     | 9              | 138               | 138.4    | 4           | True            |
| 1 in canada chocolate chip cookies   | 3.82864     | 11             | 111               | 595.1    | 5           | True            |
| 412 broccoli casserole               | 3.71357     | 9              | 129               | 194.8    | 5           | True            |
| millionaire pound cake               | 4.79579     | 7              | 76                | 878.3    | 5           | True            |
| 2000 meatloaf                        | 4.51086     | 13             | 179               | 267      | 5           | True            |


### Univariate Analysis

<iframe
  src="assets/log-recipe-duration-dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This histogram shows the distribution of `log_minutes`. Most recipes fall between log-minutes 2 and 6 (roughly 7 to 400 minutes), indicating a wide but interpretable range of durations.


<iframe
  src="assets/ingredients-per-recipe.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot illustrates the distribution of `n_ingredients`. The majority of recipes use between 5 and 15 ingredients, with a peak around 10, suggesting most users prefer moderately complex recipes.

### Bivariate Analysis

<iframe
  src="assets/rating-vs-ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here, I plotted the average rating by number of ingredients. Ratings increase steadily up to about 14 ingredients before leveling off. This suggests users appreciate recipes that are more involved—but only up to a point.

These insights helped shape the directions of my hypothesis and aggregate analyses.

### Interesting Aggregates

I explored the relationship between cooking time and sugar content by grouping recipes into cook time bins and computing the proportion of sugar in each group. Specifically, I calculated the mean, median, minimum, and maximum proportion of sugar in calories within each time bin.

| cook_time_bin | mean     | median   | min | max     |
|---------------|----------|----------|-----|---------|
| 0–10          | 0.268152 | 0.172331 | 0   | 1.07898 |
| 11–30         | 0.156582 | 0.069091 | 0   | 1.07995 |
| 31–60         | 0.151015 | 0.073364 | 0   | 1.03926 |
| 61–120        | 0.168158 | 0.088607 | 0   | 1.03097 |
| 120+          | 0.159572 | 0.074746 | 0   | 1.08049 |

These statistics suggest that the shortest recipes (0–10 minutes) tend to have a higher average and median sugar content, possibly because quick recipes include items like smoothies, desserts, or sweetened beverages. Longer recipes show a lower proportion of sugar, which may correspond to savory main dishes or multi-component meals.
 
<iframe
  src="assets/prop-sugar-by-cook-time-bin.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This visualization clearly shows that recipes in the 0–10 minute bin have the highest average proportion of sugar, while other bins remain relatively low and flat. This supports the idea that quick recipes often contain high sugar content.

These findings are helpful not only for dietary analysis but also for feature selection in downstream modeling.

## Assessment of Missingness

### NMAR Analysis

One column in the dataset with substantial missing values is the description column, which contains short blurbs that introduce or summarize the recipe. I wanted to determine if the missingness of this column followed a particular pattern.

On Food.com, a description is an optional field when users upload recipes. This makes it likely that the choice to not enter a description is related to unobserved user characteristics like laziness or inexperience. Since these traits are not included as variables in the dataset, I can't use the observed data to fully explain the missingness.

Because of this, I believe the description column is **Not Missing At Random (NMAR)**. That is, the probability of a description being missing is likely related to something unmeasured, such as the user's writing habits or willingness to promote their recipe, and not just to any of the recipe’s measurable features.

### Missingness Dependency

Although I concluded the missingness of the description column is likely NMAR, I also tested how its missingness correlated with other columns.

To do this, I created a Boolean column `description_missing`, which was `True` if the description was missing and `False` otherwise. I then explored how this column interacted with other variables.

**Grouped Analysis**

I grouped by `description_missing` and computed the average number of ingredients and average calories.

- Recipes with missing descriptions tended to have **fewer ingredients**.

- Recipes with missing descriptions also had **lower calorie counts**.

This suggested that simpler, less caloric recipes were more likely to not have a description.

**Permutation Test**

To assess whether these differences were statistically significant, I ran permutation tests.

<iframe
  src="assets/description-ingredient-missingness.html?v=2"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I tested whether the number of ingredients differed significantly between recipes with and without missing descriptions. The observed difference (about -1.4 ingredients) was extreme relative to the null distribution, giving a low p-value.
 
<iframe
  src="assets/ingredients-by-missingness-bar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
 
<iframe
  src="assets/description-calorie-missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

A similar test was run for calorie count. Again, the observed difference lay in the tail of the null distribution.
 
<iframe
  src="assets/calories-by-missingness-bar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Conclusion**: These results confirm that missingness in the description column is related to observed features like calories and ingredient count. This suggests that while the data may be NMAR in theory, it behaves like **MAR (Missing At Random)** in practice.




## Hypothesis Testing
 
<iframe
  src="assets/healthiness-rating-permutation.html?v=2"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis

