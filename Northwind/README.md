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
