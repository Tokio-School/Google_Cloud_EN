# **Private Access Options and Cloud NAT**

Unit M2U4 - Exercise 6

**What are we going to do?**

1. Create internal VM instances, without external IP.
2. Enable Private Google Access to be able to access services such as Cloud Storage.
3. Deploy Cloud NAT to be able to access external destinations without the need for external IP.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U4-6-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You'll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Create Internal Deployment**

As usual, start by creating the network topology and a private VM instance, without an external IP:

1. Create a VPC network with the following features:
  - Name: vpc-cloud.
    - Subnet Creation Mode: Custom.
    - Subnet: vpc-cloud-subnet.
      - Region: europe-west1.
      - IP Address Range: 10.10.0.0/20
    - Dynamic Routing Mode: Global.
2. Create the following firewall rule:
  - Name: allow-ssh-iap-all.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: All instances of the network.
    - Source filter: 35.235.240.0/20
    - Priority: 1000.
    - Protocols and ports: TCP:22.
3. Finally, create the following VM instances:
  - Name: vm-cloud.
    - Zone: europe-west1-b.
    - Type of machine: e2-micro.
    - Network: vpc-cloud.
    - External IP: No external IP.

Once created, check that you can connect via SSH to the instance using Cloud Shell or a local Cloud SDK installation:

1. Connect using Cloud IAP (we&#39;ll learn more about it in the next exercise): gcloud compute ssh vm-cloud --zone europe-west1-b --tunnel-through-iap.
2. If you are asked to confirm to continue, confirm with Y.
3. If you are asked for a password or passphrase, simply press the  **ENTER**  key to continue.
4. Check that the instance does not have an external connection, since it does not have an external IP: ping -c 2 www.google.com.

Finally, let's create a Cloud Storage bucket and upload a sample file:

1. 1.	Create a bucket with a unique name, such as ID_PROJECT-m2u4-6..
2. 2.	In Cloud Shell or a local Cloud SDK installation, create a file and upload it to the bucket: echo "hello world!" > sample.txt and gsutil cp gs://BUCKET_NAME/sample.txt.
3. Check the uploaded file: gsutil cat gs://BUCKET_NAME/sample.txt.

**Task 2: Enable private access to Google Cloud services**

We are now going to enable private access to Google Cloud or Private Google Access services, in order to access Cloud Storage from the private instance.

First check that the instance cannot connect to Cloud Storage at this time:

1. Connect to the instance via SSH again: gcloud compute ssh vm-cloud --zone europe-west1-b --tunnel-through-iap.
2. Make sure you can't copy the file from Cloud Storage: gsutil cp gs://BUCKET\_NAME/sample.txt ..

**Enable Private Google Access**

We are now going to enable Private Google Access to create a path through the default internet gateway so that the private instance can connect to Cloud Storage:

1. Navigate to  **VPC Network > VPC Networks** , click on vpc-cloud and on vpc-cloud-subnet.
2. Select  **Edit**  and turn on private access to Google.

Finally go back via SSH connection to the VM instance and check that you can now download the file.

This way we can allow internal instances to access most Google Cloud services, external to our VPC.

_DELIVERABLE:_ 

M2U4-6-task_2-file_1-screenshot_1.jpg: Screenshot showing subnet details page with PGA enabled.

**Task 3: Enable Cloud NAT to external destinations**

In this final task we are going to enable Cloud NAT as managed NAT in order to have external connectivity to the outside (only in the outgoing direction).

Return via SSH connection to instance:

1. _QUESTION: What happens if you need to update or install distribution packages?_
2. You can check this with the sudo apt update and sudo apt install http2 commands.
3. _Note:_ Different packages may come from different sources...

**Configure Cloud NAT**

Let&#39;s set up a Cloud NAT gateway:

1. Navigate to  **Network Services > Cloud NAT**  and select  **Start**.
2. Explore the options and create a NAT gateway with the following features:
  - Name: nat-euw1.
  - Network: vpc-cloud.
  - Region: europe-west1.
  - Cloud Router: create new router, router-euw1, keepalive interval: 20 s.
  - Source (internal): Primary and secondary ranges for all subnets.
  - NAT IP addresses: Automatic.

Now go back and check if you can update the VM instance&#39;s packages, ping outside, etc.

_BONUS:_ You can check from which external IP your connection comes out, e.g. with curl ifconfig.me.

_DELIVERABLE:_ 

M2U4-6-task_3-file_1-screenshot_1.jpg: Screenshot showing details of NAT gateway.

**Enable logs for Cloud NAT**

To view the different logs available on Cloud NAT connections, follow these steps:

1. Navigate to  **Network Services > Cloud NAT** , click on the NAT gateway name and click on  **Edit**.
2. In advanced configuration,  **Stackdriver Logging** , enables translation logs and errors.

Once enabled, navigate to Cloud Logging in the  **Logs**  tab of the NAT Gateway details page:

1. You will not see any logs because there has been no traffic since the logs have been enabled.
2. Go back to the instance and generate some outgoing traffic to external destinations, e.g. with www.google.com, refreshing the packets again, checking the IP, etc., several times.
3. Go back to Cloud Logging, refresh the logs, and you will see the logs for that traffic.

_Note:_ If you don't see any logs yet, wait a couple of minutes and generate more traffic again.

_DELIVERABLE:_ 

M2U4-6-task_3-file_2-screenshot_2.jpg: Screenshot showing traffic logs via Cloud NAT.

This allows us to enable a NAT gateway to enable a shared external connection to instances that do not have an external IP directly assigned to them.

**Deliverables Summary**

1. M2U4-6-questions.txt: Answers to all questions raised in the exercise.
2. M2U4-6-task_2-file_1-screenshot_1.jpg: Screenshot showing subnet details page with PGA enabled.
3. M2U4-6-task_3-file_1-screenshot_1.jpg: Screenshot showing details of NAT gateway.
4. M2U4-6-task_3-file_2-screenshot_2.jpg: Screenshot showing traffic logs via Cloud NAT.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instance created.
2. Remove the NAT gateway and router created.
3. Delete the created network.
