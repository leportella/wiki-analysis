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
|   |   |-- 00-Insights-Comments.ipynb
|   |   |-- 01-Insights-Edits.ipynb
|   |   `-- Sampling.ipynb
|   |-- modelling
|   |   `-- 01-Baseline_LogisticRegression.ipynb
|   |-- provision
|   |   `-- 00-Edits-Feature-Engineering.ipynb
|   `-- tools.py
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

All analysis and insights will be added as Jupyter Notebook on the folder `notebooks > insights`.
