# Wikipedia Analysis

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


## Initial Analysis and Insights

All analysis and insights will be added as Jupyter Notebook on the folder `notebooks > insights`.
