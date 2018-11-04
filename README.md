# Wikipedia Dataset Analysis

## Goal

The goal this analysis is to obtain insights on the data of the Wiki Challange and to 
predict if a user is likely to contribute again after an update was reverted. 

Obs: This is **not** the same goal as the original challenge on Kaggle.

## Environment

This analysis was made using Python 3.7.

Create an virtualenv using either virtualenv or conda. Example using conda:

`$ conda create -n wiki python=3.7`

Then install the requirements using *pip*:

`$ pip install -r requirements.txt`

## Dataset

The data is available [on this kaggle page](https://www.kaggle.com/c/wikichallenge).
Dataset was divided into 5 separated files.

* Categories
* Comments
* Edits
* Namespaces
* Titles

## Project Structure

This project is structure as such:

```
|-- README.md
|-- data
|   |-- external
|   |   |-- categories.tsv
|   |   |-- comments.tsv
|   |   |-- edits.tsv
|   |   |-- namespaces.tsv
|   |   `-- titles.tsv
|   |-- processed
|   |   `-- users_behavior_complete.csv
|   `-- wikipedia.tar.bz2
|-- notebooks
|   |-- insights
|   |   |-- 01-Insights-Edits.ipynb
|   |   `-- Sampling.ipynb
|   |-- modelling
|   |   |-- 01-Baseline_LogisticRegression.ipynb
|   |   |-- 02-LogisticRegression-undersampling.ipynb
|   |   |-- 03-RandomForest.ipynb
|   |   |-- 04-RandomForest-undersampling.ipynb
|   |   |-- 05-GradientBoosting.ipynb
|   |   |-- 06-GradientBoosting-undersampling.ipynb
|   |   `-- tools.py
|   |-- provision
|   |   `-- 00-Edits-Feature-Engineering.ipynb
`-- requirements.txt
```

However, the data folder will not be included to this folder due to the large 
amount of data. The processed data can be obtained with the notebooks inside the 
`provision` folder, and the external data can be found on the 
[kaggle website](https://www.kaggle.com/c/wikichallenge).


## Feature Engineering

All feature engineering were made using [this notebook](https://github.com/leportella/wiki-analysis/blob/master/notebooks/provision/00-Edits-Feature-Engineering.ipynb).

The initial dataset used was a timeseries of edits of Wikipedia articles made 
by users. Since we wanted to predict the user behavior, the dataset needed to be 
worked with. Thus, all dataset used on the prediction was engineered based on the 
original `edits.tsv` dataset.

We needed to separate information of each user and only of users that had at least one 
article reverted, since we wanted to predict whether the user would edit again after the first 
revert. 

The original dataset had 22,126,031 records of article edition (22 million) with a total 
of 44,514 unique users. The final dataset had only 14,829 unique users, since not all users 
had reverts.

From this timeseries records of the original dataset, we engineered 9 features.
Each feature is described below:

**Time of first revert**: my original intuition was that the time of the first 
revert was important to the user behavior. In my experience, the sooner you face a 
problem, the higher the chances that you'll be unmotivated. This time was used to 
get other features to check if my assumptions were correct. 

**Time of first contribution**: necessary to construct other features, based on the 
insight exposed above.

**Last contribution**: necessary to construct other features, based on the 
insight exposed above.

**Days until revert**: using the `time of first contribution` and `time of first revert` we 
could achieve the number of days it took the user to have a revert. The guess was that, 
the longer the time, the higher the chances of not stopping the contributions.

**Days of contribution**: using the `time of last contribution` and `time of 
first contribution` we could check the total number of days the user contributed considering 
this particular dataset.

**Total updated**: number of updates the user made during the time of this dataset 

**Updated after revert**: a boolean that check if the user continued editing after the first 
revert. This is the target variable.

**Updates per day**: mean of updated the user made per day considering the total days of 
contribution.  

**Edits before revert**: the number of editing the user made before having it's first revert.


## Insights

All analysis and insights about the edits were made [in this
notebook](https://github.com/leportella/wiki-analysis/blob/master/notebooks/insights/01-Insights-Edits.ipynb).

There were 22,126,031 records on the `edits.tsv` file. Near 9% of these edits
were reverted. The dataset started in May 2001 and ended in Aug 2010. 

Most of the edits were made in 2008 and 2009. Although 2010 had a smaller
number, we must consider that the records stopped in August. So, only we have
records of 66% of the year but, nevertheless, the number of edits until August
represents 74% of the edits recorded in 2009. Thus, we can consider that the
number of edits per year had an increase from 2001 to 2010. 

![](https://i.imgur.com/Zel9kcN.png)

It is possible to see that from 2002 to 2006 there was a huge increase in the
number of new edits per year. After 2006 the growth was smaller, but the edits
kept increasing overall.

![](https://i.imgur.com/uwrAHzu.png).

We can observe that the months from March until August have the higher number
of editing, while from September until December are slightly smaller. This could
be result of holidays. 

![](https://i.imgur.com/S1pK74Z.png)

Although no pattern appeared during the days of the month a good pattern
emerged from the data of hours of day. Usually, users tend to contribute on
afternoons and nights. Morning periods have a lot fewer edits, specially with
the minimum being between 7 and 10 am. 

![](https://i.imgur.com/OHQodon.png)

The users have an average of 134 edits reverted, with a standard deviation of
1063.9 edits. The max reverts per user was 59323, by user `416473`. Although
this is a very high number, this user made a total contribution of 146134. This 
represents only 40% of the user total contribution, so even if it very high, it's a 
number reasonable considering all users, and can't be considered an outlier.

The mean percent of edits reverted is 9%, but it can normally reach up to 50%.
Users that reached 100% of reverts, usually did a single edit that was
reverted.

![](https://i.imgur.com/dy2g2kT.png)

## Models

The notebooks with the attempts of getting the best model are available [here](https://github.com/leportella/wiki-analysis/tree/master/notebooks/modelling).

The initial strategy was to construct a "quick and dirt" model, with minimal features and 
a simple model, to understand how far from our goal we were. The notebook is 
available [here](https://github.com/leportella/wiki-analysis/blob/master/notebooks/modelling/01-Baseline_LogisticRegression.ipynb).

The baseline model (notebook 1) presented a high accuracy (0.95) but with no classification of 
class 0 (users that didn't update). Thus, the high accuracy was mainly due to a problem of unbalanced classes. 

The [second notebook](https://github.com/leportella/wiki-analysis/blob/master/notebooks/modelling/02-LogisticRegression-undersampling.ipynb) tried a strategy of undersampling. We took all 
records of class 0 (756) and the same amount of class 1, and end up with a total of 1512 records.This strategy resulted in an accuracy score of 0.73, but with high values of precision and recall for both class 0 and 1. 

On the [third notebook](https://github.com/leportella/wiki-analysis/blob/master/notebooks/modelling/03-RandomForest.ipynb), we used a Random Forest Classifier model, adding values of unbalanced 
classes as a parameter of the model. This is, we let the model know that our class 0 had just a few records, while class 1 prevailed with most of records. The precision of class 0 improved, but the recall remained too low for this model to be considered good enough.

On [notebook 4](https://github.com/leportella/wiki-analysis/blob/master/notebooks/modelling/04-RandomForest-undersampling.ipynb) the undersampling strategy was used along with the Random Forest model. The main accuracy was of 0.81 will all precision and recall values of both classes with values above 0.8. This was considered the best model.

Notebooks [5](https://github.com/leportella/wiki-analysis/blob/master/notebooks/modelling/05-GradientBoosting.ipynb) and [6](https://github.com/leportella/wiki-analysis/blob/master/notebooks/modelling/06-GradientBoosting-undersampling.ipynb) tried both strategies with Gradient Boosting. However, these models were not considered as good as the model achieved on Notebook 4. 

One observation must be made: the best hyperparameters for each models were obtained by the 
use of [GridSearchCV](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html), which a technique specialized on finding the best hyperparameters based on a measure of accuracy.


### Final Model

The model had as input 1512 users that had at least one edit reverted. The dataset was divided 
into two parts: the **train** dataset and the **test** dataset, which represented 20% of the 
1512 users.

We used the classifier Random Forest, from the Scikit-learn, the GridSearchCV for finding the 
best hyperparameters and the accuracy score, as the measure of efficiency. The model was 
trained using the **train** dataset (80% of the data). Once the model was trained, the features 
of the **test** dataset was used to get the prediction based on this trained model, and the 
results were compared with the target variable of this **test** dataset. 

We achieved a model with a accuracy of 0.85, which was considered a good prediction. 
The model used three features only: the number of days 
before the first revert, the number of updated per day and the number of edits before the first 
revert. The model showed that the number of updates per day is the most important feature.

The results of this model can be seen below:

![](https://i.imgur.com/FekOlNw.png)

![](https://i.imgur.com/2jze6aE.png)
 


