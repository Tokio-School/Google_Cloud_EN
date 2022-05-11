# **Cloud SQL**

Unit M2U3 - Exercise 3

**What are we going to do?**

1. Create an instance of Cloud SQL.
2. Connect to it publicly and privately.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

Before you begin, enable the Cloud SQL and Cloud SQL Admin APIs.

**Task 1: Create an instance of Cloud SQL**

Let's start by creating the Cloud SQL instance and a VM instance that acts as a client:

**Create Client VM Instance**

In this first step, create a VM instance with the following features:

- Name: client-vm.
- Zone: europe-west1-b.
- Type of machine: e2-micro.
- Boot disk: Debian 11, 10 GB standard.
- Networks: Disable external IP, so that it only has internal IP.

**Create an instance of Cloud SQL.**

Now let&#39;s create the MySQL instance of Cloud SQL:

1. In the console, navigate to  **SQL**  and click on  **Create Instance**.
2. Choose  **MySQL**.
3. Explore the options and create an instance with the following features (_note:_ remember to display the  **Customise Your Instance** menu):
  - Instance ID: mysql-bdd.
  - Root password: Choose a password you remember or write it down, such as root-passw. We will refer to it as ROOT_PASSW.
  - Version: MySQL 8.0.
  - Region: europe-west1.
  - Zonal availability: several zones, european-west1 as main zone and any as secondary zone.
  - Type of machine: 1 vCPU 1.7 GB shared core.
  - Storage: 20GB HDD.
  - Connections: Public IP.

_Note:_ The Cloud SQL instance may take up to 3-5 minutes to become available.

In this simple way, we can create an instance of Cloud SQL with MySQL 8.0.

_DELIVERABLES:_ 

M2U3-3-task_1-file_1-screenshot_1.jpg: Screenshot from the Cloud SQL Instance  **Overview**  page.

**Task 2: Public connection to the instance**

Now we're going to connect to the Cloud SQL instance publicly, through your public IP.

For this we will use Cloud Shell or a local installation of Cloud SDK, environments that cannot be accessed with internal IP as they are not internally connected to Cloud SQL.

**Connection via MySQL user**

To connect to the instance, we must first create a user for the DB and not use the "root" user.

1. In the console, navigate to  **SQL > Users**.
2. Click  **Add User Account**  and create one with the following features:
  1. Select  **Built-in authentication**.
  2. Username: user_sql.
  3. Password: Choose a password you remember or write it down, such as user-passw. We will refer to it as USER_PASSW.
  4. Hostname: Allow any host.

Now let's connect using the Cloud SDK in Cloud Shell or locally, enabling the source IP as the network authorised to connect to Cloud SQL:

1. Connect to the instance with the created user: gcloud sql connect mysql-bdd -u user\_sql.
2. Using the mysql terminal, check the connection: show databases;
3. Once you've checked this, close the connection: exit.

_DELIVERABLES:_ 

M2U3-3-task_2-file_1-screenshot_1.jpg: Screenshot showing the result of the gcloud sql connect mysql-bdd -u user_sql command.

You can check that the source IP (Cloud Shell or local) has been approved as an authorised network:

1. Navigate to  **SQL > Connections** , under  **Authorised networks**.
2. Cloud SDK automatically logs the source IP and authorises it for at least 5 extendable minutes after the last command.

This way we can easily connect to Cloud SQL from an external source.

**Connection via Cloud SQL Auth proxy**

We are now going to use the Cloud SQL Auth proxy to establish a connection automatically and encrypted from any environment or application:

1. Download the executable and enable it as executable following the instructions in the [documentation](https://cloud.google.com/sql/docs/mysql/connect-admin-proxy#install).
2. In the console, navigate to  **SQL** , click on the instance, and review the  **Overview** page.
3. Locate the card  **Connect to this instance** , and into it copy the instance  **Connection name**.
4. If you are not using Cloud Shell, where we have a MySQL client already installed, please install the client locally: [instructions](https://dev.mysql.com/doc/refman/5.7/en/installing.html).
5. Start the Cloud SQL Auth proxy:./cloud_sql_proxy -instances= INSTANCE_CONNECTION_NAME =tcp:3306.
6. Open another terminal tab and connect to the local proxy: mysql -u user\_sql -p --host 127.0.0.1 (use the user password).
7. Using the mysql terminal, check the connection: show databases;.

_Note:_ If you want to check how to upload and work with data in MySQL, you can follow [the quickstart instructions](https://cloud.google.com/sql/docs/mysql/quickstart#create_a_database_and_upload_data) in the documentation.

This way we can connect to Cloud SQL through a local proxy with Cloud SQL Auth proxy.

_DELIVERABLES: _ 

M2U3-3-task_2-file_2-screenshot_2.jpg: Screenshot showing the result of  ./cloud_sql_proxy -instances = INSTANCE_CONNECTION_NAME =tcp:3306.

**Task 3: Private connection to the instance**

Now let's connect privately to the instance, via internal IP.

**Configure the Cloud SQL instance as internal**

Let's configure Private Servicess Access to enable a private connection between a range of private IPs in our VPC and the Cloud SQL instance:

1. In the console, navigate to  **SQL** , click on your instance, and in the left column, navigate to  **Connections**.
2. Enable  **private IP**  for the instance, select the default network, and in the warning box, tap  **Configure Connection**.
3. Follow the instructions to enable private services access connection:
  1. Enables the Service Networking API.
  2. Uses an automatically assigned IP range.
4. Remember to save your changes when you finish.

In this way we have enabled Private Services Access on our VPC default and a private IP for the Cloud SQL instance.

_Note:_ Instance configuration needs to be restarted and can take up to 10 minutes.

**Connect to private instance**

To check the private connection, connect to the previously created client VM instance, install the Cloud SQL Auth Proxy and mysql client by following the instructions above, and check the connection to the instance.

_DELIVERABLES:_ 

M2U3-3-task_3-file_1-screenshot_1.jpg: Screenshot showing connection from client VM instance.

**Connection via authentication in the database with IAM**

To authenticate with our Cloud IAM user and access in Cloud SQL, the first thing to do is to enable authentication with IAM on the instance:

1. In the console, navigate to the Cloud SQL instance and click  **Edit**.
2. Under  **Customize your instance**  expand  **Brands**.
3. Select cloudsql_iam_authentication and set it to On.

Now let's create a user in the database with authentication enabled:

1. On the details page of the Cloud SQL instance, select  **Users**  in the left column.
2. Click  **Add User Account**  and select  **Cloud IAM**.
3. Add your user account, including your email address.

As we have the project owner role, we already have enough permissions to log in to the instance. However, let&#39;s see how they would be added for another user:

1. In the console, navigate to  **IAM and admin > IAM** , find your user, and click the edit button (pencil icon).
2. 2.	Adds "Cloud SQL Client and " Cloud SQL Instance User" role.

_Note:_ The &quot;Cloud SQL Instance User&quot; role may have been automatically assigned.

Now finally let's connect to the instance with IAM authentication:

_Note:_ For simplicity, we will connect externally via Cloud Shell or a local Cloud SDK installation.

1. Follow the instructions to create a new certificate and keys: [documentation](https://cloud.google.com/sql/docs/mysql/configure-ssl-instance#new-client).
2. In the console, copy the external IP address of the Cloud SQL instance, which we will refer to as EXTERNAL_IP_SQL.
3. Use the mysql client to connect to the instance:

MYSQL_PWD=`gcloud auth print-access-token` mysql --enable-cleartext-plugin--ssl-ca= server-ca.pem--ssl-cert= client-cert.pem--ssl-key= client-key.pem --host= EXTERNAL_IP_SQL --user=USERNAME

1. Make sure the certificate and keys are in the current directory, or modify the path to them.

1. As 'USERNAME', use the name part of your email address, without' @ 'or the domain. E.g. 'info@indavelopers.com' -> 'info'.

1. Check access to the database: show databases;.

This way we can authenticate ourselves in the database with a user without having to use a password in it.

_DELIVERABLES:_

M2U3-3-task_3-file_2-screenshot_2.jpg: Screenshot showing the result of the connection command using the authentication token.

**Deliverables Summary**

1. M2U3-3-questions.txt: Answers to all questions raised in the exercise.
2. M2U3-3-task_1-file_1-screenshot_1.jpg: Screenshot from the Cloud SQL Instance  **Overview**  page.
3. M2U3-3-task_2-file_1-screenshot_1.jpg: Screenshot showing the result of the gcloud sql connect mysql-bdd -u user_sql command.
4. M2U3-3-task_2-file_2-screenshot_2.jpg: Screenshot showing the result of  ./cloud_sql_proxy -instances = INSTANCE_CONNECTION_NAME =tcp:3306.
5. M2U3-3-task_3-file_1-screenshot_1.jpg: Screenshot showing connection from client VM instance.
6. M2U3-3-task_3-file_2-screenshot_2.jpg: Screenshot showing the result of the connection command using the authentication token.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the client VM instance.
2. Delete the Cloud SQL instance.
3. Follow the instructions to [delete the private connection to services](https://cloud.google.com/vpc/docs/configure-private-services-access#removing-connection).
