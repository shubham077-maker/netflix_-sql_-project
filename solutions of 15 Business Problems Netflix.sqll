

drop table if exist

create table netflix(

	show_id VARCHAR(6),
	type  VARCHAR(10),
	title  VARCHAR(150),
	director	VARCHAR(208),
	casts	VARCHAR(1000),
	country	VARCHAR(150),
	date_added	VARCHAR(50),
	release_year INT	,
	rating	VARCHAR(10),
	duration VARCHAR(15),
	listed_in varchar(100),
	description VARCHAR(250)



);


SELECT*FROM NETFLIX;




        -------15 Business Problems 

-----1. Count the number of Movies vs TV.

SELECT 
	type,
	COUNT(*) AS TOTAL_CONTENT
	
FROM  NETFLIX
GROUP BY TYPE;





----2.Find the most common rating for movies and TV shows.
      

WITH RatingCounts AS (
    SELECT 
        type,
        rating,
        COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT 
        type,
        rating,
        rating_count,
        RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT 
    type,
    rating AS most_frequent_rating
FROM RankedRatings
WHERE rank = 1;






---3.List all movies released in a specific year (e.g., 2020)
   
SELECT
   	*FROM  netflix
   WHERE TYPE='Movie' 
 AND release_year=2020;

----4.Find the top 5 countries with the most content on Netflix.

SELECT * 
FROM
(
	SELECT 
		-- country,
		UNNEST(STRING_TO_ARRAY(country, ',')) as country,
		COUNT(*) as total_content
	FROM netflix
	GROUP BY 1
)as t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5






----5.Identify the longest movie or TV show duration.
 SELECT *from netflix where 
 type ='Movie' AND duration=(select max(duration) from netflix)


	 
----6.Find content added in the last 5 years.

select *from netflix 
where To_Date(date_added,'Month DD,YYYY') >= current_date -interval'5 year'



----7.Find all the movies/TV shows by director 'Rajiv Chilaka.

   SELECT*FROM netflix
   where director = 'Rajiv Chilaka'



-----8.List all TV shows with more than 5 seasons.



select*from netflix
where type='TV Show' and duration>'5 seasons '







-----9.Count the number of content items in each genre.


SELECT 
unnest(string_to_array(listed_in,',')) as genre,
count(show_id) as total_content
from netflix
group by genre



-----10.Find each year and the average number of content release by india  on netflix. return top 5 year with highest avg content release.
   SELECT
   		extract (year from TO_DATE(date_added,'Month DD,YYYY'))as year,
		   count(*) from  netflix
   where country='India'
   group by 1
             



-----11.List all movies that are documentaries.
       


Select *from netflix
where type='Movie' and listed_in ilike '%documentaries%'


-----12.Find all content without a director.


select *from netflix
where director is null



---13.Find how many movies actor 'Salman Khan' appeared in last 10 years.

select*from netflix
where casts ilike '%salman khan%'
and release_year>extract (year from current_date)-10


----14.Find the top 10 actors who have appeared in the highest number of movies produced in India.

select   unnest(string_to_array(casts,',')) as actors,
count(*) as total_content
from netflix
where country ilike'%India%' 
group by 1
order by 2 desc

-------15.Categorize the content based on the presence of the keywords 'kill' and 'violence' in the description field. Label content containing these keywords as 'Bad' and all other content as 'Good'. Count how many items fall into each category




SELECT 
    category,
	TYPE,
    COUNT(*) AS content_count
FROM (
    SELECT 
		*,
        CASE 
            WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad_content'
            ELSE 'Good_content'
        END AS category
    FROM netflix
) AS categorized_content
GROUP BY 1,2
ORDER BY 2
