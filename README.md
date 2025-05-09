# Netflix Movies and TV Shows Data Analysis Using SQL

![Netflix Logo](logo.png)

## Table of Contents
- [Overview](#overview)
- [Objectives](#objectives)
- [Dataset](#dataset)
- [Schema](#schema)
- [Business Problems and Solutions](#business-problems-and-solutions)
  - [Count the Number of Movies vs TV Shows](#count-the-number-of-movies-vs-tv-shows)
  - [Find the Most Common Rating for Movies and TV Shows](#find-the-most-common-rating-for-movies-and-tv-shows)
  - [List All Movies Released in a Specific Year](#list-all-movies-released-in-a-specific-year)
  - [Find the Top 5 Countries with the Most Content on Netflix](#find-the-top-5-countries-with-the-most-content-on-netflix)
  - [Identify the Longest Movie](#identify-the-longest-movie)
  - [Find All Movies/TV Shows by a Specific Director](#find-all-movies-tv-shows-by-a-specific-director)
  - [List All TV Shows with More Than 5 Seasons](#list-all-tv-shows-with-more-than-5-seasons)
  - [Count the Number of Content Items in Each Genre](#count-the-number-of-content-items-in-each-genre)
  - [Find the Top 5 Years with the Highest Average Content Release in United Kingdom](#find-the-top-5-years-with-the-highest-average-content-release-in-United-Kingdom)
  - [Categorize Content Based on Specific Keywords](#categorize-content-based-on-specific-keywords)
- [Findings and Conclusion](#findings-and-conclusion)
- [Author](#author)

## Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset.

## Objectives
- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## Dataset
[Download netflix_titles.csv](netflix_titles.csv)

## Schema
```sql
DROP TABLE IF EXISTS netflix;

CREATE TABLE netflix (
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
```
## Business Problems and Solutions
## Count the Number of Movies vs TV Shows

```sql
SELECT type, COUNT(*) FROM netflix GROUP BY 1;
```
![Netflix Logo](netone.png)

Objective: Determine the distribution of content types on Netflix.

## Find the Most Common Rating for Movies and TV Shows
```sql
WITH RatingCounts AS (
    SELECT type, rating, COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT type, rating, rating_count,
           RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT type, rating AS most_frequent_rating FROM RankedRatings WHERE rank = 1;
```
![Netflix Logo](nettwo.png)

Objective: Identify the most frequently occurring rating for each content type.

## List All Movies Released in a Specific Year
```sql
SELECT * FROM netflix WHERE release_year = 2020;
```
![Netflix Logo](netthr.png)

Objective: Retrieve all movies released in a specific year.

## Find the Top 5 Countries with the Most Content on Netflix
```sql
SELECT country, COUNT(*) AS total_content
FROM netflix
WHERE country IS NOT NULL
GROUP BY country
ORDER BY total_content DESC
LIMIT 5;
```
![Netflix Logo](netfr.png)

Objective: Identify the top 5 countries with the highest number of content items.

## Identify the Longest Movie
```sql
SELECT * FROM netflix WHERE type = 'Movie'
ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC;
```
![Netflix Logo](netfv.png)

Objective: Find the movie with the longest duration.


## Find All Movies TV Shows by a Specific Director

```sql
SELECT * FROM netflix WHERE director = 'Martin Scorsese';
```
![Netflix Logo](netsix.png)

Objective: List all content directed by 'Martin Scorsese'.

## List All TV Shows with More Than 5 Seasons
```sql
SELECT * FROM netflix
WHERE type = 'TV Show' AND SPLIT_PART(duration, ' ', 1)::INT > 5;
```
![Netflix Logo](netsv.png)

Objective: Identify TV shows with more than 5 seasons.

## Count the Number of Content Items in Each Genre
```sql
SELECT UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre, COUNT(*) AS total_content
FROM netflix
GROUP BY genre;
```
![Netflix Logo](neteig.png)

Objective: Count the number of content items in each genre.

## Find the Top 5 Years with the Highest Average Content Release in United Kingdom

```sql
SELECT release_year, COUNT(show_id) AS total_release
FROM netflix
WHERE country = 'United Kingdom'
GROUP BY release_year
ORDER BY total_release DESC
LIMIT 5;
```
![Netflix Logo](netnine.png)

Objective: Identify the years with the highest average content release in United Kingdom.

## Categorize Content Based on Specific Keywords

```sql
SELECT category, COUNT(*) AS content_count
FROM (
    SELECT CASE 
            WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad'
            ELSE 'Good'
          END AS category
    FROM netflix
) AS categorized_content
GROUP BY category;
```
![Netflix Logo](netten.png)

Objective: Categorize content as 'Bad' if it contains 'kill' or 'violence' and 'Good' otherwise.

## Findings and Conclusion

Content Distribution: The dataset contains a diverse range of movies and TV shows with varying ratings and genres.

Common Ratings: Insights into the most common ratings provide an understanding of the content's target audience.

Geographical Insights: The top countries and the average content releases by India highlight regional content distribution.

Content Categorization: Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.

## Author
This project is part of my portfolio, showcasing SQL skills essential for data analyst roles.
