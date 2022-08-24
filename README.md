## Final Project Submission

Please fill out:
* Student name:  Abel Otieno Odhiambo
* Student pace:  full time
* Scheduled project review date/time: 
* Instructor name: Antonny Muiko
* Blog post URL:


# Your code here - remember to use markdown cells for comments as well!


   ##                      MICROSOFT DATA ANALYSIS PROJECT

# 1.Business Understanding

In my project Microsoft wants to start a movie studio and my analysis is based on my main objectives, which will enable microsoft to come_up with a profitable movie studio.

### Objectives

* Which genre of Movie is the best
* What is the distribution of a Movie genre and audience

# 2.Data Understanding

In my project i need to get data that shows movie categories and sales

### Collecting Our Data

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import sqlite3
%matplotlib inline

## we load the provided data to see the most preferable dataset to use in our project 

df = pd.read_csv('bom.movie_gross.csv')
df.head()

df.info()

df2 = pd.read_csv('tmdb.movies.csv')
df2.head()

df2.info()

conn =sqlite3.connect("im.db")

tables_name = """SELECT name 
                      AS 'Table Names' 
                      FROM sqlite_master 
                      WHERE type='table';"""

pd.read_sql(tables_name, conn)

## Preffered Data

 From the above data we can see that  `tmdb.movies.csv`   and  `im.db`  data are our best preferred data due to the data accuracy
  
  

# Cleaning our data

df = pd.read_csv("tmdb.movies.csv",index_col=0)
df.head()

df.info()

The above data is more accuate since all columns have all entries

Checking for null Values

df.isnull().sum()

We can clear see out of the 26517 there is no single null value so the data is accurate.

# 3.Data Analysis

## FIRST DATASET ANALYSIS 

df.describe()

# checking no of rows and columns
df.shape















# SECOND DATA SET ANALYSIS

conn = sqlite3.connect('im.db')

tables_name = """SELECT name 
                      AS 'Table Names' 
                      FROM sqlite_master 
                      WHERE type='table';"""

pd.read_sql(tables_name, conn)

### How Many Genres Do We have?

# lets see how many genre do we have do we have
genres = """
   SELECT genres
     FROM movie_basics
 GROUP BY genres

;
"""
data = pd.read_sql(genres ,conn).dropna()
data.count()

### Which Are The Top Genres?

genre_ratings = """
   SELECT genres,avg(averagerating) AS average_ratings
     FROM movie_basics
     JOIN movie_ratings
    USING(movie_id)
 GROUP BY genres
 ORDER BY average_ratingS DESC
 


;
"""
pd.read_sql(genre_ratings, conn).dropna()

### The top five highest rated genres are;
* Comedy,Documentary,Fantasy	
* Documentary,Family,Musical	
* History,Sport	                
* Music,Mystery	                
* Game-Show	                    

### The least rated genres;
* Comedy,Musical,Sport	
* Adult,Horror	
* Adventure,Crime,Romance	
* History,Sci-Fi,Thriller	
* Crime,Music	

### Which are the most viewed genres?

genre_ratings = """
   SELECT genres,sum(numvotes) AS People_viewed
     FROM movie_basics
     JOIN movie_ratings
    USING(movie_id)
 GROUP BY genres
 ORDER BY people_viewed asc

 


;
"""
pd.read_sql(genre_ratings, conn).dropna()

### The top five most popular movies genre viewed are;
* Action,Adventure,Sci-Fi	
* Action,Adventure,Fantasy	
* Adventure,Animation,Comedy	
* Drama	
* Comedy,Drama,Romance	

### The least popular movies;
* Action,Crime,Musical	
* Action,Documentary,Horror	
* Comedy,Documentary,Fantasy	
* Crime,Western	
* Family,War	


### Does the numbers of viewers relate with the ratings?

genre_counts = """
   SELECT genres, sum(numvotes) AS people_viewed, avg(averagerating) as average_rating
     FROM movie_basics
     JOIN movie_ratings
    USING(movie_id)
 GROUP BY genres
 HAVING people_viewed between 50000 and 1000000
 ORDER BY people_viewed
 



;
"""
pd.read_sql(genre_counts, conn)
data = pd.read_sql(genre_counts, conn).dropna()
data.describe()

data["average_rating"].corr(data["people_viewed"])

plt.boxplot(data.people_viewed)

plt.boxplot(data.average_rating)

plt.scatter(data.people_viewed, data.average_rating)
plt.xlabel("people_viewed")
plt.ylabel("average_rating")

There is no relationship between number of people viewing a genre and it's rating

### Does a movie length affect its rating?

movie_length = """
  SELECT movie_id, runtime_minutes AS length, averagerating AS rating
    FROM movie_basics
    JOIN movie_ratings
    USING(movie_id)
GROUP BY movie_id
HAVING length < 200

    


"""
pd.read_sql(movie_length, conn).dropna()

data = pd.read_sql(movie_length,conn).dropna()
data["rating"].corr(data["length"])

plt.scatter(data.length, data.rating)
plt.xlabel("length")
plt.ylabel("rating")
plt.title("Movie_length and Rating")

There is no relationship between the legth of a movie and it's rating

