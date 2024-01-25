#1. Constraints

In SQL, constraints are used to enforce rules on the data in your tables, ensuring data integrity. Common constraints include:

####Primary Key:

`
CREATE TABLE table_name (
  column1 INT PRIMARY KEY,
  column2 VARCHAR(255)
);`
Specifies a column or set of columns as a unique identifier for a record in a table. Each table can have only one primary key.

###Foreign Key:

```


CREATE TABLE orders (
  order_id SERIAL PRIMARY KEY,
  customer_id INT REFERENCES customers(customer_id),
  order_date DATE
);
```

Establishes a link between two tables by referencing the primary key of one table as a foreign key in another. It enforces referential integrity.

####Unique Constraint:

```
CREATE TABLE employees (
  employee_id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE,
  -- Other columns
);
```

Ensures that values in a specified column or group of columns are unique across the table, preventing duplicate values.

#2. Joins
SQL is powerful for its ability to join tables. Common join types include INNER JOIN, LEFT JOIN, and RIGHT JOIN.

####INNER JOIN:

```
SELECT \*
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;
```

Retrieves rows where there is a match in both tables based on the specified join condition. It excludes rows without a match.

####LEFT JOIN:

```
SELECT \*
FROM employees
LEFT JOIN departments ON employees.department_id = departments.department_id;
```

Retrieves all rows from the left table and the matched rows from the right table. If there is no match, NULL values are returned for columns from the right table.

#3. Aggregate Functions
SQL provides various aggregate functions for data analysis.

####SUM:

`SELECT SUM(salary) AS total_salary
FROM employees;`

Calculates the sum of values in a column, often used for numeric data.

####COUNT:

`SELECT COUNT(_) AS employee_count
FROM employees;`

Counts the number of rows in a result set. COUNT(\_) counts all rows, while COUNT(column) counts non-null values in the specified column.

#4. Subqueries
You can use subqueries to nest one query inside another.

```
SELECT \*
FROM employees
WHERE department_id IN (SELECT department_id FROM departments WHERE location = 'New York');
```

This retrieves data based on the result of the inner query.

#5. Views
Views allow you to create virtual tables based on the result of a SELECT query.

```
CREATE VIEW high_salary_employees AS
SELECT \*
FROM employees
WHERE salary > 50000;
```

Simplifies complex queries and encapsulates logic, creating a virtual table.

#6. Stored Procedures
Stored procedures are sets of SQL statements stored in the database.

```
CREATE OR REPLACE FUNCTION get_employee_count()
RETURNS INT AS $$
DECLARE
count INT;
BEGIN
SELECT COUNT(\*) INTO count FROM employees;
RETURN count;
END;
$$ LANGUAGE plpg;
```

Executes a series of SQL statements as a single unit, often used for automation and modularity.

#7. Triggers

Triggers are SQL procedures that are automatically executed in response to certain events.

```
CREATE TRIGGER update_last_modified
BEFORE UPDATE ON employees
FOR EACH ROW
SET NEW.last_modified = NOW();
```

Automatically executed before an UPDATE on the "employees" table, updating the "last_modified" column for each affected row.

#8. Variables
Variables are used to store data values.

###Declare a variable

DECLARE x INT; -- INT is an integer data type
DECLARE is used to declare a variable, and the data type follows the variable name.

###Assign a value to a variable

`SET x = 5;`
SET is used to assign a value to a variable.

###Single Line

`DECLARE y INT := 10;`
A variable can be declared and assigned a value in a single line.

#9. Comparison Statements
Comparison statements are used to compare values and make decisions.

####Equality

`IF x = y THEN`
-- Code to execute if x is equal to y
`END IF;`

`IF x = y THEN`
-- Code to execute if x is equal to y
`END IF;`

####Inequality

`IF x <> y THEN`
-- Code to execute if x is not equal to y
`END IF;`

`IF x IS DISTINCT FROM y THEN`
-- Code to execute if x is not equal to y
`END IF;`
####Less than, less than or equal to, greater than, greater than or equal to

`IF x < y THEN`
-- Code to execute if x is less than y
`END IF;`

`IF x <= y THEN`
-- Code to execute if x is less than or equal to y
`END IF;`

`IF x > y THEN`
-- Code to execute if x is greater than y
`END IF;`

`IF x >= y THEN`
-- Code to execute if x is greater than or equal to y
`END IF; `

#10. Boolean Statements/Logical Operators
Logical operators are used to determine the logic between variables or values.

###AND

`IF x < 10 AND y > 1 THEN`
-- Code to execute if both conditions are true
`END IF;`

###ELSE

`ELSE`
-- Code to execute if the conditions are not met
`END IF;`

###OR

`IF x = 5 OR y = 5 THEN`
-- Code to execute if at least one condition is true
`END IF;`

###NOT

`IF NOT (x = y) THEN`
-- Code to execute if the condition is false
`END IF; `

#11. Arrays
Arrays are not a native concept in SQL, but you can use tables to achieve a similar effect.

#####`DECLARE animals TABLE (name VARCHAR(255));`

#####`INSERT INTO animals VALUES ('cat'), ('dog'), ('bird');`
Target an element in the "array" by index

#####`DECLARE firstanimal VARCHAR(255);`

#####`SELECT name INTO firstanimal FROM animals WHERE rownum = 1;`
Modify/reassign an element in the "array"

#####`UPDATE animals SET name = 'moose' WHERE rownum = 2; `
#12. Objects
Objects are not a native concept in SQL, but you can use tables or user-defined types to achieve a similar effect.

```
DECLARE user_table TABLE (
firstName VARCHAR(255),
lastName VARCHAR(255),
age INT
);
```

`INSERT INTO user_table VALUES ('Sequoyah', 'Geber', 18);`
####Target a property in the "object"

`DECLARE firstName VARCHAR(255);`

`SELECT firstName INTO firstName FROM user_table WHERE rownum = 1;`
####Modify object properties

`UPDATE user_table SET age = 20 WHERE rownum = 1;`
####Set object property name as a variable

`DECLARE property_name VARCHAR(255) := 'firstName';`

`DECLARE user_table TABLE (
[property_name] VARCHAR(255)
); `

#13. Functions
Functions in SQL are slightly different than in JavaScript, often used to encapsulate logic or calculations.

```
CREATE FUNCTION printValue() RETURNS INT AS
$$

DECLARE
x INT := 10;
BEGIN
RETURN x;
END;

$$
LANGUAGE plpg;

$$ LANGUAGE plpg;
```

####Call the function

`SELECT printValue();`

####Nested function and function parameters

CREATE FUNCTION add() RETURNS INT AS

```
$$ DECLARE
x INT := 5;
BEGIN
x := x + 1;
RETURN x;
END;
$$ LANGUAGE plpg;
CREATE FUNCTION printValue() RETURNS INT AS
$$

DECLARE
x INT := 5;
BEGIN
RETURN add();
END;

$$
LANGUAGE plpg;
```

####Functions and Parameters

```
CREATE FUNCTION greet(name VARCHAR(255)) RETURNS VARCHAR(255) AS
$$

BEGIN
RETURN 'Hello, ' || name || '!';
END;

$$
LANGUAGE plpg;
SELECT greet('Alice');
```

#14. Loops

####"FOR" Loop

```
FOR i IN 1..5 LOOP
-- Code to repeat 5 times
END LOOP;
```

####"WHILE" Loop

```
DECLARE count INT := 0;
WHILE count < 5 LOOP
-- Code to repeat as long as the condition is true
count := count + 1;
END LOOP;
```

#15. Data Types

The various data types in SQL include NULL, numbers (INT, FLOAT), strings (VARCHAR), booleans (BOOLEAN), arrays (ARRAY), and objects (JSON).

`DECLARE x INT; -- INT is an integer data type`
`DECLARE x VARCHAR(255); -- VARCHAR is a variable character string data type`
`DECLARE x BOOLEAN; -- BOOLEAN can be true or false`
`DECLARE x ARRAY[INT]; -- ARRAY is an array data type`
`DECLARE x JSON; -- JSON is an object data type`

#16. Comments
Comments in SQL are similar to JavaScript.

####Single-line comment:

`-- This is a single-line comment in SQL`

####Multi-line comment:

```
/_
This is a
multi-line
comment
_/
```

$$
$$
