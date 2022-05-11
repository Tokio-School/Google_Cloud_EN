# **Cloud Spanner**

Unit M2U3 - Exercise 4

**What are we going to do?**

1. Create an instance and database in Cloud Spanner.
2. Manage tables and diagrams.
3. Manage and query data in the database.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Create an instance of Cloud Spanner**

In this task we are going to start by creating an instance of Cloud Spanner to work with databases on it at a later stage.

To do this, start by enabling the Cloud Spanner API in **APIs and Services > Library".

**Create the Cloud Spanner Instance**

Let's create a Cloud Spanner instance using the web console:

1. In the console, navigate to  **Spanner > Create Instance**.
2. Explore all options and create an instance with the following features:
  - Name and instance ID: test-instance, we will refer to it as INSTANCE_ID.
  - Configuration: regional in europe-west1.
  - Processing capacity: 1 node or 1000 processing units.

Now simply wait for the Cloud Spanner instance to be ready to use.

**Create the database**

Once the instance has been created, we will create a database within it:

1. On the  **Cloud Spanner** page, click on the name of the instance you created.
2. Click  **Create Database**.
3. Create a database with the name example-db, without including a schema at this point.

This allows us to create a database in Cloud SQL.

**Creating a table**

Once a database has been created, we will create a table and its schema within it:

1. On the example-db Database  **Overview**  page, click on  **Create Table**.
2. In the  **Write DDL statements** box, enter the following SQL statement CREATE TABLE:

CREATE TABLE Singers (

SingerId INT64 NOT null,

FirstName STRING(1024),

LastName STRING(1024),

SingerInfo BYTES(MAX),

BirthDate DATE,

) PRIMARY KEY(SingerId);

Once the schema is updated, the table will be displayed in the console.

Now let's update the database by creating the Albums table dependent on the Singers table using the command line:

1. In Cloud Shell, update the database: gcloud spanner databases ddl update example-db --ddl ='CREATE TABLE Albums (SingerId INT64 NOT NULL, AlbumId INT64 NOT NULL, AlbumTitle STRING(MAX)) PRIMARY KEY (SingerId, AlbumId), INTERLEAVE IN PARENT singers ON DELETE CASCADE'.

This allows us to create tables and update schemas with the console and Cloud SDK.

_DELIVERABLE:_ 

M2U3-4-task_1-file_1-screenshiot_1.jpg: Screenshot showing the tables in the database.

**Task 2: Manage data in Cloud Spanner**

In this task we will manage data using the console, command line and client libraries.

**Using the web console**

Let&#39;s insert a few rows into the table:

1. On the example-db database page, click on the Singers table.
2. On the side menu, click  **Data** , then click  **Insert**.
3. Using the SQL template, replace the values in it and enter 3 rows with SingerID 1, 2 and 3.
4. Finally, on the table page, check the inserted entries.

Now let&#39;s edit one of the rows:

1. On the table page, select one of the rows and click  **Edit**.
2. Using the SQL template again, modify some values and update that row.
3. Finally, on the table page, check for the updated entry.

Let's delete one of the rows:

1. On the table page, select one of the rows and click  **Edit**.

Finally, let's perform an SQL search:

1. On the Database page, click  **Search**  in the side menu to display the searches section.
2. Click  **New Tab**
3. Run the following SQL search: SELECT \* FROM Singers;

_DELIVERABLE:_ 

M2U3-4-task_2-file_1-screenshot_1.jpg: Screenshot showing SQL search result.

**Using Cloud SDK**

We'll use the on-premises Cloud SDK or Cloud Shell command line to write data:

1. Insert a new row, replacing the corresponding values for SingerID (other than existing 1, 2, and 3) and the rest: gcloud spanner rows insert --database = example-db--table = Singers --data = SingerId= ROW_ID,FirstName= first NAME,LASTNAME=LAST NAME.
2. Insert at least 3 rows.
3. 3.	Now insert also at least 3 rows in the table Albums, related to the rows of Singers you just added: gcloud spanner rows insert --database = example-db--table =Albums --data= SingerId = SINGERS_ROW_ID, AlbumId = ALBUMS_ROW_ID,AlbumTitle="ALBUM_NAME_BETWEEN_QUOTES"

Now let's launch an SQL query from Cloud SDK:

1. 1.	Select all albums: gcloud spanner databases execute-sql example-db --sql='SELECT SingerId, AlbumId, AlbumTitle FROM Albums'

Finally, let's update the data. To do this, return to the web console to find the SQL UPDATE statement template to update data and use the previous Cloud SDK command to launch an SQL statement that updates any of the rows you have inserted in any of the tables.

_DELIVERABLE:_ 

M2U3-4-task_2-file_2-screenshot_2.jpg: Screenshot showing the result of the last SQL statement.

**Using the Client Library for Python**

Finally we are going to perform several data management actions using examples with the Python client library.

To do this, first start a Cloud Shell work environment or your on-premises Cloud SDK installation:

1. Clone the sample repository: git clone https://github.com/googleapis/python-spanner.
2. Access the working directory: cd python-spanner/samples/samples.
3. Create a virtual environment isolated from Python and install the dependencies:

virtualenv env

source env/bin/activate

pip install -r requirements.txt

Check the code to be executed is available on GitHub: [snippets.py](https://github.com/googleapis/python-spanner/blob/HEAD/samples/samples/snippets.py).

We&#39;re going to run some function snippets from the snippets.py file with the python command snippets.py test-instance --database-id example-db FUNCTION\_NAME.

Below is a list of functions to run with a reference link in the documentation and the name of the function:

1. [Write data with DML](https://cloud.google.com/spanner/docs/getting-started/python#write-data): insert_with_dml
2. [Write data with mutations](https://cloud.google.com/spanner/docs/getting-started/python#write-data-with-mutations): insert_data
3. [SQL Searches](https://cloud.google.com/spanner/docs/getting-started/python#using_the_client_library_for): query_data
4. [SQL queries with parameters](https://cloud.google.com/spanner/docs/getting-started/python#query_using_a_sql_parameter): query_data_with_parameter
5. [Read data with the read API](https://cloud.google.com/spanner/docs/getting-started/python#read_data_using_the_read_api): read_data
6. [Update data](https://cloud.google.com/spanner/docs/getting-started/python#update-data): write_with_dml_transaction
7. [Searches with read-only transactions](https://cloud.google.com/spanner/docs/getting-started/python#retrieve_data_using_read-only_transactions): read_only_transaction

_DELIVERABLES:_ 

M2U3-4-task_2-file_3-screenshot_3.jpg to M2U3-4-task_2-file_9-screenshot_9.jpg: Screenshots of the result of each function's code snippets.

**Deliverables Summary**

1. M2U3-4-task_1-file_1-screenshiot\_1.jpg: Screenshot showing the tables in the database.
2. M2U3-4-task_2-file_1-screenshot\_1.jpg: Screenshot showing SQL search result.
3. M2U3-4-task_2-file_2-screenshot\_2.jpg: Screenshot showing the result of the last SQL statement.
4. M2U3-4-task_2-file_3-screenshot\_3.jpg to M2U3-4-task_2-file_9-screenshot_9.jpg: Screenshots of the result of each function's code snippets.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the Cloud Spanner database and instance in the console.
