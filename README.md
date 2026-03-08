# SQL Query Collection

A structured collection of SQL queries organized by category.  
This repository is designed as a learning and reference resource where each query is stored in its own file and includes a clear explanation of how it works.

## Repository Purpose

This repository serves two main purposes:

1. **Learning Resource**  
   Each SQL query includes a detailed explanation so readers can understand what the query does, how it works, and why certain SQL functions or clauses are used.

2. **Organized Query Library**  
   Queries are grouped into folders based on their purpose or topic, making it easy to browse and locate examples.

## Repository Structure

The repository is organized into category folders. Each folder contains SQL files that focus on a specific topic.

Example structure:

```
sql-queries/
│
├── aggregation/
│   ├── sum_views.sql
│   ├── average_sales.sql
│
├── filtering/
│   ├── where_examples.sql
│   ├── like_operator.sql
│
├── joins/
│   ├── inner_join_example.sql
│   ├── left_join_example.sql
│
└── datasets/
    ├── wikipedia_queries.sql
```

## File Format

Each SQL file contains:

1. **The SQL Query**
2. **Explanation of Each Clause**
3. **Description of the Dataset or Table**
4. **Expected Output**
5. **Optional Notes or Variations**

Example file format:

```sql
-- Query Description
-- This query calculates the total number of views for Wikipedia pages
-- that contain the word "Google" in the title.

SELECT
  language,
  title,
  SUM(views) AS views
FROM
  `bigquery-samples.wikipedia_benchmark.Wiki10B`
WHERE
  title LIKE '%Google%'
GROUP BY
  language, title
```

Explanation sections typically describe:

- What the **SELECT** clause retrieves
- Which **dataset and table** are used
- How **filtering conditions** work
- How **aggregation functions** operate
- What the **final result** represents

## Categories

Queries in this repository may include topics such as:

- Aggregation
- Filtering
- Joins
- Grouping
- Window functions
- Data exploration
- Dataset-specific examples

Each category helps group similar query patterns together.


## How to Use This Repository

1. Browse the folders by topic.
2. Open any `.sql` file.
3. Read the query and the explanation included in the file.
4. Run the query in your SQL environment if the dataset is available.


## License

This repository is open for educational and reference use.

