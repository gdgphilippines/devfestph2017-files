# 4. Find a Dataset

We need a dataset to query with. We'll talk about loading data into BigQuery in another lab. In this section, we'll query the Wikimedia pageviews dataset that's part of many [Public Datasets](https://cloud.google.com/bigquery/public-data/) available in BigQuery today, including Wikimedia, Hacker News, GitHub, GDELT (News events), and many more.

For this lab, we'll use the Wikimedia public data set. To add the data set, visit this URL:

[https://bigquery.cloud.google.com/table/bigquery-samples:wikimedia_pageviews.201112](https://bigquery.cloud.google.com/table/bigquery-samples:wikimedia_pageviews.201112)

> This will open a BigQuery console with access to this dataset. Many of these datasets can be found in the BigQuery console on the left.

![BigQuery Console Image 1](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/27200dfe92db4aa7.png)

Click on `bigquery-samples:wikimedia_pageviews`, this is the dataset we will use.

![BigQuery Console Image 2](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/f74add152723c891.png)

Scroll down and find and click the table **201112**:

![BigQuery Console Image 3](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/5bd76e91985d035b.png)

You can see the table schema in the **Schema** view on the right:

![BigQuery Console Image 4](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/4747b8f41e0a06a7.png)

You can find out how much data is in the table, by navigating to the **Details** view on the right:

![BigQuery Console Image 5](https://codelabs.developers.google.com/codelabs/cloud-bigquery-wikipedia/img/13869def9f58bc6e.png)

Alright! Let's write a query to see what's the most popular Wikipedia page in December, 2011 using BigQuery.