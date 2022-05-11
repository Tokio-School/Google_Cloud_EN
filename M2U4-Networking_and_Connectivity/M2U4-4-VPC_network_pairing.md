# **VPC network pairing**

Unit M2U4 - Exercise 4

**What are we going to do?**

1. Establish internal connectivity between networks with VPC network pairing.
2. Check the decentralisation of network management through firewall rules.
3. Check how the pairing between networks is not transitive.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U4-4-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You'll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Deploy network topology**

Once again, let's begin by deploying the network topology and VM instances to use during the exercise.

**Create networks and subnets**

Create the following VPC networks:

1. Create a VPC network with the following features:
  - Name: vpc-a.
    - Subnet Creation Mode: Custom.
    - Subnet: vpc-a-subnet.
      - Region: europe-west1.
      - IP Address Range: 10.10.0.0/20.
  - Name: vpc-b.
    - Subnet Creation Mode: Custom.
    - Subnet: vpc-b-subnet.
      - Region: europe-west1.
      - IP Address Range: 10.20.0.0/20
  - Name: vpc-c.
    - Subnet Creation Mode: Custom.
    - Subnet: vpc-c-subnet.
      - Region: europe-west1.
      - IP Address Range: 10.30.0.0/20.
2. Create the following firewall rules on each of the networks:
  - Name: allow-ssh-ping-internet-all.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: All instances of the network.
    - Priority: 1000.
    - Source filter: 0.0.0.0/0
    - Protocols and ports: TCP:22 and ICMP.
  - Name: allow-all-internal-all.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow
    - Destinations: All instances of the network.
    - Source filter: 10.0.0.0/8
    - Priority: 1000.
    - Protocols and ports: All.

**Create VM Instances**

Finally, create the following VM instances:

1. Name: vm-a1 and vm-a2.
  - Region: europe-west1.
  - Type of machine: e2-micro.
  - Network: vpc-a.
2. Name: vm-b1 and vm-b2.
  - Region: europe-west1.
  - Type of machine: e2-micro.
  - Network: vpc-b.
3. Name: vm-c1 and vm-c2.
  - Region: europe-west1.
  - Type of machine: e2-micro.
  - Network: vpc-c.

**Task 2: Pair VPC networks**

In this task we will start with pairing or "peering" VPC networks and checking that they allow connectivity between networks.

**Pair VPC A and B**

First let's start by checking that there is no possible connectivity between instances:

1. Connect via SSH to each of the vm-a1, vm-b1, and vm-c1 instances.
2. Check the connectivity to the internal IP of the other 2 instances, e.g., vm-a1 to vm-b1 and vm-c1 with their internal IP: ping -c 3 INTERNAL_IP_DESTINATION.
3. As you will see, there is no possible connectivity between these instances as they are in different VPCs.

Now let&#39;s pair VPCs A and B:

1. Navigate to  **VPC Network > Traffic Exchange between VPC networks**  and click  **Create Traffic Exchange Connection**.
2. Explore the available options and create a pairing with the following features:
  - Name: peering-vpc-a-to-b.
  - Your VPC network: vpc-a.
  - VPC network with traffic exchange: In the same project.
  - VPC network name: vpc-b.
3. Repeat the operation to create a reverse pair, peering-vpc-b-to-a.

_QUESTION: It warns us that a network pairing cannot be created if there is IP address overlap, is there IP address overlap between the 3 VPCs?_

Once created, navigate to  **VPC Network > Routes**  and verify that routes have been created on each network to connect to the target network.

To finish, double-check the connectivity, this time between the vm-a1 and vm-b1 instances.

_DELIVERABLE:_ 

M2U4-4-task_2-file_1-screenshot_1.jpg: Screenshot checking the connectivity between the 2 VM instances in one of the directions.

This is a simple way to establish a pairing of VPC networks between 2 networks, in the same project or in different projects.

**Restrict traffic**

VPC network pairing allows us to maintain independent, distributed network management, so we can always control connectivity on a network-by-network level with firewall rules.

Once the VPCs A and B are paired, the vm-a1 andvm-a2 instances could be connected to the vm-b1 and vm-b2 instances, and vice versa. Let's create a firewall rule so that we can only connect from the other VPC to the vm-a1 and vm-b1 instances.

1. Edit the vm-a2 and vm-b2 instances by adding the no-peering network tag to them.
2. Create a firewall rule with the following features:
  - Name: block-all-peering-all.
    - Network: vpc-a.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: Specific destination tags, no-peering.
    - Source filter: IPv4 ranges, 10.20.0.0/20
    - Priority: 100.
    - Protocols and ports: All.
  - Name: block-all-peering-all.
    - Network: vpc-b.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: Specific destination tags, no-peering.
    - Source filter: IPv4 ranges, 10.10.0.0/20
    - Priority: 100.
    - Protocols and ports: All.

Do the following connectivity checks again:

1. There is connectivity between vm-a1 and vm-a2.
2. There is connectivity between vm-b1 and vm-b2.
3. There is no connectivity between vm-a1 and vm-b2.
4. There is no connectivity between vm-b1 and vm-a2.
5. There is connectivity between vm-a2 and vm-b1.

_DELIVERABLE:_ 

M2U4-4-task_2-file_2-screenshot_2.jpg: Screenshot checking connectivity between vm-a1 and vm-b2 VM instances.

This allows us to maintain control of connectivity in a distributed or independent way in each of the networks.

**Task 3: Check that the pairing is not transitive**

In this last task we will perform another pairing between the VPC B and C networks to demonstrate that the peering is not transitive, that said pairing does not allow there to be a connection between the VPC A and C.

**Pair VPC B and C**

1. Follow the instructions above to create a pairing between the vpc-b and vpc-c networks in both directions.
2. Once created, check that there are automatically managed routes to enable such a connection.
3. Check the connectivity between the vm-b1 and vm-c1 instances, which will now be able to connect to each other.
4. Check connectivity between vm-a1 and vm-c1 instances, which will not be able to connect.

_QUESTION: Return to the list of routes, to which VPCs are there routes available on VPC B? And on A, are there routes to VPC C and vice versa?_

**Pair VPC A and C**

However, we can always solve this connectivity by creating a direct pairing between the VPCs A and C in both directions.

1. Again, create another pair of pairs between the vpc-a and vpc-c networks in both directions.
2. Once created, verify that there are now routes between both VPC networks.
3. Check the connectivity between instances vm-a1 and vm-c1, which will now be able to connect.

_DELIVERABLE:_ 

M2U4-4-task_3-file_1-screenshot_1.jpg: Screenshot checking connectivity between vm-a1 and vm-b2 VM instances.

This allows us to verify that the peering is not transitive, that there is only connectivity if there is a direct pairing between VPC networks since these routes are not transferred through other pairs.

**Stop Pairing**

Just as we can manage our networks on a distributed and individual basis, we can stop the pairing if we delete it:

1. Navigate to  **VPC Network > Traffic Exchange between VPC networks**.
2. Remove pairing from network A to network C, maintaining pairing from network C to network A.
3. Check connectivity between vm-a1 and vm-c1 instances in both directions.
4. Check if the routes between the two networks are maintained.

Thus, we can see that once a network pairing is removed, connectivity is interrupted regardless of whether the pairing in the other network is also removed in the reverse direction.

**Deliverables Summary**

1. M2U4-4-questions.txt: Answers to all questions raised in the exercise.
2. M2U4-4-task_2-file_1-screenshot_1.jpg: Screenshot checking the connectivity between the 2 VM instances in one of the directions.
3. M2U4-4-task_2-file_2-screenshot_2.jpg: Screenshot checking connectivity between vm-a1 and vm-b2 VM instances.
4. M2U4-4-task_3-file_1-screenshot_1.jpg: Screenshot checking connectivity between vm-a1 and vm-c1 VM instances.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instances created.
2. Delete the VPC networks created, removing the subnets, firewall rules and pairings used.
