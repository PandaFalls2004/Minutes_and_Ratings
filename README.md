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