# 5. Compose a query

Click **Compose Query** on the top left:

![Compose BigQuery Image 1](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/c5a5a91714ab397.png)

This will bring up the **New Query** view:

![Compose BigQuery Image 2](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/d4db0e758c045881.png)

Let's find out the total number of Wikimedia views in December, 2011, by writing this query:

```sql
SELECT SUM(views)
FROM `bigquery-samples.wikimedia_pageviews.201112`
```

Before we run the query, for the purpose of this lab, let's disable data caching so that we are not using any cached results. Click **Show Options**:

![Show Options Image 1](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/6722f3c1025f1b24.png)

Then, uncheck **Use Cached Results**:

![Show Options Image 2](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/793f2e5b36c6b2aa.png)

Also, uncheck **Use legacy SQL**:

![Show Options Image 3](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/78af860603245d56.png)

Click **Run Query**:

![Compose BigQuery Image 3](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/bec8eca026029b10.png)

In a few seconds, the result will be listed in the bottom, and it'll also tell you how much data was proccessed:

![Compose BigQuery Image 4](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/42ed9fe46ca28710.png)

In seconds, we queried over a 105 GB table, but we only needed to process 12.2GB of data to get to the result! This is because BigQuery is a columnar database. Because we only queried a single column, the total amount of data processed is significantly less than the total table size.

> The total amount of data in the table is billed as monthly storage price. The amount of data processed is what will be billed separately. You can process/query up to 1 TB of data for free each month. See the [BigQuery pricing page](https://cloud.google.com/bigquery/pricing) for more details.