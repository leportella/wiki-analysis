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
├── data
│   └─ wikipedia 
│      ├── categories.tsv
│      ├── comments.tsv
│      ├── edits.tsv
│      ├── namespaces.tsv
│      └── titles.tsv
├── notebooks 
│   └─ insights
├── requirements.txt
└── README.md
```

However, the data folder will not be included to this folder due to the large 
amount of data.


## Initial Analysis and Insights

All analysis and insights will be added as Jupyter Notebook on the folder `notebooks > insights`.
