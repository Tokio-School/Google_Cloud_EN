# **Disaster Recovery**

Unit M2U8 – Exercise 7

**What are we going to do?**

1. Deploy a disaster recovery environment in a temperate pattern.
2. Deploy a primary environment in one region and recover it from any disaster in another region.
3. Automate recovery with a script.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Solution Deployment**

Deploy the following solution in your work environment:

_Note:_ If any technical requirements are not expressly described, use the default values or choose the solution you deem most convenient.

Our main region will be europe-west1 and the backup region will be europe-west3.

- Network:
  - Subnet Creation Mode: Custom
  - Subnets in europe-west1 and europe-west3
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
  - Individual service account for MIG, with read/write access to Cloud Storage
  - No external IPs
  - Automatic snapshots of the boot disks programmed hourly and stored in multi-region &quot;europe&quot;
- HTTPS load balancer balancing traffic to MIG:
  - Front with IPv4 and IPv6 reserved
  - HTTP protocol in TCP:80
  - Session Affinity on L7
- Cloud SQL:
  - MySQL 8.0
  - Region: europe-west1.
  - Type of machine: 1 vCPU, 1.7 GB memory
  - 100GB HDD
  - Internal connection, no external IP (_note:_ enable Private Service Access)
  - High Availability Configuration
  - Backups stored in European multi-region
  - Read replica in europe-west3
- Cloud storage:
  - Multi-regional bucket in Europe
  - Standard Storage Class
- Cloud IAP enabled to allow SSH tunneling to instances without external IP
- Firewall rules:
  - Application: input from HTTPS LB allowed to TCP:80
  - SSH (TCP:22) from any source enabled to all instances
  - ICMP from internal subnet source enabled to all instances
  - Applied according to service accounts

Technical Requirements:

- The application must be accessible from the load balancer.
- The application must be able to access Cloud SQL and Cloud Storage.

Data:

- Create some files on the disks of all instances to be included in the backups (e.g. in the initial image).
- Create a database and table and upload some sample data to Cloud SQL ([example](https://cloud.google.com/sql/docs/mysql/quickstart#create_a_database_and_upload_data)).

Backups:

- Make sure you have at least one backup of the disks and Cloud SQL before continuing.
- You can force them or create them manually if necessary.

**Task 2: Disaster Recovery**

Simulate the recreation of the environment in another region from the snapshots of the disks and the read replica of the DB:

1. Create a new instance template from the last available disk snapshot.
2. Recreate the MIG in europe-west3 using the new instance template.
3. Upgrade the Cloud SQL read replica to master, configure high availability and create a read replica in europe-west1.
4. Replace the backend in europe-west1 of the LB with a new backend with the MIG of europe-west3.
5. Verify that the new environment works correctly, with the LB pointing to the instances, the instances and DB with the corresponding information and access to Cloud SQL and Cloud Storage.

**Deliverables Summary**

Screenshots showing the listing or details of each item created and the intended connections:

- Details of the network and subnets.
- Listing of firewall rules.
- Details of the group of europe-west1 instances.
- Details of the Cloud SQL instance in europe-west1
- Details of the load balancer with the initial configuration.
- Access to the web application through the external IP of the LB.
- Access from instances to Cloud SQL.
- Access from instances to Cloud Storage.
- Same information after recovery to backup region.

_Note:_ Follow the same numbering proposed up to now: &quot;M2U[Unit #]-[Exercise #] -task\_[Task #] -file\_[File # on Task]-[File Type: Screenshot, Export, Script]\_[File # if more than 1 of the same type]&quot;, e.g. &quot;M2U8-1-task\_1-file\_1-screenshot\_1.jpg&quot;.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete all resources created, making sure not to leave any that may continue to incur costs or lead to an error in another exercise.
