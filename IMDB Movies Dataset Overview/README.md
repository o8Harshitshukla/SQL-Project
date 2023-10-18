# IMDB Movies Dataset Overview

Welcome to the IMDB Movies Dataset! This dataset provides valuable information about various movies, including details such as movie titles, ratings, revenue, release year, genre, director, runtime, and the number of votes. This dataset is a valuable resource for movie enthusiasts, analysts, and data scientists, as it allows for in-depth exploration of movie-related trends and insights.

Let's take a closer look at the data dictionary to understand the available fields and their meanings:

## Data Dictionary

![image](https://github.com/Nasir151/SQL-Projects/assets/94509995/844cb861-c7e9-431c-a6ea-5efbbc2a5da6)


With this dataset, you can explore a wide range of questions related to movie ratings, revenue, genre trends, and more. Whether you're interested in analyzing the factors that contribute to a movie's success or studying the preferences of moviegoers, this dataset provides a rich source of information to delve into the world of cinema. Enjoy your exploration and analysis!

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Question 1: How many movies are ‘Superhit’ in the year 2014?

    Segment the movies based on the below criteria using CASE WHEN
    Revenue greater than or equal to 300 million: Blockbuster
    Revenue between 200 and 299.99 million: Superhit
    Revenue between 100 and 199.99 million: Hit
    Else: Normal

```sql
SELECT result, Count(title) AS number_of_movies
FROM (
    SELECT title, genre, revenue_millions, year,
    CASE
        WHEN revenue_millions >= 300 THEN 'Blockbuster'
        WHEN revenue_millions BETWEEN 200 AND 299.99 THEN 'Super Hit'
        WHEN revenue_millions BETWEEN 100 AND 199.99 THEN 'Hit'
        ELSE 'Normal'
    END AS result
    FROM imdb_movies
) A
WHERE year = 2014
GROUP BY result;
```

#### Question 2: What is the total revenue of the Blockbuster movies in the year 2015?

    Segment the movies based on the below criteria using CASE WHEN
    Revenue greater than or equal to 300 million: Blockbuster
    Revenue between 200 and 299.99 million: Superhit
    Revenue between 100 and 199.99 million: Hit
    Else: Normal

```sql
SELECT result, Sum(revenue_millions) AS tot_revenue
FROM (
    SELECT title, genre, revenue_millions, year,
    CASE
        WHEN revenue_millions >= 300 THEN 'Blockbuster'
        WHEN revenue_millions BETWEEN 200 AND 299.99 THEN 'Super Hit'
        WHEN revenue_millions BETWEEN 100 AND 199.99 THEN 'Hit'
        ELSE 'Normal'
    END AS result
    FROM imdb_movies
) A
WHERE year = 2015
GROUP BY result;
```

#### Question 3: How many movies are a 'Must Watch' in the entire dataset?

    Segment the movies based on ratings using IF statement as below:
    Rating greater than or equal to 8: ‘Must Watch’
    Rating between 6 and 7.9: ‘Can Watch’
    Rating below 6: ‘Avoid’

```sql
SELECT result, Count(title) AS number_of_movies
FROM (
    SELECT title, genre, rating, year,
    IF(rating >= 8, 'Must Watch',
        IF(rating BETWEEN 6 AND 7.9, 'Can Watch',
            'Avoid')
    ) AS result
    FROM imdb_movies
) A
GROUP BY result;
```
#### Question 4: Compute total revenue by year and genre for all the movies with ratings less than 7. What is the total revenue of movies with ratings less than 7 in the year 2016 for 'Drama'?

```sql
SELECT year, genre, SUM(IF(rating < 7, revenue_millions, 0))
FROM imdb_movies
WHERE year = 2016 AND genre = 'Drama'
GROUP BY year, genre;
```

#### Question 5: What is the revenue contribution of 'The Avengers' in the genre 'Action' in the year 2012? (Answer without % sign)

```sql
SELECT title, revenue_millions * 100 / (
    SELECT Sum(revenue_millions)
    FROM imdb_movies
    WHERE genre = 'Action' AND year = 2012
) AS perc_revenue
FROM imdb_movies
WHERE title = 'The Avengers';
```

#### Question 6: What is the revenue contribution of the genre 'Comedy' of total revenue in the year 2016? (Answer it without % sign)

```sql
SELECT genre, Sum(revenue_millions) * 100 / (
    SELECT Sum(revenue_millions)
    FROM imdb_movies
    WHERE year = 2016
) AS perc_revenue
FROM imdb_movies
WHERE genre = 'Comedy' AND year = 2016;
```

#### Question 7: Which genre has the highest revenue collection for the last 3 years? Enter its revenue.

```sql
SELECT Max(A.tot_revenue)
FROM (
    SELECT genre, Sum(revenue_millions) AS tot_revenue
    FROM imdb_movies
    WHERE year IN (2014, 2015, 2016)
    GROUP BY genre
) AS A;
```

#### Question 8: Find out the director who has the highest average rating of his/her movies. Enter the rating.

```sql
SELECT Max(A.avg_rating)
FROM (
    SELECT director, Avg(rating) AS avg_rating
    FROM imdb_movies
    GROUP BY director
) AS A;
```

#### Question 9: How many movies have revenue greater than the highest movie revenue of the 'Adventure' genre?

```sql
SELECT Count(*)
FROM imdb_movies
WHERE revenue_millions > (
    SELECT Max(revenue_millions)
    FROM imdb_movies
    WHERE genre = 'Adventure'
);
```

#### Question 10: How many movies have ratings greater than the highest movie rating of the year 2015?

```sql
SELECT Count(*)
FROM imdb_movies
WHERE rating > (
    SELECT Max(rating)
    FROM imdb_movies
    WHERE year = 2015
);
```

#### Question 11: What is the minimum revenue corresponding to a movie such that its rating is higher than the highest movie rating of the year 2015 and its revenue is greater than the highest movie revenue of the 'Comedy' genre?

```sql
SELECT Min(revenue_millions)
FROM imdb_movies
WHERE rating > (
    SELECT Max(rating)
    FROM imdb_movies
    WHERE year = 2015
) AND revenue_millions > (
    SELECT Max(revenue_millions)
    FROM imdb_movies
    WHERE genre = 'Comedy'
);
```

#### Question 12: How many movies contribute more than 10% of revenue to the total revenue of all movies in a year?

```sql
SELECT Count(*)
FROM (
    SELECT title, (revenue_millions * 100 / tot_revenue) AS perc_revenue
    FROM imdb_movies A
    INNER JOIN (
        SELECT year, Sum(revenue_millions) AS tot_revenue
        FROM imdb_movies
        GROUP BY year
    ) b
    ON A.year = b.year
    WHERE (revenue_millions * 100 / tot_revenue) > 10
) C;
```

#### Question 13: How many movies contribute more than 5% of revenue to the total revenue of all movies in their respective genre?

```sql
SELECT Count(*)
FROM (
    SELECT title, (revenue_millions * 100 / tot_revenue) AS perc_revenue
    FROM imdb_movies A
    INNER JOIN (
        SELECT genre, Sum(revenue_millions) AS tot_revenue
        FROM imdb_movies
        GROUP BY genre
    ) b
    ON A.genre = b.genre
    WHERE (revenue_millions * 100 / tot_revenue) > 5
) C;
```
