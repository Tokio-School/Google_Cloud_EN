# **BigQuery**

Unit M2U3 - Exercise 8

**What are we going to do?**

1. Manage datasets and tables.
2. Consult public data.
3. Manage data with the bq command-line tool.
4. Query data from the client libraries.
5. Working with partitioned tables.
6. View data with Data Studio.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U3-8-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Search public datasets**

In this task we will start by launching SQL searches in the public datasets available in BigQuery.

First enable the BigQuery API.

Explore available public datasets:

1. On the console, navigate to  **BigQuery**.
2. Explore the options available in the BigQuery console.
3. In the  **Explorer** , locate the public bigquery-public-data and bigquery-samples projects.
  1. If you can't find them, tap the 3 vertical dots in the  **Explorer** drop-down menu, click  **+ Add data \&gt; Set a project \&gt; Enter project name**.
4. Analyse the datasets available in both projects and some of their tables. By clicking on the table, you will be able to see information about it.

Run 2 searches on public datasets:

1. Most used commit messages in GitHub:

SELECT subject AS subject,

COUNT(\*) AS num_duplicates

FROM `bigquery-public-data.github_repos.sample_commits`

GROUP BY subject

ORDER BY num_duplicates DESC

LIMIT 100

1. Most popular projects in Libraries.io deprecated or not maintained yet used as dependencies in other projects:

SELECT

name,

dependent_projects_count,

language,

status

FROM

`bigquery-public-data.libraries_io.projects_with_repository_fields`

WHERE status IN ('Deprecated', 'Unmaintained')

ORDER BY dependent_projects_count DESC

LIMIT 100

1. Wikipedia visits to pages that include "Google" in their title:

SELECT

LANGUAGE,

SUM(views) AS views

FROM

`bigquery-samples.wikipedia_benchmark.Wiki1k`

WHERE

REGEXP\CONTAINS(title, "Google")

GROUP BY

LANGUAGE

ORDER BY

views DESC

1. Repeat the previous query on the Wiki1B table:
  1. _QUESTIONS: How much data would such a query process? How long does it take? How much data would such a query process on the Wiki10B and Wiki100B tables (without executing them)?_

This allows us to execute SQL searches on BigQuery public datasets.

_DELIVERABLES:_

1. M2U3-8-task_1-file_1-screenshot_1.jpg: Screenshot showing the search and results of the first SQL search.
2. M2U3-8-task_1-file_2-screenshot_2.jpg: Screenshot showing the search and results of the second SQL search.

**Task 2: Upload and query data using the console**

For this task we are going to use the BigQuery web console to create a dataset and table, upload data and perform SQL queries on it.

**Create a dataset**

Let's start by creating a dataset to organise the data:

1. In the  **BigQueryweb** console, locate your project in  **Explorer**  and click on it.
2. Click on the 3 vertical dots drop-down menu and click ** Create a dataset**.
3. Explore the options and create a dataset with the following features:
  - Dataset ID: babynames.
  - Location: europe-west1.

**Upload data**

We are going to use a public dataset with the most popular baby names in the US, of about 7 MB in CSV format. You can find more information about it [here](http://www.ssa.gov/OACT/babynames/background.html).

1. Download the dataset in zip format: [https://www.ssa.gov/OACT/babynames/names.zip](https://www.ssa.gov/OACT/babynames/names.zip)
2. See the outline in the NationalReadMe.pdf document.
3. Consult one of the files to explore the information contained therein.

Now let's load the data into a BigQuery table:

1. In  **Explorer** , select the previously created dataset.
2. Tap the 3 vertical dots drop-down menu and click on  **Open**.
3. In the area on the right, select  **Create Table**.
4. Explore the options and create a table by uploading the data with the following features:
  - Source > Create table from > Upload > Select file: yob2014.txt> File format: CSV.
  - Destination: dataset babynames, table names_2014.
  - Schema: edit as text: name:string,gender:string,count:integer

Wait for BigQuery to finish creating the table and uploading data on the  **Job History** panel.

Once the data is uploaded, refer to the table:

1. In  **Explorer** , locate the dataset and click on the table.
2. On the details panel, click  **Preview**  and preview the data.
3. In the workspace, click  **Compose new query**  and launch the following SQL search:

SELECT

name,

count

FROM

`babynames.names_2014`

WHERE

gender = "M"

ORDER BY

count DESC

LIMIT

5

This allows us to upload and search data using the BigQuery web console.

_DELIVERIES:_ 

M2U3-8-task_2-file_1-screenshot_1.jpg: Screenshot showing the result of the SQL search on the uploaded data.

**Task 3: Upload and query data using the command line**

In this task we will create datasets, tables and upload and query data using the bq command line.

**Create a dataset**

Use Cloud Shell or your on-premises Cloud SDK installation:

1. Create a dataset called bq_load_test: bq mk bq_load_test.
2. Display its properties: bq show bq_load_test.

**Create data to upload**

Create a sample CSV file:

1. In Cloud Shell: touch customer_transactions.csv and cloudshell edit customer_transactions.csv.
2. On-premises: Use any text editor to create a file called customer_transactions.csv.
3. Add the following values:

ID,Zipcode,Timestamp,Amount,Feedback,SKU

c123,78757,2018-02-14 17:01:39Z,1.20,4.7,he4rt5

c456,10012,2018-03-14 15:09:26Z,53.60,3.1,ppiieee

c123,78741,2018-04-01 05:59:47Z,5.98,2.0,ch0c0

**Create a table and upload the data**

Load the data into a new table with the command:

bq load \

--source_format=CSV \

--skip_leading_rows=1 \

bq_load_test.customer_transactions \

./customer_transactions.csv \

id:string,zip:string,ttime:timestamp,amount:numeric,fdbk:float,sku:string

Once created, check its properties: bq show bq_load_test.customer_transactions.

**View the data**

Now we're going to do a joint search by combining the data uploaded with the US state zip code data:

bq query --nouse_legacy_sql '

SELECT SUM(c.amount) AS amount_total, z.state_code AS state_code

FROM `bq_load_codelab.customer_transactions` c

JOIN `bigquery-public-data.utility_us.zipcode_area` z

ON c.zip = z.zipcode

GROUP BY state_code

'

_Note:_ If you cannot run the query because the dataset created is not in the multi-regional US location, create another dataset in that location and upload the data using the web console.

This allows us to manage datasets and tables and upload and query data using the bq command line.

_DELIVERABLES:_ 

M2U3-8-task_3-file_1-screenshot_1.jpg: Screenshot showing the result of the command with the SQL search.

**Task 4: Manage data with client libraries**

In this task we will manage data with the client libraries, using Python code for this case.

For this task, you will find instructions and code in one of the public codelabs: [Using BigQuery with Python](https://codelabs.developers.google.com/codelabs/cloud-bigquery-python).

Remember that you don't need to create a new project or enable the API, so you can get started directly with step 4 of the project.

_DELIVERABLES:_ 

M2U3-8-task_4-file_1-screenshot_1.jpg: Screenshot showing the preview in the web console with the data uploaded to the table.

**Task 5: Working with partitioned and grouped tables**

In this task we will check the differences between several searches, using unpartitioned, partitioned and grouped tables.

First, using the console or the command line, create a dataset called stackoverflow in the multi-region US.

**Create a table with StackOverflow&#39;s 2018 posts**

Let's start by creating a new table with some StackOverflow public dataset information.

In the console, create a new search and run this SQL query:

CREATE OR REPLACE TABLE `stackoverflow.questions_2018` AS

SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count, tags

FROM `bigquery-public-data.stackoverflow.posts_questions`

WHERE creation_date BETWEEN '2018-01-01' AND '2019-01-01';

Wait for the job to finish and check the data uploaded in the new table.

This search returns questions created in January 2018 with the androidtag:

SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count

FROM `stackoverflow.questions_2018`

WHERE creation_date BETWEEN &#39;2018-01-01&#39; AND '2018-02-01'

AND tags = 'android';

Now, go to  **More > Query Settings**  and disable the cached results. We'll use this setting for the rest of the task to check the differences between the different searches.

Now re-run the last search and make sure it shows un-cached results _QUESTION: How long did the first search take and how many bytes did it process?_

**Using a Partitioned Table**

Now repeat the previous steps to create another table, this time partitioned:

CREATE OR REPLACE TABLE `stackoverflow.questions_2018_partitioned`

PARTITION BY DATE(creation_date) AS

SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count, tags

FROM `bigquery-public-data.stackoverflow.posts_questions`

WHERE creation_date BETWEEN '2018-01-01' AND '2019-01-01';

Run the above search on the partitioned table and compare its performance:

SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count

FROM `stackoverflow.questions_2018_partitioned`

WHERE creation_date BETWEEN '2018-01-01' AND '2018-02-01'

AND tags = 'android';

_QUESTION: How long did it take and how many bytes did the search process on the partitioned table? What improvement in performance have we achieved as a percentage?_

**Use a grouped and partitioned table**

Finally, let's check the improvement by grouping the columns and sorting them with a new table:

CREATE OR REPLACE TABLE `stackoverflow.questions_2018_clustered`

PARTITION BY

DATE(creation_date)

CLUSTER BY

tags AS

SELECT

id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count, tags

FROM

`bigquery-public-data.stackoverflow.posts_questions`

WHERE

creation_date BETWEEN '2018-01-01' AND '2019-01-01';

Run the above search on the partitioned and grouped table and compare its performance:

SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count

FROM `stackoverflow.questions_2018_clustered`

WHERE creation_date BETWEEN '2018-01-01' AND '2018-02-01'

AND tags = 'android'

_QUESTION: How long did it take and how many bytes did the search process on the partitioned and grouped table? What performance improvement have we achieved in percentage versus searching on the partitioned table?_

**Task 6: View Data with Data Studio**

Data Studio is a data display and reporting service, which we can connect to Google Cloud DBs, Google Drive, Google marketing services and a multitude of other sources, which is commonly used to display data stored in BigQuery.

To do this, follow the instructions in the following codelab[: Viewing your BigQuery Data in Data Studio](https://codelabs.developers.google.com/codelabs/bigquery-data-studio).

_DELIVERABLES:_ 

M2U3-8-task_6-file_1-screenshot_1.jpg: Screenshot of the final dashboard.

**Deliverables Summary**

1. M2U3-8-questions.txt: Answers to all questions raised in the exercise.
2. M2U3-8-task_1-file_1-screenshot_1.jpg: Screenshot showing the search and results of the first SQL search.
3. M2U3-8-task_1-file_2-screenshot_2.jpg: Screenshot showing the search and results of the second SQL search.
4. M2U3-8-task_2-file_1-screenshot_1.jpg: Screenshot showing the result of the SQL search on the uploaded data.
5. M2U3-8-task_3-file_1-screenshot_1.jpg: Screenshot showing the result of the command with the SQL search.
6. M2U3-8-task_4-file_1-screenshot_1.jpg: Screenshot showing the preview in the web console with the data uploaded to the table.
7. M2U3-8-task_6-file_1-screenshot_1.jpg: Screenshot of the final dashboard.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Remove datasets created in BigQuery with the console.
