# Recipe-Reviews-Analysis
In this project we studied the relationship between the minutes and average rating of a recipe. We wanted to learn if the time it takes for a recipe to be cooked had any effect on the average rating of that recipe. More specifically, we wanted to know if shorter recipes (40 minutes or less) would have a higher average rating than longer recipes (40 minutes+). 

---
## Introduction 
The dataset we used for our analysis was made up of two datasets, one with recipes and the other with ratings. The following data can be found here: ***include link to dataset***. 

Breakdown of datasets:

***RAW_RECIPES.CSV***
  
| Variables | Description |
| ----------- | ----------- |
| name | Recipe name |
| id | Recipe Id |
| minutes | Minutes to prepare recipe |
|contributor_id | Recipe Id |
| submitted | Recipe name |
| tags | Recipe Id |
| nutrition | Recipe name |
| n_steps | Recipe Id |
| steps' | Recipe name |
| description | Recipe Id |

 ***RAW_INTERACTIONS.CSV***
  
| Variables | Description |
| ----------- | ----------- |
| user_id | Title |
| recipe_id | Text |
| date | Title |
| rating | Text |
| review | Title |

-------

# Cleaning and EDA 
  
## Data Cleaning
To start we began by merging the two datasets. We then noticed that the merged dataset, had many values in the `rating` column with the value 0. This could pose a problem for our analysis since we will be using the average rating of recipes and values of 0 would decrease our averages. After looking at the website where the data had been scraped from (food.com) we realised that it was not possible to rate a recipe less than 1 star. Upon further analysis we identified a section labeled 'tweaks' which contained reviews but no ratings. Thus, we concluded that the rows with 0 star rating were from the 'tweak' section and that those stars are acutally supposed to be missing by design. Therefore, we changed the ratings with 0 to NaN so that these rows did not affect our average ratings. 

We then grouped all the recipes with the same `id` and calculated the mean to get the average rating and added it as a column labled `avg_rating'. We then added the column `time label` which contains the string values '40<=' and >40', then dropped the following columns as they would not be used for further analysis:
1. 'submitted',
2. 'user_id',
3. 'recipe_id',
4. 'date',
5. 'rating' 


Afterwards we are left with the follow DataFrame:

## Univariate Analysis

## Bivariate Analysis

## Interesting Aggregates

# Assessment of Missingness 

## NMAR Analysis

## Missingness Dependency

# Hypothesis Testing

