# **Intermediate Level SQL Course Using PostgreSQL**

## **Introduction**

Welcome back! Now that you've mastered the basics of SQL, it's time to delve deeper into more advanced concepts. This intermediate course will expand your SQL skills, introducing you to complex querying techniques, performance optimization, and database management features using PostgreSQL.

By the end of this course, you will be able to:

- Write complex subqueries and use them effectively.
- Utilize set operations like UNION, INTERSECT, and EXCEPT.
- Understand and implement Common Table Expressions (CTEs).
- Apply window functions for advanced data analysis.
- Use advanced JOIN techniques, including self joins and non-equijoins.
- Create and manage views.
- Optimize queries using indexes and understand performance considerations.
- Manage transactions and understand concurrency control.

Let's dive in!

---

## **Database Schema Reminder**

We'll continue using the video rental store database schema provided earlier. This schema includes the following tables:

- **actor**
- **address**
- **category**
- **city**
- **country**
- **customer**
- **film**
- **film_actor**
- **film_category**
- **inventory**
- **language**
- **payment**
- **rental**
- **staff**
- **store**

Refer to the schema as needed throughout the course.

---

# **Lesson 1: Advanced Subqueries**

## **1.1 Subqueries in the FROM Clause**

Subqueries can be used as temporary tables in the `FROM` clause, often referred to as derived tables or inline views.

**Example:**

Retrieve the average rental rate and list films that have a higher rental rate than the average.

```sql
SELECT f.title, f.rental_rate
FROM film f,
     (SELECT AVG(rental_rate) AS avg_rate FROM film) AS avg_rates
WHERE f.rental_rate > avg_rates.avg_rate;
```

**Explanation:**

- The subquery calculates the average rental rate and aliases it as `avg_rates`.
- The main query selects films where the `rental_rate` is greater than the average.

## **1.2 Correlated Subqueries**

A correlated subquery is a subquery that references columns from the outer query. It is evaluated once for each row processed by the outer query.

**Example:**

List customers who have rented more films than the average number of rentals per customer.

```sql
SELECT c.customer_id, c.first_name, c.last_name
FROM customer c
WHERE (SELECT COUNT(*)
       FROM rental r
       WHERE r.customer_id = c.customer_id) >
      (SELECT AVG(rental_count)
       FROM (SELECT customer_id, COUNT(*) AS rental_count
             FROM rental
             GROUP BY customer_id) AS customer_rentals);
```

**Explanation:**

- The inner subquery calculates the number of rentals per customer.
- The outer query compares each customer's rental count to the average.

## **1.3 EXISTS and NOT EXISTS**

The `EXISTS` operator checks for the existence of rows returned by a subquery.

**Example:**

Find customers who have never made a payment.

```sql
SELECT c.customer_id, c.first_name, c.last_name
FROM customer c
WHERE NOT EXISTS (SELECT 1
                  FROM payment p
                  WHERE p.customer_id = c.customer_id);
```

**Explanation:**

- The subquery checks for payments made by each customer.
- `NOT EXISTS` filters customers without any payments.

---

## **Exercise 1**

### **Objective:**

Practice writing advanced subqueries.

### **Tasks:**

1. List films that are longer than the average length of all films.
2. Find actors who have acted in more than 10 films.
3. Retrieve categories that have more than 50 films associated with them.
4. List customers who have spent more than the average total payment amount.
5. Find films that have more actors than the average number of actors per film.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Films longer than average length:**

   ```sql
   SELECT title, length
   FROM film
   WHERE length > (SELECT AVG(length) FROM film);
   ```

2. **Actors in more than 10 films:**

   ```sql
   SELECT a.actor_id, a.first_name, a.last_name
   FROM actor a
   WHERE (SELECT COUNT(*)
          FROM film_actor fa
          WHERE fa.actor_id = a.actor_id) > 10;
   ```

3. **Categories with more than 50 films:**

   ```sql
   SELECT c.category_id, c.name
   FROM category c
   WHERE (SELECT COUNT(*)
          FROM film_category fc
          WHERE fc.category_id = c.category_id) > 50;
   ```

4. **Customers who have spent more than average total payment:**

   ```sql
   SELECT c.customer_id, c.first_name, c.last_name
   FROM customer c
   WHERE (SELECT SUM(p.amount)
          FROM payment p
          WHERE p.customer_id = c.customer_id) >
         (SELECT AVG(total_payment)
          FROM (SELECT customer_id, SUM(amount) AS total_payment
                FROM payment
                GROUP BY customer_id) AS customer_payments);
   ```

5. **Films with more actors than average:**

   ```sql
   SELECT f.film_id, f.title
   FROM film f
   WHERE (SELECT COUNT(*)
          FROM film_actor fa
          WHERE fa.film_id = f.film_id) >
         (SELECT AVG(actor_count)
          FROM (SELECT film_id, COUNT(*) AS actor_count
                FROM film_actor
                GROUP BY film_id) AS film_actor_counts);
   ```

</details>

---

# **Lesson 2: Set Operations**

Set operations allow you to combine results from multiple `SELECT` statements.

## **2.1 UNION and UNION ALL**

- **`UNION`** combines results from two queries and removes duplicates.
- **`UNION ALL`** combines results and includes duplicates.

**Example:**

List all city names from the `city` and `address` tables.

```sql
SELECT city
FROM city

UNION

SELECT address
FROM address;
```

**Explanation:**

- Combines city names from both tables, removing duplicates.

## **2.2 INTERSECT**

`INTERSECT` returns only the records that are common to both queries.

**Example:**

Find names of cities that are listed both in the `city` and `address` tables.

```sql
SELECT city
FROM city

INTERSECT

SELECT address
FROM address;
```

**Explanation:**

- Retrieves city names present in both tables.

## **2.3 EXCEPT**

`EXCEPT` returns records from the first query that are not in the second query.

**Example:**

List city names in the `city` table that do not appear in the `address` table.

```sql
SELECT city
FROM city

EXCEPT

SELECT address
FROM address;
```

**Explanation:**

- Retrieves city names unique to the `city` table.

---

## **Exercise 2**

### **Objective:**

Practice using set operations.

### **Tasks:**

1. List all unique first names from the `customer` and `staff` tables.
2. Find film titles that are in the `film` table but not associated with any category.
3. Retrieve actor IDs that are present in both `actor` and `film_actor` tables.
4. List customer emails that are not present in the `staff` table.
5. Combine all postal codes from `address` and `city` tables, including duplicates.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Unique first names from `customer` and `staff`:**

   ```sql
   SELECT first_name FROM customer

   UNION

   SELECT first_name FROM staff;
   ```

2. **Film titles not associated with any category:**

   ```sql
   SELECT title FROM film

   EXCEPT

   SELECT f.title
   FROM film f
   JOIN film_category fc ON f.film_id = fc.film_id;
   ```

3. **Actor IDs present in both `actor` and `film_actor`:**

   ```sql
   SELECT actor_id FROM actor

   INTERSECT

   SELECT actor_id FROM film_actor;
   ```

4. **Customer emails not in `staff` table:**

   ```sql
   SELECT email FROM customer

   EXCEPT

   SELECT email FROM staff WHERE email IS NOT NULL;
   ```

5. **All postal codes from `address` and `city` including duplicates:**

   ```sql
   SELECT postal_code FROM address

   UNION ALL

   SELECT postal_code FROM city;
   ```

</details>

---

# **Lesson 3: Common Table Expressions (CTEs)**

CTEs allow you to define temporary result sets that can be referenced within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement.

## **3.1 Writing a CTE**

**Syntax:**

```sql
WITH cte_name (column1, column2, ...) AS (
    SELECT ...
)
SELECT ...
FROM cte_name;
```

**Example:**

Calculate the average payment amount and list payments above the average.

```sql
WITH avg_payment AS (
    SELECT AVG(amount) AS avg_amount FROM payment
)
SELECT p.payment_id, p.amount
FROM payment p, avg_payment
WHERE p.amount > avg_payment.avg_amount;
```

**Explanation:**

- The CTE `avg_payment` computes the average payment amount.
- The main query selects payments above this average.

## **3.2 Recursive CTEs**

Recursive CTEs are used to perform hierarchical queries.

**Example:**

Suppose we have an organizational hierarchy (not present in our schema). Here's how a recursive CTE might look:

```sql
WITH RECURSIVE employee_hierarchy AS (
    SELECT employee_id, manager_id, first_name, last_name
    FROM employee
    WHERE manager_id IS NULL

    UNION ALL

    SELECT e.employee_id, e.manager_id, e.first_name, e.last_name
    FROM employee e
    INNER JOIN employee_hierarchy eh ON e.manager_id = eh.employee_id
)
SELECT * FROM employee_hierarchy;
```

**Note:** This is just an illustrative example as our schema doesn't include such a hierarchy.

---

## **Exercise 3**

### **Objective:**

Practice using CTEs.

### **Tasks:**

1. Use a CTE to list films along with their actor count.
2. Create a CTE to find the top 5 customers who have made the most payments.
3. Use a CTE to compute the average film length per rating and list ratings with an average length over 120 minutes.
4. Write a recursive CTE to generate a sequence of numbers from 1 to 10.
5. Use a CTE to list categories along with the total number of films in each.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Films with actor count:**

   ```sql
   WITH film_actor_count AS (
       SELECT fa.film_id, COUNT(*) AS actor_count
       FROM film_actor fa
       GROUP BY fa.film_id
   )
   SELECT f.title, fac.actor_count
   FROM film f
   JOIN film_actor_count fac ON f.film_id = fac.film_id;
   ```

2. **Top 5 customers by total payments:**

   ```sql
   WITH customer_payments AS (
       SELECT customer_id, SUM(amount) AS total_paid
       FROM payment
       GROUP BY customer_id
   )
   SELECT c.customer_id, c.first_name, c.last_name, cp.total_paid
   FROM customer c
   JOIN customer_payments cp ON c.customer_id = cp.customer_id
   ORDER BY cp.total_paid DESC
   LIMIT 5;
   ```

3. **Ratings with average film length over 120 minutes:**

   ```sql
   WITH rating_avg_length AS (
       SELECT rating, AVG(length) AS avg_length
       FROM film
       GROUP BY rating
   )
   SELECT rating, avg_length
   FROM rating_avg_length
   WHERE avg_length > 120;
   ```

4. **Recursive CTE to generate numbers from 1 to 10:**

   ```sql
   WITH RECURSIVE numbers AS (
       SELECT 1 AS n
       UNION ALL
       SELECT n + 1 FROM numbers WHERE n < 10
   )
   SELECT n FROM numbers;
   ```

5. **Categories with total number of films:**

   ```sql
   WITH category_film_count AS (
       SELECT c.category_id, c.name, COUNT(fc.film_id) AS film_count
       FROM category c
       LEFT JOIN film_category fc ON c.category_id = fc.category_id
       GROUP BY c.category_id, c.name
   )
   SELECT * FROM category_film_count;
   ```

</details>

---

# **Lesson 4: Window Functions**

Window functions perform calculations across a set of table rows that are somehow related to the current row.

## **4.1 Introduction to Window Functions**

Common window functions include:

- **`ROW_NUMBER()`**: Assigns a unique sequential integer to rows within a partition.
- **`RANK()`**: Assigns a rank to each row within a partition, with gaps for ties.
- **`DENSE_RANK()`**: Similar to `RANK()`, but without gaps.
- **Aggregate functions** over a window: `SUM()`, `AVG()`, etc.

## **4.2 Syntax**

```sql
SELECT column1,
       window_function() OVER (
           PARTITION BY column2
           ORDER BY column3
       ) AS alias
FROM table_name;
```

## **4.3 Examples**

**Example 1: Ranking Films by Rental Rate**

```sql
SELECT title, rental_rate,
       RANK() OVER (ORDER BY rental_rate DESC) AS rental_rate_rank
FROM film;
```

**Explanation:**

- Assigns a rank to each film based on the `rental_rate`, in descending order.

**Example 2: Calculating Running Total of Payments per Customer**

```sql
SELECT p.customer_id, p.payment_date, p.amount,
       SUM(p.amount) OVER (
           PARTITION BY p.customer_id
           ORDER BY p.payment_date
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total
FROM payment p;
```

**Explanation:**

- Calculates a running total of payments for each customer over time.

---

## **Exercise 4**

### **Objective:**

Practice using window functions.

### **Tasks:**

1. Assign a sequential row number to each customer based on their last name in ascending order.
2. For each film category, rank films by their length in descending order.
3. Calculate the cumulative amount paid by each customer over time.
4. Find the average rental duration for each film, compared to the average rental duration across all films.
5. List the top 3 films with the highest rental rates in each category.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Row number for customers by last name:**

   ```sql
   SELECT c.customer_id, c.first_name, c.last_name,
          ROW_NUMBER() OVER (ORDER BY c.last_name ASC) AS row_num
   FROM customer c;
   ```

2. **Ranking films by length within each category:**

   ```sql
   SELECT f.title, c.name AS category_name, f.length,
          RANK() OVER (PARTITION BY c.category_id ORDER BY f.length DESC) AS length_rank
   FROM film f
   JOIN film_category fc ON f.film_id = fc.film_id
   JOIN category c ON fc.category_id = c.category_id;
   ```

3. **Cumulative amount paid by each customer:**

   ```sql
   SELECT p.customer_id, p.payment_date, p.amount,
          SUM(p.amount) OVER (
              PARTITION BY p.customer_id
              ORDER BY p.payment_date
              ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
          ) AS cumulative_amount
   FROM payment p;
   ```

4. **Average rental duration per film compared to overall average:**

   ```sql
   SELECT f.title, f.rental_duration,
          AVG(f.rental_duration) OVER () AS overall_avg_duration,
          AVG(f.rental_duration) OVER () - f.rental_duration AS difference
   FROM film f;
   ```

5. **Top 3 films with highest rental rates in each category:**

   ```sql
   SELECT category_name, title, rental_rate
   FROM (
       SELECT c.name AS category_name, f.title, f.rental_rate,
              DENSE_RANK() OVER (PARTITION BY c.category_id ORDER BY f.rental_rate DESC) AS rate_rank
       FROM film f
       JOIN film_category fc ON f.film_id = fc.film_id
       JOIN category c ON fc.category_id = c.category_id
   ) sub
   WHERE rate_rank <= 3;
   ```

</details>

---

# **Lesson 5: Advanced Joins**

## **5.1 Self Joins**

A self join is a regular join, but the table is joined with itself.

**Example:**

Find pairs of films released in the same year.

```sql
SELECT f1.title AS film1, f2.title AS film2, f1.release_year
FROM film f1
JOIN film f2 ON f1.release_year = f2.release_year AND f1.film_id < f2.film_id;
```

**Explanation:**

- Joins `film` table to itself to find films with the same release year.
- The condition `f1.film_id < f2.film_id` avoids duplicate pairs and self-pairing.

## **5.2 Non-Equijoins**

Joins based on conditions other than equality.

**Example:**

Find rentals where the return date is after the due date (assuming due date is rental date plus rental duration).

```sql
SELECT r.rental_id, r.rental_date, r.return_date, f.rental_duration
FROM rental r
JOIN inventory i ON r.inventory_id = i.inventory_id
JOIN film f ON i.film_id = f.film_id
WHERE r.return_date > (r.rental_date + (f.rental_duration || ' days')::interval);
```

**Explanation:**

- Calculates due date by adding `rental_duration` to `rental_date`.
- Compares `return_date` to due date.

---

## **Exercise 5**

### **Objective:**

Practice advanced join techniques.

### **Tasks:**

1. Find all pairs of customers who have the same address.
2. List films where the rental rate is higher than the replacement cost of other films.
3. Identify staff members who live in the same city as their store.
4. Find films that have never been rented.
5. List customers who have rented all films in a particular category (e.g., 'Action').

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Customers with the same address:**

   ```sql
   SELECT c1.customer_id AS customer1_id, c2.customer_id AS customer2_id, a.address
   FROM customer c1
   JOIN customer c2 ON c1.address_id = c2.address_id AND c1.customer_id < c2.customer_id
   JOIN address a ON c1.address_id = a.address_id;
   ```

2. **Films where rental rate > replacement cost of other films:**

   ```sql
   SELECT f1.title AS expensive_film, f1.rental_rate, f2.title AS cheap_film, f2.replacement_cost
   FROM film f1
   JOIN film f2 ON f1.rental_rate > f2.replacement_cost;
   ```

3. **Staff living in the same city as their store:**

   ```sql
   SELECT s.staff_id, s.first_name, s.last_name, ci.city
   FROM staff s
   JOIN address sa ON s.address_id = sa.address_id
   JOIN store st ON s.store_id = st.store_id
   JOIN address sta ON st.address_id = sta.address_id
   JOIN city ci ON sa.city_id = ci.city_id AND sta.city_id = ci.city_id;
   ```

4. **Films never rented:**

   ```sql
   SELECT f.film_id, f.title
   FROM film f
   LEFT JOIN inventory i ON f.film_id = i.film_id
   LEFT JOIN rental r ON i.inventory_id = r.inventory_id
   WHERE r.rental_id IS NULL;
   ```

5. **Customers who have rented all 'Action' films:**

   ```sql
   SELECT c.customer_id, c.first_name, c.last_name
   FROM customer c
   WHERE NOT EXISTS (
       SELECT fc.film_id
       FROM film_category fc
       JOIN category cat ON fc.category_id = cat.category_id
       WHERE cat.name = 'Action' AND
             fc.film_id NOT IN (
                 SELECT r2.inventory_id
                 FROM rental r2
                 JOIN inventory i2 ON r2.inventory_id = i2.inventory_id
                 WHERE r2.customer_id = c.customer_id AND i2.film_id = fc.film_id
             )
   );
   ```

</details>

---

# **Lesson 6: Working with Views**

## **6.1 Creating Views**

A view is a virtual table based on the result set of a SQL statement.

**Syntax:**

```sql
CREATE VIEW view_name AS
SELECT columns
FROM tables
WHERE conditions;
```

**Example:**

Create a view that lists customer payments.

```sql
CREATE VIEW customer_payments AS
SELECT c.customer_id, c.first_name, c.last_name, p.amount, p.payment_date
FROM customer c
JOIN payment p ON c.customer_id = p.customer_id;
```

## **6.2 Using Views**

You can query a view just like a regular table.

```sql
SELECT * FROM customer_payments WHERE amount > 5.00;
```

## **6.3 Updating Data Through Views**

If a view is updatable, you can perform `INSERT`, `UPDATE`, or `DELETE` operations on it.

---

## **Exercise 6**

### **Objective:**

Practice creating and using views.

### **Tasks:**

1. Create a view named `top_customers` that lists customers with total payments over $100.
2. Create a view `film_list` that includes film title, category name, and language.
3. Use the `top_customers` view to find out how many customers have total payments over $150.
4. Update the `film_list` view to include the rental rate.
5. Drop the `top_customers` view.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Create `top_customers` view:**

   ```sql
   CREATE VIEW top_customers AS
   SELECT c.customer_id, c.first_name, c.last_name, SUM(p.amount) AS total_payments
   FROM customer c
   JOIN payment p ON c.customer_id = p.customer_id
   GROUP BY c.customer_id, c.first_name, c.last_name
   HAVING SUM(p.amount) > 100;
   ```

2. **Create `film_list` view:**

   ```sql
   CREATE VIEW film_list AS
   SELECT f.title, c.name AS category_name, l.name AS language_name
   FROM film f
   JOIN film_category fc ON f.film_id = fc.film_id
   JOIN category c ON fc.category_id = c.category_id
   JOIN "language" l ON f.language_id = l.language_id;
   ```

3. **Find customers with total payments over $150 using `top_customers`:**

   ```sql
   SELECT COUNT(*) FROM top_customers WHERE total_payments > 150;
   ```

4. **Update `film_list` view (must drop and recreate):**

   ```sql
   DROP VIEW film_list;

   CREATE VIEW film_list AS
   SELECT f.title, c.name AS category_name, l.name AS language_name, f.rental_rate
   FROM film f
   JOIN film_category fc ON f.film_id = fc.film_id
   JOIN category c ON fc.category_id = c.category_id
   JOIN "language" l ON f.language_id = l.language_id;
   ```

5. **Drop `top_customers` view:**

   ```sql
   DROP VIEW top_customers;
   ```

</details>

---

# **Lesson 7: Indexes and Performance Tuning**

## **7.1 Understanding Indexes**

Indexes improve the speed of data retrieval but can slow down write operations.

**Creating an Index:**

```sql
CREATE INDEX idx_customer_last_name ON customer(last_name);
```

## **7.2 Types of Indexes**

- **B-tree Indexes:** Default type; good for most cases.
- **Hash Indexes:** Useful for equality comparisons.
- **GIN and GiST Indexes:** Used for full-text search and geometric data types.

## **7.3 Query Optimization**

Use `EXPLAIN` to analyze query performance.

**Example:**

```sql
EXPLAIN ANALYZE
SELECT * FROM customer WHERE last_name = 'Smith';
```

---

## **Exercise 7**

### **Objective:**

Practice creating indexes and analyzing query performance.

### **Tasks:**

1. Create an index on the `rental_date` column in the `rental` table.
2. Analyze the performance difference when querying `rental` with and without the index.
3. Create a composite index on `first_name` and `last_name` in the `customer` table.
4. Use `EXPLAIN` to compare query plans for a search on `first_name` and `last_name`.
5. Drop the index on the `rental_date` column.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Create index on `rental_date`:**

   ```sql
   CREATE INDEX idx_rental_date ON rental(rental_date);
   ```

2. **Analyze performance:**

   - Before index creation, run:

     ```sql
     EXPLAIN ANALYZE
     SELECT * FROM rental WHERE rental_date = '2005-05-25';
     ```

   - After index creation, run the same query to see performance improvement.

3. **Create composite index on `first_name` and `last_name`:**

   ```sql
   CREATE INDEX idx_customer_name ON customer(first_name, last_name);
   ```

4. **Compare query plans:**

   ```sql
   EXPLAIN ANALYZE
   SELECT * FROM customer WHERE first_name = 'John' AND last_name = 'Doe';
   ```

5. **Drop index on `rental_date`:**

   ```sql
   DROP INDEX idx_rental_date;
   ```

</details>

---

# **Lesson 8: Transactions and Concurrency Control**

## **8.1 Transactions**

A transaction is a unit of work that is either fully completed or not done at all.

**Starting a Transaction:**

```sql
BEGIN;
-- SQL statements
COMMIT;
```

**Rolling Back a Transaction:**

```sql
BEGIN;
-- SQL statements
ROLLBACK;
```

## **8.2 Isolation Levels**

Isolation levels define how transactions interact with each other.

- **Read Uncommitted**
- **Read Committed** (PostgreSQL default)
- **Repeatable Read**
- **Serializable**

**Setting Isolation Level:**

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

## **8.3 Concurrency Control**

Handles multiple transactions occurring at the same time.

---

## **Exercise 8**

### **Objective:**

Practice managing transactions and understanding isolation levels.

### **Tasks:**

1. Start a transaction and insert a new customer, but do not commit.
2. In another session, try to read the uncommitted data.
3. Commit the transaction and verify the data is now visible.
4. Set the isolation level to `SERIALIZABLE` and attempt concurrent transactions that would cause a conflict.
5. Explain what happens when conflicts occur under different isolation levels.

### **Solutions:**

<details>
  <summary>Click here to view solutions</summary>

1. **Start transaction and insert customer:**

   ```sql
   BEGIN;
   INSERT INTO customer (store_id, first_name, last_name, email, address_id) VALUES
   (1, 'Alice', 'Smith', 'alice.smith@example.com', 1);
   -- Do not commit yet
   ```

2. **In another session, try to read uncommitted data:**

   ```sql
   SELECT * FROM customer WHERE first_name = 'Alice';
   ```

   - Under the default `Read Committed` level, the new customer will not be visible until committed.

3. **Commit transaction and verify data:**

   ```sql
   COMMIT;
   ```

   - Now the new customer is visible in other sessions.

4. **Set isolation level to `SERIALIZABLE`:**

   ```sql
   SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
   BEGIN;
   -- Transaction statements
   ```

   - Attempt to perform conflicting transactions in different sessions.

5. **Explanation:**

   - Under `SERIALIZABLE`, PostgreSQL ensures transactions are executed in a way that they could have been run serially.
   - If conflicts occur, one transaction may be rolled back with a serialization error.

</details>

---

# **Conclusion**

Congratulations on completing the intermediate level SQL course! You've expanded your knowledge and are now equipped with advanced SQL techniques. You've learned to:

- Write complex subqueries and use set operations.
- Utilize Common Table Expressions (CTEs), including recursive CTEs.
- Apply window functions for advanced data analysis.
- Perform advanced joins, including self joins and non-equijoins.
- Create and manage views.
- Optimize queries with indexes and analyze performance.
- Manage transactions and understand concurrency control.

## **Next Steps**

- **Practice:** Continue writing complex queries to reinforce your learning.
- **Advanced Level:** You're now ready to tackle advanced topics like stored procedures, triggers, and database administration.
- **Explore PostgreSQL Features:** Dive deeper into PostgreSQL-specific features and extensions.

Feel free to reach out with any questions or for further assistance. Happy querying!
