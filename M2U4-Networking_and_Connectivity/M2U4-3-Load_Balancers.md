# **Load balancers**

Unit M2U4 - Exercise 3

**What are we going to do?**

1. Create 2 regional frontend clusters and one regional backend cluster with a web application.
2. Deploy a regional internal load balancer for the backend cluster and a global external load balancer for the frontend clusters.
3. Add a Cloud Storage bucket to the external load balancer and redirect traffic according to destination.
4. Check the high scalability and availability of the clusters.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Creating the backend cluster and internal load balancer**

Let&#39;s start with the deployment of the network, backend cluster and internal load balancer for it. In this case, to simplify, we are going to deploy only one backend cluster, although in many applications duplicates can be deployed in several regions.

**Create the VPC network**

We start by creating the VPC network, subnets, and firewall rules:

1. Create a VPC network with the following features:
  - Name: custom-mode-net.
  - Subnet Creation Mode: Custom.
2. Create 2 subnets with the following features:
  - Name: custom-mode-net-subnet-euw1.
    - Region: europe-west1.
    - IP Address Range: 10.10.0.0/20.
  - Name: custom-mode-net-subnet-euw3.
    - Region: europe-west3.
    - IP Address Range: 10.20.0.0/20
3. Create the following firewall rules:
  - Name: allow-ssh-ping-internet-all.
    - Network: custom-mode-net.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: All instances of the network.
    - Protocols and ports: TCP:22 and ICMP.
  - Name: allow-http-frontend-backend.
    - Network: custom-mode-net.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow
    - Destinations: Specified destination tags, webapp-apache2-backend.
    - Source filter: Source tags, webapp-apache2-frontend.
    - Protocols and ports: TCP:80.

**Create the backend cluster**

Now let's begin the process of deploying an application to a regional instance cluster. To do this we first have to create a disk image, then an instance template that includes it and finally the cluster.

Start by creating a disk image with the application deployed:

1. Create a VM instance with the following characteristics:
  - Instance type: e2-micro.
  - Boot Disk: New Balanced Persistent Disk, 10 GB, Debian 11.
  - Network: custom-mode-net.
2. Connect via SSH to the instance and install the Apache2 web server: sudo apt update &amp;&amp; sudo apt install -y apache2.
3. Check that the web server is active: curl localhost:80.
4. Turn off the VM instance, navigate to  **Compute Engine > Images**  and click  **Create Image**.
5. Explore the available options and create a disk image with the following features:
  - Name: webapp-apache2-v1.
  - Source: disk, source disk: boot disk of the previously created instance.
  - Location: Multiregional.
  - Family: webapp-apache2.
6. Then navigate to  **Compute Engine > Instance Templates** , select  **Create Instance Template**  and create one with the following features:
  - Name: webapp-apache2-backend-template.
  - Type of machine: e2-small.
  - Boot disk: Custom image, webapp-apache2-v1, balanced persistent disk, 10 GB.
  - Network tags: webapp-apache2-backend.
  - Network: custom-mode-net.
  - No external IP.

Finally, we are going to create the group of instances managed with the application:

1. Navigate to  **Compute Engine > Instance Groups**  and click  **Create Instance Group**.
2. Select **New Managed Instance Group (Stateless)** in the left column.
3. Explore the available options and create an MIG with the following features:
  - Name: webapp-apache2-backend.
  - Location: various areas (all), region: europe-west1.
  - Instance template: webapp-apache2-backend-template.
  - Auto scale adjustment: Auto scale adjustment.
  - Minimum number of instances: 3, maximum number: 10.
  - Auto Repair: Create a status check
  - Status check: hc-http, TCP:80

Once the cluster is created, open an SSH session for each of the instances and verify that the web server is active: curl localhost:80.

We have thus followed all the steps to deploy a web application in a managed instance group.

_DELIVERABLE:_ 

M2U4-task_1-file_1-screenshot_1.jpg: Screenshot showing backend MIG detail page.

**Create the internal load balancer**

Finally, let's create an internal network load balancer to balance traffic from the frontend to the backend clusters:

1. Navigate to  **Network Services > Load Balancing**  and select  **Create Load Balancer**.
2. Select  **TCP Load Balancing** ,  **Only between my VMs** , and  **Only in a region**.
3. Explore the available options and create a load balancer with the following features:
  - Name: webapp-apache2-backend-lb.
  - Region: europe-west1.
  - Network: custom-mode-net.
  - Backend settings:
    - New backend with webapp-apache2-backend instance group.
    - Status check: create new, hc-http global scope, TCP:80
  - Frontend Settings:
    - Subnet: custom-mode-net-subnet-euw1.
    - Ports: Single, 80
    - Global Access: Enable
    - Service Tag: webapp-apache2-backend.
  - Use the  **Review and Finish**  option to check your settings and create the balancer.

Once created, we need to create an internal instance to check access to it.

1. Create a VM instance with the following characteristics:
  - Name: internal-client.
  - Region: europe-west1.
  - Machine type: e2-standard-4.
  - Network: custom-mode-net.
  - Network tags: webapp-apache2-frontend.
2. Connect to it via SSH.
3. Check that you can ping the MIG backend instances: ping -c 3 IP_INTERNAL_BACKEND.
4. Check that you can access their web application: curl http://IP_LB_INTERNO:80 and curl webapp-apache2-frontend.

This allows us to deploy a load balancer to allow internal traffic between the frontends and the group of backend instances of an application.

_DELIVERABLE:_ 

M2U4-task_1-file_2-screenshot_2.jpg: Screenshot showing the details page of the internal load balancer.

**Task 2: Creating the frontend clusters and the external load balancer**

In this task we are going to create the frontend managed instance group and the external load balancer for them.

**Create frontend clusters**

Just as we have done before, we are going to deploy a frontend managed instance group.

Since in this case they will use the same application, you can duplicate the instance template:

1. Navigate to  **Compute Engine > Instance Templates** , select the created instance template, and click  **Copy**.
2. Make sure you have the same settings as the previous template, and simply change your network tag: webapp-apache2-backend-template to webapp-apache2-frontend-template.

Now create the 2 instance groups for the frontends:

1. Navigate to  **Compute Engine > Instance Groups**  and click  **Create Instance Group**.
2. Select **New Managed Instance Group (Stateless)** in the left column.
3. Create an MIG with the following features:
  - Name: webapp-apache2-frontend-usc1.
  - Location: various areas (all), region: us-central1.
  - Instance template: webapp-apache2-frontend-template.
  - Auto scale adjustment: Auto scale adjustment.
  - Minimum number of instances: 3, maximum number: 10.
  - Auto Repair: Create a status check
  - Status check: hc-http, TCP:80
4. Create a second MIG with the following features:
  - Name: webapp-apache2-frontend-euw1.
  - Location: various areas (all), region: europe-west1.
  - Instance template: webapp-apache2-frontend-template.
  - Auto scale adjustment: No auto scale adjustment.
  - Minimum number of instances: 1.
  - Auto Repair: hc-http status check.

We have thus been able to create 2 instance clusters, the one closest to us with low capacity and no autoscaling (for later checks), and a distant cluster with autoscaling.

**Upload static files to Cloud Storage**

We&#39;ll also see how we can use a Cloud Storage bucket as a backend service to an HTTPS load balancer.

To do this, create a regional Cloud Storage bucket in europe-west1 and upload any image in JPG called image.jpg, e.g., downloaded from the internet. Finally, edit the access permissions of the bucket to allow public access to all objects.

**Create the external global load balancer**

For the last step of the task, we will create the HTTP(S) global external load balancer for the 2 regional frontend clusters:

1. Navigate to  **Network Services > Load Balancing**  and select  **Create Load Balancer**.
2. Select **HTTP(S) Load Balancing** and  **From Internet to My VMs**.
3. Explore the different options and create a load balancer with the following features:
  - Name: web-apache2-frontend-lb.
  - Backend settings:
    - Backend services:
      - Name: web-apache2-frontend-lb-webserver.
      - Backend Type: Instance Group
      - Protocol: HTTP
      - Port with name: http.
      - Backends:
        - Instance Group:
          - Group: webapp-apache2-frontend-euw1.
            - Capacity: 50%
        - Instance Group:
          - Group: webapp-apache2-frontend-usc1.
            - Capacity: 100%.
    - Backend Bucket:
      - Name: web-apache2-frontend-lb-bucket.
      - Bucket: The previously created GCS bucket.
  - Host and path rules:
    - Host and paths: any mismatch (default), backends: web-apache2-frontend-lb-webserver.
    - Host: \*, Paths:/static/\*, backends: web-apache2-frontend-lb-bucket.
  - Frontend Settings:
    - Name: web-apache2-frontend-lb-fe.
    - Protocol: HTTP
    - IP version: IPv4.
    - IP address: create static IP address, web-apache2-frontend-lb-fe-ipv4.

Once the process is complete, it will take a couple of minutes to create.

In the meantime, you can create the firewall rule that will allow frontend MIG instances to receive traffic from the load balancer proxy:

- Name: allow-http-lbproxy-frontend.
  - Network: custom-mode-net.
  - Traffic direction: Entry.
  - Action in case of coincidence: Allow
  - Destinations: Specified destination tags, webapp-apache2-frontend.
  - Source Filter: IP Ranges, 130.211.0.0/22 and 35.191.0.0/16.
  - Protocols and ports: TCP:80.

Once deployed, carry out the following connectivity checks:

1. Connect to one of the frontend instances and try to connect with curl to the websites of the backend instances and to curl http://IP_LB_INTERNAL:80 and curl webapp-apache2-frontend.
2. Connect to the HTTP(S) load balancer&#39;s public IP from your browser (_note:_ make sure you use HTTP instead of HTTPS).
3. Check that you can access the Cloud Storage image behind the load balancer: http://IP_LB_EXTERNAL/static/image.jpg.

Thus, we have been able to deploy 2 groups of frontend instances in different regions and a global load balancer that grants access to them.

_DELIVERABLE:_ 

M2U4-task_2-file_1-screenshot_1.jpg: Screenshot showing the details page of the internal load balancer.

**Task 3: Check for high scalability and availability**

In this final task we will check the high scalability and availability of the groups of instances.

**High level of availability**

To check for high availability, let's turn off some VM instances and see how the managed instance group recreates them again:

1. Navigate to  **Compute Engine >Instances**.
2. Locate any of the instances in each instance group, select them, and delete them.
3. Navigate to  **Compute Engine > Instance Groups**  and see how the number of current and target instances of the groups do not match (e.g. "2/3").
4. Next to each group, an icon will show that the group&#39;s status is being refreshed, and after a few seconds it will show how the number of current and target instances matches again (e.g. "3/3").

**High scalability**

To test the high scalability of the clusters, we will test how traffic is redirected to the next closest region if the closest cluster cannot sustain as much load, or a target cluster will auto-scale in number of instances:

1. Connect via SSH to the internal-client instance.
2. Install the Apache2 web server, which will give us access to the ab: sudo apt update && sudo apt install -y apache2tool.
3. Locate and copy the external IP address of the HTTP(S) load balancer, LB_IP_EXTERNAL.
4. Runs a load test with ab: ab -n 500000 -c 1000 http://LB_IP_EXTERNAL, leave the command and SSH session running in the background.
5. Observe backend traffic by navigating to  **Network Services > Load Balancing** , clicking on the  **Backends**  tab and clicking on web-apache2-frontend-lb-webserver.

You will see how traffic is originally directed to the nearest frontend MIG, at europe-west1, and when it can't cope with any more load, some of it is redirected to the other frontend MIG at us-central1.

You can also check the status and number of instances in the europe-west1 MIG in  **Compute Engine > Instance Groups** , by clicking on that MIG.

_Note:_ If you need to force more load, you can re-execute the ab command by expanding the number of concurrent connections with -c N_CONNECTIONS.

_DELIVERABLE:_ 

M2U4-task_3-file_1-screenshot_1.jpg: Screenshot showing the traffic routing page of the load balancer backend service.

**Deliverables Summary**

1. M2U4-task_1-file_1-screenshot_1.jpg: Screenshot showing backend MIG detail page.
2. M2U4-task_1-file_2-screenshot_2.jpg: Screenshot showing the details page of the internal load balancer.
3. M2U4-task_2-file_1-screenshot_1.jpg: Screenshot showing the details page of the internal load balancer
4. M2U4-task_3-file_1-screenshot_1.jpg: Screenshot showing the traffic routing page of the load balancer backend service.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the disk image created.
2. Delete the instance groups created.
3. Delete the client instance.
4. Delete the instance template.
5. Remove the created network, load balancers and the reserved external IP.
