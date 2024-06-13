# Analyzing Relationships Between Minutes and Ratings of Recipes
### By Angela Shen and Kendall Underwood
## Overview
This is a project for DSC 80 at UCSD investigating the relationship between ratings, minutes, and predicting minutes based on other variables.
## Introduction
Food is essential to human life and is a very big part of peoples' cultures. Not only does it provide
basic sustenance like energy and nutrients, but it is often at the center of celebrations, festivals, 
and gatherings, which fosters a sense of community and value. With the vast amount of recipes out there, it is important to understand certain factors such as cook time, ingredients and steps to allow others to replicate the meal. Through the rise of cooking channels and online platforms to share recipies, many have begun to rate the recipes that they have tried to cook based of different factors includng cook time. **With this in mind, we want to understand the relationship between cooking time and average rating of recipes**. We want to explore if differnet cook times that are considered 'low' or 'high' have different average ratings. In order to do this we analyzed two datasets, one ratings and the other recipes. These were found on https://food.com/ and consist of recipes that have been posted since 2008.

Our first dataset is the **recipes** dataset. This dataset has 83782 rows and 12 columns.

![Screenshot 2024-06-11 172039](https://github.com/PandaFalls2004/Minutes_and_Ratings/assets/129922943/fa254737-6bf3-4324-b9ed-a184475d2dc9)

The second dataset, **interactions** contains 731927 rows and 5 columns.

![Screenshot 2024-06-11 174207](https://github.com/PandaFalls2004/Minutes_and_Ratings/assets/129922943/d1593147-985e-4970-bd6b-3076e2dd8909)

**With this project we are investigating the relationship between cooking time and average rating in recipes** The main columns that were important to us were *id*, *minutes*, *recipe_id*, and *rating* as described above. These columns and the manipulation of these columns allowed us to gain insights on a relationship, or lack thereof between average rating and cook time. Understanding this relationship would allow cooks and cookbook authors to tailor their recipes' cook time to suit the needs and wants of the consumers.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
In order to gain insights on our question we needed to do some data cleaning and manipulation. Below are the steps we took in order to make our analysis more convienent.


Step 1:
    Because we had two datasets with the information that we needed, the first step was to merge the datasets together. We left merged the recipes and interactions dataset left on *id* and right on *recipe_id*. This allowed us to match up the recipes and have the minutes and ratings for each different recipe.

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

<iframe src="assets/boxplot.html" width="800" height="600" frameborder="0" ></iframe>

### Bivariate Analysis
For this analysis, we examined the distribution of minutes and average rating in order to identify any possible associations. The box plot below shows that average ratings of 4 and 5 tend to have higher medians than ratings of 1, 2, or 3. We see a difference in the medians, but the overall distributions seem pretty similar.

<iframe src="assets/boxplot_bivariate.html" width="800" height="600" frameborder="0" ></iframe>


### Interesting Aggregates
The columns that we chose to use in our pivot table are *avg_rating* and *minutes*. For this section we wanted to explore the relationship between each distince average rating and the median and mean of the minutes asscociated with it. The table below shows that as the avergage rating increases, the median minutes decrease and so does the mean minutes. This suggest there might be a coorelation with lower cook time and higher ratings.

|   avg\_rating |    mean |   median |
|-------------:|--------:|---------:|
|            1 | 39.126  |       35 |
|            2 | 38.769  |       35 |
|            3 | 38.4271 |       35 |
|            4 | 36.9375 |       30 |
|            5 | 35.8803 |       30 |


## Assessment of Missingness

### NMAR Analysis
We believe that the missingness of the *description* column is NMAR because when looking at the website you can choose to not leave a description under your recipe. Since it is up to human choice, people can choose not to add a description for any random reason or circumstance.

### Missingness Dependency
We then moved our analysis to help determine the missingness of the column *ratings*. In order to do this we tested if this column depended on *n_steps* and *minutes*. For this analysis, we used merged\_dataset instead of our filtered dataset so that we can look at more null values. 

Below are the histograms comparing the distributions of *n_steps* and *minutes* when *rating* is missing and when it is not. Minutes had to be filtered so that the histogram would be visible, however we will use the entirety of merged\_dataset in our analysis. The distributions look fairly similar in both cases, but this could be a result of the large amount of data we have.

<iframe src="assets/minutes_histogram.html" width="800" height="600" frameborder="0" ></iframe>

<iframe src="assets/n_steps_histogram.html" width="800" height="600" frameborder="0" ></iframe>

First we start with *n_steps* and *rating*.

Null Hypothesis: The missingness of the *rating* column does not depend on *n_steps*

Alternative Hypothesis: The missingness of the *rating* column does depend on *n_steps*

The test statistc that we used in our permutation test was **absolute difference in means** and our significance level
was **.05**

We ran a permutation test by shuffling the *n_steps* column 500 times to get 500 simulations of the mean differences. 

Our p-value was 0.0, so we reject the null hypothesis. It seems like the missingness of the *rating* column does depend on *n_steps*

Below is the histogram of our results:

<iframe src="assets/n_steps_results.html" width="800" height="600" frameborder="0" ></iframe>

Our second permutation test was with *minutes* and *review*.

Null Hypothesis: The missingness of the *review* column does not depend on the *minute*.

Alternative Hypothesis: The missinness of the *review* column does depend on the *minute* column.

We again used the **absolute difference in means** for our test statistic and **.05** as our significance level.

Our p-value was consistently 0.1 or greater, so we fail to reject the null hypothesis. We cannot conclude that *rating* is MAR dependent on *minutes*.

Below is the histogram of our results:

<iframe src="assets/minutes_results.html" width="800" height="600" frameborder="0" ></iframe>


## Hypothesis Testing
As stated above, in the analysis we are intersted in investigating the relationship between cook time and average rating. For our analysis, we categorized avergage ratings into low and high categories. We consider ratings to be low if the are 1-3, and high ratings are average rating values of 4 or 5. We created a function that would categorize each recipe and added that as an addition column in our dataframe named **categorized_data**. We then ran our permuation test.

Null Hypothesis: High rated recipes have the same cooking times as low rated recipes
Alternative Hypothesis: High rated recipes have shorter cooking times than low rated recipes
Test Statistic: The difference in mean between high rated and low rated recipes
Signifigance level: 0.05

We chose to do a permutation test because we are trying to determine if the distributions look like they could come from the same population. We proposed that High rated recipes have shorter cooking times because of the business of people in their everyday lives and wanting to have a quick, yet healty and nutritious meal. With work, school, and other activities having a meal that doesnt take long to cook saves time and energy. This could be an added plus for people when deciding what recipes they like and how they want to rate them. Since we are stating that High rated recpes have shorter cooking times, we used difference in means as our test statistic because it is directional.

To run the test we first calulated our observed difference. Then we shuffled the minutes column 500 times to collect 500 simulations of the mean difference. We got a p-value of **.512**

### Conclusion
Since the p-value we found is .512, which is more than our signifigance level of .05, we fail to reject the null hypothesis. There is no sufficient evidence that high rated recipes have shorter cooking times than low rated recipes.

## Framing a Prediction Problem
We plan to predict the **number of minutes to prepare recipes**. This is a **regression** problem because minutes is a numerical variable.

We chose minutes as our response variable because it is valuable to know how long a recipe will take to cook. This would be valuable information when planning when and what to cook at certain times.