# Movies ETL

## Background

Britta is excited to prepare for the hackathon. In data analysis, a hackathon is an event where teams of analysts collaborate to work intensively on a project, using data to solve a problem. Hackathons generally last several days, and teams work around the clock on their projects.

Britta needs to gather data from both Wikipedia and Kaggle, combine them, and save them into a SQL database so that the hackathon participants have a nice, clean dataset to use. To do this, she will follow the ETL process: extract the Wikipedia and Kaggle data from their respective files, transform the datasets by cleaning them up and joining them together, and load the cleaned dataset into a SQL database.

Wikipedia has a ton of information about movies, including budgets and box office returns, cast and crew, production and distribution, and so much more. Luckily, one of Britta's coworkers created a script to go through a list of movies on Wikipedia from 1990 to 2018 and extract the data from the sidebar into a JSON. Unfortunately, her coworker can't find the script anymore and just has the JSON file. We'll need to load in the JSON file into a Pandas DataFrame.

Now that we've loaded the Wikipedia scrape, Britta wants us to include ratings data. However, she knows that her employer, Amazing Prime, won't want to give out their proprietary ratings data to all the hackathon teams. Luckily, she found a dataset on Kaggle that contains ratings data from MovieLens, a site run by the GroupLens research team, which has over 20 million ratings.

Wikipedia doesn't have strict standards on how movie data is presented, so it needs a lot of work to clean up the data and make it usable. Like most web-scraped data, it's in the flexible JSON format to store all kinds of data, but Britta needs to organize it in a structured format before she can send it to SQL—and she's asked me to assist with this task.

Our Wikipedia data is especially messy. As much as editors try to be consistent, each page can be edited by a different person. Besides, because the movie data comes from the sidebar, different movies can have different columns. However, after cleaning the data, the result will be a nice, organized table of data, where every row is a single movie.

## Overview of Project
### Purpose
Amazing Prime loves the dataset and wants to keep it updated on a daily basis. Britta needs my help to create an automated pipeline that takes in new data, performs the appropriate transformations, and loads the data into existing tables. I will need to refactor the code from this module to create one function that takes in the three files—Wikipedia data, Kaggle metadata, and the MovieLens rating data—and performs the ETL process by adding the data to a PostgreSQL database.

Therefore creating an automated ETL pipeline that can be run once daily. The database containing tables: moves and ratings will be refreshed with new data by replace and drop table with each run.

## Analysis And Challenges

## Methodology: Analytics Paradigm

#### 1. Decomposing the Ask


#### 2. Identify the Datasource
* Wikipedia data
  - wiki_file = f'{file_dir}/wikipedia_movies.json'
* Kaggle metadata
  - kaggle_file = f'{file_dir}/movies_metadata.csv'
* MovieLens rating data.
  - ratings_file = f'{file_dir}/ratings.csv'


#### 3. Define Strategy & Metrics
**Resource:** Jupyter Notebook, Python, SQLAlchemy, Postgres 14, pgAdmin, SQL

1. load all the 3 datasets as stated above into Jupyter Notebook to be analysed & potentially cleaned.
1. after cleaning, the dataset will be divided into movies dataframe and ratings dataframe. These dataframes will be insert into Postgres database as 2 tables.

#### 4. Data Retrieval Plan
1. Import raw csv & json data into Jupyter notebook.
1. After cleaning, we will merge the 3 datasets into 2 dataframes: movies and ratings. These 2 dataframes will be inserted into Postgres database: movie_data.


#### 5. Assemble & Clean the Data

##### 5.1 What defines bad data?

Bad data comes in three states:
* Beyond repair
Data beyond repair could be data that has been overwritten or has suffered severe data corruption during storage or transfer (such as power loss during writing, voltage spikes, or hard-drive failures). The worst-case example would be having data with every value missing. All the information is lost and unrecoverable. For data beyond repair, all we can do is delete it and move on.

* Badly damaged
Data that is badly damaged may have good data that we can recover, but it will take time and effort to repair the damaged data. This can be garbled data, with a lot of missing values, from inconsistent sources, or existing in multiple columns. Consider trade-offs to pick the best solution (even if the "best" solution isn't perfect, but rather the "best-available" solution). To repair badly damaged data, try these strategies:
  -Filling in missing data by
  -interpolating between existing data points, or
  -extrapolating from existing data
  -Standardizing units of measure (e.g., monetary values stored in multiple currencies)
  -Consolidating data from multiple columns

* Wrong form
data in the wrong form should usually be fixed—that is, the data is good but can't be used in its current form. "Good" data in the wrong form can be data that is too granular or detailed, numeric data stored as strings, or data that needs to be split into multiple columns (e.g., address data). To remedy good data in the wrong form, try these strategies:
  -Reshape the data
  -Convert data types
  -Parse text data to the correct format
  -Split columns


It's important to document data cleaning assumptions as well as decisions and their motivations. Later decisions depend on earlier decisions made, which can be too much to remember. Any assumptions that were part of an earlier decision can, if forgotten, ruin later steps.
Transforming a messy dataset into a clean dataset is an iterative process. As you clean one part of the data, you may reveal something messy in another part of the data. Sometimes that means unwinding a lot of work that you've already done and having to redo it with a slight change. Documenting why a particular step is necessary will show you how to redo it without introducing more errors.

One thing to watch out for is to make nondestructive edits as much as possible while designing your pipeline. That means it's better to keep your raw data in one variable, and put the cleaned data in another variable. It takes up more memory, but it makes tracking the iterative process of data cleaning easier.


##### 5.2 Data Cleaning Strategy

After inspecting the competing data side by side, this is the decision on cleaning the dataset:
```
# Competing data:
# Wiki                     Movielens                Resolution
#--------------------------------------------------------------------------
# title_wiki               title_kaggle             Drop Wikipedia
# running_time             runtime                  Keep Kaggle; fill in zeros with Wikipedia data.
# budget_wiki              budget_kaggle            Keep Kaggle; fill in zeros with Wikipedia data.
# box_office               revenue                  Keep Kaggle; fill in zeros with Wikipedia data.
# release_date_wiki        release_date_kaggle      Drop Wikipedia
# Language                 original_language        Drop Wikipedia
# Production company(s)    production_companies     Drop Wikipedia
```


#### 6. Analyse for Trends

Results from data filter above will give us more in-depth information regarding the different departments, roles and future retirees.

#### 7. Acknowledging Limitations
There are moments when we have to analyse the "unclean" dataset and make decision on the tradeoff between time spent vs how many dataset to save / drop. In this situation, due to the low percentage of the "corrupt" data, the decision has been to drop the data from our analysis.

#### 8. Making the Call:
The "Proper" Conclusion is indicated below on [Summary](#summary)

## Analysis


>Total Employees

![Total Employees](total_employees.png)


## Summary


## Appendix
