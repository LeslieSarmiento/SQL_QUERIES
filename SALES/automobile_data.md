# SQL Data Cleaning Project – Automobile Sales Dataset

## Project Overview
This project focuses on cleaning and validating automobile sales data using SQL in Google BigQuery. The dataset contains historical information about cars, including their specifications, fuel types, engine details, and prices.

The goal of this project is to prepare the dataset for reliable analysis by identifying and correcting inconsistencies, missing values, and data entry errors. Clean data is essential before performing any analysis, especially when the results will guide business decisions.

The main objective of this project is to demonstrate common SQL data cleaning techniques directly within a database environment. This includes validating column values, identifying inconsistencies, correcting errors, and ensuring the dataset is ready for analysis.

## Data Cleaning Query

The following SQL script demonstrates the queries used to inspect and clean the automobile dataset stored in Google BigQuery.

```sql
SELECT DISTINCT fuel_type
FROM cars.car_info;

SELECT
  MIN(length) AS min_length,
  MAX(length) AS max_length
FROM cars.car_info;

SELECT *
FROM cars.car_info
WHERE num_of_doors IS NULL;

UPDATE cars.car_info
SET num_of_doors = "four"
WHERE make = "dodge"
  AND fuel_type = "gas"
  AND body_style = "sedan";

SELECT DISTINCT num_of_cylinders
FROM cars.car_info;

UPDATE cars.car_info
SET num_of_cylinders = "two"
WHERE num_of_cylinders = "tow";

SELECT
  MIN(compression_ratio) AS min_compression_ratio,
  MAX(compression_ratio) AS max_compression_ratio
FROM cars.car_info;

SELECT COUNT(*) AS num_of_rows_to_delete
FROM cars.car_info
WHERE compression_ratio = 70;

DELETE FROM cars.car_info
WHERE compression_ratio = 70;

SELECT DISTINCT drive_wheels
FROM cars.car_info;

SELECT
  DISTINCT drive_wheels,
  LENGTH(drive_wheels) AS string_length
FROM cars.car_info;

UPDATE cars.car_info
SET drive_wheels = TRIM(drive_wheels)
WHERE TRUE;
```


## Business Scenario
In this scenario, a data analyst is working with a used car dealership startup. The company's investors want to understand which cars are most popular with customers so they can stock their inventory accordingly.

Before analyzing popularity or trends, the dataset must first be cleaned to ensure the results are accurate and trustworthy. Incorrect or inconsistent data could lead to poor purchasing decisions and financial losses.

## Dataset
The dataset used in this project is a CSV file containing historical automobile sales data.

It includes information such as:
- Car manufacturer
- Fuel type
- Body style
- Engine specifications
- Number of doors and cylinders
- Vehicle dimensions
- Price

The data is imported into **Google BigQuery** and stored in a custom dataset and table. From https://drive.google.com/u/0/uc?id=1cJtuw-6mxZk7BNkcsLYEvfjW0l_PdKxA&export=download

Dataset structure used in this project:

```
Dataset: cars
Table: car_info
```

## Data Cleaning Tasks
Several SQL techniques are used throughout this project to inspect and clean the data, including:

- Inspecting categorical columns using `SELECT DISTINCT`
- Validating numeric ranges using `MIN()` and `MAX()`
- Identifying missing values using `IS NULL`
- Updating incorrect or missing values using `UPDATE`
- Detecting spelling errors or inconsistent values
- Removing incorrect records using `DELETE`
- Fixing formatting inconsistencies using functions like `TRIM()`
- Verifying string lengths using `LENGTH()`

These steps ensure that the dataset is consistent, accurate, and suitable for further analysis.

## SQL Skills Demonstrated
This project demonstrates several practical SQL skills commonly used by data analysts:

- Data inspection
- Data validation
- Handling NULL values
- Data correction using UPDATE statements
- Removing incorrect records
- String cleaning and formatting
- Data quality verification


## Tools Used
- SQL
- Google BigQuery
- CSV data import

