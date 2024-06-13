# Analyzing Relationships Between Minutes and Ratings of Recipes
### By Angela Shen and Kendall Underwood
## Overview
This is a project for DSC 80 at UCSD investigating the relationship between ratings, minutes, and predicting minutes based on other variables.
## Introduction
Food is essential to human life and is a very big part of peoples' cultures. Not only does it provide
basic sustenance like energy and nutrients, but it is often at the center of celebrations, festivals, 
and gatherings, which fosters a sense of community and value. With the vast number of recipes out there, it is important to understand certain factors such as cook time, ingredients and steps to allow others to replicate the meal. Through the rise of cooking channels and online platforms to share recipes, many have begun to rate the recipes that they have tried to cook based of different factors including cook time. **With this in mind, we want to understand the relationship between cooking time and average rating of recipes**. We want to explore if different cook times that are considered 'low' or 'high' have different average ratings. In order to do this, we analyzed two datasets, one ratings and the other recipes. These were found on https://food.com/ and consist of recipes that have been posted since 2008.

Our first dataset is the **recipes** dataset. This dataset has 83782 rows and 12 columns.

![Screenshot 2024-06-11 172039](https://github.com/PandaFalls2004/Minutes_and_Ratings/assets/129922943/fa254737-6bf3-4324-b9ed-a184475d2dc9)

The second dataset, **interactions** contains 731927 rows and 5 columns.

![Screenshot 2024-06-11 174207](https://github.com/PandaFalls2004/Minutes_and_Ratings/assets/129922943/d1593147-985e-4970-bd6b-3076e2dd8909)

**With this project we are investigating the relationship between cooking time and average rating in recipes** The main columns that were important to us were *id*, *minutes*, *recipe_id*, and *rating* as described above. These columns and the manipulation of these columns allowed us to gain insights on a relationship, or lack thereof between average rating and cook time. Understanding this relationship would allow cooks and cookbook authors to tailor their recipes' cook time to suit the needs and wants of the consumers.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
In order to gain insights on our question we needed to do some data cleaning and manipulation. Below are the steps we took in order to make our analysis more convenient.


Step 1:
    Because we had two datasets with the information that we needed, the first step was to merge the datasets together. We left merged the recipes and interactions dataset left on *id* and right on *recipe_id*. This allowed us to match up the recipes and have the minutes and ratings for each different recipe.

Step 2:
    After we had our merged dataset, we filled all of the ratings that had a value of 0 with NaN values. This makes sense because most ratings scales are from 1 to 5 and don’t include 0. This helped reduce rating bias in our dataset.

Step 3:
    Our dataset only has a rating column, but we are interested in average ratings. In this step we created a series of average ratings by using *groupby* method to group by *id* and then get the mean of the rating. Because a recipe could be rated my multiple people, this allowed us to get an overall rating and look at the rating for a recipe as a whole. 

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
For this analysis we wanted to look at the distribution of minutes or cooking time for the recipes. We noticed when we first created our visualization that we had a large number of outliers in our minutes column. This made our visualizations very hard to interpret. We then decided to create a helper function that created upper and lower bounds based on the Interquartile Range formula. This gave us upper and lowers bounds that we used to filter our data.

Our plot below is a box plot of the minutes column in our filtered dataframe. As the plot below shows, our upper quartile value is 50. This means that 75% of the data falls below 50 minutes. The median value is 30 minutes and the max value is 118 minutes. The distribution of our data is positively skewed.

<iframe src="assets/boxplot.html" width="800" height="600" frameborder="0" ></iframe>

### Bivariate Analysis
For this analysis, we examined the distribution of minutes and average rating in order to identify any possible associations. The box plot below shows that average ratings of 4 and 5 tend to have higher medians than ratings of 1, 2, or 3. We see a difference in the medians, but the overall distributions seem pretty similar.

<iframe src="assets/boxplot_bivariate.html" width="800" height="600" frameborder="0" ></iframe>


### Interesting Aggregates
The columns that we chose to use in our pivot table are *avg_rating* and *minutes*. For this section we wanted to explore the relationship between each distinct average rating and the median and mean of the minutes associated with it. The table below shows that as the average rating increases, the median minutes decrease and so does the mean minutes. This suggest there might be a correlation with lower cook time and higher ratings.

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
We then moved our analysis to help determine the missingness of the column *ratings*. In order to do this, we tested if this column depended on *n_steps* and *minutes*. For this analysis, we used merged\_dataset instead of our filtered dataset so that we can look at more null values. 

Below are the histograms comparing the distributions of *n_steps* and *minutes* when *rating* is missing and when it is not. Minutes had to be filtered so that the histogram would be visible, however we will use the entirety of merged\_dataset in our analysis. The distributions look fairly similar in both cases, but this could be a result of the large amount of data we have.

<iframe src="assets/minutes_histogram.html" width="800" height="600" frameborder="0" ></iframe>

<iframe src="assets/n_steps_histogram.html" width="800" height="600" frameborder="0" ></iframe>

First we start with *n_steps* and *rating*.

Null Hypothesis: The missingness of the *rating* column does not depend on *n_steps*

Alternative Hypothesis: The missingness of the *rating* column does depend on *n_steps*

The test statistic that we used in our permutation test was **absolute difference in means** and our significance level
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
As stated above, in the analysis we are interested in investigating the relationship between cook time and average rating. For our analysis, we categorized average ratings into low and high categories. We consider average ratings to be low if the rating is 1-3, and high ratings are average rating values of 4 or 5. We created a function that would categorize each recipe and added that as an addition column in our dataframe named **categorized_data**. We then ran our permutation test.

Null Hypothesis: High rated recipes have the same cooking times as low rated recipes
Alternative Hypothesis: High rated recipes have shorter cooking times than low rated recipes
Test Statistic: The difference in mean between high rated and low rated recipes
Significance level: 0.05

We chose to do a permutation test because we are trying to determine if the distributions look like they could come from the same population. We proposed that High rated recipes have shorter cooking times because of the business of people in their everyday lives and wanting to have a quick and nutritious meal. With work, school, and other activities having a meal that doesn’t take long to cook saves time and energy. This could be an added plus for people when deciding what recipes, they like and how they want to rate them. Since we are stating that High rated recipes have shorter cooking times, we used difference in means as our test statistic because it is directional.

To run the test, we first calculated our observed difference. Then we shuffled the minutes column 500 times to collect 500 simulations of the mean difference. We got a p-value of **.512**


### Conclusion
Since the p-value we found is .512, which is more than our significance level of .05, we fail to reject the null hypothesis. There is no sufficient evidence that high rated recipes have shorter cooking times than low rated recipes.

## Framing a Prediction Problem
We plan to predict the **number of minutes to prepare recipes**. This is a **regression** problem because minutes is a numerical variable.

We chose minutes as our response variable because it is valuable to know how long a recipe will take to cook. This would be valuable information when planning when and what to cook at certain times.

The metrics we used are RMSE and R^2. We used these metrics because they are common Linear Regression criterion measures. The information that we would know at the time of prediction would be all the columns in the recipe dataset that was first introduced besides the *minute* column. All of that information is related to the actual recipe themselves and is inputted as features of that recipe. Based off of those features,  we would want to be able to predict how much time said recipe takes to cook.

## Baseline Model
For our baseline model, we are utilizing a pipeline to implement a Linear Regression model and we split our data into training and testing sets The features that we are using in this model are *n_steps*, *n_ingrediants*, and *calories*. Both of these columns contain quantitative numerical values.

The *calories* values came from grabbing them out of the *nutrition* column, which was a list of nutritional values. We then used our IQR function again to obtain upper and lower limits for calories so we would reduce outliers.

We used StandardScalar to standardize minutes, ensuring that cooking times are in ranges that are comparable, especially because we discovered outliers in our previous model.

The R^2 of training was **.24** and the R^2 for testing was **.25**. The RMSE for training was **20.60** and the RMSE for testing was **20.71**. These low metric scores, lets us know that we need to improve our model and that there possibly could be better features that we could utilize to make our predictions more accurate.



## Final Model 
We decided to use the same 3 variables from our baseline model, since the graphs demonstrated that these variables did positively correlate with minutes. We tried a different regressor, specifically a decision tree regressor.  

Using GridSearchCV, we tested the hyperparameters "min sample leaf" and "max depth". GridSearchCV returned that our best hyperparameters were a max depth of 100 and a minimum sample leaf of 1, which is the highest complexity of the hyperparameters we tested. This resulted in overfitting from our model, though it was a significant improvement over our initial model.

We then manually experimented with multiple hyperparameters, which would usually either greatly decrease our R^2 or reinforce the overfitting observed. We settled with a max depth of 50 and a minimum sample leaf of 5. This resulted in an R^2 of about 0.55 for the test and 0.75 for the training, which was still overfitting but much less so than the GridSearchCV results while still being much better than our initial model. We also calculated the RMSE, and got about 15.6 on our test RMSE and 11.4 for our training RMSE. 

## Fairness Analysis
To evaluate our model's fairness, we thought about analyzing our model based on date. We converted the date recipes were submitted and extracted the year. Most of the data is concentrated within the first few years, so our model could be biased towards older data and therefore might not be accurate for current recipes. Therefore, we split our data into pre-2010 data (about 2/3rds of the data) and 2010 and onwards (remaining 1/3rd) using the Binarizer function from sklearn. 

We chose to use the absolute difference of RMSEs as a test statistic for the test since we have previously used it to evaluate our models and because it is larger than the R^2, so the differences might be more evident. Our null hypothesis is that the RMSE for pre-2010 data will have the same RMSE as data from 2010 and onwards and our alternative hypothesis is that the RMSE for pre-2010 data will be different from the RMSE for data from 2010 and onwards. We will again use a significance level of 0.05. Based on our observed data, pre-2010 data has a lower RMSE. After multiple permutations, we find that this difference is significant. Our p-value is 0.0, so we reject the null hypothesis. It does seem like there is a difference in our model's performance between pre-2010 data and post-2009 data. 