# Week-3 Workshop Answers

Huanfa Chen

2022/01/26

---

This file contains the answers to the questions and tasks in the Week-3 workshop of CASA0006 and CASA0009 (2021-2022).



## Q1: export only the city name, the country name, and the proportion of the country’s population that live in that city

The SQL query:

```sql
SELECT a.name cities_name, b.name countries_name, a.pop/b.pop pop_prop FROM cities a, countries b WHERE a.country_cd = b.country_cd;
```

Then you will get a table with three columns: **cities_name**, **countries_name**, **pop_prop**. 

Note that ```a.name cities_name``` means that **cities_name** is an alias for **a.name**.

Adding another where clause to limit the join to only cities in Europe:

```sql
SELECT a.name cities_name, b.name countries_name, a.pop/b.pop pop_prop FROM cities a, countries b WHERE a.country_cd = b.country_cd AND b.continent = 'EU';
```

## Q2: run a query to keep all records from the cities table, plus records from the countries table where a match exists (using left join)

The query is:

```sql
SELECT * FROM cities a LEFT JOIN countries b ON a.name = b.capital AND a.country_cd = b.country_cd;
```

Now adjust this query to list all of the matching records first, followed by the unmatched city records.

```sql
SELECT * FROM cities a LEFT JOIN countries b ON a.name = b.capital AND a.country_cd = b.country_cd ORDER BY b.name DESC;
```

## Q3: yield a list of cities from across the world whose name matches the name of a UK city. Find the average population of this subset of world cities.

The query to yield a list of cities from across the world whose name matches (or, is identical with) the name of a UK city is:

```sql
select * from cities a join uk_cities b where a.name=b.name and a.country_cd!=b.country_cd;
```

One example is **the London city in Canada (‘CA’)**.

To find the average population of this subset of cities:

```sql
select avg(a.pop) from cities a join uk_cities b where a.name=b.name and a.country_cd!=b.country_cd; #other countries
```

This query will return **90656.5149**.

## Creating index of a table using SQL query
```sql
# create an index
create index cities_index on cities (name);
# some queries on the table
select * from cities a join cities b on a.name=b.name and a.country_cd!=b.country_cd;
# drop the index if you want
drop index cities_index on cities;
```

## Temporal data

You can add a new timestamp column call **date_uploaded_ts** using SQL query as below:

```sql
alter table photos add date_uploaded_ts timestamp(6); 
update phootos
set date_uploaded_ts=from_unixtime(date_uploaded);
```

The answer to **Q4: extract only photos taken on the 17th January 2010 after 19:00 and before 21:00, and order them in increasing time**:

```sql
select * from photos where date_uploaded_ts >= '2010/01/17 19:00:00' and date_uploaded_ts <= '2010/01/17 21:00:00' order by date_uploaded_ts asc;
```

This query will return **250 rows**.

## Spatial data

The query to extract photo_locations within UCL is:

```sql
select * from photo_locations where ST_Contains(ST_GeomFromText('POLYGON((-0.1463 51.5333,-0.1222 51.5333, -0.1463 51.5171, -0.122 51.5171, -0.1463 51.5333))'),coords);
```

It will return 189 photos.

To answer **Q5: join these results to the photos table through the shared pid value, using**
**whatever join method you like**, you can use **left join** to do this, as follows:

```sql
select * from (select * from photo_locations where ST_Contains(ST_GeomFromText('POLYGON((-0.1463 51.5333,-0.1222 51.5333, -0.1463 51.5171, -0.122 51.5171, -0.1463 51.5333))'),coords)) a left join photos b on a.pid=b.pid;
```

Other options are okay.