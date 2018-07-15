# METHODOLOGY: Using machine learning to predict the best distributors in the 2018 draft

If the .ipynb titled "master-draft-class-distributors-ml" does not open, try opening the "master-draft-class-distributors-ml-NO-OUTPUTS" file. The original has all the outputs saved, making it a large file which GitHub sometimes can't open.

[Link to blog post.](https://dribbleanalytics.blogspot.com/2018/07/draft-class-distributors-ml.html)

## Data collection: current players

First, I created a database of all guards taken in the first round who played in college drafted since 2003. The cap was set at 2003 because players before then do not have AST% data.

133 players met these restrictions. The following stats were recorded for each player:

| College stats | NBA stats | Other information |
| ------------- | ------------- | ------------- |
| MPG | MPG | Pick selected |
| AST | AST | Age at draft |
| TOV | TOV | Height (in) |
| AST/TOV | AST/TOV |
| AST%  | AST% |
| TOV%  | TOV% |
| SOS% |  |

College data was taken from [Sports Reference](http://sports-reference.com/cbb). NBA data was taken from [Basketball Reference](http://basketball-reference.com). I used [this draft finder from bbref](https://www.basketball-reference.com/play-index/draft_finder.cgi?request=1&year_min=2003&year_max=2018&college_id=1&pick_overall_min=1&pick_overall_max=30&pos_is_g=Y&order_by=ws) to get the list of qualifying players.

Height data was taken from Sports Reference.

## Data collection: draft class

I recorded the same data from the aforementioned sources for all players listed as guards in [ESPN's draft summary](http://www.espn.com/nba/draft/rounds).

## Model creation

Using scikit-learn, I created four models: a linear regression, a support vector regression (using a radial basis function), a random forest regression, and a k-nearest neighbors regression. Each model used scikit-learn's train test split function using 80% of the data as the training set and 20% of the data as the validation set. The same models and train/test percentage was used in both the model predicting assists and the model predicting turnovers.

The models used all the college stats listed in the table above except for MPG and AST/TOV as inputs. These inputs were used to predict assists and turnovers in the NBA.

Each model's mean squared error and variance score (rsquared) was measured to find the most accurate regression. Each model's cross-validation score for explained variance was also measured to help determine the most accurate model. Note that a higher rsquared and explained variance indicates a more accurate regression, but a lower mean squared error indicates a more accurate regression.

To test for overfitting, I performed k-Fold cross-validation (with n_splits = 4) on all the models, and examined their cross-validation scores for explained variance.

To further test each regression's accuracy, I performed a standardized residuals test, Durbin-Watson test, and Jarque-Bera test. These all test that the errors in a model are random and not autocorrelated.

## Predicting the best distributors in the draft

After making the models mentioned above, I recorded the college data (all the same parameters as the college data for the original 133 players) of college players listed as guards in ESPN's draft summary.

Using the aforementioned methods, the players were plugged into the four models to predict their assists and turnovers in the NBA. A prediction for assist to turnover ratio was made by dividing the models' average predicted assists by the average predicted turnovers.
