# **Movies-ETL**

####  In this module we are using ELT method to clean the data files, parse the data that we extracted to make it looks how we want. Merge parsed data sets and load the load the merged sets into pgAdmin for further use.

## Step1 
---
*Using the ***ELT_function_test*** file by step:*

1. Created a function to read in three separate files
2. Read in the kaggle metadata and MovieLens ratings CSV files as Pandas DataFrames.
3. Opened the Wikipedia JSON file and use the json() function to convert the JSON data to raw data.
4. Read in the raw wiki movie data as a Pandas DataFrame.
5. Use the code return the three DataFrames
``` python
return wiki_movies_df, kaggle_metadata, ratings
``` 
6. Create a variables provide to a path to the Wikipedia data, the Kaggle metadata, and the MovieLens rating data files.
7. Set the three variables in Step 6 equal to the function created in Step 1.
8. Set the DataFrames from the return statement equal to the file names in Step 6. In this step, you are reassigning the variables created in Step 6 to the variables in the return statement.
To make sure your dataframes look correct we use the ***.head()***.
```python
kaggle_metadata.head()
```  
![example](/Images/kaggle_metadata_head.png)

```python 
ratings
```
![example](/Images/ratings.png)


## Step 2
---
*Extract and Transform the Wikipedia Data with the ETL_clean_wiki_movies.ipynb:*

1. Create the code for the clean movie function that takes in the argument "movie".
2. Add the function you created in step 1 that reads in the three data files.
3. Inside the function you created in step 1's python file, remove the code that creates the ***wiki_movies_df DataFrame*** from the ***wiki_movies_raw file***, then write a list comprehension that filters out TV shows from the ***wiki_movies_raw*** file.
4. Write a list comprehension to iterate through the cleaned wiki movies list that you created in Step 3.
5. Read in the cleaned movies list from Step 4 as a DataFrame.
6. Write a **try-except block** that will catch errors while extracting the IMDb IDs with a regular expression string and dropping any imdb_id duplicates. If there is an error, capture and print the exception.
7. Write a list comprehension to keep the columns that have non-null values from the DataFrame created in Step 5, then create a ***wiki_movies_df*** DataFrame from the list. 
8. Create a variable that will hold all the non-null values from the **"Box office"** column
9. Convert the box office data created in Step 8 to string values using the **lambda and join functions**.
10. Write a regular expression to match the six elements of **form_one** of the box office data.
11. Write a regular expression to match the three elements of **form_two** of the box office data.
12. Add the **parse_dollars()** function.
13. Add the code that cleans the box office column in the ***wiki_movies_df*** DataFrame using the **form_one and form_two** lists created in Steps 10 and 11, respectively.
14. Add code that cleans the budget column in the ***wiki_movies_df*** DataFrame.
15. Add code that cleans the release date column in the ***wiki_movies_df*** DataFrame. 
16. Add code that cleans the running time column in the ***wiki_movies_df*** DataFrame.
17. Use the variables provided to create a path to the Wikipedia data, the Kaggle metadata, and the MovieLens rating data files.
18. Set the three variables in Step 17 equal to the function created in Deliverable 1.
19. Set the ***wiki_movies_df*** equal to the wiki_file variable.
20. Add the columns from ***wiki_movies_df*** DataFrame to a list, and confirm that they are the same as this image:
```python
wiki_movies_df.columns.to_list()
```
![](/Images/wiki_movie_list.png)

## Step 3
---
  *Extract and Transform the Kaggle Data using the ETL_clean_kaggle_data.ipynb*

1. Add the function was created in our very first python file that reads in the three data files and creates the kaggle_metadata and ratings DataFrames. Also, make sure you add in all of the code from the **ETL_clean_wiki_movies.ipynb file**
2. Below the code that cleans the running time column in the **wiki_movies_df** DataFrame from **ETL_clean_wiki_movies.ipynb**, add the code that cleans the **Kaggle metadata**.
3. Merge the **wiki_movies_df** DataFrame and the **kaggle_metadata** DataFrames, then name the new DataFrame, **movies_df**.
4. Drop unnecessary columns from the **movies_df** DataFrame.
5. Add the ***fill_missing_kaggle_data()*** function that fills in the missing Kaggle data on the **movies_df** DataFrame.
6. Call the ***fill_missing_kaggle_data()*** function with the **movies_df** DataFrame and the Kaggle and Wikipedia columns to be cleaned as the arguments.
7. Filter the **movies_df** DataFrame to keep the necessary columns.
8. Rename the columns in the **movies_df** DataFrame.
9. Transform and merge the ratings DataFrame with the **movies_df** DataFrame, name the new DataFrame **movies_with_ratings_df**, then clean the **movies_with_ratings_df** DataFrame.
10. Use the variables provided to create a path to the Wikipedia data, the Kaggle metadata, and the MovieLens rating data files.
11. Set the three variables from Step 17 of **ETL_clean_wiki_movies.ipynb** equal to the function created in the first python file we created.
12. Set the DataFrames from the return statement after Step 9 equal to the file names in Step 11.
13. Check that your **wiki_movies_df** DataFrame is the same as in **ETL_clean_wiki_movies.ipynb.**
14. Confirm that your DataFrames look correct using the ***.head()*** method.
  

```python 
movies_with_ratings_df.head()
```
![](/Images/movies_vs_raitings.png)

```python 
movies_df.head()
```
![](/Images/movies_df.png)

## Step 4
---
*By using our knowledge of Python, Pandas, the ETL process, code refactoring, and PostgreSQL to add the movies_df DataFrame and MovieLens rating CSV data to a SQL database.*

1.  Create database engine to communicate with SQL server.
```python
"postgres://[user]:[password]@[location]:[port]/[database]"
```
2. Create the database engine.

```python
engine = create_engine(db_string)
```
3. Save the **movies_df** DataFrame to a SQL table.
```python
movies_df.to_sql(name='movies', con=engine, if_exists = 'replace')
```
4. The ratings data is too large to import in one statement, so it has to be divided into "chunks" of data. 
         
    + create a variable for the number of rows imported
    + print out the range of rows that are being imported
    + increment the number of rows imported by the size of 'data'
    + print that the rows have finished importing
    + also as an optional step we added the bild in ***time*** module to print the total amount of time elapsed at every step.
```python
start_time = time.time()

time.time() - start_time
```
---
![](/Images/Time.png)

*Once the cell finishes running, we check that the tables imported correctly using the pgAdmin. Confirm that the movies table has 6,052 rows and the ratings table has 26,024,289 rows. We just extracted really messy and almost unusable data, combed through it carefully to transform it, and then loaded it into a SQL database.*
 ---
  
![](/Images/movies_query.png)
![](/Images/ratings_query.png)