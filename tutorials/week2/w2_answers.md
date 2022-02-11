# Week-2 Workshop Answers

Huanfa Chen

2022/01/21

---

This file contains the answers to the questions and tasks in the Week-2 workshop of CASA0006 and CASA0009 (2021-2022).



# Q1: How many rows are there in Asia in the table of countries?

The SQL query to get all countries in Asia is:

```sql
SELECT * FROM countries WHERE continent = 'AS';
```

We can use the ```count``` function from SQL to get the number of countries in Asia:

```sql
SELECT count(*) FROM countries WHERE continent = 'AS';
```

This query will return **52**.

## Q2: how many countries have an area greater than 5,000,000 sqkm2?

The query is:

```sql
SELECT country_cd, name FROM countries WHERE area_sqkm > 5000000;
```

To get the number of these countries:

```sql
SELECT count(*) FROM countries WHERE area_sqkm > 5000000;
```

This query will return **7**.

## Q2.5: write a query to produce all of the rows for only
the countries neighbouring Russia.

The query is:

```sql
SELECT * FROM countries WHERE neighbours LIKE '%RU%';
```

This query will go through each row and return the ones with ‘RU’ in the neighbours column. It uses the partial match function of ‘LIKE’.

It will return a list of **14** countries.

An equivalent way to do this is:

```sql
SELECT * FROM countries WHERE FIND_IN_SET(country_cd, (SELECT neighbours FROM countries WHERE name = 'Russia'));
```

This query will go through each row and return the ones whose **country_cd** is within the neighbours of ‘Russia’.

**FIND_IN_SET** is a SQL function that returns the position of a string within a list of strings. Reference: https://www.w3schools.com/sql/func_mysql_find_in_set.asp

## 

## Q3: Try finding out the mean population size for countries in Europe alone

The query is:

```sql
SELECT AVG(pop) FROM countries where continent = 'EU';
```

The answer is **15297178.5510**.

## Q4: [Aggregating Data] Which continent has the highest average population?

The query is:

```sql
SELECT continent, AVG(pop) FROM countries GROUP BY continent ORDER BY AVG(pop) DESC;
```

It will return the following table:

| continent | AVG(pop)      |
| --------- | ------------- |
| AS        | 79429201.2885 |
| SA        | 28581683.4286 |
| NA        | 22637849.5217 |
| AF        | 17508113.5862 |
| EU        | 15297178.5510 |
| OC        | 1491455.2917  |
| AN        | 34.0000       |

The answer is **Asian**.

## Q5: [Amending Table Contents] Add a made up country

The query is:

```sql
INSERT INTO countries (country_cd, name, capital, area_sqkm, pop, continent, currency_name) VALUES ('CASA', 'CASA', 'CASA office', '0.001', 200, 'EU', 'GBP');
```

To double check, you can run this query:

```sql
SELECT * FROM countries WHERE country_cd = 'CASA';
```



## Q6: Change the population of your made up country to 500, and change the capital city to ‘CASAville’
The query is:

```sql
UPDATE countries SET pop = 500, capital = 'CASAville' WHERE country_cd = 'CASA';
```

To double check, you can run this query:

```sql
SELECT * FROM countries WHERE country_cd = 'CASA';
```





