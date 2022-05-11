# **Cloud Memorystore**

Unit M2U3 - Exercise 6

**What are we going to do?**

1. Create a Cloud Memorystore instance.
2. Connect from a client VM instance.
3. Deploy a sample web application.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Connect to a Cloud Memorystore instance**

In this task we are going to create a Cloud Memorystore instance and connect from a client VM instance.

First enable the Cloud Memorystore for Redis API.

**Create a Cloud Memorystore instance.**

Let's start by creating the Cloud Memorystore instance:

1. In the console, navigate to  **Memorystore > Redis**  and select  **Create Instance**.
2. Explore the various options and create an instance with the following features:
  1. Instance ID: redis-instance.
  2. Level: basic.
  3. Zone: europe-west1-b.
  4. Capacity: 5GB.
  5. Version: 6. x
  6. Authorized VPC network: projects/PROJECT\_ID/global/networks/default.
3. Copy the instance IP address

_Note:_

1. If you have a problem with the IP ranges when creating the instance, navigate to  **VPC Network > VCP Networks > default network** , select  **Private Connection to Services**  and assign an automatic IP range with a length of 20.
2. If you're still having problems, create a new custom VPC and a subnet in europe-west1 ([docs](https://cloud.google.com/vpc/docs/using-vpc#create-custom-network)) and enable Private Services Accesss in it ([docs](https://cloud.google.com/vpc/docs/configure-private-services-access)).
3. Wait until the Cloud Memorystore for Redis instance has been created and copy the instance IP, which we will call the REDIS_INSTANCE IP.
4. This allows us to deploy a Cloud Memorystore instance compatible with Redis for use as cache, in-memory database, etc.
5. **Create client VM instance**
6. Now let's create a VM instance to act as a client and connect to the Cloud Memorystore instance.
7. Create a VM instance with the following characteristics:

- Name: client-vm.
- Zone: europe-west1-b.
- Type of machine: e2-micro.
- Boot disk: Debian 11, 10 GB standard.
- Networks: Disable external IP, so that it only has internal IP.
- _Note:_ If you used a new VPC for your Cloud Memorystore instance, select that network under  **Networks**.

1. **Connection from the client**
2. Now configure and check access from the VM instance:

1. Install the telenet tool: sudo apt-get install telnet.
2. Connect to the Redis instance using telnet: telnet IP_INSTANCE_REDIS 6379.
3. Check the connection on the terminal to Redis: PING. You must receive PONG as an answer.
4. Check the key creation: SET HELLO WORLD and GET HELLO. You must answer WORLD.

1. We have thus connected to the Redis instance from a client VM instance.

2. _DELIVERABLES:_ 

M2U3-6-task_1-file_1-screenshot_1.jpg: Screenshot of the connection to the Redis terminal showing the response of the PING, SET HELLO WORLD and GET HELLO commands.

3. **Task 2: Deploy a sample web application**
4. Now let's deploy a sample web application that uses a Cloud Memorystore Redis instance.
5. The app keeps a count of visits to your/Redis endpoint. Start by looking over the files in your [repository](https://github.com/GoogleCloudPlatform/python-docs-samples/tree/master/memorystore/redis).
6. **Deploy the web application**
7. The application needs a Cloud Memorystore instance already deployed, with its internal IP localised and copied.
8. You also need a GCS bucket, so create a regional bucket in europe-west1 and write down its name, BUCKET\_NAME.
9. Once the bucket is deployed, deploy the application from Cloud Shell or your on-premises Cloud SDK installation:

1. Clone the repository: git clone https://github.com/GoogleCloudPlatform/python-docs-samples.
2. Open the corresponding directory: cd python-docs-samples/memorystore/redis/gce_deployment.
3. The deploy.sh script deploys the application artifact to GCS, deploys GCE instance, runs the application with an instance startup script, and creates a firewall rule to enable traffic to it.
4. Set the local environment variables to configure access to the Redis instance: export redishost =IP_INSTANCE_redis and export redisport=6379.
5. Set the local environment variable to configure access to the GCS bucket: export GCS_BUCKET_NAME= BUCKET_NAME.
6. Enable the script as executable and run it: chmod +x deploy.sh and./deploy.sh.

1. **Check the web application**
2. Once the VM instance is ready, copy its external IP and accesses it from your browser: http://EXTERNAL\_IP:8080.
3. You can use the local  teardown.sh script to remove the VM instance and firewall rule: chmod +x teardown.sh and ./teardown.sh.
4. _DELIVERABLES:_ 

M2U3-6-task\_2-file\_1-screenshot\_1.jpg: Screenshot of the website showing its URL.

 **Deliverables Summary**

1. M2U3-6-task_1-file_1-screenshot_1.jpg: Screenshot of the connection to the Redis terminal showing the response of the PING, SET HELLO WORLD and GET HELLO commands.
2. M2U3-6-task_2-file_1-screenshot_1.jpg: Screenshot of the web page showing its URL.

1. **Clean up resources**
2. Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instances created.
2. Delete the Cloud Memorystore instance.
3. Delete the GCS bucket.
4. If you have created an additional VPC, please delete it.
