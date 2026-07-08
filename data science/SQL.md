
Functions:
1. The SUBSTR() function extracts a substring from a string (starting at any position).

```mysql
SUBSTR(string, start, length) 

or 

SUBSTR(_string_ FROM _start_ FOR _length_)
```

2. The INSTR() function returns the position of the first occurrence of a string in another string.
 This function performs a case-insensitive search.
 1 indexing

```mysql
INSTR(string1, string2);
```

3. A wildcard character is used to substitute one or more characters in a string.

Wildcard characters are used with the `[LIKE](https://www.w3schools.com/sql/sql_like.asp)` operator. The `LIKE` operator is used in a `WHERE` clause to search for a specified pattern in a column.
The `%` wildcard represents any number of characters, even zero characters.
The `_` wildcard represents a single character.
```
SELECT * FROM Customers  
WHERE CustomerName LIKE '%mer%';

SELECT * FROM Customers  
WHERE City LIKE '_ondon';
```

For a view to be updatable, the database must be able to map the new row you are inserting directly back to one specific row in the underlying base table. Because of this, **a view becomes read-only (non-updatable) if it contains any of the following:**

- Aggregate functions (`COUNT`, `SUM`, `MAX`, etc.)
- A `GROUP BY` clause
- A `DISTINCT` keyword
- `UNION` operations

If you try to insert data into a view that has a `GROUP BY` or `DISTINCT`, the database will throw an error because it mathematically cannot figure out how to distribute your inserted data into the raw, un-grouped base table.

Adding `WITH CHECK OPTION` prevents this. It forces the database to check every `INSERT` or `UPDATE` against the view's `WHERE` clause. If the new data doesn't match the criteria the database blocks the insertion.

### Window Functions

### Value Window Functions (Column Required)

These functions pull actual values from other rows (like the row before, or the first row in the group). Just like aggregates, they need a column name so they know **which data** to fetch.

- `LEAD(hire_date)`: Grabs the hire date of the _next_ row.
    
- `LAG(hire_date)`: Grabs the hire date of the _previous_ row.
    
- `FIRST_VALUE(employee_id)`: Grabs the ID from the _first_ row in the partition.
    
- `LAST_VALUE(employee_id)`: Grabs the ID from the _last_ row in the partition.

```mysql
SELECT 
    department,
    employee_name,
    revenue,
    
    -- Grab the name from the absolute TOP of the sorted department list
    FIRST_VALUE(employee_name) OVER (
        PARTITION BY department 
        ORDER BY revenue DESC
    ) AS top_performer,
    
    -- Grab the name from the absolute BOTTOM of the sorted department list
    LAST_VALUE(employee_name) OVER (
        PARTITION BY department 
        ORDER BY revenue DESC 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS bottom_performer
    
FROM 
    department_sales;
```

When you use `GROUP BY department_id`, SQL enforces a strict rule: **The final result can only have exactly one row for each unique department.**

```mysql
SELECT column_name1,   
       window_function()   
       OVER ([PARTITION BY column_name3] [ORDER BY column_name4]) AS new_column  
FROM table_name;
```

**`PARTITION BY` preserves rows:** When used inside an `OVER()` clause, `PARTITION BY department_id` tells the window function to logically group the calculations by department, **without** squashing the rows together. You still get to see every single employee row, but the rank counter resets back to 1 every time it encounters a new department.

- **`ROW_NUMBER()`:** Assigns a strictly unique, sequential integer to every row (1, 2, 3, 4). If two employees were hired on the exact same day, one gets 1 and the other gets 2 randomly.

- **`RANK()`:** Assigns the same rank to ties but skips subsequent numbers (1, 1, 3, 4).

- **`DENSE_RANK()`:** Assigns the same rank to ties but does _not_ skip subsequent numbers (1, 1, 2, 3). If two people were hired on Day 1, they both tie for 1st. The next person hired gets 2nd place.
### note on ROW_NUMBER()

**What it does:** Acts as a strict, sequential counter (1, 2, 3...). It does _not_ rank based on the data's actual mathematical value.
**Syntax:** `ROW_NUMBER() OVER (PARTITION BY [Col A] ORDER BY [Col B])`

**The 3 Steps:**

1. **`PARTITION BY` (The Buckets):** Groups rows by `Col A`. **Crucial rule:** Every time the group changes, the **counter resets to 1**.
2. **`ORDER BY` (The Sorting):** Looks strictly _inside_ the current bucket and lines rows up by `Col B` (decides who gets #1, #2, etc.).
3. **The Counting:** Blindly assigns 1, 2, 3 down the sorted line until the bucket ends.

```mysql
SELECT 
    employee_id, 
    hire_date, 
    department_id 
FROM (
    -- INNER QUERY (Subquery): Calculates the rank for every employee
    SELECT 
        employee_id, 
        hire_date, 
        department_id, 
        
-- WINDOW FUNCTION: Assigns a rank to each row without skipping numbers for ties
        DENSE_RANK() OVER(
 -- PARTITION BY: Divides the data into groups; the rank resets for each new department
            PARTITION BY department_id 
            
 -- ORDER BY: Sorts the employees chronologically by their hire date within their department group
            ORDER BY hire_date
            
        ) AS rn -- 'rn' is the alias created for this new calculated rank column
        
    FROM 
        employees
) 
-- FILTER: Evaluates the subquery results and retrieves only the employees who ranked 2nd
WHERE 
    rn = 2;
```


4. == not used here, only = used to compare
5. null cannot be compared usually, use "is null"
6. distinct is used to remove duplicates
7. order by is used to sort..

The order:
**1.FROM and JOIN:** Where is the data?.
The database must first find the tables you are talking about. It goes to the hard drive, grabs the `Employee` table, and if there are any `JOIN`s, it links the tables together into one massive temporary grid.

**2.WHERE:** The Front Door Bouncer.
Before doing any math or grouping, it looks at every single raw row in that grid. If a row doesn't meet the `WHERE` condition, it is instantly thrown in the trash.

**3.GROUP BY:** Creating the Buckets.
The database takes the surviving rows and squishes them into buckets based on the column you specified (e.g., throwing all employees who make $300 into one bucket, and $200 into another).

**4.HAVING:** Filtering the Buckets.
This is the second bouncer. It looks at the _buckets_ created in Step 3. If a bucket doesn't meet the `HAVING` condition (like `COUNT(*) > 1`), the entire bucket is thrown in the trash.

**5.SELECT:** Grabbing the Columns.
Now that the data is fully filtered and grouped, the database finally looks at the top of your query. It chops off all the columns you didn't ask for and calculates any math you requested (like `MAX(salary)` or `AVG(rating)`).

**6.DISTINCT:** Removing Duplicates.
_This is where your question comes in!_ The database looks at the exact columns you just `SELECT`ed. If it sees identical rows, it deletes the duplicates, leaving only unique values.

**7.ORDER BY:** Sorting the Final List.
The database takes the deduplicated, finalized columns and alphabetizes or sorts them numerically (`ASC` or `DESC`).

**8.LIMIT and OFFSET:** Slicing the Cake.
Finally, as the very last step before handing the data back to your screen, the database skips over the rows you told it to ignore (`OFFSET`) and hands you only the exact number of rows you requested (`LIMIT`).


```sql
select distinct author_id as "id" from Views where author_id = viewer_id order by author_id;
```

char_length() - gives number of char, length()- gives number of bytes
for ascii characters both give same value
```sql
select tweet_id from Tweets where char_length(content) > 15;
```

Left join - keep all the rows of left table, if corresponding row is in right table then fine else null
```mysql
# Write your MySQL query statement below
--                                              left table            right table
select EmployeeUNI.unique_id, Employees.name from Employees left join EmployeeUNI on Employees.id = EmployeeUNI.id;
```

```mysql
select v.customer_id, count(v.visit_id) as count_no_trans
from Visits v
left join Transactions t
on v.visit_id = t.visit_id
where t.amount is null
group by v.customer_id;
```

```mysql
select a1.machine_id, round(avg(a2.timestamp-a1.timestamp), 3) as processing_time from Activity a1
join Activity a2
on a1.machine_id=a2.machine_id and a1.process_id=a2.process_id and a1.activity_type='start' and a2.activity_type='end' group by a1.machine_id
```

```mysql
-- Problem: LeetCode 1251 (Average Selling Price)
-- Goal: Calculate the average selling price per product, accounting for date-specific price changes.

SELECT 
    p.product_id, 
    
    -- STEP 4: Calculate the average price.
    -- SUM(p.price * u.units) = Total Revenue
    -- SUM(u.units) = Total Items Sold
    -- ROUND(..., 2) forces exactly 2 decimal places.
    -- IFNULL(..., 0) ensures products with zero sales output '0' instead of 'NULL'.
    IFNULL(ROUND(SUM(p.price * u.units) / SUM(u.units), 2), 0) AS average_price

-- STEP 1: Start with the Prices table (aliased as 'p')
FROM 
    Prices p
    
-- STEP 2: Attach the UnitsSold table (aliased as 'u')
-- A LEFT JOIN guarantees we keep all products, even if they had 0 sales.
LEFT JOIN 
    UnitsSold u 
    
-- STEP 3: Define the strict rules for joining the tables.
ON 
    -- 1st Rule: The Product IDs must match.
    p.product_id = u.product_id 
    
    -- 2nd Rule: The sale date must fall within that specific price's active window.
    AND u.purchase_date BETWEEN p.start_date AND p.end_date

-- STEP 5: Group the massive combined table into buckets by product.
-- This ensures our SUM() math above is calculated per product, not for the whole table.
GROUP BY 
    p.product_id;
```


```mysql
# Write your MySQL query statement below

select p.project_id , round(avg(e.experience_years) , 2) as average_years

from Project p

left join Employee e

on p.employee_id = e.employee_id

group by project_id;
```

577. Employee Bonus
```mysql
select e.name as name, b.bonus as bonus

from Employee e

left join Bonus b

on e.empId  = b.empId

where b.bonus < 1000
-- When a `LEFT JOIN` fails to find a match, it fills the `b.bonus` column with `NULL`
or b.bonus is null;
```

Here is the secret of how SQL joins work: **When a row on the left side finds MULTIPLE matching rows on the right side, SQL duplicates the left row so it can display every single match.** (This is often called "row multiplication" or "fan-out").

if you use a `GROUP BY`, any raw column in your `SELECT` statement _must_ also exist in your `GROUP BY` statement. 

1280. Students and examinations
```mysql
select s.student_id , s.student_name, sub.subject_name,  count(e.student_id) as attended_exams

from Students s cross join Subjects sub
left join Examinations e
on s.student_id = e.student_id and sub.subject_name = e.subject_name

group by
s.student_id,
s.student_name,
sub.subject_name

order by

s.student_id,
sub.subject_name;
```

570. Managers with at least 5 direct reports
The secret is understanding that `GROUP BY` doesn't instantly delete the raw data. Instead, it creates **invisible buckets** in the database engine's memory.
```mysql
select e1.name as name
from Employee e1
join Employee e2
on e1.id = e2.managerId
group by e1.id
having count(e2.id) >= 5;
```

1934. Confirmation Rate
```mysql
select s.user_id,

round(ifnull(sum(c.action = 'confirmed')/(count(c.action)) ,0 ),  2) as confirmation_rate

from Signups s
left join Confirmations c
on s.user_id = c.user_id

group by s.user_id;
```
1731. Number of employees which report to each employee
```mysql
select e1.employee_id, e1.name, count(e2.employee_id) as reports_count, round(avg(e2.age), 0) as average_age

from Employees e1
join Employees e2
on e1.employee_id = e2.reports_to

group by e1.employee_id, e1.name
order by e1.employee_id;
```

1789. Primary department for each employee
```mysql
select employee_id, department_id

from Employee

where primary_flag = 'Y' OR
    employee_id in
    (select employee_id
    from Employee
    group by employee_id
    having count(*) = 1);
```



```mysql
-- Problem: LeetCode 1633 (Percentage of Users Attended a Contest)
-- Goal: Find the percentage of total users registered for each contest.

SELECT 
    contest_id, 
    
    -- Calculate the percentage
    ROUND(
        -- NUMERATOR: Count of users in this specific contest bucket
        COUNT(DISTINCT user_id) * 100 
        / 
        -- DENOMINATOR: Mini-query to grab the total number of users on the platform
        (SELECT COUNT(user_id) FROM Users), 
        
    2) AS percentage

-- Base table
FROM 
    Register
    
-- Bucket the data by contest
GROUP BY 
    contest_id
    
-- Sort by highest percentage first. Break ties using contest_id (ascending).
ORDER BY 
    percentage DESC, 
    contest_id;
```


```mysql
select query_name, round(avg(rating / position), 2) as quality,
round(sum(case when rating < 3 then 1 else 0 end) * 100 / count(*), 2) as poor_query_percentage
from queries
group by query_name;
```

- **The `CASE` Statement:** `case when rating < 3 then 1 else 0 end`. This tells the database to look at every row in the current bucket. If the rating is 1 or 2, it assigns a value of `1`. If the rating is 3, 4, or 5, it assigns a `0`.
    
- **The Numerator:** `sum(...)` adds up all those `1`s and `0`s, which gives you the exact count of poor queries.
    
- **The Denominator:** `count(*)` gets the total number of rows in the current bucket.
    
- **The Math:** It divides the poor queries by the total queries, multiplies by `100` to make it a percentage, and rounds it to two decimal places.


```mysql
-- Problem: LeetCode 1193 (Monthly Transactions I)

SELECT 
    -- Grab the first 7 characters of the date for the YYYY-MM format
    LEFT(trans_date, 7) AS month, 
    
    country, 
    
    -- Total transactions in this bucket
    COUNT(*) AS trans_count, 
    
    -- Conditional aggregation for approved transaction count
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count, 
    
    -- Total amount of all transactions in this bucket
    SUM(amount) AS trans_total_amount, 
    
    -- Conditional aggregation for approved transaction total amount
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount

FROM 
    Transactions

-- Group by multiple columns using a comma!
GROUP BY 
    month, 
    country;
```


(tuple) IN (subquery)
```mysql
-- Problem: LeetCode 1174 (Immediate Food Delivery II)

SELECT 
    -- 2. Calculate the overall percentage of those first orders
    ROUND(SUM(IF(order_date = customer_pref_delivery_date, 1, 0)) * 100 / COUNT(*), 2) AS immediate_percentage
FROM 
    Delivery
WHERE 
    -- 1. Filter the table so we are ONLY looking at the first order for each customer
    (customer_id, order_date) IN (
        SELECT 
            customer_id, 
            MIN(order_date)
        FROM 
            Delivery
        GROUP BY 
            customer_id
    );
```

You cannot put an aggregate function like `MIN()` directly next to a standard column like `order_date` inside a `CASE` statement.

- SQL processes aggregate functions on the _entire bucket_ at once.
    
- `CASE WHEN` statements are processed _row by row_. Because SQL cannot evaluate a single row and the whole bucket at the exact same time, this throws an error.

2356. Number of Unique Subjects Taught by Each Teacher
```mysql
# Write your MySQL query statement below

select teacher_id, count(distinct subject_id) as cnt
from Teacher
group by teacher_id;
```

1141. User Activity for the past 30 days
```mysql
# Write your MySQL query statement below

select activity_date as day, count(distinct user_id) as active_users from Activity where datediff('2019-07-27' , activity_date) between 0 and 29 group by activity_date;

-- same as

select activity_date as day, count(distinct user_id) as active_users from Activity where datediff('2019-07-27' , activity_date) <= 29 and datediff('2019-07-27' , activity_date) >=0  group by activity_date;
```

1070.   **you cannot put an aggregate function like `MIN()` inside a `WHERE` clause.** * The `WHERE` clause acts as a filter _before_ any grouping happens.

- `MIN()` requires the database to look at the whole bucket of data at once. Because the database hasn't built the buckets yet, it panics and throws an error when it sees `MIN()` here.

```mysql
# Write your MySQL query statement below

select product_id , year as first_year, quantity , price

from Sales

where (product_id, year) in (

    select product_id, min(year)

    from Sales

    group by product_id

);
```

596. Classes with at least 5 students
```mysql
wrong: XXXX
select class from Courses where count(student) >=5 group by class;

correct: 
select class
from courses
group by class
having count(student) >=5;
```

619. Biggest Single Number

```mysql
# Write your MySQL query statement below
select max(num) as num
from (
    select num
    from MyNumbers
    group by num
    having count(*) = 1
) as single_nums;
```

1045. Customers who bought all products
```mysql
SELECT 
    customer_id 
FROM 
    Customer 

-- STEP 1: Group the purchase history into buckets by customer
GROUP BY 
    customer_id

-- STEP 4: Filter the grouped buckets using HAVING
HAVING 
    -- STEP 2: Count how many UNIQUE products this specific customer bought
    COUNT(DISTINCT product_key) 
    = 
    -- STEP 3: Subquery to find the total number of products that exist in the store
    (SELECT COUNT(product_key) FROM Product);
```

1978. Employees whose manager left the company
```mysql
SELECT employee_id
FROM Employees
WHERE salary < 30000 
  -- Check if their manager's ID is missing from the master list of employees
  AND manager_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;
```

In SQL, **`WHERE`** is used to filter individual rows _before_ any grouping happens. **`HAVING`** is strictly used to filter buckets of data _after_ you use a `GROUP BY`.

Since you are just looking at individual employees and not grouping them into buckets, you must use a `WHERE` clause.

626. Exchange Seats
```mysql
# Write your MySQL query statement below
SELECT 
    CASE 
        -- Just check if this ID is NOT the absolute highest ID in the table
        WHEN id % 2 = 1 AND id != (SELECT MAX(id) FROM Seat) THEN id + 1
        WHEN id % 2 = 0 THEN id - 1
        ELSE id
    END AS id, 
    student
FROM 
    Seat
ORDER BY 
    id;
```

1341. Movie Rating
```mysql
-- Problem: LeetCode 1341 (Movie Rating)
-- QUERY 1: Find the user with the most ratings
(
    SELECT 
        u.name AS results
    FROM 
        Users u
    JOIN 
        MovieRating mr ON u.user_id = mr.user_id
    GROUP BY 
        u.user_id
    ORDER BY 
        COUNT(mr.movie_id) DESC, 
        u.name ASC
    LIMIT 1
)
UNION ALL
-- QUERY 2: Find the highest-rated movie in February 2020
(
    SELECT 
        m.title AS results
    FROM 
        Movies m
    JOIN 
        MovieRating mr ON m.movie_id = mr.movie_id
    -- Filter strictly for February 2020
    WHERE 
        mr.created_at LIKE '2020-02%'
    GROUP BY 
        m.movie_id
    ORDER BY 
        AVG(mr.rating) DESC, 
        m.title ASC
    LIMIT 1
);
```








String Functions

1667. Fix names in a table
```mysql
select user_id,
concat(upper(substr(name,1 ,1)), lower(substr(name, 2))) as name
from Users
order by user_id;
```

1527. Patients with a condition
```mysql
select patient_id, patient_name, conditions
from Patients
where conditions like "% DIAB1%" or conditions like "DIAB1%";
```


196. Delete Duplicate Emails
```mysql
DELETE p1 FROM Person p1 
JOIN Person p2 
ON p1.email = p2.email AND p1.id > p2.id;
```

176. Second Highest Salary
```mysql
SELECT 
    (
        SELECT DISTINCT salary 
        FROM Employee 
        ORDER BY salary DESC 
        -- LIMIT 1 means "grab exactly 1 row"
        -- OFFSET 1 means "skip the first 1 row before grabbing"
        LIMIT 1 OFFSET 1
    ) AS SecondHighestSalary;
    
    
    or 
    
    
    SELECT 
    MAX(salary) AS SecondHighestSalary
FROM 
    Employee
WHERE 
    salary < (SELECT MAX(salary) FROM Employee);
```

1484. Group sold products by the date
```mysql
select sell_date, count(distinct product) as num_sold , 
group_concat( distinct product order by product separator ',') as products

from Activities

group by sell_date

order by sell_date;
```

syntax for group_concat: only column name is compulsory
```MYSQL
GROUP_CONCAT( 
    [DISTINCT] 
    column_name 
    [ORDER BY column_name ASC/DESC] 
    [SEPARATOR 'your_string'] 
)
```

550. Game play analysis 4

```mysql
# Write your MySQL query statement below

select round(count(*)/(select count(distinct player_id) as total_players from Activity), 2) as fraction

from Activity a1

join

(
    select player_id, min(event_date) as first_login
    from Activity
    group by player_id

) a2

on a1.player_id = a2.player_id and datediff(a1.event_date , a2.first_login) = 1;
```

another method
```mysql
SELECT
  ROUND(COUNT(DISTINCT player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM
  Activity
WHERE
  -- Tuple matching: treats (player_id, prev_date) as a single unit to 
  -- see if it exists in the list of first-login dates from the subquery
  (player_id, DATE_SUB(event_date, INTERVAL 1 DAY))
  IN  (SELECT player_id, MIN(event_date) AS first_login FROM Activity GROUP BY player_id
  );
```

1321. Restaurant Growth

```mysql
select c1.visited_on,
(
    select sum(c2.amount)
    from Customer c2
    where c2.visited_on between date_sub(c1.visited_on, interval 6 day) and c1.visited_on

) as amount
, (

    select round(sum(c2.amount)/7 , 2)
    from Customer c2
    where c2.visited_on between date_sub(c1.visited_on, interval 6 day) and c1.visited_on

) as average_amount

from customer c1

where visited_on >= (select date_add(min(visited_on), interval 6 day) from customer)

group by c1.visited_on
order by c1.visited_on;
```

602. Friend request II : who has most friends
```mysql
SELECT id, COUNT(*) AS num
FROM (

    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
) AS all_friends
GROUP BY id
ORDER BY num DESC
LIMIT 1;
```

1327. List the products ordered in a period
```mysql
select p.product_name, sum(o.unit) as unit

from Products p

join Orders o

on p.product_id = o.product_id

where o.order_date between '2020-02-01' and '2020-02-29'

group by p.product_id

having sum(o.unit) >= 100;
```

1517. Find Users with valid e mails
```mysql
# Write your MySQL query statement below
SELECT user_id, name, mail
FROM Users
WHERE 
  -- REGEXP checks the structural rules of the email
  mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$'
  
  -- LIKE BINARY enforces strict case-sensitivity for the domain (rejects .COM)
  AND mail LIKE BINARY '%@leetcode.com';

```


Subqueries

585. Investments in 2016
```mysql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE (lat, lon) IN (
    -- Condition 1: Unique locations
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
)
AND tiv_2015 IN (
    -- Condition 2: Non-unique tiv_2015 values
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
);
```

185. Department Top three salaries
```mysql
select Department, Employee, Salary

from ( select d.name as Department, e.name as Employee, e.salary as Salary,

dense_rank() over(

    partition by d.id
    order by e.salary desc
) as rn

from Employee e
join Department d
on e.departmentId = d.id
) as ranked_salaries

where rn <= 3;
```

Advanced Joins and selects

610. Triangle Judgement
```mysql
select x, y, z, case when (x + y > z and y + z > x and z + x > y) then 'Yes' else 'No' end as triangle

from Triangle;
```

180. Consecutive Numbers
```mysql
SELECT DISTINCT num AS ConsecutiveNums

FROM (
    SELECT num,
           -- Subtracting row_number from id to create the grouping constant
           id - ROW_NUMBER() OVER(PARTITION BY num ORDER BY id) as grp
    FROM Logs
) AS xyz

GROUP BY num, grp
HAVING COUNT(*) >= 3;
```

another method:
```mysql
WITH NextLogs AS (
    SELECT num,
           LEAD(num, 1) OVER(ORDER BY id) AS next_1,
           LEAD(num, 2) OVER(ORDER BY id) AS next_2
    FROM Logs
)
SELECT DISTINCT num AS ConsecutiveNums
FROM NextLogs
WHERE num = next_1 AND num = next_2;
```
#### The `WITH` Clause (Common Table Expressions)

- **What it is:** A way to create a named temporary table (called a CTE) that exists only in memory for the exact duration of your query.
- **Why use it here:** It allows you to build a neat, pre-calculated table (e.g., `NextLogs`) where every row has its normal data _plus_ the data from the upcoming rows.
- **Key rule:** The main `SELECT` query must immediately follow the closing parenthesis of the `WITH` clause.
#### 2. The `LEAD()` Window Function

- **What it does:** Looks forward a specific number of rows and grabs a value from them.
- **Syntax:** `LEAD(column_name, offset_number) OVER (ORDER BY sort_column)`
- **How it works in this problem:** * `LEAD(num, 1)` pulls the `num` from exactly 1 row ahead.
    - `LEAD(num, 2)` pulls the `num` from exactly 2 rows ahead.
    - This places the current number and the next two numbers side-by-side in a single row.

1164. Product price at a given date
```mysql
-- GROUP 1: The latest price on or before the target date
SELECT product_id, new_price AS price
FROM Products
WHERE (product_id, change_date) IN (
    SELECT product_id, MAX(change_date)
    FROM Products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
)

UNION

-- GROUP 2: The default price for products with no changes by the target date
SELECT DISTINCT product_id, 10 AS price
FROM Products
WHERE product_id NOT IN (
    -- If a product is NOT in this subquery, it had no valid changes by the 16th
    SELECT product_id
    FROM Products
    WHERE change_date <= '2019-08-16'
);
```
- **`MAX(change_date)`:** In the first block, grouping by the product and taking the maximum date (filtered to before the 16th) perfectly isolates the most recent price.
- **`DISTINCT`:** In the second block, we must use `DISTINCT` because a product might have had multiple price changes _after_ the 16th, and we only want it to show up in our final table once.
- **`NOT IN`:** This is a very clean way to say "find me the products that don't have any rows before the 16th."

1204. Last person to fit in the bus
```mysql
WITH RunningTotal AS (
    SELECT person_name,
           weight,
           turn,
           -- This creates a running cumulative sum of the weight based on turn order
           SUM(weight) OVER (ORDER BY turn) AS total_weight
    FROM Queue
)
SELECT person_name
FROM RunningTotal
WHERE total_weight <= 1000
ORDER BY total_weight DESC
LIMIT 1;
```
- **`SUM(weight) OVER ()`** (No `ORDER BY`): The database looks at the entire table, adds everyone up (e.g., 1150 kg), and blindly pastes `1150` on every single row. The window is the whole table.

- **`SUM(weight) OVER (ORDER BY turn)`**: Adding `ORDER BY` tells the database to process the rows sequentially. It changes the "window" from the whole table to **"the beginning of the table up to the current row."** In strict database terminology, adding `ORDER BY` silently applies a hidden rule called: `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`.

1907. Count Salary Categories
```mysql
SELECT 'Low Salary' AS category, COUNT(*) AS accounts_count
FROM Accounts
WHERE income < 20000

UNION ALL

SELECT 'Average Salary' AS category, COUNT(*) AS accounts_count
FROM Accounts
WHERE income >= 20000 AND income <= 50000

UNION ALL

SELECT 'High Salary' AS category, COUNT(*) AS accounts_count
FROM Accounts
WHERE income > 50000;
```

**The difference between `UNION` and `UNION ALL`:**

- **`UNION`:** Stacks the tables together, but then scans the whole final table and **deletes any duplicate rows**. (This scanning process makes it slower).
- **`UNION ALL`:** Blindly stacks the tables together and keeps everything, including duplicates. Because it doesn't waste time checking for duplicates, **`UNION ALL` is faster** and is the preferred choice unless you specifically need to eliminate duplicate rows.




