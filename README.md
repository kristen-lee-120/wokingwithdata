# Woking with Data
By: Kristen Lee and Jordi Pham


## Introduction

 <!-- Provide an introduction to your dataset, and clearly state the one question your project is centered around. Why should readers of your website care about the dataset and your question specifically? Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns. -->

The recipes and ratings dataset is a dataset that revolves around recipes for food and reviews for how well those recipes did. For our project, we are particularly interested in what is the relationship between cooking time and the average rating of recipes. Readers of should care about our dataset and questions because they can provide a baseline when choosing a recipe to cook for their next meal. Per our given csv files, `interactions` is a dataset with 731927 rows, `recipes` is a dataset with 83782 rows, and the left-merged dataset evaluates to 234429 rows of data. The columns that will prove most relevant to our question are `rating` (user-given rating of the recipe) and `minutes` (preparation time of the recipe). Using these two columns, we believe we will be able to compile the right information to hopefully answer our data science question.

The recipe dataset contains these relevant columns:

| **Column** | **Description** |
|------------------|-----------------|
| `name` | Name of the recipe |
| `id` | ID of the recipe |
| `minutes` | Cooking time of the recipe |
| `contributor_id` | ID of the person who created the review |
| `submitted` | Date the review was logged |
| `tags` | Miniature descriptive tags for each recipe (string format resembling a list) |
| `nutrition` | List of nutrition facts shown in string format |
| `n_steps` | Number of steps required for the recipe |
| `steps` | Text description of each step for the recipe |
| `ingredients` | Text description of the required ingredients for the recipe |
| `n_ingredients` | Number of ingredients for the recipe |

The interaction dataset contains these relevant columns:

| **Column** | **Description** |
|---------------|-----------------|
| `user_id` | ID of the user |
| `recipe_id` | ID of the recipe |
| `date` | Logged date of the review |
| `rating` | Rating out of 5 given by the user |
| `review` | Description text of the user review |

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
<!-- Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions). -->

<!-- We did not fill or impute missing data, since intructions said ... -->
In order to clean our dataframe, we first looked to clean columns where certain values failed to make sense or the values came in an unusable form. For the `ratings` column, some recipes had a rating of 0; however, we do not have any external sources to confirm what this zero truly means: could this 0 be a real rating of 0 out of 5 or is it 0 because it hasn't been rated? Using the ratings we did have, though, we created and mapped a series of average ratings onto their respective recipes. Columns like `tags`, `nutrition`, and `ingredients` were columns where their values were strings that appeared as lists, which we stripped the values of their list brackets and split accordingly to create actual lists. Specifically for nutrition, we further created a column for each nutritional statistic and then removed the original nutrition column. Another column we removed was a column called `Unnamed: 0`: it was unclear as to what values were contained in this column. On top of cleaning these columns, we also created an `n_missing` variable that is a series containing the counts of missing values in each column.



### Univariate Analysis
<!-- we discovered BIG outliers in minutes, we decided to remove these rows using the IQR as boundaries-->
In our first attempt at univariate analysis, we chose to focus on the column for cook time `minutes`; creating a histogram, the first thing we noticed is some of the recipes have an outrageous cook time comnpared to other recipes. Thus, we decided to add in another cleaning step here: using the interquartile range, we deicded to eliminate rows from our main dataframe that were bigger or smaller than the range.

<!--add graph here-->
<iframe 
    src="https://kristen-lee-120.github.io/wokingwithdata/assets/uni-outliers.html"
    width="800" 
    height="600" 
    frameborder="0">
</iframe>
<!--add descriptive analysis after-->
Looking at the graph above, we saw that there was recipies that had cook times of hundreds of thousands of minutes, even a million! Thus we decided to reduce our dataframe using the IQR (Interquartile Range), where we would only look at the data where the cooktime in `minutes` fell above Q1 - 1.5 x IQR$ and below Q3 + 1.5 x IQR.

The follow graph is the distribution of the `minutes` column *after* these outliers are removed. 

<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/uni-no-outliers.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Looking at this graph, we see a lot of cooking times are centered around 20-40 minutes, and that the distribution has a right skew. 

We then looked at the distribution of average ratings:
<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/uni-box.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
`avg_ratings` sees a hard left skew. Most of the average ratings are concentrated at 5, with this being the max, Q3, and the median. The lowest average rating a recipe received is 1. 

### Bivariate Analysis
For our bivariate analysis, we chose to make a scatterplot for `avg_rating` on `minutes` and then added an OLS line estimator on top. The plotly graph below appears to have no visible trends as the OLS line seems to have a very slight negative slope. This may imply that a higher average rating weakly correlates with a slightly shorter cooktime.
<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/bivariate-minutes-avg-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
One way to read part of the pivot table below is, say, let's look at the first cell where the average rating is 1 and the recipe takes 1 step. A recipe that has an average rating of 1 and takes 1 step has an average cooking time of 12 minutes flat. All other cells can be read in a similar manner.

From this table, we can also see that the most amount of steps in the entire main dataframe is a 86-step recipe. At 86 steps and an average rating of 5, the average cook time was 18 minutes (somehow??).

<iframe src="https://kristen-lee-120.github.io/wokingwithdata/assets/pivot_table.html"
        width="100%" height="500px" frameborder="0"></iframe>

## Assessment of Missingness
### NMAR Analysis
The reviews column is a column that could be deemed as NMAR, or not missing at random. After all, this is a dataset of food recipes and their respective reviews; a missing review for a recipe can really only be traced back to how a reviewer interacted with the recipe. The food could've have been so bad that the person felt no need to even leave a review, the recipe may or may not have ever been tried before for a review to be made, or human nature got in the way and the reviewer essentially just forgot to leave a review when logging their personal interaction. 

If we were to change the missingness from NMAR to MAR, where some other column in the dataset could explain the missingness of the `review` column, the additional data that we could possibly obtain would be information about user behavior or recipe characteristics. For example, there could be a column containing data on the difficulty level of the recipe. If a recipe is hard, maybe the person never even finished their attempt to finish the review. Another column of additional data we could possibly obtain is a column that evaluates the ingredients needed in the recipe. Some ingredients could be hard to find and some could be very expensive, which can affect whether or not a review is left. Other indicative data would be view count of a recipe. If a recipe is never viewed, it can not have a review. 

### Missingness Dependency
For both tests, our chosen **test statistic** was the **difference of means** between the two groups (group 1: rows with missing average rating, group 2: rows). We used **permutation testing** through 500 iterations and operated under a **significance level of 0.01**. In our algorithm, we would shuffle the `avg_ratings` to mix up the groups between those that had an average rating and those with the lack thereof.

Some pre-processing before testing for missingness was removing the rows with `minutes` outliers, and because we are looking at columns that are intrinsic to the recipe, and not interactions with the recipe (`protien_pvd` and `calories`), we decided to keep only the first occurance of every recipe in the DataFrame, removing rows corresponding to additional reviews. 

#### First Test For A Missingness Mechanism: `avg_ratings` vs. `protein_pdv`

* Null: The missingness of the `avg_ratings` column is not dependent on the `protein_pdv` column.

* Alt: The missingness of the `avg_ratings` column is dependent on the `protein_pdv` column.

The p-value we found was above 0.5 (**0.542**), which is higher than our significance level 0.01, meaning we fail to reject the null hypothesis that the missingness of the `avg_ratings`** column is not dependent on the `protein_pdv` (percent daily value of protein) column. 

<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/mar-no-sig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The **observed statistic** of **0.531** is indicated by the red verticle line on the graph. Since the p-value that we found (0.542) is > 0.01, we failed to reject the null hypothesis. 


#### Second Test For A Missingness Mechanism: `avg_ratings` vs.`calories`

* Null: The missingness of the `avg_ratings` column is not dependent on the `calories` column.

* Alt: The missingness of the `avg_ratings` column is dependent on the `calories` column.

The p-value we found was **0.0**, which is lower than our significance level of 0.01, meaning we reject the null hypothesis that the missingness of the `avg_ratings` column is not dependent on the `calories` column. 


<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/mar-sig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The **observed statistic** of **85.444** is indicated by the red verticle line on the graph. Since the p-value that we found (0.0) is < 0.01, we reject the null hypothesis. Looking at the graph, we can see that the observed statistic is very far from the distribution of created test statistics. 

## Hypothesis Testing
* Null: There is no relationship between average rating of a recipe and its cooking time.

* Alt: There _IS_ a relationship between the average rating of a recipe and its cooking time. 

* Test statistic: **Pearson's R**, otherwise known as the correlation coefficient.

* Significance Level: 0.01 

After performing a permutation test by shuffling the average rating of recipies, we rejected our null hypothesis. 
<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/hyp-test-corr.html"
  
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/hyp-test-plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem
Our prediction problem's goal is to predict the number of steps required in a recipe, which will be a regression-type problem. Our response variable is `n_steps`, and we chose it because the number of steps in a recipe is dependent on a lot of different factors and does not exactly increase or decrease in an intutitive manner. For example, a recipe could take over 60 minutes of cook time but that does not necessarily mean it takes a lot of steps. Through this model, we hope to isolate different regressors that can play a role in determining how many steps a recipe takes, and in the end, we look to evaluate our model through the r-square metric. R-square has mathematically relations to another peformance metric, RMSE; however, it has its pros and cons. R-square is a performance metric that is unit-independent, making it easier to interpret and compare across different models. RMSE follows the same units as our response variable; however, across different models it can be a lot harder to compare and interpret. Hence, we are taking advatange of the intepretability of the r-square performance metric.

At the time of prediction, there are a few initial columns for us to consider in our prediction model: `minutes` and `n_ingredients`. These two initial regressors we consider to be highly relevant to our response variable: on average, we would expect that a recipe  of longer time probably required more steps and that a recipe that requires more ingredients probably adds more steps in regards to preparation and such. More regressors can be engineered later to improve the model.

## Baseline Model
To create our model, we chose to perform a linear regression with two features, `n_ingedients` and `minutes`, with `minutes` specifically being our main regressor since we believe it has the most relevance to our response variable `n_steps`. 'n_ingredients' is a quatitative and discrete variable whereas minutes is a quatitative and continuous variable. With both variables being quantitative, we did not have a need to perform any special encodings and plugged directly into a pipeline containing the linear regression. Through GridSearchCV, we explored between models with and without negative coefficients, models with and without intercepts, and polynomial features ranging from degrees 1 to 4.

Choosing to tune our model with the inclusion of polynomial degrees was a matter of how the graphs looked like when we created scatterplots of minutes on number of steps and number of ingredients on number of steps. Both of those scatters seem to display a non-linear relationship individually between our regressors and our response variable

<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/6-steps-v-minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/6-steps-v-ing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We concluded that our model performed best with hyperparameters tuned to include an intercept, no negative coefficients, and a polynomial degree of 4 for both regressors. Our R-squared score was 0.2811, indicating that the model can explain about 28% of the data's variation, which we believe to be good. When we inputted our regressors into their respective histograms, we noticed that the data was extremely dense, and furthermore, when we conducted bivariate analysis, we noticed that lot of the relationships between different columns appeared flat. Considering that a lot of this data doesn't exactly give many implications, we believe this model performed relatively well. 


## Final Model

## Fairness Analysis
<iframe
  src="https://kristen-lee-120.github.io/wokingwithdata/assets/fairness-perm-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>