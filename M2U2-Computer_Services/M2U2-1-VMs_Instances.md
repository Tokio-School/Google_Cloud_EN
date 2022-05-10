# **VM Instances**

Unit M2U2 - Exercise 1

**What are we going to do?**

1. Create a Linux VM instance in the web console.
2. Customise the instance and deploy a web page.
3. Use startup and shutdown scripts to execute commands automatically when starting or stopping the instance.
4. Modify the instance once stopped.
5. Create a Windows Server instance.
6. Explore SSH and RDP connection methods to instances.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U2-1-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Create a Linux VM instance**

Let&#39;s start by creating a Linux VM instance in the Google Compute Engine service.

**Create a Linux VM Instance**

Let&#39;s create a Linux VM instance and explore the available features.

1. In the Google Cloud web console, navigate to the  **Compute Engine > VM Instances** service, or search for the service or section in the search box in the blue top bar.
2. If it redirects to the Compute Engine API page, enable it or check that it is enabled, wait for it to be enabled and return to the  **VM Instances** page. You can check the progress of the operation by clicking on the notifications button (bell icon or a number on the right in the blue top bar).
3. In the upper (or central if no instance is deployed) area, press the  **Create Instance** button.
4. You will then have accessed the instance creation menu. Explore this menu to get familiar with all the available sections, menus and options:
  1. Look at the different sections of the menu, such as name, tags and location, machine settings, boot disk and the final drop-down menu and its options.
  2. Above on the right you will see an estimate of the monthly cost of that VM instance, and its details.
  3. A large number of machine configurations are available, combining machine family, series and machine type.
  4. Start by keeping the default machine configuration (family: general purpose, series: E2, machine type: e2-medium), change the region and see if the price changes:
    1. _QUESTION: What regions are available in Europe?_
    2. _QUESTION: Are any region(s) significantly more expensive than others?_
    3. _QUESTION: What is the most expensive and cheapest region in Europe?_
  5. Now select the default region of "us-central1" again and explore the different machine configurations available:
    1. Explore the families and series first, looking at the description of each option.
    2. Explore machine types now, looking at how prices change.
    3. Explore custom machine types. Try various settings to find their limitations.
    4. Selecting an E2 general purpose instance, in the cost details you have a link to the Compute Engine costs. Explore the monthly costs of "m1-ultramem" instances, purely out of curiosity.
    5. Finally, explore various combinations of regions, zones, and machine settings.
      1. _QUESTION: Are all settings available in all regions and zones? In which region and areas of Europe are the most combinations available?_
  6. Check the equivalent commands for the REST API or Cloud SDK at the bottom of the page. Note that the command line option includes a button to copy and import that command into Cloud Shell, which may be useful at some point.
5. Create a VM instance with the following characteristics:

  1. Name: vm-linux.
  2. Region and area: europe-west1-b.
  3. Type of machine: e2-micro.
  4. Boot Disk: Debian GNU/Linux.
  5. Firewall: Allow HTTP and HTTPS traffic.
  6. Leave the rest of the options with the default values.
1. Press the Create Instance button and wait for the Instance to finish creating.

  1. _QUESTION: How long does it take to create a shared kernel instance?_
1. When the VM instance has been created, click on its name in the list to check its details. Explore all options, both the features and settings of the instance and the top action buttons.

_DELIVERABLE:_ M2U2-1-task_1-file_1-screenshot_1.jpg: Take a screenshot of the instance detail page.

**Explore Connection Methods**

For Linux VM instances we have 2 connection methods available: SSH and serial port, the latter normally being used only to debug instances that we cannot connect to via SSH or are not started correctly.

We have several ways to SSH connect to a Linux instance:

- Using the web console directly.
- From Cloud Shell or local Cloud SDK.
- Using a local SSH client or terminal, from the Linux/macOS terminal or e.g. PuTTy for Windows.

Try using the 3 ways to explore the possibilities of SSH connection to the instance:

1. **Cloud Console:**  On the instance listing and instance details pages, you will find an SSH button that will allow you to open a terminal in the browser directly to the instance, creating and uploading a set f public-private keys.
2. **Cloud SDK/Cloud Shell:**  Use the gcloud compute ssh INSTANCE\_NAME ([reference](https://cloud.google.com/sdk/gcloud/reference/compute/ssh)) command.
3. _ **BONUS:** _ **Local Client/PuTTy:**  Create an SSH key and manually add it to the instance ([instructions](https://cloud.google.com/compute/docs/instances/connecting-advanced#provide-key)) and use it to connect from your local terminal, WSL or PuTTY ([instructions](https://cloud.google.com/compute/docs/instances/connecting-advanced#thirdpartytools)).
  1. _Note:_ For your username, use your email address excluding "@domain.com", only the name.
  2. _Note:_ Although using OS Login is the most advisable in an organisation, for simplicity in the exercise we will use the manual method instead.

_DELIVERABLE:_ M2U2-1-task_1-file_2-screenshot_2.jpg: Take a screenshot of the SSH terminal opened with the web console._DELIVERABLE:_ M2U2-1-task_1-file_3-screenshot_3.jpg: Take a screenshot of the open SSH connection with local Cloud SDK or Cloud Shell.

Once the various options have been explored, keep an SSH terminal window open to the instance to proceed to the next step.

**Customise the instance with a web page**

We are going to deploy a web server with its default page as an example of instance customisation.

Using your SSH connection to it, run the following commands:

1. Update the list of available packages: sudo apt-get update.
2. Install the Apache web server: sudo apt-get install apache2.
3. Check the installation and its operation: sudo systemctl status apache2.
4. Check the local site: curl localhost:80.

Finally, check the external connection to the web:

1. In the console, in  **Compute Engine > VM Instances** , navigate to the instance details page by clicking on its name.
2. Verify that the firewall rules for enabling HTTP and HTTPS traffic are active.
3. Find the list of the network interfaces of the instance and click on  **nic0**  or on  **view details**  of the nic0.
4. Browse the details of the instance's network interface. This page will help you debug connectivity issues if they arise in the future.
  1. Check that the instance has 2 network labels assigned to it, http-server and https-server.
  2. As for firewall rules and routing details, review the firewall rules related to those network tags (default-allow-http and default-allow-https) and check which protocols and ports they enable.
  3. In  **Network Scan > Input Scan** , review the ports and protocols enabled by source.
  4. _QUESTION: Should the VM instance be able to accept external traffic to your web on TCP port:80?_
5. Return to the Instance List page in  **Compute Engine > VM Instances** , find the external IP of the instance and click on it to open it in another tab of your browser.
6. Verify that the connection is made by HTTP to port 80. Current browsers generally redirect to HTTPS via port 443 and our website is only accessible via HTTP and port 80.
7. The default Apache server page should be displayed, confirming that we have been able to customise the instance and allow external traffic.

_DELIVERABLE:_ M2U2-1-task_1-file_4-screenshot_4.jpg: Take a screenshot of the server web, where you can also see the instance IP in the browser URL.

_BONUS:_ Ping from your local terminal or Cloud Shell to the instance&#39;s external IP to check connectivity with the ping IP_INSTANCE command.

**Task 2: Start-up and shut-down the instance**

We are going to explore some options related to instance startup and shutdown, such as startup and shutdown scripts and configuration changes available in execution and undo state.

**Startup and Shutdown Scripts**

The startup and shutdown scripts are executed when starting or shutting down the instance (whenever reasonably possible) respectively.

Scripts can be added at the time of instance creation or at a later time, and are registered as values in the metadata server for the instance.

First we will check how they would be added in the creation of a new instance and then we will add them to the instance already created:

**When creating a new instance**

1. In the web console, navigate to  **Compute Engine > VM Instances**  and click  **Create Instance**.
2. Expand the  **Administration, Security, Disks, Networks, Single User**  drop-down menu at the bottom of the options.
3. Locate the text box where the startup script would be added in  **Administration > Automation > Startup Script**.
4. The shutdown script would be added as a key-value in a metadata field, manually.
5. Do not create the VM instance, since we are going to add the scripts to the already created instance instead.

**In an already-created instance**

1. In  **Compute Engine > VM Instances** , locate the created instance and click on its name.
2. Click the  **Edit** button.
3. In  **Custom Metadata** , enter 2 fields, one for the startup script and one for the shutdown script, listed below.
  1. Key: startup-script, value: the script content shown below.
  2. Key: shutdown-script, value: the script content shown below.
4. Save changes to the instance

Startup Script:

#! /bin/bash

apt update

apt -y install apache2

cat <<EOF >/var/www/html/index.html

  Startup <html><body><p>script added successfully</p></body></html>

Shutdown Script:

#! /bin/bash

echo "+++ shutting down instance +++" >> /var/tmp/shutdown\_script\_output.txt

To check startup and shutdown scripts:

1. On the instance listing or instance details page, stop the instance and restart it once stopped.
2. To check the startup script:
  1. Access again through the external IP of your web page instance.
  2. Check its new content.
3. To check the shutdown script:
  1. Connect by SSH to the instance using one of the methods used above.
  2. Run the following command to check the contents of the file created by the script: ls /var/tmp and cat /var/tmp/shutdown_script_output.txt.

_DELIVERABLE:_ M2U2-1-task_2-file_1-screenshot_1.jpg: Take a screenshot of new web content._DELIVERABLE:_ M2U2-1-task_2-file_2-screenshot_2.jpg: Takes a screenshot of the result of the previous cat command.

**Stop and modify instance configuration**

Let's check what options we have when modifying the running instance and for which cases we must stop the instance first

1. Navigate to the instance details page ( **Compute Engine > VM Instances > INSTANCE\_NAME** ).
2. We are going to review the configuration options that we can modify for the instance when it is running and when it is stopped, for which you will answer the following questions.
3. Click  **Edit**  and review all the settings that you can change for the instance without stopping it, answering the corresponding questions.
4. Stop the instance, and once stopped, click  **Edit**  to review the settings you can change once stopped.

**Questions to answer:**  For each one answer whether it can be modified while it is running, stopped or not possible.

1. _QUESTION: Is it possible to change the type of machine?_
2. _QUESTION: Is it possible to change the zone?_
3. _QUESTION: Is it possible to add or modify tags?_
4. _QUESTION: Is it possible to add a new network interface?_
5. _QUESTION: Is it possible to modify network tags?_
6. _QUESTION: Is it possible to modify the boot disk?_
7. _QUESTION: Is it possible to add additional discs?_
8. _QUESTION: Is it possible to change custom metadata?_
9. _QUESTION: Is it possible to change the assigned service account?_

Finally, change the machine type of the instance to "e2-standard-2" and start it again.

**Task 3: Create a Windows Server VM Instance**

_Note: This task is optional, since due to a change in the free credit program, Windows Server-based instances cannot be used. You can complete it in a paid billing account environment or read the instructions for reference._

Let's create a VM instance with Windows Server and check the access options to it.

**Create a VM Windows Server instance.**

To create a Windows Server instance, follow these instructions:

1. In the console, navigate to  **Compute Engine > VM Instances**  and click  **Create Instance**.
2. Create an instance with the following settings:
  - Name: vm-windows.
  - Region and area: europe-west1-b.
  - Machine type: e2-standard-2.
  - Disk Image: Windows Server 2019 Datacenter.
  - Boot Disk: Standard 50 GB persistent disk.
  - Leave the rest of the options with the default values.

_OPTIONAL DELIVERABLE_: M2U2-1-task_3-file_1-screenshot_1.jpg: Take a screenshot of the Windows Server instance details window.

**Explore Connection Methods**

To connect to a Windows Server instance, we need to set the Windows password, access an RDP client, and connect to it by RDP.

First, set the Windows password for the instance:

1. Navigate to the Windows Server instance details page.
2. Under  **Remote access** , click "Set Windows password."
3. Enter your username or a new username (your current username will appear by default).
4. Click on  **Set**.
5. Copy the auto-generated password and save it, as you will not be able to access it again from the console.

To connect via RDP to the instance:

1. If you use Chrome as your browser, you can download the [Chrome RDP for Google Cloud Platform](https://chrome.google.com/webstore/detail/chrome-rdp-for-google-clo/mpbbnannobiobpnfblimoapbephgifkm/) plugin. The plugin supports the  **RDP**  button on the instance or instance detail page, similar to the  **SSH**  button for Linux.
2. If not, you can use any [remote RDP client](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).

In both cases, use the external IP of the instance, your username and password to connect to the instance.

_OPTIONAL DELIVERABLE:_ M2U2-1-task_3-file_2-screenshot_2.jpg: Take a screenshot of the Windows Server remote desktop by RDP connection.

**Deliverables Summary**

1. M2U2-1-questions.txt: Answers to all questions raised in the exercise.
2.  M2U2-1-task\_1-file\_1-screenshot\_1.jpg: Take a screenshot of the instance detail page.
3.  M2U2-1-task\_1-file\_2-screenshot\_2.jpg: Take a screenshot of the SSH open terminal with the web console.
4.  M2U2-1-task\_1-file\_3-screenshot\_3.jpg: Take a screenshot of the open SSH connection with local Cloud SDK or Cloud Shell.
5.  M2U2-1-task\_1-file\_4-screenshot\_4.jpg: Take a screenshot of the server web, where you can also see the instance IP in the browser URL.
6.  M2U2-1-task\_2-file\_1-screenshot\_1.jpg: Take a screenshot of new web content.
7. M2U2-1-task\_2-file\_2-screenshot\_2.jpg: Screenshot of the result of the previous "cat" command.
8. (_Optional_) M2U2-1-task_3-file_1-screenshot_1.jpg: Take a screenshot of the Windows Server instance details window.
9. (_Optional_) M2U2-1-task_3-file_2-screenshot_2.jpg: Take a screenshot of the Windows Server remote desktop by RDP connection.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. In  **Compute Engine > VM Instances** , select both created instances and delete them.
