![Oracle Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Oracle_logo.svg/1200px-Oracle_logo.svg.png)


# Cheat Sheet
A concise set of notes about Oracle Database used for quick reference

## About Oracle Database
Oracle Database is a Relational Database Management System with the largest market share. Oracle Database allows you to quickly and safely store and retrieve data.
Some features to highlight are:
* **Cross-platform** – It can run on various hardware across operating systems including Windows Server, Unix, and various distributions of GNU/Linux.
* **ACID-compliant** – Oracle is ACID-compliant Database that helps maintain data integrity and reliability.
* **Logical data structure** – Oracle uses the logical data structure to store data so that you can interact with the database without knowing where the data is stored physically.
* **Partitioning** – is a high-performance feature that allows you to divide a large table into different pieces and store each piece across storage devices.
* **Memory caching** – the memory caching architecture allows you to scale up a very large database that still can perform at a high speed.
* **Backup and recovery** – ensure the integrity of the data in case of system failure. Oracle includes a powerful tool called Recovery Manager (RMAN) – allows DBA to perform cold, hot, and incremental database backups and point-in-time recoveries.
* **Clustering** – Oracle Real Application Clusters (RAC) – Oracle enables high availability that enables the system is up and running without interruption of services in case one or more server in a cluster fails.

## Main data types
An overview of the built-in Oracle data types:
<h4>CHAR(length)</h4>
<p>- stores fixed-length string</p>
<p>- the <b>length</b> parameter specifies the length of the string, if the length is shorter, the rest will be filled with spaces</p>

<h4>VARCHAR2(length)</h4>
<p>- stores variable length string</p>
<p>- the <b>length</b> parameter specifies the maximum length of the string</p>

<h4>DATE</h4>
<p>- stores date and time</p>

<h4>INTEGER</h4>
<p>- stores integer values</p>

<h4>NUMBER(x,y)</h4>
<p>- stores float or integer numbers</p>
<p>- <b>x</b>: maximum number of digits in all number</p>
<p>- <b>y</b>: maximum number of digits to the right of the decimal point</p>

<h4>BINARY_FLOAT</h4>
<p>- stores a 32 bits float number</p>

<h4>BINARY_DOUBLE</h4>
<p>- stores a 64 bits double number</p>

<p></p>

> The complete list of data types can be checked [in here](https://docs.oracle.com/cd/E11882_01/appdev.112/e10646/oci03typ.htm#LNOCI030)

<p></p>


# SQL Commands

![SQL commands](https://i.stack.imgur.com/7uUaJ.png)


# DDL - Data Definition Language
<p>For instance here is going to be used a small market database</p>
<h4>CREATE TABLE</h4>
<p>- To create database and its objects like (table, index, views, store procedure, function and triggers)</p>

#### Primary Key

The PRIMARY KEY constraint uniquely identifies each record in a table.

#### Costumers table

```sql
CREATE TABLE tb_customers(
  id_customer INTEGER,
  first_name  VARCHAR2(30) NOT NULL,
  last_name   VARCHAR2(30) NOT NULL,
  email       VARCHAR2(40) NOT NULL,
  phone       VARCHAR2(12) NOT NULL,
  is_active   INTEGER,
  CONSTRAINT pk_customer PRIMARY KEY(id_customer)
);
```
#### Products categories table

```sql
CREATE TABLE tb_prod_categories(
  id_category    INTEGER,
  category_name  VARCHAR2(20) NOT NULL,
  is_active      INTEGER,
  CONSTRAINT pk_category PRIMARY KEY(id_category)
);
```

#### FOREIGN KEY

The FOREIGN KEY constraint is a key used to link two tables together.

#### Products table

```sql
CREATE TABLE tb_products(
  id_product        INTEGER,
  id_prod_category  INTEGER,
  product_name      VARCHAR2(30) NOT NULL,
  product_descr     VARCHAR2(140),
  price             NUMBER(5,2) NOT NULL,
  is_active         INTEGER,
  CONSTRAINT pk_product PRIMARY KEY(id_product),
  CONSTRAINT fk_prod_category FOREIGN KEY(id_prod_category) REFERENCES tb_prod_categories(id_category)
);
```

#### Sales table

```sql
CREATE TABLE tb_sales(
  id_product    INTEGER,
  id_customer   INTEGER,
  quantity      INTEGER,
  is_active     INTEGER,
  CONSTRAINT fk_product FOREIGN KEY(id_product) REFERENCES tb_products(id_product),
  CONSTRAINT fk_customer FOREIGN KEY(id_customer) REFERENCES tb_customer(id_customer),
  CONSTRAINT pk_sales PRIMARY KEY (id_product, id_customer)
);
```

<h4>ALTER TABLE</h4>
<p>- Used to add, modify, or drop/delete columns in a table</p>

```sql
ALTER TABLE table_name
  ADD column_name column_definition;
```

<h4>DROP TABLE</h4>
<p>- Allows you to remove or delete a table from the Oracle database</p>

```sql
DROP TABLE table_name;
```

<h4>TRUNCATE TABLE</h4>
<p>- Remove all records from a table, including all spaces allocated for the records are removed</p>

```sql
TRUNCATE TABLE table_name;
```


# DML - Data Manipulation Language

<h4>SELECT</h4>
<p>- Retrieve data from the a database</p>

```sql
/*Returns all registers on the table*/
SELECT *
FROM table_name;
```

```sql
/*Returns the registers based in a condition*/
SELECT *
FROM table_name
WHERE condition;
```

<h4>DESCRIBE</h4>
<p>- Command to describe the structure of a table</p>

```sql
DESCRIBE table_name;
```

<h4>INSERT</h4>
<p>- Add data into a table</p>
<p>- The order in <b>VALUES</b> corresponds to the order in which the table columns are</p>

```sql
INSERT INTO table_name (column1, column2, column3, ... columnN)
VALUES (value1, value2, value3, ... valueN);
```

<h4>UPDATE</h4>
<p>- Update existing records in a table in an Oracle database</p>
<p>- In the statement, the table containing the rows to be changed must be specified</p>
<p>- A <b>SET</b> clause that contains the columns and the new values</p>
<p>- A <b>WHERE</b> clause that specifies the rows to be changed</p>

```sql
UPDATE table_name
  SET column1 = new_value1, column2 = new_value2,
WHERE condition;
```

⚠️ **DO NOT FORGET THE "WHERE" CLAUSE**

<p>You can also use it to clear all values in a column:</p>

```sql
UPDATE table_name
  SET column4 = NULL;
```

<h4>DELETE</h4>
<p>- Remove rows in a table</p>
<p>- You can use <b>WHERE</b> clause to specifies the rows to be removed, if you don't use it, all the rows will be removed</p>

```sql
DELETE table_name
WHERE condition;
```

# TCL - Transaction Control Language 

<h4>COMMIT</h4>
<p>- Makes the operations permanent</p>

<h4>ROLLBACK</h4>
<p>- Undo operations</p>


# PL/SQL Procedural Language for SQL
<p>- Allows to use programming in SQL instructions</p>
<p>- Widely used in creating procedures and functions</p>

Some features to highlight are:
* Variables
* Conditional logic (**if-then-else**)
* Loops
* Procedures
* Functions


<p>Below is an example of a procedure to search for a customer:</p>

```sql
CREATE OR REPLACE PROCEDURE get_customer(p_id_customer IN tb_customers.id_customer%TYPE)
AS
v_first_name      tb.customers.first_name%TYPE;
v_last_name       tb.customers.last_name%TYPE;
v_control         INTEGER;

BEGIN
  SELECT COUNT(*) INTO v_control
  FROM tb_customers
  WHERE id_customer = p_id_customer;
  
  IF(v_control == 1) THEN
    SELECT first_name, last_name INTO v_first_name, v_last_name
    FROM tb_customers
    WHERE id_customer = p_id_customer;
    
    DBMS_OUTPUT.PUT_LINE('Customer found: ' || v_first_name || ' ' || v_last_name);
  
  ELSE
    DBMS_OUTPUT.PUT_LINE('Customer not found');
  END IF;
  
  EXCEPTION
    WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('Error');

END get_customer;
```

<p>Calling the procedure:</p>

```sql
SET SERVEROUTPUT ON /*Shows the DBMS outputs on console*/

/*You can call the procedure by:*/
CALL get_customer(7);

/*Or in an anonymous PL/SQL block*/
BEGIN
  get_customer(7);
END;
```

# SQL OPERATORS

 <table style="width:100%">
  <tr>
    <td style="text-align:center"><h5>Operator</h5></td>
    <td style="text-align:center"><h5>Description</h5></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">LIKE</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Matches a string pattern</p></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">IN</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Matches a list of values</p></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">BETWEEN</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Matches a range of values</p></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">IS NULL</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Matches null values</p></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">IS NAN</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Matches to a special value (Not a Number)</p></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">IS INFINITE</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Matches to infinite numbers</p></td>
  </tr>
  <tr>
    <td style="text-align:center"><h5>Logical Operator</h5></td>
    <td style="text-align:center"><h5>Description</h5></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">x AND y</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Returns TRUE when x and y are true</p></td>
  </tr>
  <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">x OR y</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Returns TRUE when x or y are true</p></td>
  </tr>
   <tr>
    <td style="text-align:center"><b><p style="color:cornflowerblue; font-size:20px; text-align:center">NOT x</p></b></td>
    <td style="text-align:center"><p style="color:cornflowerblue; font-size:20px; text-align:center">Returns TRUE if x is false and returns false if x is true</p></td>
  </tr>
</table> 

<p>Searching for customers with the initial letter of the name 'J':</p>

```sql
SELECT *
FROM tb_customers
WHERE first_name LIKE 'J%';
```

<p>Searching for customers with the second letter of the name 'o':</p>

```sql
/* the _ corresponds to any character in that position and % to all characters after it*/
SELECT *
FROM tb_customers
WHERE first_name LIKE '_o%';
```

<p>Searching for customers with with the ids corresponding to those shown:</p>

```sql
/* the _ corresponds to any character in that position and % to all characters after it*/
SELECT *
FROM tb_customers
WHERE id_customer IN (104, 120, 117);
```

<p>Filtering products with values between 50 and 100 dollars:</p>

```sql
/* the _ corresponds to any character in that position and % to all characters after it*/
SELECT *
FROM tb_products
WHERE price BETWEEN 50 AND 100;
```

<h4>ORDER BY</h4>

<p>Sort rows in a query</p>

```sql
/* SORTING THE PRODUCTS BY PRICE*/
SELECT *
FROM tb_products
ORDER BY 5; /*You can use Column index or Column name*/
```

# FUNCTIONS
## Some of the most used functions:

<h4>LOWER(column_name)</h4>
<p>- Converts all registers in the column to lower case</p>
<h4>UPPER(column_name)</h4>
<p>- Converts all registers in the column to upper case</p>

```sql
SELECT UPPER(first_name), LOWER(last_name)
FROM tb_customers;
```

<h4>CAST(x AS type)</h4>
<p>- Converts <b>x</b> to the <b>type</b></p>

```sql
SELECT
  CAST(price AS VARCHAR2(10)),
  CAST(price + 5 AS NUMBER(7,2))
FROM tb_products
WHERE id_product = 117;
```

<h4>AVG(x)</h4>
<p>- Returns the average value of <b>x</b></p>

```sql
SELECT AVG(price)
FROM tb_products;
```

<h4>COUNT(x)</h4>
<p>- Returns count of <b>x</b></p>

```sql
SELECT COUNT(ROWID)
FROM tb_sales;
```

<h4>MAX(x)   MIN(x)</h4>
<p>- Used to get the max or min values of <b>x</b></p>

```sql
SELECT product_name, price
FROM tb_products
WHERE price = (SELECT MAX(price) FROM tb_products);
```

<h4>SUM(x)</h4>
<p>- Sums up all values in column X and returns the total</p>

```sql
SELECT SUM(price)
FROM tb_products;
```


---

### About

* Feel free to contribute.
* If you liked it, give it a ⭐️ **star** in the [repository](https://github.com/josehenriqueroveda/oracle-cs.github.io).

Find me on [LinkedIn](https://www.linkedin.com/in/jhroveda/)