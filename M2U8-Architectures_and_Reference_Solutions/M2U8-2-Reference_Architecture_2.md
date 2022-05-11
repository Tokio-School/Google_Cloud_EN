# **Reference Architecture 2**

Unit M2U8 - Exercise 2

**What are we going to do?**

1. Deploy an internal enterprise application.

This exercise is a challenge exercise, where you will have to deploy the described architecture without associated step-by-step instructions. To complete it, we recommend that you draw from what you have seen in other exercises, the official documentation, or any other online resource (tutorials, StackOverflow, videos, etc.).

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Single task**

Deploy the following solution in your work environment:

_Note:_ If any technical requirements are not expressly described, use the default values or choose the solution you deem most convenient.

- Network:
  - Subnet Creation Mode: Custom
  - 1 subnet in europe-west1
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
  - Individual service account for MIG, with read/write object access to Cloud Storage
  - No external IPs
- Cloud Filestore:
  - NFS mounted on application instances in R/W mode
  - Instance Tier: Basic
  - Storage: 1TB HDD
  - Connection by Private Services Access (via internal IPs)
  - Shared filename: main\_volume
- Regional internal TCP load balancer balancing traffic to the MIG application:
  - Session Affinity (5 Element Tuple)
  - Global Access
- Cloud NAT:
  - Access to subnetwork in europe-west1
  - Automatic NAT IP addresses
- Cloud storage:
  - Multi-regional bucket in Europe
  - Standard Default Storage Class
  - Detailed access control
- Cloud IAP enabled to allow SSH tunnelling to instances without external IP
- Firewall rules:
  - Application: input from TCP LB allowed to TCP:80
  - SSH (TCP:22) from any source enabled to all instances
  - ICMP from internal subnet source enabled to all instances
  - Applied according to service accounts
  - Disable all other traffic
- Internal VM instance (without external IP) to check access to internal LB

Technical Requirements:

- The application must be accessible from the internal LB
- The application must be able to access Cloud Storage.
- The internal VM instance must be able to access the internal LB

**Deliverables Summary**

Screenshots showing the listing or details of each item created and the intended connections:

- Network details and list of subnets
- Listing of firewall rules
- Instance Group Detail
- Load Balancer Detail
- Cloud NAT Details
- Cloud Storage Bucket Details
- Connection from internal instance to internal LB
- Connecting from an internal MIG instance to an external destination via Cloud NAT (e.g. www.gooogle.com)
- Read and write access from instances to the Cloud Storage bucket
- Read and write access of instances to shared NFS (_note:_ to display the script, you can use the echo command &quot;hello world&quot; \&gt; path/a/montage/nfs/filename.txt)

_Note:_ Follow the same numbering proposed up to now: &quot;M2U[Unit #]-[Exercise #] -task\_[Task #] -file\_[File # on Task]-[File Type: Screenshot, Export, Script]\_[File # if more than 1 of the same type]&quot;, e.g. &quot;M2U8-1-task\_1-file\_1-screenshot\_1.jpg&quot;.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete all resources created, making sure not to leave any that may continue to incur costs or lead to an error in another exercise.
