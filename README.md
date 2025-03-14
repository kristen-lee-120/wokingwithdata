# Woking with Data
By: Kristen Lee and Jordi Pham


## Introduction

 <!-- Provide an introduction to your dataset, and clearly state the one question your project is centered around. Why should readers of your website care about the dataset and your question specifically? Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns. -->

The recipes and ratings dataset is a dataset that revolves around recipes for food and reviews for how well those recipes did. For our project, we are particularly interested in what is the relationship between cooking time and the average rating of recipes. Readers of should care about our dataset and questions because they can provide a baseline when choosing a recipe to cook for their next meal. Per our given csv files, `interactions` is a dataset with 731927 rows, `recipes` is a dataset with 83782 rows, and the left-merged dataset evaluates to 234429 rows of data. The columns that will prove most relevant to our question are `rating` (user-given rating of the recipe) and `minutes` (preparation time of the recipe). Using these two columns, we believe we will be able to compile the right information to hopefully answer our data science question.

The recipe dataset contains these relevant columns:
* `name` - name of the recipe
* `id` - id of the recipe
* `minutes` - cooking time of the recipe
* `contrbutor_id` - id of person who created the review
* `submitted` - date in which the review was logged
* `tags` - miniature descriptive tags for each recipe (string that looks like a list)
* `nutrition` - list of nutrition facts shown in the form of a string
* `n_steps` - number of steps required for the recipe
* `steps` - text description of each step for the recipe
* `ingredients` - text description of the required ingredients for the recipe
* `n_ingredients` - number of ingredients for the recipe

The interaction dataset contains these relevant columns:
* `user_id` - id of the user
* `recipe_id` - id of the recipe
* `date` - logged date of the review
* `rating` - rating out of 5 given by user
* `review` - description text of what the user review

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
<!-- Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions). -->

<!-- We did not fill or impute missing data, since intructions said ... -->



### Univariate Analysis
<!-- we discovered BIG outliers in minutes, we decided to remove these rows using the IQR as boundaries-->

### Bivariate Analysis

### Interesting Aggregates

## Assessment of Missingness
### NMAR Analysis
The reviews column is a column that could be deemed as NMAR, or not missing at random. After all, this is a dataset of food recipes and their respective reviews; a missing review for a recipe can really only be traced back to how a reviewer interacted with the recipe. The food could've have been so bad that the person felt no need to even leave a review, the recipe may or may not have ever been tried before for a review to be made, or human nature got in the way and the reviewer essentially just forgot to leave a review when logging their personal interaction. 

If we were to change the missingness from NMAR to MAR, where some other column in the dataset could explain the missingness of the `review` column, the additional data that we could possibly obtain would be information about user behavior or recipe characteristics. For example, there could be a column containing data on the difficulty level of the recipe. If a recipe is hard, maybe the person never even finished their attempt to finish the review. Another column of additional data we could possibly obtain is a column that evaluates the ingredients needed in the recipe. Some ingredients could be hard to find and some could be very expensive, which can affect whether or not a review is left.

### Missingness Dependency
Missingess: avg ratings on diff-of-means for protein pdv (percent daily value) ...

Non-missingness: avg ratings on diff-of-means for calories ...

Our chosen siginficance level was 0.01. 

## Hypothesis Testing
Our null hypothesis **is that there is no relationship between average rating of a recipe and its cooking time**.

Our alternative hypothesis is **that there _IS_ a relationship between the average rating of a recipe and its cooking time**. 

Our test statistic is **Pearson's R**, otherwise known as the correlation coefficient, which we chose a significance level of **0.01**. `JUSTIFY`

After performing a permutation test by shuffling the average rating of recipies, we rejected our null hypothesis. 

## Framing a Prediction Problem

## Final Model

## Fairness Analysis
