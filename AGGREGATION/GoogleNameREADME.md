# Wikipedia "Google" Page View Analysis (BigQuery)

This repository demonstrates a simple SQL query that analyzes Wikipedia page view data using Google BigQuery. The query searches for Wikipedia articles whose titles contain the word **"Google"**, calculates the total number of views for each article, and sorts them by popularity.

The purpose of this example is to show how filtering, grouping, and aggregation work when analyzing large datasets with SQL.

---

## SQL Query

```sql
SELECT
  language,
  title,
  SUM(views) AS views
FROM
  `bigquery-samples.wikipedia_benchmark.Wiki10B`
WHERE
  title LIKE '%Google%'
GROUP BY
  language,
  title
ORDER BY
  views DESC;
```

---

## Dataset Source

The data comes from a **public dataset available in Google BigQuery**.

Dataset path:

```
bigquery-samples.wikipedia_benchmark.Wiki10B
```

BigQuery uses the following structure to reference tables:

```
project.dataset.table
```

For this query:

| Component | Value                 | Description                                  |
| --------- | --------------------- | -------------------------------------------- |
| Project   | `bigquery-samples`    | Public sample datasets provided by Google    |
| Dataset   | `wikipedia_benchmark` | Collection of Wikipedia benchmark datasets   |
| Table     | `Wiki10B`             | Table containing Wikipedia page view records |

The table includes information such as:

* The language of the Wikipedia page
* The page title
* The number of views for that page

---

## What the Query Does

### 1. Selects Columns

The query retrieves three pieces of information:

* **language** – the language edition of Wikipedia (for example `en`, `es`, `fr`)
* **title** – the title of the Wikipedia page
* **SUM(views)** – the total number of views for that page

The `SUM(views)` result is renamed as **views** using `AS views`.

---

### 2. Filters Pages Containing "Google"

```
WHERE title LIKE '%Google%'
```

This condition returns only rows where the page title contains the word **Google**.

The `%` symbol is a wildcard that means **any characters before or after the word**.

Examples of matching titles:

* Google
* Google Search
* History of Google
* Google Maps

---

### 3. Groups the Data

```
GROUP BY language, title
```

Because multiple rows may exist for the same page, the query groups rows together by:

* language
* title

This allows the view counts to be combined into one total per page.

Example before grouping:

| language | title  | views |
| -------- | ------ | ----- |
| en       | Google | 500   |
| en       | Google | 300   |

After grouping and summing:

| language | title  | views |
| -------- | ------ | ----- |
| en       | Google | 800   |

---

### 4. Aggregates Views

```
SUM(views)
```

The `SUM()` function adds all view counts together for each grouped page and language combination.

---

### 5. Sorts the Results

```
ORDER BY views DESC
```

This sorts the output from **highest views to lowest**, so the most viewed Google-related pages appear first.

---

## Example Output

| language | title         | views      |
| -------- | ------------- | ---------- |
| en       | Google        | 12,000,000 |
| en       | Google Search | 8,500,000  |
| es       | Google        | 6,200,000  |
| fr       | Google        | 5,900,000  |

This result shows which Google-related Wikipedia pages receive the most traffic across different languages.

---

## Requirements

To run this query you need:

* A Google Cloud account
* Access to **BigQuery**
* Permission to query public datasets

---

## How to Run the Query

1. Open the BigQuery Console.
2. Create a new SQL query.
3. Paste the SQL query from this repository.
4. Click **Run**.

BigQuery will process the dataset and return the results.

---

## SQL Concepts Demonstrated

This example uses several core SQL concepts:

* `SELECT`
* `WHERE` filtering
* `LIKE` pattern matching
* `GROUP BY`
* Aggregation with `SUM()`
* `ORDER BY`

These are fundamental operations when analyzing data with SQL.

---

## Possible Improvements

You could extend this query in several ways:

* Limit results to the **Top 10 pages**
* Filter results by **specific language**
* Compare **Google-related pages across languages**
* Build a visualization or dashboard from the results

Example improvement to show only the top 10 pages:

```sql
LIMIT 10;
```

---

## Summary

This project demonstrates how SQL can be used in BigQuery to analyze large datasets. By filtering titles containing the word **Google**, grouping them by language and title, and summing the views, the query identifies the most viewed Google-related articles on Wikipedia.

