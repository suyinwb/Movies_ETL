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
Amazing Prime loves the dataset and wants to keep it updated on a daily basis. Britta needs my help to create an automated pipeline that takes in new data, performs the appropriate transformations, and loads the data into existing tables. I will need to refactor the code from this module to create one function that takes in the three files -  Wikipedia data, Kaggle metadata, and the MovieLens rating data; and performs the ETL process by adding the data to a PostgreSQL database.

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

* Load all the 3 datasets as stated above into Jupyter Notebook to be analysed & potentially cleaned.
* After cleaning, the dataset will be divided into movies dataframe and ratings dataframe. These dataframes will be insert into Postgres database as 2 tables.

#### 4. Data Retrieval Plan
* Import raw csv & json data into Jupyter notebook.
* After cleaning, we will merge the 3 datasets into 2 dataframes: movies and ratings. These 2 dataframes will be inserted into Postgres database: movie_data.


#### 5. Assemble & Clean the Data

##### 5.1. What defines bad data?

Bad data comes in three states:

5.1.1. Beyond repair
Data beyond repair could be data that has been overwritten or has suffered severe data corruption during storage or transfer (such as power loss during writing, voltage spikes, or hard-drive failures). The worst-case example would be having data with every value missing. All the information is lost and unrecoverable. For data beyond repair, all we can do is delete it and move on.

5.1.2. Badly damaged
Data that is badly damaged may have good data that we can recover, but it will take time and effort to repair the damaged data. This can be garbled data, with a lot of missing values, from inconsistent sources, or existing in multiple columns. Consider trade-offs to pick the best solution (even if the "best" solution isn't perfect, but rather the "best-available" solution). To repair badly damaged data, try these strategies:
  * Filling in missing data by
  * interpolating between existing data points, or
  * extrapolating from existing data
  * Standardizing units of measure (e.g., monetary values stored in multiple currencies)
  * Consolidating data from multiple columns

5.1.3. Wrong form
data in the wrong form should usually be fixed—that is, the data is good but can't be used in its current form. "Good" data in the wrong form can be data that is too granular or detailed, numeric data stored as strings, or data that needs to be split into multiple columns (e.g., address data). To remedy good data in the wrong form, try these strategies:
  * Reshape the data
  * Convert data types
  * Parse text data to the correct format
  * Split columns


It's important to document data cleaning assumptions as well as decisions and their motivations. Later decisions depend on earlier decisions made, which can be too much to remember. Any assumptions that were part of an earlier decision can, if forgotten, ruin later steps.
Transforming a messy dataset into a clean dataset is an iterative process. As cleaning one part of the data, it may reveal something messy in another part of the data. Sometimes that means unwinding a lot of work that have already done and having to redo it with a slight change. Documenting why a particular step is necessary will show how to redo it without introducing more errors.

One thing to watch out for is to make nondestructive edits as much as possible while designing the pipeline. That means it's better to keep raw data in one variable, and put the cleaned data in another variable. It takes up more memory, but it makes tracking the iterative process of data cleaning easier.


##### 5.2. Data Cleaning Strategy

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

Results from data cleaning above gives us a more defined and consistent data to be added into our Postgres database on a regular basis.
As we progressed with the ETL process as stated here:
1. Wikipedia Data Cleaning
2. Kaggle Data Cleaning
3. Connect To database
* 3.1 Replace movies table
* 3.2. Drop ratings table
4. Insert ratings data into database

Add all the 4 steps above into the function below to enable automation.
```
extract_transform_load()
```


#### 7. Acknowledging Limitations
There are moments when we have to analyse the "unclean" dataset and make decision on the tradeoff between time spent vs how many dataset to save / drop. In this situation, due to the low percentage of the "corrupt" data, the decision has been to drop the data from our analysis.

As I have both PostgreSQL 11 and PostgreSQL 14, I have to use port 5433 for PostgreSQL 14.

#### 8. Making the Call:
The "Proper" Conclusion is indicated below on [Summary](#summary)

## Analysis

After loading the raw data, we start by asking these questions:
1. Which dataset seems to have more outliers?
2. Which dataset seems to have more missing data points?
3. If we were to fill in the missing data points of one set with the other, which would be more likely to give us consistent data?
4. Is it better to start with a base of consistent data and fill in missing points with possible outliers?
5. Or is it better to start with a base of data with outliers and fill in missing points with more consistent data?

To check, we start by plotting the data into histograms and/or scatter plots.
* A histogram is a bar chart that displays how often a data point shows up in the data. A histogram is a quick, visual way to get a sense of how a dataset is distributed.A quick, easy way to do this is to look at a histogram of the rating distributions, and then use the describe() method to print out some stats on central tendency and spread.
* A scatter plot is a great way to give us a sense of how similar the columns are to each other. If the two columns were exactly the same, we'd see a scatter plot of a perfectly straight line. Any wildly different values will show up as dots far from that central line, and if one column is missing data, those values will fall on the x-axis or y-axis.


>Ratings Histogram

![Ratings](resources/ratings_histogram.png)

For ratings data: look at the statistics of the actual ratings and see if there are any glaring errors. That seems to make sense. People are more likely to give whole number ratings than half, which explains the spikes in the histogram. The median score is 3.5, the mean is 3.53, and all the ratings are between 0 and 5.

>Wikipedia Running Time VS Kaggle Runtime

![Wikipedia Running Time VS Kaggle Runtime](resources/running_time_vs_runtime.png)

There are more data points on the origin of the Y axis than on the origin of the X axis. Since the X axis is Wikipedia and the Y axis is Kaggle, this means there are more missing entries in the Wikipedia data set than in the Kaggle data set. Also, most of the runtimes are pretty close to each other but the Wikipedia data has some outliers, so the Kaggle data is probably a better choice here. However, we can also see from the scatter plot that there are movies where Kaggle has 0 for the runtime but Wikipedia has data, so we'll fill in the gaps with Wikipedia data.

>Box Office vs Revenue Scatter Plot

![Box Office vs Revenue](resources/box_office_vs_revenue.png)

We might be getting thrown off by the scale of that large data point, so we will only look at the scatter plot for everything less than $1 billion in box_office with **[movies_df['box_office'] < 10**9]**.


>Wikipedia VS Kaggle Release Dates

![Wikipedia VS Kaggle Release Dates](resources/wiki_vs_kaggle_releasedates.png)

For release_date_wiki and release_date_kaggle, we can't directly make a scatter plot, because the scatter plot only works on numeric data. However, there's a tricky workaround that we can use. We'll use the regular line plot (which can plot date data), and change the style to only put dots by adding style='.' to the plot() method. There is a wild outlier around 2006. Choose some rough cutoff dates to single out that one movie, look for any movie whose release date according to Wikipedia is after 1996, but whose release date according to Kaggle is before 1965.


## Summary

Once we combine all the components of [Clean Wikipedia Data](#clean-wikipedia-data), [Clean Kaggle Data](#clean-kaggle-data) and [Database Connect & Insertion](#database-connect--insertion) into a function, we can invoke that function to run automatically with a cron as an automated ETL data pipeline. This in turn will update the database daily with the results from our ETL pipeline. See the below output from our database.

>Checking data in Movies table

![Movies Query](resources/movies_query.png)



>Checking data in Ratings table

![Ratings Query](resources/ratings_query.png)

## Appendix

### Clean Wikipedia Data

```
####################################################################################################
#WIKIPEDIA DATA CLEANING
####################################################################################################


# 4. Write a list comprehension to iterate through the cleaned wiki movies list
wiki_movies=[movie for movie in wiki_movies_raw if "No. of episodes" not in movie]
# and call the clean_movie function on each movie.
clean_movies=[clean_movie(movie) for movie in wiki_movies]

# 5. Read in the cleaned movies list from Step 4 as a DataFrame.
wiki_movies_df = pd.DataFrame(clean_movies)

# 6. Write a try-except block to catch errors while extracting the IMDb ID using a regular expression string and
#  dropping any imdb_id duplicates. If there is an error, capture and print the exception.
try:
    wiki_movies_df['imdb_id']=wiki_movies_df['imdb_link'].str.extract(r'(tt\d{7})')    
    wiki_movies_df.drop_duplicates(subset='imdb_id', inplace=True)
except:
    print(wiki_movies_df)

#  7. Write a list comprehension to keep the columns that don't have null values from the wiki_movies_df DataFrame.
wiki_columns_to_keep = [column for column in wiki_movies_df.columns if wiki_movies_df[column].isnull().sum() < len(wiki_movies_df) * 0.9]
wiki_movies_df = wiki_movies_df[wiki_columns_to_keep]

# 8. Create a variable that will hold the non-null values from the “Box office” column.
box_office = wiki_movies_df['Box office'].dropna()
print(len(box_office))

# 9. Convert the box office data created in Step 8 to string values using the lambda and join functions.
box_office = box_office.apply(lambda x: ' '.join(x) if type(x) == list else x)   

# 10. Write a regular expression to match the six elements of "form_one" of the box office data.
form_one = r'\$\s*\d+\.?\d*\s*[mb]illi?on'
matches_form_one=box_office.str.contains(form_one, flags=re.IGNORECASE, na=False)
# 11. Write a regular expression to match the three elements of "form_two" of the box office data.
form_two = r'\$\s*\d{1,3}(?:[,\.]\d{3})+(?!\s[mb]illion)'
matches_form_one=box_office.str.contains(form_two, flags=re.IGNORECASE, na=False)


# 12. Add the parse_dollars function.
def parse_dollars(s):
    # if s is not a string, return NaN
    if type(s) != str:
        return np.nan

    # if input is of the form $###.# million
    if re.match(r'\$\s*\d+\.?\d*\s*milli?on', s, flags=re.IGNORECASE):

        # remove dollar sign and " million"
        s = re.sub('\$|\s|[a-zA-Z]','', s)

        # convert to float and multiply by a million
        value = float(s) * 10**6

        # return value
        return value

    # if input is of the form $###.# billion
    elif re.match(r'\$\s*\d+\.?\d*\s*billi?on', s, flags=re.IGNORECASE):

        # remove dollar sign and " billion"
        s = re.sub('\$|\s|[a-zA-Z]','', s)

        # convert to float and multiply by a billion
        value = float(s) * 10**9

        # return value
        return value

    # if input is of the form $###,###,###
    elif re.match(r'\$\s*\d{1,3}(?:[,\.]\d{3})+(?!\s[mb]illion)', s, flags=re.IGNORECASE):

        # remove dollar sign and commas
        s = re.sub('\$|,','', s)

        # convert to float
        value = float(s)

        # return value
        return value

    # otherwise, return NaN
    else:
        return np.nan


# 13. Clean the box office column in the wiki_movies_df DataFrame.
wiki_movies_df['box_office'] = box_office.str.extract(f'({form_one}|{form_two})', flags=re.IGNORECASE)[0].apply(parse_dollars)
wiki_movies_df.drop('Box office', axis=1, inplace=True)

# 14. Clean the budget column in the wiki_movies_df DataFrame.
budget = wiki_movies_df['Budget'].dropna()
budget = budget.map(lambda x: ' '.join(x) if type(x) == list else x)
budget = budget.str.replace(r'\$.*[-—–](?![a-z])','$', regex=True)
matches_form_one = budget.str.contains(form_one, flags=re.IGNORECASE, na=False)
matches_form_two = budget.str.contains(form_two, flags=re.IGNORECASE, na=False)
budget = budget.str.replace(r'\[\d+\]\s*', '', regex=True)
#budget[~matches_form_one & ~matches_form_two]
wiki_movies_df['budget'] = budget.str.extract(f'({form_one}|{form_two})', flags=re.IGNORECASE)[0].apply(parse_dollars)
#wiki_movies_df.drop('Budget', axis=1, inplace=True)

# 15. Clean the release date column in the wiki_movies_df DataFrame.
release_date = wiki_movies_df['Release date'].dropna().apply(lambda x: ' '.join(x) if type(x) == list else x)
date_form_one = r'(?:January|February|March|April|May|June|July|August|September|October|November|December)\s[123]?\d,\s\d{4}'
date_form_two = r'\d{4}.[01]\d.[0123]\d'
date_form_three = r'(?:January|February|March|April|May|June|July|August|September|October|November|December)\s\d{4}'
date_form_four = r'\d{4}'
release_date.str.extract(f'({date_form_one}|{date_form_two}|{date_form_three}|{date_form_four})', flags=re.IGNORECASE)
wiki_movies_df['release_date'] = pd.to_datetime(release_date.str.extract(f'({date_form_one}|{date_form_two}|{date_form_three}|{date_form_four})')[0], infer_datetime_format=True)

# 16. Clean the running time column in the wiki_movies_df DataFrame.
running_time = wiki_movies_df['Running time'].dropna().apply(lambda x: ' '.join(x) if type(x) == list else x)
#running_time[running_time.str.contains(r'^\d*\s*m', flags=re.IGNORECASE, na=False) != True]
running_time_extract = running_time.str.extract(r'(\d+)\s*ho?u?r?s?\s*(\d*)|(\d+)\s*m')
running_time_extract = running_time_extract.apply(lambda col: pd.to_numeric(col, errors='coerce')).fillna(0)
wiki_movies_df['running_time'] = running_time_extract.apply(lambda row: row[0]*60 + row[1] if row[2] == 0 else row[2], axis=1)
wiki_movies_df.drop('Running time', axis=1, inplace=True)
```
### Clean Kaggle Data

```
####################################################################################################
#KAGGLE DATA CLEANING
####################################################################################################

# 2. Clean the Kaggle metadata.
kaggle_metadata = kaggle_metadata[kaggle_metadata['adult'] == 'False'].drop('adult',axis='columns')
kaggle_metadata['video']=kaggle_metadata['video']=='True'
kaggle_metadata['budget'] = kaggle_metadata['budget'].astype(int)
kaggle_metadata['id'] = pd.to_numeric(kaggle_metadata['id'], errors='raise')
kaggle_metadata['popularity'] = pd.to_numeric(kaggle_metadata['popularity'], errors='raise')
kaggle_metadata['release_date'] = pd.to_datetime(kaggle_metadata['release_date'])
#ratings['timestamp'] = pd.to_datetime(ratings['timestamp'], unit='s')

# 3. Merged the two DataFrames into the movies DataFrame.
movies_df=pd.merge(wiki_movies_df, kaggle_metadata, on='imdb_id', suffixes=['_wiki','_kaggle'])

# 4. Drop unnecessary columns from the merged DataFrame.
movies_df.drop(columns=['title_wiki', 'release_date_wiki','Language', 'Production company(s)'], inplace=True)

# 5. Add in the function to fill in the missing Kaggle data.
def fill_missing_kaggle_data(df, kaggle_column, wiki_column):
    df[kaggle_column] = df.apply(
        lambda row: row[wiki_column] if row[kaggle_column] == 0 else row[kaggle_column]
        , axis=1)
    df.drop(columns=wiki_column, inplace=True)

# 6. Call the function in Step 5 with the DataFrame and columns as the arguments.
fill_missing_kaggle_data(movies_df, 'runtime', 'running_time')
fill_missing_kaggle_data(movies_df, 'budget_kaggle', 'budget_wiki')
fill_missing_kaggle_data(movies_df, 'revenue', 'box_office')

# 7. Filter the movies DataFrame for specific columns.
movies_df = movies_df.loc[:, ['imdb_id','id','title_kaggle','original_title','tagline','belongs_to_collection','url','imdb_link',
                   'runtime','budget_kaggle','revenue','release_date_kaggle','popularity','vote_average','vote_count',
                   'genres','original_language','overview','spoken_languages','Country',
                   'production_companies','production_countries','Distributor',
                   'Producer(s)','Director','Starring','Cinematography','Editor(s)','Writer(s)','Composer(s)','Based on'
                  ]]

# 8. Rename the columns in the movies DataFrame.
movies_df.rename({'id':'kaggle_id',
              'title_kaggle':'title',
              'url':'wikipedia_url',
              'budget_kaggle':'budget',
              'release_date_kaggle':'release_date',
              'Country':'country',
              'Distributor':'distributor',
              'Producer(s)':'producers',
              'Director':'director',
              'Starring':'starring',
              'Cinematography':'cinematography',
              'Editor(s)':'editors',
              'Writer(s)':'writers',
              'Composer(s)':'composers',
              'Based on':'based_on'
             }, axis='columns', inplace=True)

# 9. Transform and merge the ratings DataFrame.
rating_counts = ratings.groupby(['movieId','rating'], as_index=False).count() \
            .rename({'userId':'count'}, axis=1) \
            .pivot(index='movieId',columns='rating', values='count')

movies_with_ratings_df = pd.merge(movies_df, rating_counts, left_on='kaggle_id', right_index=True, how='left')
movies_with_ratings_df[rating_counts.columns] = movies_with_ratings_df[rating_counts.columns].fillna(0)
```
### Database Connect & Insertion

```
####################################################################################################
#CONNECT TO DATABASE
####################################################################################################

#Postgres 14 at port 5433
db_string = f"postgresql://postgres:{db_password}@127.0.0.1:5433/movie_data"

engine = create_engine(db_string)
movies_df.to_sql(name='movies', con=engine, if_exists='replace')

#DROP ratings table from movie_data if exists

#DBAPI connection
#Documentation: https://docs.sqlalchemy.org/en/12/core/connections.html
Connection = engine.raw_connection()
#Create a cursor object using the cursor() method
Cursor = Connection.cursor()

#Drop the ratings table if it already exists
Cursor.execute('DROP TABLE IF EXISTS ratings')

#Commit database changes & close connection
Connection.commit()
#Cursor.close()
Connection.close()

####################################################################################################
# DATABASE CONNECTION CLOSED
####################################################################################################


####################################################################################################
# INSERT RATINGS DATA INTO DATABASE
####################################################################################################

# create a variable for the number of rows imported
# print out the range of rows that are being imported
# increment the number of rows imported by the chunksize
# print that the rows have finished importing


rows_imported = 0
# get the start_time from time.time()
start_time = time.time()
for data in pd.read_csv(f'{file_dir}ratings.csv', chunksize=1000000):
    print(f'importing rows {rows_imported} to {rows_imported + len(data)}...', end='')
    data.to_sql(name='ratings', con=engine, if_exists='append')
    rows_imported += len(data)

    # add elapsed time to final print out
    print(f'Done. {time.time() - start_time} total seconds elapsed')
```
