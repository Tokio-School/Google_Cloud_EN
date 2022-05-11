# **Reference Architecture 1**

Unit M2U0 - Exercise 1

**What are we going to do?**

1. Deploy a three-layer web application.

This exercise is a challenge exercise, where you will have to deploy the described architecture without associated step-by-step instructions. To complete it, we recommend that you draw from what you have seen in other exercises, the official documentation, or any other online resource (tutorials, StackOverflow, videos, etc.).

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called &quot;M2U0-0-questions.txt&quot; before starting the exercise. Remember to identify and sort each question followed by your answer.

_Note: There are no questions for this exercise._

**Single task**

Deploy the following solution in your work environment:

_Note:_ If any technical requirements are not expressly described, use the default values, or choose the solution you deem most convenient.

- Network:
  - Subnet Creation Mode: Custom
  - 1 subnet in europe-west1
  - Global Routing
  - Private Google Access or &quot;Private Access to Google&quot; enabled
- Frontend:
  - CGE Managed Instance Group
  - Regional: europe-west1 (distributed across all areas)
  - Instance Type: e2-micro
  - Boot Disk: Custom Image, 20GB Balanced Persistent Disk
  - Self-scalable: 3-10 instances, CPU-based scaling (70%)
  - Health-check: TCP connection port 80
  - Custom image: Debian 11 running Apache2 web server on TCP:80
  - Individual service account for MIG
  - No external IPs
- HTTPS load balancer balancing traffic to the frontend MIG:
  - Front with IPv4 and IPv6 reserved
  - HTTP protocol in TCP:80
  - Session Affinity on L7
- Backend:
  - CGE Managed Instance Group
  - Regional: europe-west1 (distributed across all areas)
  - Type of Instances: e2-small
  - Boot Disk: Custom Image, 20GB Balanced Persistent Disk
  - Self-scalable: 3-10 instances, CPU-based scaling (70%)
  - Health-check: TCP connection port 80
  - Custom image: Debian 11 running Apache2 web server on TCP:80, mysql client and Cloud SQL Auth proxy
  - Individual service account for MIG
  - No external IPs
- Regional internal TCP load balancer balancing traffic to the backend MIG:
  - Session Affinity (5 Element Tuple)
  - Global Access
- Cloud IAP enabled to allow SSH tunnelling to instances without external IP
- Firewall rules:
  - Frontend: input from HTTPS LB allowed to TCP:80
  - Backend: input from frontends allowed to TCP:80
  - Egress traffic to DB blocked except from backend (TCP:3306) (_note: use priorities from different firewall rules_)
  - SSH (TCP:22) from any source enabled to all instances
  - ICMP from internal subnet source enabled to all instances
  - Applied according to service accounts
  - Disable all other traffic
- Cloud SQL:
  - MySQL 8.0
  - Location: europe-west1-b
  - Type of machine: 1 vCPU, 1.7 GB memory
  - 100GB HDD
  - Internal connection, no external IP (_note:_ enable Private Service Access)
  - High Availability Configuration
  - Backups and maintenance windows enabled

Technical Requirements:

- Frontends must be accessible from the load balancer
- Backends must be accessible from the frontend via their load balancer
- The database must only be accessible from the backend.

**Deliverables Summary**

Screenshots showing the listing or details of each item created and the intended connections:

- Network details and list of subnets
- Listing of firewall rules
- Detail of each instance group
- Detail of each load balancer
- Cloud SQL Instance Details
- External connection to LB HTTPS
- Connection from frontend to internal LB/backend
- Connection from backend to DB (MySQL terminal)

_Note:_ Follow the same numbering proposed up to now: &quot;M2U[Unit #]-[Exercise #] -task\_[Task #] -file\_[File # on Task]-[File Type: Screenshot, Export, Script]\_[File # if more than 1 of the same type]&quot;, e.g. &quot;M2U8-1-task\_1-file\_1-screenshot\_1.jpg&quot;.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete all resources created, making sure not to leave any that may continue to incur costs or lead to an error in another exercise.
