# **BigTable**

Unit M2U3 - Exercise 7

**What are we going to do?**

1. Create an instance of Cloud Bigtable.
2. Manage data with the cbt tool.
3. Query data from an application.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Create an instance of Cloud BigTable**

Let's create an instance of Cloud Bigtable to use in the exercise.

First enable the Bigtable and Bigtable Admin APIs in  **APIs and Services > Library**.

Then create a Bigtable instance:

1. In the console, navigate to  **BigTable**  and click on  **Create Instance**.
2. Explore all options and create an instance with the following features:
  - Name and instance ID: test-instance.
  - Storage Type: HDD.
  - Cluster ID: test-intance-c1.
  - Region and area: europe-west1-b.

This allows us to create an instance of Cloud Bigtable with a zonal cluster.

**Task 2: Manage data with cbt**

In this task we will connect and manage data with the cbt tool using Cloud Shell or your local Cloud SDK installation.

_Note:_ If you do not have the cbt component installed on your local Cloud SDK installation, please install it by following the [instructions](https://cloud.google.com/sdk/docs/components#managing_components).

**Connect to the instance**

1. 1.	Configure cbt to use your project and instance: echo "project = PROJECT_ID" > ~/.cbtrc and echo "instance = test-instance" >> ~/.cbtrc.
2. Check your settings: cat ~/.cbtrc.

**Creating a table**

1. Create a my-table: cbt createtable my-table table.
2. List your tables: cbt ls.
3. Create a family of columns cf1: cbt createfamily my-table cf1.
4. List your column families: cbt ls my-table.

**Create and read rows**

1. Create a row with the key r1 and the value test-value for column c1 of the column family cf1: cbt set my-table r1 cf1:c1=test-value.
2. Lists all the rows in the table: cbt read my-table.
3. Insert 3 new rows with the keys r2, r3, r4.
4. List all rows: cbt read my-table.

Finally, delete the table and instance created: cbt deleteable my-table and cbt deleteinstance test-instance.

_DELIVERABLES:_ 

M2U3-7-task_2-file_1-screenshot_1.jpg: Screenshot showing the reset of the last command to list all the rows.

**Task 3: Query data from an application**

For the final task, we're going to run a codelab with a sample application.

The application will create an instance and table in Bigtable, import a large dataset from Cloud Storage using Cloud Dataflow as a data pipeline, and execute queries to view data about the bus location in Manhattan.

To do this, follow the steps of the public codelab: [Introduction to Cloud Bigtable](https://codelabs.developers.google.com/codelabs/cloud-bigtable-intro-java).

_DELIVERABLES:_ 

M2U3-7-task_3-file_1-screenshot_1.jpg: Screenshot showing the reset of the last command in step 10.

**Deliverables Summary**

1. M2U3-7-task_2-file_1-screenshot_1.jpg: Screenshot showing the reset of the last command to list all the rows.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. In the console, delete the Cloud Bigtable instance.
