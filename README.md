# How does Nutrition influences Food ratings?
---

## Introduction

Food.com is a website which allows chefs share their tasty recipes with the world and internet users globally can give their comments on the recipe and a rating from 1-5 to give a unbiased view on the published recipes and answer whether these recipes are worth cooking up or not. Food.com has published a publically accessible database which has metrics like ingredients, nutritional facts, a description of the recipe, and much more. My focus with this project is to predict what a user will rate a particular recipe based on the recipes nutritional data. I chose nutritional facts to predict the ratings because I was curious what type of foods people like making for example do they like making foods like brownies ro cakes which are generally unhealthy, or foods that are high in protein and low in sugar as a healthy option.

by Ehsan Kabeer (eskabeer@umich.edu)

---

## Data Cleaning and Exploratory Data Analysis

### Cleaning

Before I got into any predictive model bulidng I had to clean up the dataset. The dataset was initially was split into two tables, one with the actual recipes and their respective details and another with the users ratings and their thoughts on the recipes. What I did was merge the two tables with the left merge, using the recipe ids. After this I dropped the null rows from table and filled the ratings in the table with a value of 0 as null (since scale is 1-5).

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

With this graph we are exploring the distribution of fat level in a recipe among the differnt ratings, the fat level was split into low fat, moderate fat, and high fat. With this visualization I wanted to see if consumers prefer a more low fat option for the recipes they are cooking up as opposed to a high fat ones. From the graph you can see that five star rated items tend to be on the lower fat side, and if you look closely at the 3 star distribution, high fat items slightly are above in percentage compared to lower fat options. So from this visualization there is slight trend pointing to the fact that low fat items are the preferred recipes.

<iframe
 src="assets/Calorie_trend_plot.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

With this graph we are exploring the distritbution of calorie level in a recipe among the different ratings, just like with fat this was split into low calorie, moderate calorie, and high calorie. With this graph I wanted to further investigate whether consumers prefer healthier or less healthy options. This graph again shows that the five star items are teh lower claorie items with low calorie being highest percentage and moderate being a close second. The lower ratings like four and three star both had high calorie as top, which further soldifies that trend that healthier options tend to be more highly rated.

### Bivariate Analysis

<iframe
 src="assets/Sodium.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

For this graph I took a simple random sample from data of 20,000 so the graph is more easily interpretable. For this graph I am how the sodium(PDV) infuences the average ratings of the recipes. I wanted to see if consumers preferred salty snacks when considering what to make. From the graph we can see that as salt content gest higher rating seem to get higher, which I was suprised by since in the other graphs it was seen that consumers preferred more healthy recipes.

### Pivot table

satFatClass represents the classification of the saturated fat content for a particular recipe. With the categories being unhealthy, caution, and heart-healthy. These designations were made since saturated fats are the actual unhealthy fats in food.

| satFatClass   |   rating_avg |   calories |
|:--------------|-------------:|-----------:|
| Caution       |            1 |    246.852 |
| Caution       |            2 |    284.562 |
| Caution       |            3 |    244.674 |
| Caution       |            4 |    262.265 |
| Caution       |            5 |    238.242 |
| Heart_healthy |            1 |    160.828 |
| Heart_healthy |            2 |    185.624 |
| Heart_healthy |            3 |    196.456 |
| Heart_healthy |            4 |    181.42  |
| Heart_healthy |            5 |    173.945 |
| Unhealthy     |            1 |    660.587 |
| Unhealthy     |            2 |    619.091 |
| Unhealthy     |            3 |    620.657 |
| Unhealthy     |            4 |    574.03  |
| Unhealthy     |            5 |    572.7   |

This grouped table is grouped on the saturated fat class and average rating, and we are getting the mean calories for each group. What I wanted to explore here is to see the behavior of consumers on how much calories they prefer to be in a recipe based on how healthy that recipe is. A interesting trend that can be gathered from this table is that when consumers choose to make a unhealthy or even moderately unhealthy recipe they prefer the lower calorie options, but when they choose to make a healthy food, they were fine with the recipe being higher in calories. 

### Imputation

One of the columns that had to be imputed was the rating column. Some of the rows in the rating columns had a value of 0 which is a invalid rating so what I chose to do is I made those rows null for their rating, as mentioned before then I did a mean imputation for the null ratings by their respective recipes, which would allow me to keep the general distribution of the dataset while not losing any data from table (some recipes only had 0 ratings, so they were dropped completely from data after mean imputation). In the figures below we show the distribution of ratings before the imputation and after.

