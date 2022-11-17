## Top Five Artists

This question got me sweating!

**Assignement**

Assume there are three Spotify tables containing information about the artists, songs, and music charts. Write a query to determine the top 5 artists whose songs appear in the Top 10 of the global_song_rank table the highest number of times. From now on, we'll refer to this ranking number as "song appearances".

Output the top 5 artist names in ascending order along with their song appearances ranking (not the number of song appearances, but the rank of who has the most appearances). The order of the rank should take precedence.

For example, Ed Sheeran's songs appeared 5 times in Top 10 list of the global song rank table; this is the highest number of appearances, so he is ranked 1st. Bad Bunny's songs appeared in the list 4, so he comes in at a close 2nd.

Assumptions:

If two artists' songs have the same number of appearances, the artists should have the same rank.
The rank number should be continuous (1, 2, 2, 3, 4, 5) and not skipped (1, 2, 2, 4, 5).

**artists Table:**
Column Name	|Type|
------------|----|
artist_id	|integer|
artist_name	|varchar|

**songs Table:**
Column Name	|Type|
------------|----|
song_id	|integer|
artist_id|	integer|

**global_song_rank Table:**
Column Name |	Type|
------------|----|
day	|integer (1-52)|
song_id	|integer|
rank	|integer (1-1,000,000)|

My first two submissions were wrong, but my third submission was accepted by the website, which made me very happy. However, I had a feeling that something wasn't right. 
Here's the query that was accepted 

```sql
WITH cte AS(
  SELECT
    a.artist_name,
    COUNT (g.song_id) AS number_of_appearances
  FROM
    global_song_rank g
    INNER JOIN songs s ON g.song_id = s.song_id
    INNER JOIN artists a ON a.artist_id = s.artist_id
  WHERE
    g.rank <= 10
  GROUP BY
    a.artist_name
)
SELECT
  artist_name,
  DENSE_RANK () OVER (
    ORDER BY
      number_of_appearances DESC
  ) AS song_rank
FROM
  cte
LIMIT
  6
  ```

