# Movies ETL

## Background

Britta is excited to prepare for the hackathon. In data analysis, a hackathon is an event where teams of analysts collaborate to work intensively on a project, using data to solve a problem. Hackathons generally last several days, and teams work around the clock on their projects.

Britta needs to gather data from both Wikipedia and Kaggle, combine them, and save them into a SQL database so that the hackathon participants have a nice, clean dataset to use. To do this, she will follow the ETL process: extract the Wikipedia and Kaggle data from their respective files, transform the datasets by cleaning them up and joining them together, and load the cleaned dataset into a SQL database.

## Overview of Project
### Purpose
After executing the code and checking the results, a few folks are are appearing twice. Maybe they moved departments?

## Analysis And Challenges

## Methodology: Analytics Paradigm

#### 1. Decomposing the Ask



#### 2. Identify the Datasource
* employees.csv
* dept_manager.csv
* dept_emp.csv
* salaries.csv
* titles.csv
* departments.csv

#### 3. Define Strategy & Metrics
**Resource:** Postgres 11, pgAdmin, SQL

1. create ERD based on the 6 csv data above
1.

#### 4. Data Retrieval Plan
Import csv data into the database tables using pgAdmin import function.


#### 5. Assemble & Clean the Data

* No data cleaning required
* ???

```
SELECT ...
FROM ...
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
```


#### 6. Analyse for Trends

Results from data filter above will give us more in-depth information regarding the different departments, roles and future retirees.

#### 7. Acknowledging Limitations
pgAdmin is an old application for Postgres front end GUI. So sometimes when closing the application, the database will be corrupted and wiped out by pgAdmin. Do a full backup before closing.

#### 8. Making the Call:
The "Proper" Conclusion is indicated below on [Summary](#summary)

## Analysis


>Total Employees

![Total Employees](total_employees.png)


## Summary


## Appendix
