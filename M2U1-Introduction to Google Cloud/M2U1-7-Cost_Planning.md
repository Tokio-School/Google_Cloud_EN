# **Cost planning**

Unit M2U1 - Exercise 7

**What are we going to do?**

1. Plan Google Cloud costs and make sample budgets.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Cost Calculator and Price List**

For the first task, let's explore some price lists for Google Cloud services and the cost calculator.

**Price list**

In the documentation of each service we can find the information on the costs of the same, along with all the necessary details and several examples of use cases with the detail and total of the associated costs.

You can find the costs at the end of the documentation page for each service (you can search for it in a web search engine) or in the general [price list](https://cloud.google.com/pricing/list).

Search and analyse the detailed costs and examples for the following services:

- Compute Engine - VM Instances
- Cloud storage
- Cloud Run
- Cloud Firestore

**Google Cloud Cost Calculator**

The Google Cloud Cost Calculator is the ideal tool to generally explore the costs of a solution or to make detailed budgets for a proposal.

Access it: [Cost Calculator](https://cloud.google.com/products/calculator).

1. Analyse the cost calculator, and familiarise yourself with the different sections.
2. Explore the different services, and try adding some VM instances (and disks) e.g.
3. Change the results to show prices in €.
4. Modify and delete resources to see how to modify a budget.
5. At the end, save the budget and/or send it by email. When the budget is created, it generates a URL with a unique budget ID.

**Task 2: Sample budgets**

In this task you will create 3 sample budgets using the cost calculator. Remember to create a different quote for each of them (removing the budget ID from the URL to create a new one if necessary).

**Budget 1 - simple environment**

Create the following budget and save your URL:

1. Region: europe-west1.
2. 3 instances of e2-standard-8 Ubuntu VM.
  1. No external IPs.
  2. Running 24/7.
  3. 200 GB zonal balanced disk per instance.
  4. 200 GB multiregional snapshot per instance.
3. 1 TB of internet download/egress traffic to Europe monthly.
4. Load balancer HTTPS:
  1. 1 forwarding rule.
  2. 200 GB processed.
5. Cloud storage:
  1. 1 TB hosted in regionally standard.
  2. 10M of Class A operations per month.
  3. 50M of Class B operations per month.
  4. No traffic outside the region.
6. Cloud NAT:
  1. 1 NAT gateway.
  2. 10 GB processed.

_DELIVERABLES_: M2U1-7-budgets.txt: Add budget URL and total cost.

**Budget 2 - complex environment**

Create the following budget and save your URL:

1. 12 instances of e2-standard-4 Ubuntu Pro VM:
  1. No external IPs.
  2. Running 24/7.
  3. 200 GB zonal balanced disk per instance.
  4. 200 GB multiregional snapshot per instance.
  5. 6 instances in europe-west1 and 6 in europe-west3.
2. 6 instances of VM e2-standard-8 Red Hat Enterprise Linux:
  1. No external IPs.
  2. Running 24/7.
  3. 300 GB zonal SSD per instance.
  4. 300 GB multiregional snapshot per instance.
  5. 3 instances in europe-west1 and 3 in europe-west3.
3. 4 instances of e2-standard-16 Windows Server VM:
  1. No external IPs.
  2. Running 24/7.
  3. 500 GB zonal SSD per instance.
  4. 400 GB multiregional snapshot per instance.
  5. Instances in europe-west3.
4. Standard GKE cluster:
  1. Region: europe-west1 (regional cluster).
  2. 6 e2-standard-8 instances.
  3. Running 24/7.
  4. 200 GB of balanced disk per instance.
  5. 100 GB multiregional snapshot per instance.
5. Download/egress resource-to-resource traffic:
  1. 24 TB of traffic between regions in Europe.
6. Internet download/egress traffic:
  1. 25 TB of traffic to Europe.
  2. 12 TB of traffic to North America.
  3. 24 TB of traffic to South America.
7. Load balancer HTTPS:
  1. 6 forwarding rules.
  2. 20 TB processed.
8. Cloud storage:
  1. 10 TB hosted in standard multi-regionally across Europe.
  2. 20 TB hosted coldline multi-regionally in Europe.
  3. 100M of Class A operations per month.
  4. 500M of Class B operations per month.
  5. 10TB of traffic to VM instances.
9. Cloud NAT:
  1. 3 NAT gateways.
  2. 50 GB processed.
10. Cloud Functions:
  1. Memory: 256 MB.
  2. Running time: 250 ms.
  3. Bandwidth: 100 MB.
  4. Number of invocations: 500,000.
11. Cloud SQL:
  1. 1 MySQL db-standard-4 instance.
  2. 500 GB of SSD storage.
  3. 1TB backup.
  4. Running 24/7.
  5. Region: europe-west1.
12. BigQuery (on demand):
  1. Region: multi-regional, European Union.
  2. 500 GB of active storage.
  3. 2TB of long-term storage.
  4. Data processed: 15 TB.

_DELIVERABLES_: M2U1-7-budgets.txt: Add budget URL and total cost.

**Budget 3 - enterprise environment**

Create the following budget and save your URL:

1. 32 instances of e2-standard-8 Ubuntu Pro VM:
  1. No external IPs.
  2. Running 24/7.
  3. 250 GB zonal balanced disk per instance.
  4. 250 GB multiregional snapshot per instance.
  5. 6 instances in europe-west1, 5 in europe-west3, 6 in us-east2, 4 in us-east1, 4 in us-west1, 7 in us-west2.
2. 20 instances of VM e2-standard-8 Red Hat Enterprise Linux:
  1. No external IPs.
  2. Running 24/7.
  3. 300 GB zonal SSD per instance.
  4. 300 GB multiregional snapshot per instance.
  5. 4 instances in europe-west1, 4 in europe-west3, 4 in us-east2, 4 in us-west1, 4 in asia-east1.
3. 15 instances of e2-standard-16 Windows Server VM:
  1. No external IPs.
  2. Running 24/7.
  3. 500 GB zonal SSD per instance.
  4. 500 GB multiregional snapshot per instance.
  5. 3 instances in europe-west1, 3 in europe-west3, 3 in us-east2, 3 in us-west1, 3 in asia-east1.
4. Standard GKE clusters:
  1. Region: europe-west1 (regional cluster):
    1. 6 e2-standard-8 instances.
    2. Running 24/7.
    3. 250 GB of balanced disk per instance.
    4. 150 GB multiregional snapshot per instance.
  2. Region: us-central1 (regional cluster):
    1. 5 e2-standard-8 instances.
    2. Running 24/7.
    3. 250 GB of balanced disk per instance.
    4. 150 GB multiregional snapshot per instance.
  3. Region: asia-east1 (regional cluster):
    1. 4 e2-standard-8 instances.
    2. Running 24/7.
    3. 250 GB of balanced disk per instance.
    4. 150 GB multiregional snapshot per instance.
5. Download/egress resource-to-resource traffic:
  1. 50 TB of traffic between regions in Europe.
  2. 24TB of inter-regional traffic in the US
  3. 20 TB of inter-regional traffic in Asia.
  4. 15 TB of traffic between Europe and the US
  5. 12 TB of traffic between Europe and Asia.
  6. 10 TB of traffic between the US and Asia.
6. Internet download/egress traffic:
  1. 40 TB of traffic to Europe.
  2. 21 TB of traffic to North America.
  3. 16 TB of traffic to Asia.
7. Load balancer HTTPS:
  1. 12 forwarding rules.
  2. 60 TB processed.
8. Cloud storage:
  1. 50 TB hosted in standard multi-regionally across Europe.
  2. 100 TB hosted in nearline multi-regionally in Europe.
  3. 300 TB hosted coldline multi-regionally in Europe.
  4. 500 TB hosted in archive multi-regionally in Europe.
  5. 500M of Class A operations per month.
  6. 1000M of Class B operations per month.
  7. 25TB of traffic to VM instances in Europe.
  8. 20TB of traffic to VM instances in the US
  9. 12TB of traffic to VM instances in Asia.
9. Cloud NAT:
  1. 5 NAT gateways.
  2. 2 TB processed.
10. Cloud Functions:
  1. Memory: 512 MB.
  2. Running time: 300 ms.
  3. Bandwidth: 150 MB.
  4. No. of invocations: 10M.
11. Cloud SQL:
  1. Region: europe-west1.
    1. 1 MySQL db-highmem-8 instance.
    2. 2TB SSD storage.
    3. 3TB backup.
    4. Running 24/7.
  2. Region: us-central1:
    1. 1 MySQL db-highmem-4 instance.
    2. 1 TB of SSD storage
    3. 2TB backup.
    4. Running 24/7.
  3. Region: asia-east1:
    1. 1 MySQL db-standard-4 instance.
    2. 500 GB of SSD storage.
    3. 1TB backup.
    4. Running 24/7.
12. BigQuery (on demand):
  1. Region: multi-regional, European Union:
    1. 1 TB of active storage.
    2. 6 TB of long-term storage.
    3. Data processed: 24 TB.
  2. Region: multi-regional, USA:
    1. 500 GB of active storage.
    2. 3 TB of long-term storage.
    3. Data processed: 12 TB.
13. Cloud Pub/Sub:
  1. Monthly message volume: 10 GB.
  2. Backlog: 50 GB.
14. BigTable:
  1. Region: europe-west1.
  2. 4 nodes.
  3. 400 GB of SSD storage.
  4. Running 24/7.
15. Cloud Memorystore for Redis:
  1. Region: europe-west1.
  2. Tier: Standard.
  3. 100 GB storage
16. Cloud Filestore:
  1. Region: europe-west1.
  2. 250 GB of premium tier storage.

_DELIVERABLES_: M2U1-7-budgets.txt: Add budget URL and total cost.

**Deliverables Summary**

1. _DELIVERABLES_: M2U1-7-budgets.txt: Add the URLs and total cost of the 3 budgets.

**Clean up resources**

_There are no resources to delete after this exercise._
