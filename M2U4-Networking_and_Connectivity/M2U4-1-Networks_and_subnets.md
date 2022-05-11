# **Networks and subnets**

Unit M2U4 - Exercise 1

**What are we going to do?**

1. Explore the possibilities of networking and subnets.
2. Check the connectivity between regions.
3. Check the connectivity between resources in the same network and in different networks.
4. Modify addressing ranges and create alias IP ranges.
5. Analyse the subnet traffic logs.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called &quot;M2U4-1-questions.txt&quot; before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Creation of networks and subnets**

In this first task we will explore the creation of networks and subnets, and the different network modes available.

**Analyse the &quot;default&quot; network**

Let&#39;s start by analysing the default network:

1. In the console, navigate to  **VPC Network > VPC Networks**.
2. Locate the default network, which is created by default when creating each new project.
3. _QUESTIONS:_
  1. _How many subnets does it have?_
  2. _Are any available regions missing, or repeated?_
  3. _What is the location of the network?_
  4. _What is the IP address range of the network?_
  5. _What is the scope of the IP address ranges of the subnets?_
  6. _Is there any IP address overlap between subnets?_
  7. _What is the &quot;mode&quot; of the network?_
4. Navigate to  **VPC Network > Firewall**.
  1. _QUESTION: Analysing the firewall rules applied, do you think it is a secure network that can be used in production?_

**Create a network in automatic mode**

Now let's create an automatic mode network:

1. Navigate to  **VPC Network >VCP Networks**  and click  **Create VPC Network**.
2. Use auto-mode-net as the VPC network name.
3. Select Automatic as the **Subnet Creation Mode**  and look at the regions and IP ranges used. We call it Automatic mode because a subnet is automatically created for each region.
4. Under  **Firewall rules** , analyse the rules available for import, which will be the same as several of those available on the default network:
  1. Look at the last two. These are the 2 implicit firewall rules applied to the entire network with minimum priority.
  2. Select to import all rules.
5. In  **Dynamic Routing Mode** , look at the description and choose Global.
6. Create the VPC network.

It will take about 2-3 minutes to create the VPC network.

This is a quick and easy way to create a new VPC network with automatically available subnets in various regions.

_Note:_ Some 15 subnets are sometimes incorrectly displayed before the network is created, although one per region is eventually created.

_QUESTIONS:_

- _Could 2 networks be paired automatically with each other, such as the default and auto-mode-net?_
- _And what does the abbreviation VPC mean?_

**Create a network in custom mode**

This time we are going to create a customised network:

1. Click on  **Create VPC Network** again.
2. Create a VPC network with the following features:
  - Name: custom-mode-net.
  - Subnet Creation Mode: Custom.
    - Subnet 1: custom-mode-net-subnet-euw1, region: europe-west1, IP address range: 10.10.0.0/20.
    - Subnet 2: custom-mode-net-subnet-euw2, region: europe-west2, IP address range: 10.20.0.0/20.
  - Dynamic Routing Mode: Global.

Once the VPC is created, we will add an extra subnet:

1. Click on the name of the VPC, custom-mode-net.
2. Explore the information and tabs, and then click Add subnet.
3. Add a new subnet:
  - Name: custom-mode-net-subnet-euw3, region: europe-west3, IP address range: 10.30.0.0/20.

This allows us to create a network in custom mode, controlling all its characteristics, and new sub-networks are added afterwards.

_DELIVERABLE_: 

M2U4-1-task_1-file_1-screenshot_1.jpg: Screenshot showing custom-mode-network detail page.

**Explore Routes**

Once the networks are created, navigate to  **VCP Network > Routes**  and explore the routes for each of the networks.

_QUESTION: Should we manage networks manually, or are they created automatically for each subnet, also for the last subnet added?_

**Task 2: Checking connectivity**

In this task we will check the connectivity between instances, depending on their location and selected network.

**Create VM Instances**

Let&#39;s start by creating the instances. Create 3 instances with the following features:

1. Name: auto-mode-net-vm1.
  - Region: europe-west1.
  - Series and type of machine: e2-micro.
  - VPC network: auto-mode-net.
2. Name: auto-mode-net-vm2.
  - Region: europe-west3.
  - Series and type of machine: e2-micro.
  - VPC network: auto-mode-net.
3. Name: custom-mode-net-vm1.
  - Region: europe-west1.
  - Series and type of machine: e2-micro.
  - VPC network: custom-mode-net.

_Note:_ You will find the network settings via the drop-down menu under  **Network Tools, Disks, Security, Administration, Single User** , under  **Network Tools \&gt; Network Interfaces** , modifying the default interface.

**Check connectivity**

Finally, let's check the connectivity between the instances.

First let's create a firewall rule that enables inbound SSH and ICMP traffic on the custom-mode-network.

To do this, in Cloud Shell or your local Cloud SDK installation run the command gcloud compute firewall-rules create allow-ingress-ssh-icmp --direction input --action allow --rules tcp:22,icmp --network custom-mode-net

To check connectivity:

1. Navigate to  **Compute Engine > VM Instances**.
2. Click on the  **SSH**  button of the source instance.
3. Check the connection to the target instance: ping TARGET_INSTANCE_IP.
4. Cancel the command with CTRL + C.

Now answer the following questions by checking the indicated connectivity. _QUESTIONS:_

1. _Between the internal IPs of auto-mode-net-vm1 and auto-mode-net-vm2, even if they are in different regions._
2. _Between the internal IPs of auto-mode-net-vm2 and auto-mode-net-vm3, even if they are in different regions._
3. _Between the external IPs of auto-mode-net-vm1 and auto-mode-net-vm2._
4. _Between the internal IPs of auto-mode-net-vm1and custom-mode-net-vm1, being in the same region but on different networks._
5. _Between the external IPs of auto-mode-net-vm1and custom-mode-net-vm1, being on different networks._

In this way we can verify that there is private connectivity between instances on the same network, regardless of the region and subnet where they are located, but not if they are on different networks, and that whenever firewall rules allow it, there will be public-only connectivity between instances on different networks.

_Note:_ Private connectivity refers to using private (generally internal) IPs, and public to using public (usually external) IPs.

**Connectivity Tests**

Now let's use the automatic connectivity testing tool.

In the console, navigate to  **Network Intelligence > Connectivity Testing**  and use the tool to replicate the above tests.

_Note:_

- You will be prompted to enable the Network Management API.
- For internal IPs, you can select the instance, but for external IPs, enter the external IP as the source or destination.
- Use ICMP protocol.

_QUESTION: What is the result of the 5 tests?_

**Task 3: Addressing**

In this task we will explore addressing internal IPs, how to expand ranges and use alias IPs.

**Expand IP address ranges**

Let&#39;s see how to expand the address range of the subnets. Remember that auto mode networks have subnets with a range/20 and can be extended up to/16, while custom subnets have no limitations in this regard.

Expand the address range of a subnet:

1. On the console, navigate to  **VPC Network > VPC Networks**  and click on the auto-mode-net network.
2. In the list of subnets, check that their address ranges are/20.
3. Click on any subnet, then click  **Edit**.
4. Change its range to a narrower one, e.g./22._QUESTION: Have you encountered any problems? If so, what?_
5. Now try switching the range to a wider one, e.g./18.
6. Once changed, try changing it to a/14._QUESTION: Has there been any problems now? If so, what?_
7. Finally, change it back to a/16 range.

This allows us to expand the range of a subnet at any time.

Now repeat the operation for one of the subnets of the custom-mode-net, the custom-mode-net-subnet-euw1 subnet, extending it up to /16.

_BONUS QUESTION: Using any online IP subnet calculator and considering the IP address ranges used, to what range could we theoretically extend it?_

**Use an Alias IP**

Finally we are going to create a secondary IP range in a subnet and use it to assign an alias IP range to a VM instance, in order to deploy several services in the same one.

Let&#39;s start by adding a secondary IP range to a subnet:

1. On the console, navigate to  **VPC Network > VPC Networks**  and click on the custom-mode-net network.
2. In the list of subnets, check their internal address ranges.
3. Tap the custom-mode-net-subnet-euw2 subnet and click **Edit**.
4. Explore the options and add a range of secondary IPs:
  - Name: alias-ip-range-euw2.
  - Range: 10.40.0.0/26.
5. In the console, navigate to the network page or network list, and check the secondary range added.

_DELIVERABLE_: 

M2U4-1-task_3-file_1-screenshot_1.jpg: Screenshot showing the subnets and their custom-mode-net network ranges.

Now let's assign an alias IP range to a VM instance.

1. In  **Compute Engine** , create a VM instance:
  - Name: custom-mode-net-vm2.
  - Region: europe-west2.
  - Series and type of machine: e2-micro.
  - Network: custom-mode-net, subnet: custom-mode-net-subnet-euw2.
  - IP alias ranges: alias-ip-range-euw2 subnet range, IP/29 alias range.
2. Once the instance has been created, click on it and, on the details page, review its  **Network Interfaces**  information and  **IP alias range**.

To check connectivity to that instance:

1. Connect via SSH to the custom-mode-net-vm1 instance.
2. Ping the internal (main) IP of the custom-mode-net-vm2 instance: ping -c 3 INTERNAL_IP_VM1.
3. Also check the connectivity to the IPs of the alias range: ping -c 3 10.40.0.0 to ping -c 3 10.40.0.7.

This way we can assign an alias IP range to a VM instance from a secondary range of a subnet.

_DELIVERABLE:_ 

M2U4-1-task_3-file_2-screenshot_2.jpg: Screenshot showing the result of the latest ping commands.

**Task 4: Traffic Logs**

In this last task we are going to enable traffic logging on a subnet, logging a sample of traffic in any direction across the subnet.

To do this, navigate to  **VPC Networks > VPC Network**  and click on the custom-mode-net-subnet-euw1 subnet.

1. Turn on  **Flow Logs**.
2. Once activated, return to the SSH session, or set up a new one.
3. Ping the other instance of the same custom-mode-net network, www.google.com, other destinations, etc., to generate some network traffic.
4. Finally, navigate to  **Cloud Logging**.
5. In the top section (with a blue button to the right of  **Run query** ), type in a search by clicking on the drop-down menus of  **Resource**  and  **Log name** :
  - Resource: Subnet > Subnetwork_ID: custom-mode-net-subnet-euw1 > Subnetwork_name: custom-mode-net-subnet-euw1.
  - Log name: Compute Engine > vpc_flows.
6. Run the query and review the logs.

If you don't see any logs, extend the query time or go back to the SSH session and generate more traffic to or from instances of that subnet.

This way we can enable and query traffic sampling logs on a subnet.

_DELIVERABLE:_ 

M2U4-1-task_4-file_1-screenshot_1.jpg: Screenshot showing subnet traffic logs.

**Deliverables Summary**

1. M2U4-1-questions.txt: Answers to all questions raised in the exercise.
2. M2U4-1-task_1-file_1-screenshot_1.jpg: Screenshot showing custom-mode-net network detail page.
3. M2U4-1-task_3-file_1-screenshot_1.jpg: Screenshot showing the subnets and their custom-mode-net network ranges.
4. M2U4-1-task_3-file_2-screenshot_2.jpg: Screenshot showing the result of the latest ping commands.
5. M2U4-1-task_4-file_1-screenshot_1.jpg: Screenshot showing subnet traffic logs.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instances created.
2. Delete the connectivity tests created.
3. Delete the VPC networks created.

_NOTE:_ Sometimes resources may take a long time to be deleted or indicate that other resources on which they depend need to be deleted first.
