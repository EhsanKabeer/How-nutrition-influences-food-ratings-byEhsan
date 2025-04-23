# How does Nutrition influences Food ratings?
---

## Introduction

Food.com is a website which allows chefs share their tasty recipes with the world and internet users globally can give their comments on the recipe and a rating from 1-5 to give a unbiased view on the published recipes and answer whether these recipes are worth cooking up or not. Food.com has published a publically accessible database which has metrics like ingredients, nutritional facts, a description of the recipe, and much more. My focus with this project is to predict what a user will rate a particular recipe based on the recipes nutritional data. I chose nutritional facts to predict the ratings because I was curious what type of foods people like making for example do they like making foods like brownies ro cakes which are generally unhealthy, or foods that are high in protein and low in sugar as a healthy option.

by Ehsan Kabeer (eskabeer@umich.edu)

---

## Data Cleaning and Exploratory Data Analysis

### Cleaning

Before I got into any predictive model bulidng I had to clean up the dataset. The dataset was initially was split into two tables, one with the actual recipes and their respective details and another with the users ratings and their thoughts on the recipes. What I did was merge the two tables with the left merge, using the recipe ids. After this I dropped the null rows from table and filled the ratings in the table with a value of 0 as null (since scale is 1-5) and after this I did a mean imputation for the null rating by their respective recipes, which would allow me to keep the general distribution of the dataset while not losing any data from table (some recipes only had 0 ratings, so they were dropped completely from data after mean imputation).

After I did this I wanted to get rid of columns from the table that weren't relevant for the model building process. This resulted in the removal of columns: recipe_id, user_id, date, submitted, minutes, tags, n_steps, steps, description, ingredients, review, contributor_id, and n_ingredients.

After this I needed to split the nutrition facts into seperate columns so I can use them. Initially they were in a list format; this included information like calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)] (PDV is percentage of daily value). I split them each into individual columns using regex and after that I dropped the nutrition column since I didn't need it.

For visualization purposes I made new columns for each nutrition fact that split it into 3 bins, for example for the Protein(PDV) I split into low protein, moderate protein, and high protein with low being less than 5%, moderate being less than 19% and high being above that; this will allow the visualization I am going to be showing to be more clean.

| name                                 |     id |   rating |   calories |   total_fat(PDV) |   sugar(PDV) |   sodium(PDV) |   protein(PDV) |   saturated_fat(PDV) |   carbohydrates(PDV) |   rating_avg | fatClass     | satFatClass   | sugarClass   | sodiumClass   | proteinClass     | carbClass     | calClass     |
|:-------------------------------------|-------:|---------:|-----------:|-----------------:|-------------:|--------------:|---------------:|---------------------:|---------------------:|-------------:|:-------------|:--------------|:-------------|:--------------|:-----------------|:--------------|:-------------|
| 1 brownies in the world    best ever | 333281 |        4 |      138.4 |               10 |           50 |             3 |              3 |                   19 |                    6 |            4 | Moderate_fat | Unhealthy     | High_sugar   | low_sodium    | low_protein      | Moderate_carb | Moderate_cal |
| 1 in canada chocolate chip cookies   | 453467 |        5 |      595.1 |               46 |          211 |            22 |             13 |                   51 |                   26 |            5 | High_fat     | Unhealthy     | High_sugar   | high_sodium   | Moderate_protein | High_carb     | High_cal     |
| 412 broccoli casserole               | 306168 |        5 |      194.8 |               20 |            6 |            32 |             22 |                   36 |                    3 |            5 | High_fat     | Unhealthy     | low_sugar    | high_sodium   | High_protein     | Low_carb      | Moderate_cal |
| 412 broccoli casserole               | 306168 |        5 |      194.8 |               20 |            6 |            32 |             22 |                   36 |                    3 |            5 | High_fat     | Unhealthy     | low_sugar    | high_sodium   | High_protein     | Low_carb      | Moderate_cal |
| 412 broccoli casserole               | 306168 |        5 |      194.8 |               20 |            6 |            32 |             22 |                   36 |                    3 |            5 | High_fat     | Unhealthy     | low_sugar    | high_sodium   | High_protein     | Low_carb      | Moderate_cal |

### Univariate Analysis

 <iframe
 src="assets/Rating_distribution_by_fat.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

