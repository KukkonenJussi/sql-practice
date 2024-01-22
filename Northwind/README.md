# Northwind

This repository is for the Northwind database. Below is the schema of the database, the questions according to difficulty levels (easy, intermediate, hard) and my own answer to each question.

## Database schema

![schema](src/schema.png)

## Questions

### <u>Easy:</u>

1. Show the category_name and description from the categories table sorted by category_name.

```
SELECT
  category_name,
  description
FROM categories
ORDER BY category_name;
```

2. Show all the contact_name, address, city of all customers which are not from 'Germany', 'Mexico', 'Spain'

```
SELECT
  contact_name,
  address,
  city
FROM customers
WHERE Country NOT IN ('Germany','Mexico', 'Spain')
```

3. Show order_date, shipped_date, customer_id, Freight of all orders placed on 2018 Feb 26

```
SELECT
	order_date,
    shipped_date,
    customer_id,
    freight
FROM orders
WHERE order_date = '2018-02-26';
```

4. Show the employee_id, order_id, customer_id, required_date, shipped_date from all orders shipped later than the required date

```
SELECT
	employee_id,
    order_id,
    customer_id,
    required_date,
    shipped_date
FROM orders
WHERE shipped_date > required_date;
```

5. Show all the even numbered Order_id from the orders table

```
SELECT order_id
FROM orders
WHERE mod(order_id,2) = 0;
```

6. Show the city, company_name, contact_name of all customers from cities which contains the letter 'L' in the city name, sorted by contact_name

```
SELECT
  city,
  company_name,
  contact_name
FROM customers
WHERE city LIKE '%L%'
ORDER BY contact_name;
```

7. Show the company_name, contact_name, fax number of all customers that has a fax number. (not null)

```
SELECT
  company_name,
  contact_name,
  fax
FROM customers
WHERE fax NOT NULL;
```

8. Show the first_name, last_name. hire_date of the most recently hired employee.

```
SELECT
  first_name,
  last_name,
  MAX(hire_date) as hire_date
FROM employees;
```

9. Show the average unit price rounded to 2 decimal places, the total units in stock, total discontinued products from the products table.

```
SELECT
  ROUND(AVG(unit_price), 2) AS average_unit_price,
  SUM(units_in_stock) AS total_units_in_stock,
  SUM(discontinued) AS discontinued_products
FROM products
```

---

### <u>Medium:</u>

1. Show the ProductName, CompanyName, CategoryName from the products, suppliers, and categories table
