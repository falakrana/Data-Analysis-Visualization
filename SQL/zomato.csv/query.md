### Basic queries

#### 1. Query to top rated orders. Where we will filter orders, with top `Rating Text` and `Aggregate rating`

```
SELECT * FROM index1.`zomato.csv`;
-- WHERE Cuisines = "Japanese";
WHERE "Rating Text" = "Excellent" or "Rating Text" = "Very Good";
```
#### 2. Getting distinct city `City`

```
SELECT distinct(City) FROM index1.`zomato.csv`;
```
#### 3. Query three

Now there is a issue. `select distinct(\``restaurants id\``) from zomato.csv` and `select distinct(\``restaurants name\``) from zomato.csv`, restaurants ids are 26, however restaurants name are 27.

```
SELECT `Restaurant Name`,
       COUNT(DISTINCT `Restaurant ID`) AS id_count
FROM index1.`zomato.csv`
GROUP BY `Restaurant Name`
HAVING COUNT(DISTINCT `Restaurant ID`) > 1;
```
```
select `Restaurant Name`, `Restaurant ID` from index1.`zomato.csv`
where `Restaurant Name` = "Silantro Fil-Mex";
```
#### 4. Restaurants offering table booking.

```
select * from index1.`zomato.csv`
where `Has Table booking` = "YES";
```
Vice versa for "No"
#### 5. Cities having the most restaurants

```
select `City`, count(distinct `Restaurant Name`) from index1.`zomato.csv`
group by `City`;
```
#### 6. Average restaurant rating by city

```
select `City`, avg(`Aggregate rating`) from index1.`zomato.csv`
group by `City`;
```
#### 7. Average price range by city

```
select `City`, round(avg(`Price range`), 2) as `Avg Price range` from index1.`zomato.csv`
group by `City`;
```
#### 8. Most popular cuisine

```
select `Restaurant Name`, `City`, `Cuisines` from index1.`zomato.csv`
where (`Aggregate rating` > 4.0) and (`Votes` > 600);
```
#### 9. Restaurants with both online delivery and table booking

```
select `Restaurant Name`, `City` from index1.`zomato.csv`
where `Has Table booking` = "Yes" and `Has Online delivery` = "Yes";
```
Well here there is no such restaurants.
#### 10. Highest-rated restaurant in each city

```
SELECT `City`,
       COUNT(*) AS Total_Restaurants
FROM index1.`zomato.csv`
WHERE `Aggregate rating` > 4.0
  AND `Votes` > 600
GROUP BY `City`;
```

### JOIN Queries:
#### 11. Restaurant with Country Name

```
select z.`Restaurant Name`, z.`City`, c.`Country` from index1.`zomato.csv` as z
join index1.`country-code` as c
on z.`Country Code` = c.`Country Code`;
```
#### 12. Number of Restaurants in Each Country

```
select c.`Country`, count(*) as `Total Restaurants` from index1.`zomato.csv` as z
join index1.`country-code` as c
on z.`Country Code` = c.`Country Code`
group by c.`Country`
order by `Total Restaurants` desc;
```
#### 13. Average Rating by Country

```
select c.`Country`, AVG(z.`Aggregate Rating`) as `Average Rating` from index1.`zomato.csv` as z
join index1.`country-code` as c
on z.`Country Code` = c.`Country Code`
group by c.`Country`
order by `Average Rating` desc;
```
#### 14. Top Rated Restaurant from Each Country
Refer this query again, it's tricky.
```
SELECT c.`Country`,
       z.`Restaurant Name`,
       z.`Aggregate Rating`
FROM index1.`zomato.csv` AS z
JOIN index1.`country-code` AS c
ON z.`Country Code` = c.`Country Code`
WHERE z.`Aggregate Rating` = (
    SELECT MAX(z2.`Aggregate Rating`)
    FROM index1.`zomato.csv` AS z2
    WHERE z2.`Country Code` = z.`Country Code`
);
```
#### 15. Highest voted restaurant in every country

```
select c.`Country`, Max(z.`Votes`) from index1.`zomato.csv` as z
join index1.`country-code` as c
on z.`Country Code` = c.`Country Code`
group by c.`Country`;
```
