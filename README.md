# Analyzing Relationships Between Minutes and Ratings of Recipes
### By Angela Shen and Kendall Underwood
## Overview
This is a project for DSC 80 at UCSD investigating the relationship between ratings, minutes, and predicting minutes based on other variables.
## Introduction
Food is essential to human life and is a very big part of peoples cultures. Not only does it provide
basic sustencance like energy and nutrients, but it is often at the center of celebrations, festivals, 
and gatherings fostering a sense of community and value. With the vast amount of recipes out there, it is important to understand certian factors such as cook time, ingrediants and steps to allow others to replicate the meal. Through the rise of cooking channels and online platforms to share recipies, many have begun to rate the recipes that they have tried to cook based of different factors includng cook time. **With this in mind, we want to understand the relationship between cooking time and average rating of recipes**. We want to explore if differnet cook times that are considered 'low' or 'high' have different average ratings. In order to do this we analyzed two datasets, one ratings and the other recipes. These were found on https://food.com/ and consist of recipes that have been posted since 2008.

Our first dataset is the **recipes** dataset. This dataset has 83782 rows and 12 columns.

![Screenshot 2024-06-11 172039](https://github.com/PandaFalls2004/Minutes_and_Ratings/assets/129922943/fa254737-6bf3-4324-b9ed-a184475d2dc9)

The second dataset, **interactions** contains 731927 rows and 5 columns.

![Screenshot 2024-06-11 174207](https://github.com/PandaFalls2004/Minutes_and_Ratings/assets/129922943/d1593147-985e-4970-bd6b-3076e2dd8909)

**With this project we are investigating the relationship between cooking time and average rating in recipes** The main columns that were important to us were *id*, *minutes*, *recipe_id*, and *rating* as described above. These columns and the manipulation of these columns allowed us to gain insights on a relationship, or lack thereof between average rating and cook time. Understanding this relationship would allow cooks and cookbook authors to tailor their recipes' cook time to suit the needs and wants of the consumers.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
In order to gain insights on our question we needed to do some data cleaning and manipulation. Below are the steps we took in order to make our analysis more convienent.


Step 1:
    Because we had two datasets with information we needed our first step was to merge the datasets together. We left merged the recipes and interactions dataset left on *id* and right on *recipe_id*. This allowed us to match up the recipes and have the minutes and ratings for each different recipe.

Step 2:
    After we had our merged dataset, we filled all of the ratings that had a value of 0 with NaN values. This makes sense because most ratings scales are from 1 to 5 and dont include 0. This helped reduce rating bias in our dataset.

Step 3:
    Our dataset only has a rating column, but we are interested in avergae ratings. In this step we created a series of average ratings by using *groupby* method to group by *id* and then get the mean of the rating. Because a recipe could be rated my multiple people, this allowed us to get an overall rating and look at the rating for a recipe as a whole. 

Step 4:
    We added our average ratings series as a column to our merged dataframe and named it *avg_rating*. Our final dataframe now has 18 columns and 234429 rows.

Below are the first 5 rows of our cleaned *final_dataset*. Even though in total it has 18 columns, we only included the 5 that we really needed for our analysis including, *recipe_id*, *id*, *minutes*, *rating*, and *avg_rating*.

|   recipe_id |     id |   minutes |   rating |   avg_rating |
|------------:|-------:|----------:|---------:|-------------:|
|      333281 | 333281 |        40 |        4 |            4 |
|      453467 | 453467 |        45 |        5 |            5 |
|      306168 | 306168 |        40 |        5 |            5 |
|      306168 | 306168 |        40 |        5 |            5 |
|      306168 | 306168 |        40 |        5 |            5 |

### Univariate Analysis
For this analysis we wanted to look at the distribution of minutes or cooking time for the recipes. We noticed when we first created our visualization that we had a large amount of outliers in our minutes column. This made our visualizations very hard to interpret. We then decided to create a helper function that created upper and lower bounds based on the Interquartile Range formula. This gave us upper and lowers bounds that we used to filter our data.

Our plot below is a box plot of the minutes column in our filtered dataframe. As the plot below shows, our upper quartile value is 50. This means that 75% of the data falls below 50 minutes. The median value is 30 minutes and the max value is 118 minutes. The distribution of our data is positively skewed.

### Bivariate Analysis
For this analysis, we examined the distribution of 
