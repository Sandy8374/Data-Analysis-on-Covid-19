Sure, here is the question and answer with just the SQL solution for your notes.

### Question

**Recyclable and Low Fat Products**

Given a table `Products` with the following schema:

| Column Name | Type  |
|-------------|-------|
| product_id  | int   |
| low_fats    | enum  |
| recyclable  | enum  |

- `product_id` is the primary key (column with unique values) for this table.
- `low_fats` is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is low fat and 'N' means it is not.
- `recyclable` is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is recyclable and 'N' means it is not.

Write a solution to find the IDs of products that are both low fat and recyclable.

Return the result table in any order.

**Example 1:**

Input: 
Products table:

| product_id | low_fats | recyclable |
|------------|----------|------------|
| 0          | Y        | N          |
| 1          | Y        | Y          |
| 2          | N        | Y          |
| 3          | Y        | Y          |
| 4          | N        | N          |

Output:

| product_id |
|------------|
| 1          |
| 3          |

Explanation: Only products 1 and 3 are both low fat and recyclable.

### Answer

**SQL Solution:**

To find the IDs of products that are both low fat and recyclable, you can write the following SQL query:

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```
### Question

**Find Customer Referee**

Given a table `Customer` with the following schema:

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| referee_id  | int     |

- `id` is the primary key column for this table.
- Each row of this table indicates the `id` of a customer, their `name`, and the `id` of the customer who referred them.

Write a solution to find the names of the customers who are **not referred** by the customer with `id = 2`.

Return the result table in any order.

**Example 1:**

Input: 
Customer table:

| id | name | referee_id |
|----|------|------------|
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |

Output:

| name |
|------|
| Will |
| Jane |
| Bill |
| Zack |

Explanation: The customers `Will`, `Jane`, `Bill`, and `Zack` are not referred by the customer with `id = 2`.

### Answer

**SQL Solution:**

To find the names of customers who are not referred by the customer with `id = 2`, you can write the following SQL query:

```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

This query selects the `name` of the customers where the `referee_id` is not equal to `2` or is `NULL`.
Sure, here are multiple SQL solutions using different approaches, including `IFNULL` and `COALESCE`.

### SQL Solution using `IFNULL`

The `IFNULL` function can be used to handle `NULL` values in the `referee_id` column, treating them as not being referred by `id = 2`.

```sql
SELECT name
FROM Customer
WHERE IFNULL(referee_id, 0) != 2;
```

### SQL Solution using `COALESCE`

The `COALESCE` function can also be used to handle `NULL` values in the `referee_id` column, treating them as not being referred by `id = 2`.

```sql
SELECT name
FROM Customer
WHERE COALESCE(referee_id, 0) != 2;
```

### SQL Solution using Regular Condition

The regular condition can be used without special functions to check for both `referee_id != 2` and `referee_id IS NULL`.

```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

### Explanation

- **Using `IFNULL`**: This function returns the first argument if it is not `NULL`, otherwise it returns the second argument. Here, we use `0` as the second argument, which effectively treats `NULL` values as `0`, thus ensuring they are included in the result.
  
- **Using `COALESCE`**: This function returns the first non-`NULL` value in the list of arguments. Here, we use `0` as the second argument, which effectively treats `NULL` values as `0`, ensuring they are included in the result.

- **Regular Condition**: This condition directly checks if `referee_id` is not equal to `2` or if it is `NULL`, ensuring all such rows are included in the result.

All three solutions will provide the same result for this problem. Choose the one that best fits your style or the requirements of your specific SQL environment.

### Question

**Big Countries**

Given a table `World` with the following schema:

| Column Name | Type    |
|-------------|---------|
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |

- `name` is the primary key (column with unique values) for this table.
- Each row of this table gives information about the name of a country, the continent to which it belongs, its area, the population, and its GDP value.

A country is considered "big" if:

- It has an area of at least three million (i.e., 3,000,000 km²), or
- It has a population of at least twenty-five million (i.e., 25,000,000).

Write a solution to find the `name`, `population`, and `area` of the big countries.

Return the result table in any order.

**Example 1:**

Input: 
World table:

| name        | continent | area    | population | gdp          |
|-------------|-----------|---------|------------|--------------|
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |

Output:

| name        | population | area    |
|-------------|-------------|---------|
| Afghanistan | 25500100    | 652230  |
| Algeria     | 37100000    | 2381741 |

### Answer

**SQL Solution:**

To find the names, populations, and areas of big countries, you can write the following SQL query:

```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;
```

This query selects the `name`, `population`, and `area` columns from the `World` table where the `area` is at least 3,000,000 km² or the `population` is at least 25,000,000.

### Question

**Article Views I**

Given a table `Views` with the following schema:

| Column Name | Type    |
|-------------|---------|
| article_id  | int     |
| author_id   | int     |
| viewer_id   | int     |
| view_date   | date    |

There is no primary key (column with unique values) for this table, and the table may have duplicate rows. Each row of this table indicates that some viewer viewed an article (written by some author) on some date. Note that equal `author_id` and `viewer_id` indicate the same person.

Write a solution to find all the authors that viewed at least one of their own articles.

Return the result table sorted by `id` in ascending order.

**Example 1:**

Input: 
Views table:

| article_id | author_id | viewer_id | view_date  |
|------------|-----------|-----------|------------|
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |

Output:

| id   |
|------|
| 4    |
| 7    |

### Answer

**SQL Solution:**

To find all the authors that viewed at least one of their own articles, you can write the following SQL query:

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY author_id;
```

This query selects the distinct `author_id` from the `Views` table where the `author_id` is equal to the `viewer_id`, and returns the results sorted by `author_id` in ascending order.

### Question

**Invalid Tweets**

Given a table `Tweets` with the following schema:

| Column Name | Type    |
|-------------|---------|
| tweet_id    | int     |
| content     | varchar |

- `tweet_id` is the primary key (column with unique values) for this table.
- This table contains all the tweets in a social media app.

Write a solution to find the IDs of the invalid tweets. A tweet is considered invalid if the number of characters used in the content of the tweet is strictly greater than 15.

Return the result table in any order.

**Example 1:**

Input: 
Tweets table:

| tweet_id | content                          |
|----------|----------------------------------|
| 1        | Vote for Biden                   |
| 2        | Let us make America great again! |

Output:

| tweet_id |
|----------|
| 2        |

Explanation: 
- Tweet 1 has length = 14. It is a valid tweet.
- Tweet 2 has length = 32. It is an invalid tweet.

### Answer

**SQL Solution:**

To find the IDs of the invalid tweets, you can write the following SQL query:

```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
```

This query selects the `tweet_id` from the `Tweets` table where the length of the `content` is strictly greater than 15 characters.

### Question

**Replace Employee ID With The Unique Identifier**

Given two tables `Employees` and `EmployeeUNI` with the following schema:

**Table: Employees**

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |

- `id` is the primary key (column with unique values) for this table.
- Each row of this table contains the `id` and the `name` of an employee in a company.

**Table: EmployeeUNI**

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| unique_id   | int     |

- `(id, unique_id)` is the primary key (combination of columns with unique values) for this table.
- Each row of this table contains the `id` and the corresponding `unique_id` of an employee in the company.

Write a solution to show the `unique_id` of each user. If a user does not have a `unique_id`, show `null`.

Return the result table in any order.

**Example 1:**

Input: 
Employees table:

| id | name     |
|----|----------|
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |

EmployeeUNI table:

| id | unique_id |
|----|-----------|
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |

Output:

| unique_id | name     |
|-----------|----------|
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |

Explanation: 
- Alice and Bob do not have a `unique_id`, so we will show `null` instead.
- The `unique_id` of Meir is 2.
- The `unique_id` of Winston is 3.
- The `unique_id` of Jonathan is 1.

### Answer

**SQL Solution:**

To show the `unique_id` of each user and `null` if a user does not have a `unique_id`, you can use a `LEFT JOIN` between the `Employees` and `EmployeeUNI` tables. The `LEFT JOIN` will ensure that all employees are included, even if they do not have a corresponding `unique_id`.

```sql
SELECT E.name, U.unique_id
FROM Employees E
LEFT JOIN EmployeeUNI U
ON E.id = U.id;
```

This query selects the `name` from the `Employees` table and the `unique_id` from the `EmployeeUNI` table, joining the tables on the `id` column. The `LEFT JOIN` ensures that all employees are included, and if there is no matching `unique_id`, `null` will be shown.
