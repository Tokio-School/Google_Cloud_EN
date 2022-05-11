# **High Availability Architecture**

Unit M2U8 - Exercise 6

**What are we going to do?**

1. Deploy a hybrid cloud environment with high availability in a cold pattern, with deployment in the cloud in the event of any disaster.
2. Deploy an application simulating an on-premises infrastructure, configuring its backups and storing data in the cloud to prevent disasters.
3. In case of any problem at regional level affecting the application, deploy a high availability or disaster recovery environment from backups and replicated information.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: On-Premises Solution Deployment**

Deploy the following solution in your work environment:

_Note:_ If any technical requirements are not expressly described, use the default values, or choose the solution you deem most convenient.

We will use an internal Cloud DNS zone to simulate an external zone that redirects our public domain to the load balancer IP.

On-premises environment:

- Network:
  - Name: on-prem.
  - Subnet Creation Mode: Custom
  - Subnet in europe-west1
  - Global Routing
  - Private Google Access or &quot;Private Access to Google&quot; enabled
- Application:
  - CGE Managed Instance Group
  - Regional: europe-west1 (distributed across all areas)
  - Instance Type: e2-micro
  - Boot Disk: Custom Image, 20GB Balanced Persistent Disk
  - Self-scalable: 3-10 instances, CPU-based scaling (70%)
  - Health-check: TCP connection port 80
  - Custom image: Debian 11 running Apache2 web server on TCP:80
  - Individual service account for MIG
  - No external IPs
  - Automatic snapshots of the boot disks programmed hourly and stored in multi-region &quot;europe&quot;
- HTTPS load balancer balancing traffic to MIG:
  - Front with IPv4 reserved
  - HTTP protocol in TCP:80
  - Session Affinity on L7
- Cloud SQL:
  - MySQL 8.0
  - Region: europe-west1.
  - Type of machine: 1 vCPU, 1.7 GB memory
  - 100GB HDD
  - Internal connection, no external IP (_note:_ enable Private Service Access)
  - Backups stored in European multi-region
- Cloud IAP enabled to allow SSH tunnelling to instances without external IP
- Firewall rules:
  - Application: input from HTTPS LB allowed to TCP:80
  - SSH (TCP:22) from any source enabled to all instances
  - ICMP from internal subnet source enabled to all instances
  - Applied according to service accounts
- Cloud DNS:
  - Private area for on-prem network.
  - DNS Name: www.myhaapp.com.
  - Register: Type A to LB HTTS External IP

Technical Requirements:

- The application must be accessible from the load balancer.
- The application must be able to access Cloud SQL
- An internal VM instance in the on-prem VPC must be able to resolve the www.myhaapp.com URL to the MIG web application.

Data:

- Create some files on the disks of all instances to be included in the backups (e.g., in the initial image).
- Create a database and table and upload some sample data to Cloud SQL ([example](https://cloud.google.com/sql/docs/mysql/quickstart#create_a_database_and_upload_data)).

Backups:

- Make sure you have at least one backup of the disks and Cloud SQL before continuing.
- You can force them or create them manually if necessary.

**Task 2: Deploying cold HA/DR environment in the cloud**

Deploy the following solution in your work environment:

_Note:_ If any technical requirements are not expressly described, use the default values, or choose the solution you deem most convenient.

The VPN will simulate the connection between the on-premises and the cloud to synchronise backups and information.

Cloud environment:

- Network:
  - Name: cloud.
  - Subnet Creation Mode: Custom
  - Subnet in europe-west3
  - Global Routing
  - Private Google Access or &quot;Private Access to Google&quot; enabled
- VPN:
  - Connection tunnels connecting both VPCs to each other, on-prem and cloud
  - HA VPN
  - Cloud Router Based Routing
- Cloud IAP enabled to allow SSH tunnelling to instances without external IP
- Firewall rules:
  - Application: input from HTTPS LB allowed to TCP:80
  - SSH (TCP:22) from any source enabled to all instances
  - ICMP from internal subnet source enabled to all instances
  - Applied according to service accounts
- Cloud DNS:
  - Update the internal zone so that it also has visibility from the cloudVPC.

Technical Requirements:

- Create an internal VM instance in the cloud network and verify connectivity to and from both environments.
- An internal VM instance in the on-prem VPC must be able to resolve the URL www.myhaapp.com to the &quot;on-prem&quot; MIG web application via the LB&#39;s public IP.

**Task 3: High Availability Recovery**

Simulate the recreation of the cloud environment in the VPC cloud:

1. Deploy another MIG in europe-west3 by creating an instance template from the last available snapshot.
2. Recreate the Cloud SQL instance in europe-west3 from the backup of the initial instance.
3. Create a new load balancer for the cloud MIG.
4. Substitute the DNS resolution to the external IP of the new &quot;cloud&quot; LB.
5. Verify that the new environment works correctly, with the LB pointing to the instances, the instances and DB with the corresponding information and access to Cloud SQL.

**Deliverables Summary**

Screenshots showing the listing or details of each item created and the intended connections:

- On-prem network details
- Listing of firewall rules
- On-prem instance group details
- Load balancer on-prem details
- Resolution from the internal instance to www.myhaapp.com, showing the IP of the on-prem LB
- Access from instances to Cloud SQL.
- Cloud network details
- Cloud instance group details
- Cloud load balancer details
- Resolution from the internal instance to www.myhaapp.com, showing the IP of the cloud LB

_Note:_ Follow the same numbering proposed up to now: &quot;M2U[Unit #]-[Exercise #] -task\_[Task #] -file\_[File # on Task]-[File Type: Screenshot, Export, Script]\_[File # if more than 1 of the same type]&quot;, e.g. &quot;M2U8-1-task\_1-file\_1-screenshot\_1.jpg&quot;.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete all resources created, making sure not to leave any that may continue to incur costs or lead to an error in another exercise.
