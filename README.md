# The 40 Minute Rule!

In this project we studied the relationship between the minutes and average rating of a recipe. We wanted to learn if the time it takes for a recipe to be cooked had any effect on the average rating of that recipe. More specifically, we wanted to know:
>**Do shorter recipes (less than or equal to 40 mins) tend to have higher average ratings than longer recipes (40 mins or more)?** 

We believe answering this question will be useful to the creators who post there recipes online as this analysis would give insight on to what is the best cooking time for a dish, in order to get the best ratings. 

--- 

### Introducing the Data

The dataset we will be using for our analysis is made up of two datasets. `RAW_RECIPES.csv` which contains over  one with over 70,000 recipes `RAW_INTERACTIONS.csv` which contains over 200,000 reviews of the recipes. Both datasets were obtained through the website ***food.com***. 

The variables we will be most intersted in particular are `minutes` in recipes and `rating`.

Breakdown of datasets:

***RAW_RECIPES.CSV***
  
| Variables | Description |
| ----------- | ----------- |
| name | Recipe name |
| id | Recipe Id |
| minutes | Minutes to prepare recipe |
|contributor_id | Contributor Id |
| submitted | Date Submitted |
| tags | List of tags |
| nutrition | Nutrtion facts |
| n_steps | Number of steps |
| steps | List of steps |
| description | Description of Recipe |

 ***RAW_INTERACTIONS.CSV***
  
| Variables | Description |
| ----------- | ----------- |
| user_id | User Id |
| recipe_id | Recipe Id |
| date | Date review was written |
| rating | Rating of recipe |
| review | Review |

-------

# Cleaning and EDA 
  
## Data Cleaning

To start we began by merging the two datasets. We then noticed that the merged dataset, had many values in the `rating` column with the value 0. 

|    |   rating |
|---:|---------:|
|  5 |   169676 |
|  4 |    37307 |
|  0 |    15035 |
|  3 |     7172 |
|  1 |     2870 |
|  2 |     2368 |

This could pose a problem for our analysis since we will be using the average rating of recipes and values of 0 would decrease our averages. After looking at the website where the data had been scraped from (food.com) we realised that it was not possible to rate a recipe less than 1 star. Upon further analysis we identified a section labeled 'tweaks' which contained reviews but no ratings. Thus, we concluded that the rows with 0 star rating were from the 'tweak' section and that those stars are acutally supposed to be missing by design. Therefore, we changed the ratings with 0 to NaN so that these rows did not affect our average ratings. 


We then grouped all the recipes with the same `id` and calculated the mean to get the average rating and added it as a column labled `avg_rating`.

```py
avg_rating = merged.groupby('id')['rating'].mean()
merged['avg_rating'] = merged['id'].apply(lambda x: avg_rating[x])
```

We then added the column `time label` which contains the string values '40<=' and >40', then dropped the following columns as they would not be used for further analysis:

1. 'submitted',
2. 'user_id',
3. 'recipe_id',
4. 'date',
5. 'rating' 

### Removing outliers

When looking at the distributions of our variables we noticed the variable `minutes` some outliers that we wanted to deal with before our analysis. Plotting the distribution of the `minutes` column gave us the follwing histogram: 

<iframe src="assets/minutes_dirty.html" width=800 height=600 frameBorder=1></iframe>

In order to identify outliers we used the Inter quartile range (IQR) method. In which we define outliers as any value less than then the (25th percentile * 1.5) or greater than (75th percentile * 1.5). Then when a value was detected as an outlier it was removed from the dataset as these values would be unrepresentative of our general population of recipes. After removing the outliers our new histogram became:

<iframe src="assets/minutes_clean.html" width=800 height=600 frameBorder=1></iframe>


Afterwards we are left with the follow DataFrame:

|    | name                                 |     id |   minutes |   contributor_id | tags                                                                                                                                                                                                                                                                                               | nutrition                                     |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                                                                             |   n_ingredients |   avg_rating | time label   |
|---:|:-------------------------------------|-------:|----------:|-----------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------|----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-------------:|:-------------|
|  0 | 1 brownies in the world    best ever | 333281 |        40 |           985201 | ['60-minutes-or-less', 'time-to-make', 'course...                                                                       | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |        10 | ['heat the oven to 350f and arrange the rack i...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | these are the most; chocolatey, moist, rich, d...                                                                                                              | ['bittersweet chocolate', 'unsalted butter', '...                                                          |               9 |            4 | <=40         |
|  1 | 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | ['60-minutes-or-less', 'time-to-make', 'cuisin...                                                                                                                                      | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |        12 | ['pre-heat oven the 350 degrees f', 'in a mixi...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | this is the recipe that we use at my school ca...                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margar...                                                                             |              11 |            5 | >40          |
|  2 | 412 broccoli casserole               | 306168 |        40 |            50969 | ['60-minutes-or-less', 'time-to-make', 'course...                                                                                                                                               | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |         6 | ['preheat oven to 350 degrees', 'spray a 2 qua...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | since there are already 411 recipes for brocco...| ['frozen broccoli cuts', 'cream of chicken sou...                                                                                                                                      |               9 |            5 | <=40         |
|  6 | millionaire pound cake               | 286009 |       120 |           461724 | ['time-to-make', 'course', 'cuisine', 'prepara... | [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |         7 | ['freheat the oven to 300 degrees', 'grease a...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | why a millionaire pound cake?  because it's su...                                                                                                                                                                | ['butter', 'sugar', 'eggs', 'all-purpose flour...                                                                                                                                |               7 |            5 |   >40          |
|  7 | 2000 meatloaf                        | 475785 |        90 |          2202916 | ['time-to-make', 'course', 'main-ingredient', ...                                                                                                                                             | [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |        17 | ['pan fry bacon , and set aside on a paper tow...| ready, set, cook! special edition contest entr...                                                                                                                                                                          |['meatloaf mixture', 'unsmoked bacon', 'goat c... |              13 |            5 | >40          |


---- 



## Univariate Analysis

First we will plot the distribution of the variables `minutes` and `avg_rating` in order to get an idea of what the distriubtions look like. 

>Plotting the distribution of the `minutes` gives us the following histogram: 
 

<iframe src="assets/minutes_hist.html" width=800 height=600 frameBorder=1></iframe>


>Plotting the distribution of the `avg_rating` gives us the following histogram:

<iframe src="data_viz/avgRating_hist.html" width=800 height=600 frameBorder=1></iframe>

The average ratings in the dataset are skewed left and tend to have more positive ratings as opposed to negative ones. Over 60% of the recipes have an average rating of 5 while less than 5% have a rating of 3 or less. 


## Bivariate Analysis

Next we will plot the conditional distributions of the variables on whether or not the recipe is less than 40 minuetes or less or not. 

<iframe src="assets/conditional_hist.html" width=800 height=600 frameBorder=1></iframe>

Looking at the two distributions there seems to be a disproportionate amount of recipes on the site that take less than 40 minutes to cook, but there relative shapes look similar. 

## Interesting Aggregates

