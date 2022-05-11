# **Bastion Hosts and Cloud IAP**

Unit M2U4 - Exercise 7

**What are we going to do?**

1. Create an internal instance and a bastion host to enable connectivity to and from it.
2. Set Cloud IAP as an alternative to the bastion host.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U4-7-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Create internal deployment with bastion host**

Again, let's start by creating the network topology, a private VM instance, with no external IP, and a bastion host instance.

**Create environment**

To do this, we will take the following steps:

1. Create an internal instance, without internal IP.
2. Assign firewall paths and rules to the internal instance, such as:
  - Connection from the bastion instance and, optionally, other internal sources is allowed.
  - The path makes any outbound connections always go through the bastion host.
3. Assign a firewall rule to the bastion host to limit SSH connection sources.
4. In the bastion instance we configure:
  - IP addresses forwarding.
  - SSH key management with Cloud SDK and OS Login.
  - Permission for the bastion host service account to connect via SSH to the internal instance (required for OS Login).
5. Start by creating a VPC network with the following features:
  - Name: vpc-cloud.
  - Subnet Creation Mode: Custom.
  - Subnet: vpc-cloud-subnet.
    - Region: europe-west1.
    - IP Address Range: 10.10.0.0/20
6. Now create the internal VM instance:
  - Name: vm-internal.
  - Zone: europe-west1-b.
  - Type of machine: e2-micro.
  - Metadata: key enable-oslogin, value TRUE.
  - Network: vpc-cloud.
  - Network tag: internal-vm.
  - External IP: No external IP.

**Deploying a Bastion Host**

Now let's create the bastion host instance:

1. Create a service account:
  - Name: sa-bastion-host.
  - Access to the project/functions (roles): Compute Engine > Access to Compute OS.
  - Access to this service account: Role of service account users > your Google Account.
  - _Note:_ You will see these logins as the second and third steps, respectively, in the service account creation wizard.
2. Now create the bastion host instance:
  - Name: bastion-host.
  - Zone: europe-west1-b.
  - Type of machine: e2-small.
  - Service account: sa-bastion-host.
  - Network: vpc-cloud.
  - Network tags: bastion-host.
  - Networking Tools > IP Forwarding: Enable.

**Configure Deployment**

Once the resources have been created, we will configure the firewall paths and rules:

1. Create the following firewall rules:
  - Name: allow-all-bastion-internal.
    - Network: vpc-cloud.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: network tags, internal-vm.
    - Source filter: source tags, bastion-host
    - Priority: 100.
    - Protocols and ports: All.
  - Name: allow-all-internet-bastion.
    - Network: vpc-cloud.
    - Traffic direction: Entry.
    - Action in case of coincidence: Allow.
    - Destinations: service accounts, sa-bastion-host.
    - Source filter: 0.0.0.0/0 (_note:_ to be more specific, we could replace it with the Cloud Shell IP or a range of public company IPs).
    - Priority: 1000.
    - Protocols and ports: All.
2. Create the following custom static path:
  - Name: route-all-bastion-internal.
  - Network: vpc-cloud.
  - Target IP Range: 0.0.0.0/0
  - Priority: 100.
  - Instance tags: internal-vm.
  - Next step: Specify an instance, bastion-host.

We have thus been able to enable incoming traffic to the bastion host from the outside and to the internal instance from the bastion host and specify that the connection to the outside of the internal instances go through the bastion host.

**Check the deployment**

Finally, let's check the deployment from Cloud Shell or a local Cloud SDK installation:

1. Connect to the bastion host instance: gcloud compute ssh bastion-host.
2. Check that you can connect to the internal instance, from the bastion host: gcloud compute ssh vm internal --internal-ip.
3. Check your connectivity abroad: ping -c 2 www.google.com and curl www.google.com.
4. Find out the output IP of the internal instance: curl ipconfig.me.

_QUESTION: Does this IP correspond to the external IP of the bastion host?_

_DELIVERABLE:_ 

M2U4-7-task_1-file_1-screenshot_1.jpg: Screenshot showing the SSH connection to the internal instance and the result of the ping or curl commands.

**Task 2: Enable Cloud IAP**

For this task we are going to replace the bastion hosts with the use of Cloud IAP, which will mean that we do not have to depend on these bastions, their inconveniences and maintenance needs.

Cloud IAP will act as a proxy that allows you to control access to a VM instance, creating tunnels to its TCP ports, allowing HTTP, SSH, etc.

**Enable IAP Cloud**

We start by creating a firewall rule to allow connection from the proxy to internal instances:

- Name: allow-ssh-iap-all.
- Traffic direction: Entry.
- Action in case of coincidence: Allow.
- Destinations: All instances of the network.
- Source filter: 35.235.240.0/20
- Priority: 100.
- Protocols and ports: TCP:22.

As we have access as project owners, we have already assigned the corresponding permissions to be able to use Cloud IAP for SSH access:

1. Make sure you can connect from Cloud Shell or a local installation of Cloud SDK: gcloud compute ssh vm internal --internal-ip.
2. Navigate to  **Compute Engine > VM Instances**  and verify that you can use the SSH button to create an SSH connection to the instance.

This makes it easy for us to leverage integration with Cloud IAP to connect to an internal VM instance without the need for a bastion host.

_DELIVERABLES:_ 

M2U4-7-task_2-file_1-screenshot_1.jpg: Screenshot of the instance listing page showing that the instance has no external IP and the SSH button active.

**Deliverables Summary**

1. M2U4-7-questions.txt: Answers to all questions raised in the exercise.
2. M2U4-7-task_1-file_1-screenshot_1.jpg: Screenshot showing the SSH connection to the internal instance and the result of the ping or curl commands.
3. M2U4-7-task_2-file_1-screenshot_1.jpg: Screenshot of the instance listing page showing that the instance has no external IP and the SSH button active.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the instances created.
2. Delete the service account created.
3. Delete the VCP network created, which will delete all other network resources.
