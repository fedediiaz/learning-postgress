# **Beginner Level SQL Course Using PostgreSQL**

## **Introduction**

Welcome to the beginner's SQL course! This course is designed to teach you the fundamentals of SQL (Structured Query Language) using PostgreSQL, one of the most popular open-source relational database management systems. We will use a practical approach by working with a real database schema, which represents a video rental store.

By the end of this course, you will be able to:

- Understand basic SQL concepts and syntax.
- Write SQL queries to retrieve, filter, and sort data.
- Use aggregate functions and group data.
- Join tables to retrieve related data from multiple tables.
- Modify data using INSERT, UPDATE, and DELETE statements.

Let's get started!

---

## **Database Schema Overview**

Before diving into SQL, let's familiarize ourselves with the database schema we'll be using throughout this course. The database models a video rental store with the following tables:

- **actor**: Information about actors.
- **address**: Addresses for customers and staff.
- **category**: Categories of films.
- **city**: Cities where addresses are located.
- **country**: Countries where cities are located.
- **customer**: Customer information.
- **film**: Information about films.
- **film_actor**: Relationship between films and actors.
- **film_category**: Relationship between films and categories.
- **inventory**: Inventory of films in stores.
- **language**: Languages of films.
- **payment**: Payment transactions.
- **rental**: Rental transactions.
- **staff**: Staff information.
- **store**: Store information.

We'll explore these tables in more detail as we progress.

---

# **Lesson 1: Introduction to SQL and SELECT Statements**

## **What is SQL?**

SQL (Structured Query Language) is a standard language for accessing and manipulating relational databases. It allows you to perform tasks such as querying data, updating records, and managing database objects.

## **Basic SELECT Statement Syntax**

The `SELECT` statement is used to retrieve data from one or more tables. The basic syntax is:

```sql
SELECT column1, column2, ...
FROM table_name;
```

- `SELECT`: Specifies the columns to retrieve.
- `FROM`: Specifies the table to query.

## **Selecting All Columns from a Table**

To select all columns from a table, use the asterisk (`*`):

```sql
SELECT * FROM table_name;
```

**Example:**

Retrieve all columns from the `actor` table:

```sql
SELECT * FROM actor;
```

## **Selecting Specific Columns**

You can specify which columns to retrieve by listing them after the `SELECT` keyword.

**Example:**

Retrieve the `first_name` and `last_name` columns from the `actor` table:

```sql
SELECT first_name, last_name FROM actor;
```

---

## **Exercise 1**

### **Objective:**

Practice basic `SELECT` statements to retrieve data from tables.

### **Tasks:**

1. Retrieve all columns from the `film` table.
2. Retrieve the `title` and `description` columns from the `film` table.
3. Retrieve all columns from the `customer` table.
4. Retrieve the `first_name`, `last_name`, and `email` columns from the `customer` table.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. ```sql
   SELECT * FROM film;
   ```

2. ```sql
   SELECT title, description FROM film;
   ```

3. ```sql
   SELECT * FROM customer;
   ```

4. ```sql
   SELECT first_name, last_name, email FROM customer;
   ```

</details>

---

# **Lesson 2: Filtering Data with WHERE Clause**

## **Using WHERE Clause to Filter Data**

The `WHERE` clause is used to filter records that meet certain conditions.

**Syntax:**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

## **Operators**

- Equal to: `=`
- Not equal to: `<>` or `!=`
- Greater than: `>`
- Less than: `<`
- Greater than or equal to: `>=`
- Less than or equal to: `<=`

**Example:**

Retrieve all films released in the year 2006:

```sql
SELECT * FROM film
WHERE release_year = 2006;
```

## **Logical Operators**

- **AND**: Combines two conditions; both must be true.
- **OR**: At least one of the conditions must be true.
- **NOT**: Negates a condition.

**Example:**

Retrieve films with a rating of 'PG' released in 2006:

```sql
SELECT * FROM film
WHERE rating = 'PG' AND release_year = 2006;
```

## **Using BETWEEN, IN, LIKE**

### **BETWEEN**

Select values within a range.

**Example:**

Retrieve films with `film_id` between 10 and 20:

```sql
SELECT * FROM film
WHERE film_id BETWEEN 10 AND 20;
```

### **IN**

Select values that match any value in a list.

**Example:**

Retrieve customers from stores 1 or 2:

```sql
SELECT * FROM customer
WHERE store_id IN (1, 2);
```

### **LIKE**

Search for a pattern in a column.

- `%` represents zero or more characters.
- `_` represents a single character.

**Example:**

Retrieve customers whose last name starts with 'S':

```sql
SELECT * FROM customer
WHERE last_name LIKE 'S%';
```

---

## **Exercise 2**

### **Objective:**

Practice using the `WHERE` clause and various operators to filter data.

### **Tasks:**

1. Retrieve all films with a rating of 'R'.
2. Retrieve customers whose first name starts with 'A'.
3. Retrieve films released between 2005 and 2007.
4. Retrieve actors whose last name ends with 'son'.
5. Retrieve payments where the amount is greater than 5.00.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Retrieve all films with a rating of 'R':**

   ```sql
   SELECT * FROM film
   WHERE rating = 'R';
   ```

2. **Retrieve customers whose first name starts with 'A':**

   ```sql
   SELECT * FROM customer
   WHERE first_name LIKE 'A%';
   ```

3. **Retrieve films released between 2005 and 2007:**

   ```sql
   SELECT * FROM film
   WHERE release_year BETWEEN 2005 AND 2007;
   ```

4. **Retrieve actors whose last name ends with 'son':**

   ```sql
   SELECT * FROM actor
   WHERE last_name LIKE '%son';
   ```

5. **Retrieve payments where the amount is greater than 5.00:**

   ```sql
   SELECT * FROM payment
   WHERE amount > 5.00;
   ```

</details>

---

# **Lesson 3: Sorting Data with ORDER BY**

## **Using ORDER BY to Sort Results**

The `ORDER BY` clause is used to sort the result set by one or more columns.

**Syntax:**

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;
```

- `ASC`: Ascending order (default).
- `DESC`: Descending order.

**Example:**

Retrieve all films sorted by title in ascending order:

```sql
SELECT * FROM film
ORDER BY title ASC;
```

## **Sorting by Multiple Columns**

You can sort by multiple columns by listing them separated by commas.

**Example:**

Retrieve all customers sorted by `last_name` ascending and `first_name` descending:

```sql
SELECT * FROM customer
ORDER BY last_name ASC, first_name DESC;
```

---

## **Exercise 3**

### **Objective:**

Practice sorting data using `ORDER BY`.

### **Tasks:**

1. Retrieve all actors sorted by `last_name` in descending order.
2. Retrieve all films sorted by `rental_rate` descending and `title` ascending.
3. Retrieve all payments sorted by `payment_date` ascending.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. ```sql
   SELECT * FROM actor
   ORDER BY last_name DESC;
   ```

2. ```sql
   SELECT * FROM film
   ORDER BY rental_rate DESC, title ASC;
   ```

3. ```sql
   SELECT * FROM payment
   ORDER BY payment_date ASC;
   ```

</details>

---

# **Lesson 4: Aggregate Functions and GROUP BY**

## **Aggregate Functions**

Aggregate functions perform calculations on a set of values and return a single value.

- **COUNT**: Returns the number of rows.
- **SUM**: Returns the sum of values.
- **AVG**: Returns the average value.
- **MIN**: Returns the minimum value.
- **MAX**: Returns the maximum value.

**Example:**

Count the total number of films:

```sql
SELECT COUNT(*) FROM film;
```

Calculate the average rental rate:

```sql
SELECT AVG(rental_rate) FROM film;
```

## **Using GROUP BY to Group Data**

The `GROUP BY` clause groups rows that have the same values in specified columns into summary rows.

**Syntax:**

```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1;
```

**Example:**

Count the number of films for each rating:

```sql
SELECT rating, COUNT(*) AS film_count
FROM film
GROUP BY rating;
```

## **Filtering Groups with HAVING Clause**

The `HAVING` clause is used to filter groups based on aggregate functions.

**Example:**

Retrieve ratings with more than 200 films:

```sql
SELECT rating, COUNT(*) AS film_count
FROM film
GROUP BY rating
HAVING COUNT(*) > 200;
```

---

## **Exercise 4**

### **Objective:**

Practice using aggregate functions and grouping data.

### **Tasks:**

1. Count the total number of customers.
2. Find the minimum, maximum, and average `rental_rate` from the `film` table.
3. List the number of films in each `language_id`.
4. Find the total amount of payments made on '2005-05-25'.
5. List each film rating and the average length of films for each rating.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Count the total number of customers:**

   ```sql
   SELECT COUNT(*) FROM customer;
   ```

2. **Find the minimum, maximum, and average `rental_rate` from the `film` table:**

   ```sql
   SELECT MIN(rental_rate) AS min_rate,
          MAX(rental_rate) AS max_rate,
          AVG(rental_rate) AS avg_rate
   FROM film;
   ```

3. **List the number of films in each `language_id`:**

   ```sql
   SELECT language_id, COUNT(*) AS film_count
   FROM film
   GROUP BY language_id;
   ```

4. **Find the total amount of payments made on '2005-05-25':**

   ```sql
   SELECT SUM(amount) AS total_payments
   FROM payment
   WHERE DATE(payment_date) = '2005-05-25';
   ```

5. **List each film rating and the average length of films for each rating:**

   ```sql
   SELECT rating, AVG(length) AS average_length
   FROM film
   GROUP BY rating;
   ```

</details>

---

# **Lesson 5: Joining Tables**

## **Introduction to JOINs**

JOINs are used to combine rows from two or more tables based on related columns.

## **Tables Overview**

### **Table 1: `customer`**

| customer_id | first_name | last_name |
| ----------- | ---------- | --------- |
| 1           | John       | Doe       |
| 2           | Jane       | Smith     |
| 3           | Bob        | Johnson   |

### **Table 2: `rental`**

| rental_id | rental_date | customer_id |
| --------- | ----------- | ----------- |
| 101       | 2021-01-01  | 1           |
| 102       | 2021-01-02  | 2           |
| 103       | 2021-01-03  | 4           |

_Note:_ Customer with `customer_id` 4 does not exist in the `customer` table.

### **Table 3: `payment`**

| payment_id | amount | customer_id |
| ---------- | ------ | ----------- |
| 201        | 5.99   | 1           |
| 202        | 2.99   | 2           |
| 203        | 3.99   | 5           |

_Note:_ Customer with `customer_id` 5 does not exist in the `customer` table.

---

# **1. INNER JOIN**

![Inner Join](../img/inner-join.png)

### **Definition**

An **INNER JOIN** returns records that have matching values in both tables.

### **SQL Example**

```sql
SELECT c.customer_id, c.first_name, r.rental_id, r.rental_date
FROM customer c
INNER JOIN rental r ON c.customer_id = r.customer_id;
```

### **Explanation**

- The query joins `customer` and `rental` on `customer_id`.
- Only records where `customer_id` exists in both tables are returned.

### **Expected Output**

| customer_id | first_name | rental_id | rental_date |
| ----------- | ---------- | --------- | ----------- |
| 1           | John       | 101       | 2021-01-01  |
| 2           | Jane       | 102       | 2021-01-02  |

_Note:_ Rental with `customer_id` 4 is excluded because there is no matching customer.

### **INNER JOIN with Three Tables**

**SQL Example**

```sql
SELECT c.customer_id, c.first_name, p.payment_id, p.amount
FROM customer c
INNER JOIN payment p ON c.customer_id = p.customer_id;
```

**Explanation**

- Returns customers who have made payments.

### **Expected Output**

| customer_id | first_name | payment_id | amount |
| ----------- | ---------- | ---------- | ------ |
| 1           | John       | 201        | 5.99   |
| 2           | Jane       | 202        | 2.99   |

_Note:_ Payment with `customer_id` 5 is excluded.

---

# **2. LEFT JOIN (LEFT OUTER JOIN)**

![Inner Join](../img/left-outer-join.png)

### **Definition**

A **LEFT JOIN** returns all records from the left table (`customer`), and the matched records from the right table (`rental`). If there is no match, `NULL` values are returned for columns from the right table.

### **SQL Example**

```sql
SELECT c.customer_id, c.first_name, r.rental_id, r.rental_date
FROM customer c
LEFT JOIN rental r ON c.customer_id = r.customer_id;
```

### **Explanation**

- Retrieves all customers.
- Includes rental information if available; otherwise, `NULL`.

### **Expected Output**

| customer_id | first_name | rental_id | rental_date |
| ----------- | ---------- | --------- | ----------- |
| 1           | John       | 101       | 2021-01-01  |
| 2           | Jane       | 102       | 2021-01-02  |
| 3           | Bob        | NULL      | NULL        |

_Note:_ Bob has no rentals, so rental columns are `NULL`.

### **LEFT JOIN with Three Tables**

**SQL Example**

```sql
SELECT c.customer_id, c.first_name, p.payment_id, p.amount
FROM customer c
LEFT JOIN payment p ON c.customer_id = p.customer_id;
```

**Expected Output**

| customer_id | first_name | payment_id | amount |
| ----------- | ---------- | ---------- | ------ |
| 1           | John       | 201        | 5.99   |
| 2           | Jane       | 202        | 2.99   |
| 3           | Bob        | NULL       | NULL   |

_Note:_ Bob has not made any payments.

---

# **3. RIGHT JOIN (RIGHT OUTER JOIN)**

![Inner Join](../img/right-outer-join.png)

### **Definition**

A **RIGHT JOIN** returns all records from the right table (`rental`), and the matched records from the left table (`customer`). If there is no match, `NULL` values are returned for columns from the left table.

### **SQL Example**

```sql
SELECT c.customer_id, c.first_name, r.rental_id, r.rental_date
FROM customer c
RIGHT JOIN rental r ON c.customer_id = r.customer_id;
```

### **Explanation**

- Retrieves all rentals.
- Includes customer information if available; otherwise, `NULL`.

### **Expected Output**

| customer_id | first_name | rental_id | rental_date |
| ----------- | ---------- | --------- | ----------- |
| 1           | John       | 101       | 2021-01-01  |
| 2           | Jane       | 102       | 2021-01-02  |
| NULL        | NULL       | 103       | 2021-01-03  |

_Note:_ Rental with `customer_id` 4 has no matching customer, so customer columns are `NULL`.

---

# **4. FULL OUTER JOIN**

![Inner Join](../img/full-outer-join.png)

### **Definition**

A **FULL OUTER JOIN** returns all records when there is a match in either left or right table.

### **SQL Example**

```sql
SELECT c.customer_id, c.first_name, r.rental_id, r.rental_date
FROM customer c
FULL OUTER JOIN rental r ON c.customer_id = r.customer_id;
```

### **Explanation**

- Combines results of both LEFT and RIGHT JOIN.
- Includes all records from both tables.

### **Expected Output**

| customer_id | first_name | rental_id | rental_date |
| ----------- | ---------- | --------- | ----------- |
| 1           | John       | 101       | 2021-01-01  |
| 2           | Jane       | 102       | 2021-01-02  |
| 3           | Bob        | NULL      | NULL        |
| NULL        | NULL       | 103       | 2021-01-03  |

_Note:_ Both unmatched customers and rentals are included with `NULL` where there's no match.

---

# **5. CROSS JOIN**

### **Definition**

A **CROSS JOIN** returns the Cartesian product of the two tables, combining every row from the first table with every row from the second table.

### **SQL Example**

```sql
SELECT c.customer_id, c.first_name, r.rental_id, r.rental_date
FROM customer c
CROSS JOIN rental r;
```

### **Explanation**

- Each customer is paired with every rental.

### **Expected Output**

| customer_id | first_name | rental_id | rental_date |
| ----------- | ---------- | --------- | ----------- |
| 1           | John       | 101       | 2021-01-01  |
| 1           | John       | 102       | 2021-01-02  |
| 1           | John       | 103       | 2021-01-03  |
| 2           | Jane       | 101       | 2021-01-01  |
| 2           | Jane       | 102       | 2021-01-02  |
| 2           | Jane       | 103       | 2021-01-03  |
| 3           | Bob        | 101       | 2021-01-01  |
| 3           | Bob        | 102       | 2021-01-02  |
| 3           | Bob        | 103       | 2021-01-03  |

_Note:_ Total rows = number of customers × number of rentals (3×3=9).

## **Combining JOINs**

You can combine different JOINs to retrieve complex datasets.

**Example:** Find all customers along with their rentals and payments.

**SQL Example**

```sql
SELECT c.customer_id, c.first_name, r.rental_id, p.payment_id, p.amount
FROM customer c
LEFT JOIN rental r ON c.customer_id = r.customer_id
LEFT JOIN payment p ON c.customer_id = p.customer_id;
```

---

# **Understanding JOIN Behavior**

## **INNER JOIN**

- **Use Case:** When you need only the records that have matches in both tables.
- **Result:** Excludes records without matches.

## **LEFT JOIN**

- **Use Case:** When you need all records from the left table, regardless of matches in the right table.
- **Result:** Includes all left table records; right table columns are `NULL` when no match.

## **RIGHT JOIN**

- **Use Case:** When you need all records from the right table, regardless of matches in the left table.
- **Result:** Includes all right table records; left table columns are `NULL` when no match.

## **FULL OUTER JOIN**

- **Use Case:** When you need all records from both tables.
- **Result:** Combines LEFT and RIGHT JOIN results; `NULL` where there's no match.

## **CROSS JOIN**

- **Use Case:** When you need every combination of records from both tables.
- **Result:** Number of rows equals the product of the number of rows in both tables.

---

# **Practical Considerations**

- **Performance:** Be cautious with JOINs on large tables, especially CROSS JOINs, as they can be resource-intensive.
- **NULL Handling:** Remember that `NULL` values may require special handling in your queries (e.g., using `COALESCE` to provide default values).
- **JOIN Conditions:** Always ensure your JOIN conditions are correct to avoid unintended results.

### **4. FULL OUTER JOIN Example**

**Query:**

```sql
SELECT c.customer_id, c.first_name, r.rental_id
FROM customer c
FULL OUTER JOIN rental r ON c.customer_id = r.customer_id;
```

**Result:**

- All customers and rentals are included.
- `NULL` values where there's no match.

---

# **Conclusion**

By using these examples, you can see how different JOINs affect the result set of your queries. Experimenting with sample data helps in understanding the practical implications of each JOIN type.

Feel free to run these queries against your database to see the actual results, and don't hesitate to ask if you have further questions!

## **Exercise 5**

### **Objective:**

Practice joining tables to retrieve related data.

### **Tasks:**

1. Retrieve the first and last names of all customers along with the address they live at.
2. List the titles of all films along with their corresponding language names.
3. For each film, list the title, category name, and language name.
4. List all customers and the total amount they have paid. Include customers who have not made any payments.
5. List all payments and the first and last names of the staff members who processed them. Include payments that do not have an associated staff member (if any).
6. Retrieve a list of all actors and the films they have acted in. Include actors who have not acted in any films and films that do not have any actors (if any).
7. Find all pairs of customers who live in the same city. List the customer IDs and names.
8. Generate a list of all possible combinations of film titles and category names.
9. For each rental, list the rental ID, rental date, customer name, film title, and staff name who processed the rental.
10. List the top 5 films that have generated the highest total revenue. Include the film title and total revenue.

### **Solutions:**

#### **Exercise 1 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT c.first_name, c.last_name, a.address
FROM customer c
JOIN address a ON c.address_id = a.address_id;
```

**Explanation:**

- Performs an INNER JOIN between `customer` (alias `c`) and `address` (alias `a`) on `address_id`.
- Retrieves the first name, last name from `customer`, and address from `address`.

</details>

---

#### **Exercise 2 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT f.title, l.name AS language_name
FROM film f
JOIN "language" l ON f.language_id = l.language_id;
```

**Explanation:**

- Joins `film` (alias `f`) and `"language"` (alias `l`) on `language_id`.
- Retrieves the film title and the name of the language.
- Encloses `"language"` in double quotes because it's a reserved keyword.

</details>

---

#### **Exercise 3 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT f.title, c.name AS category_name, l.name AS language_name
FROM film f
JOIN film_category fc ON f.film_id = fc.film_id
JOIN category c ON fc.category_id = c.category_id
JOIN "language" l ON f.language_id = l.language_id;
```

**Explanation:**

- Joins `film` to `film_category` on `film_id`.
- Joins `film_category` to `category` on `category_id`.
- Joins `film` to `"language"` on `language_id`.
- Retrieves the film title, category name, and language name.

</details>

---

#### **Exercise 4 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT c.customer_id, c.first_name, c.last_name, COALESCE(SUM(p.amount), 0) AS total_paid
FROM customer c
LEFT JOIN payment p ON c.customer_id = p.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name;
```

**Explanation:**

- Performs a LEFT JOIN to include all customers, even those without payments.
- Uses `COALESCE` to replace `NULL` with `0` for customers with no payments.
- Aggregates payments per customer using `SUM(p.amount)`.
- Groups the results by customer ID and names.

</details>

---

#### **Exercise 5 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT p.payment_id, p.amount, s.first_name, s.last_name
FROM payment p
RIGHT JOIN staff s ON p.staff_id = s.staff_id;
```

**Explanation:**

- Performs a RIGHT JOIN to include all staff members, even if they haven't processed any payments.
- Retrieves payment details and staff first and last names.
- Note: In practice, every payment should have a valid `staff_id`, so unmatched payments are unlikely.

</details>

---

#### **Exercise 6 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT a.first_name, a.last_name, f.title
FROM actor a
FULL OUTER JOIN film_actor fa ON a.actor_id = fa.actor_id
FULL OUTER JOIN film f ON fa.film_id = f.film_id;
```

**Explanation:**

- Performs a FULL OUTER JOIN between `actor` and `film_actor` to include all actors, even those not in any films.
- Joins `film_actor` to `film` to get film titles.
- Retrieves actor names and film titles.
- Includes actors without films and films without actors.

</details>

---

#### **Exercise 7 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT
  c1.customer_id AS customer1_id,
  c1.first_name AS customer1_first_name,
  c1.last_name AS customer1_last_name,
  c2.customer_id AS customer2_id,
  c2.first_name AS customer2_first_name,
  c2.last_name AS customer2_last_name,
  ci.city
FROM customer c1
JOIN address a1 ON c1.address_id = a1.address_id
JOIN customer c2 ON c1.customer_id < c2.customer_id
JOIN address a2 ON c2.address_id = a2.address_id
JOIN city ci ON a1.city_id = ci.city_id AND a2.city_id = ci.city_id;
```

**Explanation:**

- Joins `customer` table to itself to find pairs living in the same city.
- Ensures that a customer is not paired with themselves and avoids duplicate pairs.
- Retrieves customer IDs, names, and the city they live in.

</details>

---

#### **Exercise 8 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT f.title, c.name AS category_name
FROM film f
CROSS JOIN category c;
```

**Explanation:**

- Performs a CROSS JOIN to generate all possible combinations of films and categories.
- Retrieves the film title and category name.

</details>

---

#### **Exercise 9 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT
  r.rental_id,
  r.rental_date,
  c.first_name AS customer_first_name,
  c.last_name AS customer_last_name,
  f.title AS film_title,
  s.first_name AS staff_first_name,
  s.last_name AS staff_last_name
FROM rental r
JOIN customer c ON r.customer_id = c.customer_id
JOIN staff s ON r.staff_id = s.staff_id
JOIN inventory i ON r.inventory_id = i.inventory_id
JOIN film f ON i.film_id = f.film_id;
```

**Explanation:**

- Joins `rental` to `customer`, `staff`, `inventory`, and `film` tables.
- Retrieves rental details along with customer name, film title, and staff name.

</details>

---

#### **Exercise 10 Solution**

<details>
  <summary>Click to view solution</summary>

```sql
SELECT f.title, SUM(p.amount) AS total_revenue
FROM payment p
JOIN rental r ON p.rental_id = r.rental_id
JOIN inventory i ON r.inventory_id = i.inventory_id
JOIN film f ON i.film_id = f.film_id
GROUP BY f.film_id, f.title
ORDER BY total_revenue DESC
LIMIT 5;
```

**Explanation:**

- Joins `payment` to `rental`, `inventory`, and `film` to relate payments back to films.
- Calculates the total revenue per film using `SUM(p.amount)`.
- Groups by film ID and title.
- Orders the results by total revenue in descending order.
- Limits the output to the top 5 films.

</details>

---

# **Lesson 6: Subqueries**

## **What Are Subqueries?**

Subqueries are queries nested inside another query. They can be used in SELECT, FROM, WHERE, and HAVING clauses.

## **Using Subqueries in SELECT**

**Example:**

Retrieve film titles along with the number of actors in each film:

```sql
SELECT film.title,
       (SELECT COUNT(*)
        FROM film_actor
        WHERE film_actor.film_id = film.film_id) AS actor_count
FROM film;
```

## **Using Subqueries in FROM**

**Example:**

Calculate the average payment amount and list customers who have payments above the average:

```sql
SELECT customer.first_name, customer.last_name, payment.amount
FROM customer
JOIN payment ON customer.customer_id = payment.customer_id
WHERE payment.amount > (SELECT AVG(amount) FROM payment);
```

---

## **Exercise 6**

### **Objective:**

Practice writing subqueries.

### **Tasks:**

1. Retrieve films that have more than 5 actors.
2. List customers who have rented more than 20 films.
3. Find the film(s) with the highest rental rate.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. ```sql
   SELECT film.title
   FROM film
   WHERE (SELECT COUNT(*)
          FROM film_actor
          WHERE film_actor.film_id = film.film_id) > 5;
   ```

2. ```sql
   SELECT customer.first_name, customer.last_name
   FROM customer
   WHERE (SELECT COUNT(*)
          FROM rental
          WHERE rental.customer_id = customer.customer_id) > 20;
   ```

3. ```sql
   SELECT title, rental_rate
   FROM film
   WHERE rental_rate = (SELECT MAX(rental_rate) FROM film);
   ```

</details>

---

# **Lesson 7: Modifying Data**

## **INSERT Statements**

Used to insert new records into a table.

**Syntax:**

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

**Example:**

Add a new category:

```sql
INSERT INTO category (name)
VALUES ('Documentary');
```

## **UPDATE Statements**

Used to modify existing records.

**Syntax:**

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**Example:**

Update a customer's email:

```sql
UPDATE customer
SET email = 'newemail@example.com'
WHERE customer_id = 1;
```

## **DELETE Statements**

Used to delete existing records.

**Syntax:**

```sql
DELETE FROM table_name
WHERE condition;
```

**Example:**

Delete a customer record:

```sql
DELETE FROM customer
WHERE customer_id = 1;
```

---

## **Exercise 7**

### **Objective:**

Practice modifying data using INSERT, UPDATE, and DELETE statements.

### **Tasks:**

1. Insert a new actor named 'John Doe'.
2. Update the rental rate of all films to 0.99 where the `rental_rate` is greater than 4.99.
3. Delete all films released before the year 2000.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. ```sql
   INSERT INTO actor (first_name, last_name)
   VALUES ('John', 'Doe');
   ```

2. ```sql
   UPDATE film
   SET rental_rate = 0.99
   WHERE rental_rate > 4.99;
   ```

3. ```sql
   DELETE FROM film
   WHERE release_year < 2000;
   ```

</details>

---

# **Conclusion**

Congratulations! You've completed the beginner level SQL course. You've learned how to:

- Retrieve data using `SELECT` statements.
- Filter data using `WHERE` clauses and various operators.
- Sort data using `ORDER BY`.
- Use aggregate functions and group data with `GROUP BY` and `HAVING`.
- Join tables to retrieve related data.
- Write subqueries for complex queries.
- Modify data using `INSERT`, `UPDATE`, and `DELETE`.

## **Next Steps**

- **Practice**: The best way to solidify your understanding is by practicing. Try writing your own queries and experimenting with the data.
- **Intermediate Level**: Once you're comfortable with these concepts, you're ready to move on to more advanced topics in the intermediate course.

Feel free to ask any questions or request clarification on any topics covered. Happy querying!
