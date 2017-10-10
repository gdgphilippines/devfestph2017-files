# 6. More advanced queries

### Find Wikipedia page views

The Wikimedia dataset contains page views for all of the [Wikimedia projects](https://www.wikimedia.org/) (including Wikipedia, Wikitionary, Wikibooks, Wikiquotes, etc). Let's narrow down the query to just Wikipedia pages by adding a `where` statement:

```sql
SELECT SUM(views), wikimedia_project
FROM `bigquery-samples.wikimedia_pageviews.201112`
WHERE wikimedia_project = "wp"
GROUP BY wikimedia_project
```

![BigQuery Image 1](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/1a3f5e05c108f99e.png)

Notice that, by querying an additional column, wikimedia_project, the amount of data processed increased from 12.2 GB to 18 GB.

BigQuery supports many of the familiar SQL clauses, such as `contains`, `group by`, `order by`, and a number of aggregation functions. In addition, you can also use regular expressions to query text fields! Let's try one:

```sql
SELECT title, SUM(views) views
FROM `bigquery-samples.wikimedia_pageviews.201112`
WHERE
  wikimedia_project = "wp"
  AND REGEXP_CONTAINS(title, 'Red.*t')
GROUP BY title
ORDER BY views DESC
```

> Learn more about BigQuery syntax in the [BigQuery Query Reference documentation](https://cloud.google.com/bigquery/docs/reference/standard-sql/).

### Query across multiple tables

You can select a range of tables to form the union using a [wildcard table](https://cloud.google.com/bigquery/docs/wildcard-tables). Let's query over the entire year of 2011 by querying tables with "2011" as a prefix:

This query will query over a total dataset size of 1 TB, but process only 672 GB of data.

```sql
SELECT title, SUM(views) views
FROM `bigquery-samples.wikimedia_pageviews.2011*`
WHERE
  wikimedia_project = "wp"
  AND REGEXP_CONTAINS(title, 'Red.*t')
GROUP BY title
ORDER BY views DESC
```

That wasn't too hard to query that much data!

You can [filter the tables more selectively with the _TABLE_SUFFIX pseudo column](https://cloud.google.com/bigquery/docs/querying-wildcard-tables#filtering_selected_tables_using_table_suffix). This query will limit to tables corresponding with the last two months of the year.

```sql
SELECT title, SUM(views) views
FROM `bigquery-samples.wikimedia_pageviews.2011*`
WHERE
  (_TABLE_SUFFIX = '11' OR _TABLE_SUFFIX = '12')
  AND wikimedia_project = "wp"
  AND REGEXP_CONTAINS(title, 'Red.*t')
GROUP BY title
ORDER BY views DESC
```

Because the query limited the tables scanned with the _TABLE_SUFFIX pseudo column, it only processed 113 GB of data.