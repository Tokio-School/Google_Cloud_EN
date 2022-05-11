# **Cloud VPN and Cloud Router**

Unit M2U4 - Exercise 5

**What are we going to do?**

1. Create a VPN connection between 2 networks with Cloud VPN
2. Check dynamic routing using Cloud Router.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U4-5-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Deploy network topology**

As usual, we start by creating the network topology, which in this case will simulate a cloud environment and an on-premise environment:

1. Create a VPC network with the following features:
  - Name: vpc-cloud.
    - Subnet Creation Mode: Custom.
    - Subnet: vpc-cloud-subnet.
      - Region: europe-west1.
      - IP Address Range: 10.10.0.0/20
    - Dynamic Routing Mode: Global.
  - Name: vpc-on-prem.
    - Subnet Creation Mode: Custom.
    - Subnet: vpc-on-prem-subnet.
      - Region: europe-west1.
      - IP Address Range: 10.10.0.0/20
    - Dynamic Routing Mode: Global.
2. Create the following firewall rules on each of the networks:
  - Name: allow-ssh-ping-internet-all.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: All instances of the network.
    - Source filter: 0.0.0.0/0
    - Priority: 1000.
    - Protocols and ports: TCP:22 and ICMP.
  - Name: allow-all-internal-all.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow
    - Destinations: All instances of the network.
    - Source filter: 10.0.0.0/8
    - Priority: 1000.
    - Protocols and ports: All.
3. Finally, create the following VM instances:
  - Name: vm-cloud.
    - Region: europe-west1.
    - Type of machine: e2-micro.
    - Network: vpc-cloud.
  - Name: vm-on-prem.
    - Region: europe-west1.
    - Type of machine: e2-micro.
    - Network: vpc-on-prem.

**Task 2: VPN connection with dynamic routing**

We are now going to establish the VPN connection between the two environments, using the High Availability (HA) option of Cloud VPN with Dynamic Routing (Cloud Router):

**Create VPN Tunnels**

We started by creating the 2 VPN gateways, each with 2 tunnels (for high availability and maximum bandwidth). Each gateway will represent the local deployment in each environment, so we will have 2 in total, or in 2 directions.

1. Navigate to  **Hybrid Connectivity > VPN**  and click  **Create VPN Connection**.
2. Select a **VPN with high availability (HA)**.
3. Create a VPN gateway or gateway with these features:
  - Name: gateway-cloud-to-on-prem.
  - Network: vpc-cloud.
  - Region: europe-west1.
4. Click  **Create and continue**  to create the gateway, but don't continue adding VPN tunnels yet, as we need to create the gateway for the other network to link them first, so click  **Cancel**.
5. Create another gateway with these features:
  - Name: gateway-on-prem-to-cloud.
  - Network: vpc-on-prem.
  - Region: europe-west1.
6. Continue adding VPN tunnels to the gateway-on-prem-to-cloud:
  - Pair VPN gateway (or remote): Google Cloud.
  - Project: Your project ID.
  - VPN gateway name: gateway-cloud-to-on-prem.
  - High Availability: Create a pair of VPN tunnels.
  - Routing Options: Create New Cloud Router:
    - Name: router-on-prem.
    - Google ASN: 64512.
    - BGP Torque Keepalive Interval: 20.
  - VPN tunnels:
    - Name: tunnel-on-prem-to-cloud-1.
    - Previously shared IKE key: hello-vpn-1.
    - Name: tunnel-on-prem-to-cloud-2.
    - Previously shared IKE key: hello-vpn-2.
7. Click on  **Configure GCP sessions later** , to have the 2 tunnels on both gateways first.
8. Back on the  **hybrid connectivity > VPN** page, click on the  **Cloud VPN gateways** tab, where you can see that there are 2 gateways, but only one has the 2 tunnels already created.
9. For the gateway-cloud-to-on-prem gateway, click  **Add VPN Tunnel**.
10. Now continue adding VPN tunnels to the gateway-cloud-to-on-prem:
  - Pair VPN gateway (or remote): Google Cloud.
  - Project: Your project ID.
  - VPN gateway name: gateway-on-prem-to-cloud.
  - High Availability: Create a pair of VPN tunnels.
  - Routing Options: Create New Cloud Router:
    - Name: router-cloud.
    - Google ASN: 64513.
    - BGP Torque Keepalive Interval: 20.
  - VPN tunnels:
    - Name: tunnel-cloud-to-on-prem-1.
    - Previously shared IKE key: hello-vpn-1.
    - Name: tunnel-cloud-to-on-prem-2.
    - Previously shared IKE key: hello-vpn-2.
11. Click on ** Set up GCP sessions later**.
12. On the  **Hybrid Connectivity > VPN** page, on the  **Cloud VPN Tunnels** tab, check the statuses of the 4 VPN tunnels, which should show "Established".

Now start setting up your BGP sessions:

- Cloud VPN Tunnel: tunnel-cloud-to-on-prem-1.
  - Name: session-cloud-to-on-prem-1.
  - ASN of the pair: 64512.
  - Announced Route Priority (MED): 1.
  - Cloud Router BGP IP: 169.254.0.1
  - BGP pair IP: 169.254.0.2
- Cloud VPN Tunnel: tunnel-cloud-to-on-prem-2.
  - Name: session-cloud-to-on-prem-2.
  - ASN of the pair: 64512.
  - Announced Route Priority (MED): 1.
  - Cloud Router BGP IP: 169.254.10.1
  - BGP pair IP: 169.254.0.2
- Cloud VPN tunnel: tunnel-on-prem-to-cloud-1.
  - Name: session-on-prem-to-cloud-1.
  - ASN of the pair: 64513.
  - Announced Route Priority (MED): 1.
  - Cloud Router BGP IP: 169.254.0.2
  - BGP pair IP: 169.254.0.1
- Cloud VPN tunnel: tunnel-on-prem-to-cloud-2.
  - Name: session-on-prem-to-cloud-2.
  - ASN of the pair: 64513.
  - Announced Route Priority (MED): 1.
  - Cloud Router BGP IP: 169.254.10.2
  - BGP pair IP: 169.254.10.1

1. Once configured, in the  **VPN Cloud Tunnels**  tab you can verify that all settings are correct (_note:_ you can scroll horizontally to see all columns).

After checking the settings, check the automatically created routes in  **VPC Network > Routes**.

Finally, check the connectivity between the instances:

1. From vm-cloud to vm-on-prem: ping -c 3 DESTINATION_INTERNAL_IP.
2. From vm-on-prem to vm-cloud: ping -c 3 DESTINATION_INTERNAL_IP.

_BONUS:_ Alternatively, you can create connectivity checks in  **Network Intelligence > Connectivity Tests**.

This allows us to establish a private connectivity between 2 networks, simulating a VPN connection between the on-premises and the cloud.

_DELIVERABLES:_

1. M2U4-5-task_2-file_1-screenshot_1.jpg: Screenshot showing  **Cloud VPN Gateways** tab.
2. M2U4-5-task_2-file_2-screenshot_2.jpg: Screenshot showing  **VPN Tunnels** tab.

**Check that the routing is dynamic**

Finally, let's check that the routing is dynamic: when we create a new path (e.g., when creating a subnet) in one environment, it is automatically published and updated by BGP in the other environment:

1. Create a new subnet in the cloud VPC:
  - Name: vpc-cloud-subnet-euw3.
  - Region: europe-west3.
  - IP Address Range: 10.128.0.0/20
2. Create a new VM instance on that subnet:
  - Name: vm-cloud-2.
  - Region: europe-west3.
  - Type of machine: e2-micro.
  - Network: vpc-cloud.
3. Check the paths again: _QUESTION: Has the new path to the subnet cloud been published in  __vpc-cloud__  to the vpc-on-prem  __network__?_
4. Now check the connectivity between the instances again:

- From vm-cloud-2 to vm-on-prem: ping -c 3 DESTINATION_INTERNAL_IP.
- From vm-on-prem-2 to vm-cloud: ping -c 3 DESTINATION_INTERNAL_IP.

Finally, clear the 2 tunnels in a gateway, e.g., tunnel-on-prem-to-cloud-1 and tunnel-on-prem-to-cloud-2 to simulate a loss of connection.

1. Double-check the routes: QUESTION_: Are the routes you created still running?_
2. Recheck connectivity between instances: QUESTION_: Is connectivity still maintained?_

This is how we have been able to verify that dynamic routing with Cloud Router automatically publishes routes between environments.

**Deliverables Summary**

1. M2U4-5-questions.txt: Answers to all questions raised in the exercise.
2. M2U4-5-task_2-file_1-screenshot_1.jpg: Screenshot showing  **Cloud VPN Gateways** tab.
3. M2U4-5-task_2-file_2-screenshot_2.jpg: Screenshot showing  **VPN Tunnels** tab.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instances created.
2. Removes Cloud VPN tunnels, gateways, and routers created.
3. Deletes the VPC networks created.
