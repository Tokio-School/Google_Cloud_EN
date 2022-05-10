# **Cost Analysis**

Unit M2U1 - Exercise 6

**What are we going to do?**

1. Automatically export GCP costs to BigQuery.
2. Analyse costs with SQL.
3. Visualise costs with Data Studio.

_NOTE: To complete this exercise you will need to export the costs to BigQuery and wait until the next day for the first export to take place._

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Export Costs to BigQuery**

In this task we will set the automatic export of costs to BigQuery and analyse them with SQL.

**Export Costs**

To export daily costs to a BigQuery table, you can follow the instructions detailed in the [documentation](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-setup#create-bq-dataset):

- You can start with step 4 (linked) directly, as you don't have to create a project, verify billing, or enable the BigQuery Data Transfer Service API.
- For a dataset name you can use billing_export.
- Use multi-region European Union ("EU") to create the dataset.
- As an export type, use standard cost export only.
- The frequency of export can vary, from a few hours for the first export to several hours the following day ([documentation](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-tables#data-loads)).

_ **Wait a few hours or even a day for the costs to be exported and continue with the next part of the exercise.** _

**Analyse costs with SQL**

Before analysing costs, familiarize yourself with the standard cost table outline: [schema documentation](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-tables#standard-usage-cost-data-schema).

You can also navigate to  **BigQuery > SQL Workspace** , navigate to the created dataset and table, and analyse the  **Schema**  tab by clicking on the table.

Once you are familiar with the export schema, navigate to the BigQuery SQL Search Editor and run the following queries:

1. Total costs added per monthly invoice: [SQL query](https://cloud.google.com/billing/docs/how-to/bq-examples#sum-costs-per-invoice).
2. Detailed costs by type, per monthly invoice: [SQL query](https://cloud.google.com/billing/docs/how-to/bq-examples#costs-details-per-invoice).
3. Detailed costs and credits per project, per monthly invoice: [SQL query](https://cloud.google.com/billing/docs/how-to/bq-examples#query-by-project-by-invoice).
4. _Note: Remember to replace the table name with the name of your BiQuery table in every SQL query._

_DELIVERABLES:_

1. M2U1-6-task_1-file_1-screenshot_1.jpg: Screenshot of the result of the first query "Total costs added per monthly invoice".
2. M2U1-6-task_1-file_2-screenshot_2.jpg: Screenshot of the result of the second query "Detailed costs by type, per monthly invoice".
3. M2U1-6-task_1-file_3-screenshot_3.jpg: Screenshot of the result of the third query "Detailed costs and credits by project, per monthly invoice".

**Task 2: Visualise costs with Data Studio**

One of the recommended first steps when starting to work with Google Cloud is to export the costs to BigQuery and visualise them with Data Studio.

Data Studio is a Google service integrated with the Google Cloud Platform databases and other services, such as Google Drive, Google Analytics, AdWords, etc.

Step-by-step instructions on how to deploy the Data Studio expense report template and connect it to the exported BigQuery table are available in the [documentation](https://cloud.google.com/billing/docs/how-to/visualize-data#start-working-with-the-sample-cloud-billing-report) and in the Data Studio template itself.

_NOTE: It may be advisable to monitor expenses during the course, using the billing reports in the console and/or using the Data Studio report_.

_BONUS: How about adding a new page to the Data Studio dashboard with customised charts?_

_DELIVERABLE_: M2U1-6-task_2-file_1-screenshot_1.jpg: Screenshot of the first page of the Data Studio report.

**Deliverables Summary**

1. M2U1-6-task_1-file_1-screenshot_1.jpg: Screenshot of the result of the first query "Total costs added per monthly invoice".
2. M2U1-6-task_1-file_2-screenshot_2.jpg: Screenshot of the result of the second query "Detailed costs by type, per monthly invoice".
3. M2U1-6-task_1-file_3-screenshot_3.jpg: Screenshot of the result of the third query "Detailed costs and credits by project, per monthly invoice".
4. M2U1-6-task_2-file_1-screenshot_1.jpg: Screenshot from the first page of the Data Studio report.

**Clean up resources**

_There are no resources to delete after this exercise._
