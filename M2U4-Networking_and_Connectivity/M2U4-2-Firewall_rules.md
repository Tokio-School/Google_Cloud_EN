# **Firewall rules**

Unit M2U4 - Exercise 2

**What are we going to do?**

1. Assign firewall rules with network tags and according to service accounts.
2. Check the order of priority of application of firewall rules.
3. See how to debug connectivity issues due to firewall rules.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U4-2-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Creating the network topology**

In this first task we will create the network topology and VM instances needed to work with firewall rules in the following tasks.

**Create the network and subnets**

Let&#39;s create a VPC network with 2 subnets. We create a custom network as a standard best practice in any project and to allow us to control the firewall rules without having to delete the default network rules.

Create a VPC network with the following features:

- Name: custom-mode-network.
- Subnet Creation Mode: Custom.

1. Subnet 1:
  - Name: custom-mode-network-subnet-euw1.
  - Region: europe-west1.
  - IP Address Range: 10.10.0.0/20
2. Subnet 2:
  - Name: custom-mode-network-subnet-euw3.
  - Region: europe-west3.
  - IP Address Range: 10.10.0.0/20

**Create VM Instances**

Now let's create the VM instances in those subnets.

First create 2 custom service accounts:

1. Service account 1: sa-frontend.
2. Service account 2: sa-backend. Don't worry about the access or roles assigned to them.

Create the following VM instances:

1. Name: frontend-euw1.
  - Region: europe-west1.
  - Type of machine: e2-micro.
  - Service account: sa-frontend.
  - Startup script: sudo apt update &amp;&amp; sudo apt install -y apache2
2. Name: backend-euw1.
  - Region: europe-west1.
  - Type of machine: e2-micro.
  - Service account: sa-backend.
  - Startup script: sudo apt update &amp;&amp; sudo apt install -y apache2
  - No external IP address.
3. Name: frontend-euw3.
  - Region: europe-west3.
  - Type of machine: e2-micro.
  - Service account: sa-frontend.
  - Startup script: sudo apt update &amp;&amp; sudo apt install -y apache2
4. Name: backend-euw3.
  - Region: europe-west3.
  - Type of machine: e2-micro.
  - Service account: sa-backend.
  - Startup script: sudo apt update &amp;&amp; sudo apt install -y apache2
  - No external IP address.
5. Name: internal-client.
  - Region: europe-west1.
  - Type of machine: e2-micro.
  - Service Account: Compute Engine&#39;s default service account.

Once the network topology and VM instances have been created, we can explore how to apply firewall rules.

**Task 2: Assigning Firewall Rules**

Now we're going to assign different firewall rules to some or all of the instances, and we're going to check for permitted or blocked traffic.

The final topology of permitted connections will be as follows:

- VM instances have been deployed in frontend-backend pairs in 2 different regions.
- Only external connection to frontend instances is allowed

**Assign firewall rules to all instances**

We will assign minimum firewall rules to all instances that allow SSH and ping connections:

1. In the console, navigate to  **VPC Network > Firewall**  and verify that there is no firewall rule for the custom-mode-network.
2. Select  **Create Firewall Rule** , explore all options, and create the following firewall rules:
  1. Name: allow-ssh-ping-internet-all.
    - Network: custom-mode-network.
    - Priority: 50000.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: All instances of the network.
    - Source Filter: IPv4 Ranges, Source IPv4 Ranges: 0.0.0.0/0
    - Protocols and ports: TCP:22.
3. Once created, navigate to  **Compute Engine > VM Instances**  and verify that you can open an SSH session to VM instances.

**Check the implicit firewall rules**

Let's check the 2 implicit firewall rules: all incoming traffic ("ingress") is blocked, all outgoing traffic ("egress") is allowed:

1. Connect via SSH to the internal-client client instance.
2. Make sure you can ping to external destinations: ping -c 3 www.google.com and ping -c 3 8.8.8.8.
  - External pings are possible because all outgoing traffic is allowed.
3. Check that you can't ping the other instances on the same network: ping -c 3 INSTANCE_NAME or ping -c 3 INSTANCE_INTERNAL_IP.
  - Pings to such instances are not possible because all incoming traffic to them is blocked.
4. Now connect via SSH to any other VM instance, to which you have verified that a ping is not permitted.
5. Make sure that pinging externally is still possible: ping -c 3 www.google.com and ping -c 3 8.8.8.8.
  - Again, external pings are possible because all outgoing traffic is allowed.
6. Now go back to  **VPC Network > Firewall**  and create another firewall rule:
  - Name: allow-ping-internet-all.
  - Network: custom-mode-network.
  - Priority: 50000.
  - Traffic direction: Entry.
  - Action in case of coincidence: Allow.
  - Destinations: All instances of the network.
  - Source Filter: IPv4 Ranges, Source IPv4 Ranges: 0.0.0.0/0
  - Protocols and ports: ICMP.
7. Reconnect via SSH to the internal-client client instance and check that you can now ping all other instances.

This allows us to verify the application of the 2 implicit firewall rules to all networks, which have the lowest priority.

_DELIVERABLE:_ 

M2U4-2-task_2-file_1-screenshot_1.jpg: Screenshot showing the ping command and its result from the client instance.

**Assign firewall rules with network tags**

Now we are going to assign firewall rules to the instances to allow traffic to their websites, according to the planned connection topology.

First check that it is not possible to connect from the internal-client client instance to the different websites with curl http://INSTANCE_IP:80.

Give each VM instance the corresponding network tags (&quot;network tag&quot; or &quot;tag&quot;):

1. Navigate to  **Compute Engine > VM Instances**.
2. To modify the network tags for a VM instance, click the instance to go to its details page, then click  **Edit** , and under  **Network Tools** , add the list of network tags by separating them with a space or comma.
3. Add the following network tags to each instance:
  - Instance: frontend-euw1, network tags: frontend.
  - Instance: backend-euw1, network tags: backend.
  - Instance: frontend-euw3, network tags: frontend.
  - Instance: backend-euw3, network tags: backend.

Now create the following firewall rules by assigning them network tags:

1. Name: allow-http-internet-frontend:
  - Network: custom-mode-network.
  - Priority: 1000.
  - Traffic direction: Entry.
  - Action in case of coincidence: Allow.
  - Destinations: Specified destination tags.
  - Destination tags: frontend.
  - Source Filter: IPv4 Ranges, Source IPv4 Ranges: 0.0.0.0/0
  - Protocols and ports: TCP:80.
2. Name: allow-http-frontend-backend:
  - Network: custom-mode-network.
  - Priority: 1000.
  - Traffic direction: Entry.
  - Action in case of coincidence: Allow.
  - Destinations: Specified destination tags.
  - Destination tags: backend.
  - Source filter: Source tags, source tags: frontend.
  - Protocols and ports: TCP:80.

Now perform the following HTTP web connectivity checks with curl http://INSTANCE_IP:80 (_note:_ use the external frontend IPs and the internal backend IPs):

1. Verify that the internal-client client instance can connect to the frontend instances.
2. Verify that the internal-client client instance cannot connect to the backend instances.
3. Verify that frontend instances can connect to backends.

This is a simple way to assign different firewall rules using a matching network tag between the firewall rule and the VM instance.

_DELIVERABLE:_ 

M2U4-2-task_2-file_2-screenshot_2.jpg: Screenshot showing list of firewall rules.

However, as mentioned above, this is not the best practice, as anyone who can manage a VM instance can modify its network tags, or an error may occur when entering the tags.

To check:

1. Navigate to one of the backend instances and edit it.
2. Add an error in your network tag, e.g. by switching backend to backends.
3. See how the frontend cannot connect to that instance now.

For these reasons, the best practice is to assign firewall rules using service accounts.

_QUESTION: What would be the easiest and safest way to assign a firewall rule to an instance to only allow traffic from your subnet?_

**Assign firewall rules with service accounts**

Next we will map the firewall rules using the VM instances service accounts.

1. Delete all firewall rules created for the custom-mode-network.
2. Create the following firewall rules:
3. Name: allow-http-internet-frontend:
  - Network: custom-mode-network.
  - Priority: 1000.
  - Traffic direction: Entry.
  - Action in case of coincidence: Allow.
  - Destinations: Service account specified.
  - Target Service Account: sa-frontend.
  - Source Filter: IPv4 Ranges, Source IPv4 Ranges: 0.0.0.0/0
  - Protocols and ports: TCP:80.
4. Name: allow-http-frontend-backend:
  - Network: custom-mode-network.
  - Priority: 1000.
  - Traffic direction: Entry.
  - Action in case of coincidence: Allow.
  - Destinations: Service account specified.
  - Target Service Account: sa-backend.
  - Source filter: Service account, service account source: sa-frontend.
  - Protocols and ports: TCP:80.

Perform the following HTTP web connectivity checks again with curl http://INSTANCE_IP:80 (_note:_ use the external frontend IPs and the internal backend IPs):

1. Verify that the internal-client client instance can connect to the frontend instances.
2. Verify that the internal-client client instance cannot connect to the backend instances.
3. Verify that frontend instances can connect to backends.

_DELIVERABLE:_ 

M2U4-2-task_2-file_3-screenshot_3.jpg: Screenshot showing list of firewall rules.

Finally, try modifying the service account of a VM instance.

_QUESTION: In view of possible errors, for which 3 reasons is it safer to assign firewall rules using service accounts ? NOTE:_ If you have any questions, you can check the [documentation](https://cloud.google.com/vpc/docs/firewalls#service-accounts-vs-tags).

**Task 3: Troubleshooting connectivity with firewall rules**

In this last task we will explore different ways to resolve connectivity issues related to firewall rules.

Start by creating a new firewall rule that again blocks all external traffic to all instances:

- Name: block-all-all-all:
  - Network: custom-mode-network.
  - Priority: 1.
  - Traffic direction: Entry.
  - Action in case of a match: Block.
  - Destinations: All instances of the network.
  - Source Filter: IPv4 Ranges, Source IPv4 Ranges: 0.0.0.0/0
  - Protocols and ports: All.

Check that you can't ping any of the instances from 1internal-client1.

**See what rules apply to the instance**

Now let's see why you can&#39;t make that connectivity.

1. In  **Compute Engine > VM Instances** , click on one of the frontend instances.
2. On the details page, find its nic0 network interface and click on it.
3. On the next connectivity details page, study all the relevant information.

_QUESTIONS:_

1. _What firewall rules apply to that VM instance?_
2. _In the inbound/"ingress" connectivity analysis, which ports and protocols are blocked and by what firewall rule?_

**Enable logs for firewall rules**

Now let's enable logs for firewall rules to check if a rule is allowing or blocking a traffic type:

1. Under  **VPC Network > Firewall** , select the block-all-all-all firewall rule and select  **Edit**.
2. Select  **Check the priority of other firewall rules**  and see if they have higher or lower priority than the rule that ICMP traffic allows.
3. Enable logs.

Now go back to the internal-client SSH session and double-check that you can&#39;t ping or access the instances' website frontend.

1. Navigate to  **Cloud Logging**.
2. In the top section (with a blue button to the right of  **Run query** ), type in a search by clicking on the drop-down menus of  **Resource**  and  **Log name** :
  - Resource: Subnet > Subnetwork_ID: custom-mode-net-subnet-euw1 > Subnetwork_name: custom-mode-net-subnet-euw1.
  - Log name: Compute Engine > compute.googleapis.com/firewall.
3. Run the query and review the logs.

This way we can see how a firewall rule is applied.

_DELIVERABLE:_ 

M2U4-2-task_3-file_1-screenshot_1.jpg: Screenshot showing logs in Cloud Logging.

Finally, delete the block-all-all-all firewall rule.

**Connectivity tests**

Connectivity testing is available at  **Network Intelligence > Connectivity Testing**.

Create a test for each of the following connectivities:

1. From internal-client to one of the frontends for the ICMP protocol.
2. From internal-client to one of the frontends for the TCP port:80.
3. From one of the frontends to one of the backends for the TCP port:80.
4. From one of the backends to one of the frontends for the ICMP protocol.
5. From one of the backends to one of the frontends for the TCP port:80.
6. From internal-client to one of the frontends for the TCP port:22.
7. From internal-client to one of the backends for the TCP port:22.

This allows us to use the network intelligence centre&#39;s connectivity tests to check that connectivity remains as expected at all times.

_DELIVERABLE:_ 

M2U4-2-task_3-file_2-screenshot_2.jpg: Screenshot showing the connectivity tests created.

**Disable firewall rules**

Finally, check how by disabling firewall rules we can debug our connectivity or disable the application of a rule for a moment for any administration operation, without deleting the rule and having to recreate it later:

1. In the console, navigate to  **VPC Network > Firewall**.
2. Click the allow-http-frontend-backend firewall rule, and then click **Edit**.
3. Disable the firewall rule.
4. Re-run the connectivity test from one of the frontend to one of the backends for TCP port:80 and verify that such a connection can no longer be established.

This allows us to solve different connectivity problems between our instances.

**Deliverables Summary**

1. M2U4-2-questions.txt: Answers to all questions raised in the exercise.
2. M2U4-2-task_2-file_1-screenshot_1.jpg: Screenshot showing the ping command and its result from the client instance.
3. M2U4-2-task_2-file_2-screenshot_2.jpg: Screenshot showing list of firewall rules.
4. M2U4-2-task_2-file_3-screenshot_3.jpg: Screenshot showing list of firewall rules.
5. M2U4-2-task_3-file_1-screenshot_1.jpg: Screenshot showing logs in Cloud Logging.
6. M2U4-2-task_3-file_2-screenshot_2.jpg: Screenshot showing the connectivity tests created.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instances created.
2. Delete the service accounts created.
3. Remove networks, which will also remove subnets and firewall rules created.
