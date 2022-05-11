# **Groups of Instances**

Unit M2U2 - Exercise 4

**What are we going to do?**

1. Create an unmanaged instance group.
2. Work with instance templates for managed instance groups.
3. Create a managed instance group for serverless applications.
4. Create a managed instance group for stateful applications.
5. Update applications in the different instance groups.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called &quot;M2U2-4-questions.txt&quot; before starting the exercise. Remember to identify and sort each question followed by your answer.

You'll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Unmanaged Instance Groups**

We are going to create an unmanaged instance group, composed of several manually created instances that are not managed by the service.

**Create base disk image**

First of all, we need to create a common base disk image:

1. From the navigation menu, navigate to  **Compute Engine > VM Instances**.
2. Click the  **Create Instance**  button to open the wizard.
3. Create an instance with the following characteristics (_Note:_ if no values are specified for a characteristic, this means that you use the default value):
  - Zone: europe-west1-b.
  - Type of machine: e2-micro.
  - Boot Disk: Debian GNU/Linux 11.
  - Firewall: Allow HTTP and HTTPS traffic.
  - As a startup script, enter the following:
4. #! /bin/bash
5. apt-get update
6. apt-get install apache2 -y
7. a2ensite default-ssl
8. a2enmod ssl
9. vm_hostname="$(curl -H "Metadata-Flavor:Google" http://169.254.169.254/computeMetadata/v1/instance/name)"
10. echo "Page served from: $vm\_hostname" | tee /var/www/html/index.html
11. systemctl restart apache2
12. Once the VM instance is created, connect to it via SSH with the console's  **SSH**  button or any of the other paths we've seen before.
13. Check the local site: curl localhost:80.

- _Note:_ If the website does not work properly, try running the startup script in the SSH terminal of the instance.
- Check access to the instance by clicking on its external IP on the  **Compute Engine > VM Instances**  page (_note:_ make sure you connect via HTTP and not HTTPS).
- Once you have checked everything, you can close the SSH connection with exit.
- Navigate to  **Compute Engine > Images**  and create a disk image:

- Name: webserver-v1.
- Source: Instance boot disk.
- Family: webserver.
- Location: multi-regional "eu".
- Continue running instance.
- Once the disk image is created, you can delete the instance. **Create an unmanaged instance group** Now let's create an unmanaged heterogeneous group of 3 instances. To do this, we must create these instances manually:
1. Navigate to  **Compute Engine > VM Instances**.
2. Create 3 different VM instances with these common features:
  - Zone: europe-west1-b.
  - Machine type: e2-micro, e2-small and e2-medium, respectively.
  - Disk: custom disk image webserver-v1.
  - Network Tags: network-lb
Now create an unmanaged instance group and add the 3 instances:
1. Navigate to  **Compute Engine > Instance groups**.
2. Create an unmanaged instance group with the following characteristics:
  - name uig-europe-west1
  - Location: europe-west1-b
  - Subnet: default.
  - Instances: Add the 3 previously-created instances.
Let's create a firewall rule to allow traffic between the load balancer and VM instances:
1. Activate your on-premises Cloud Shell environment or a Cloud SDK installation.
2. Create the firewall rule with the following command: gcloud compute firewall-rules create allow-network-lb --target-tags network-lb --allow tcp:80.
  1. This firewall rule called allow-network-lb will be applied to instances with the network-lb tag allowing traffic to TCP:80 port.
  2. We'll create it with gcloud directly, as we will see all the details of the firewall rules in a later module.
Next, let's create the load balancer, with its external IP and health check:
1. Reserve an external IP called network-lb-ip-uig with the gcloud compute addresses create network-lb-ip-uig --region europe-west1 command.
2. Create the tcp-health-check health-check with the gcloud compute health-checks create tcp tcp-health-check --region europe-west1 --port 80 command.
3. Create a network-lb-backend-service-uig backend service with the command:
4. gcloud compute backend-services create network-lb-backend-service-uig \
5. --protocol TCP \
6. --health-checks tcp-health-check \
7. --health-checks-region europe-west1 \
8. --region europe-west1
9. Add the instance group to the backend service as a backend:
10. gcloud compute backend-services add-backend network-lb-backend-service-uig \
11. --instance-group uig-europe-west1 \
12. --instance-group-zone europe-west1-b \
13. --region europe-west1
14. Create the regional external TCP load balancer by creating a forwarding rule to the backend service:
15. gcloud compute forwarding-rules create network-lb-forwarding-rule-uig \
16. --load-balancing-scheme external \
17. --region europe-west1 \
18. --ports 80 \
19. --address network-lb-ip-uig \
20. --backend-service network-lb-backend-service-uig
21. The load balancer will take a couple of minutes to become available. You can check the status and, once available, the external IP or EXTERNAL_IP, with the command gcloud compute forwarding-rules describe network-lb-forwarding-rule-uig --region europe-west1.
22. Once available, check that the load balancer and the unmanaged instance group with the application are working correctly by looping requests to its external IP: while true; do curl -m1 EXTERNAL_IP; done (_note:_ you can cancel the command with  **CTRL + X** ).
We have thus created a group of unmanaged instances from 3 manually created instances, and deployed a load balancer to allow external traffic and balance it between them._DELIVERABLES:_
1. M2U2-3-task_1-file_1-screenshot_1.jpg: Screenshot of the details page of the unmanaged instance group.
2. M2U2-3-task_1-file_2-screenshot_2.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
**Task 2: Managed Instance Groups** In addition to unmanaged instance groups, we can use managed instance groups for serverless and stateful applications. The main difference between them is that the stateless ones have auto-scaling, while the stateful ones do not, and the update policies available for both, given the characteristics of their applications. **Create instance template** Since in a managed instance group all instance replicas are homogeneous to each other, instances are defined from an instance template.To create an instance template in a similar way to the creation of a regular instance, follow the instructions below:
1. In the console, navigate to  **Compute Engine > Instance Templates**  and select  **Create Instance Template**.
2. Create a template with the following settings:
  - Type of machine: e2-micro.
  - Boot Disk: custom disk image webserver-v1.
  - Network Tags: network-lb
  - _QUESTION: Is it possible to select the location of the instances to be created in an instance template?_
_Note:_ With the network tag included in the instance template, the corresponding firewall rule will be automatically applied to those instances. **Creating a serverless managed instance group** Now let's create a serverless managed instance group by following our instance template:
1. In the console, navigate to  **Compute Engine > Instance Groups**  and click  **Create Instance Group**.
2. In the left column, select **New Managed Instance Group (Stateless)**.
3. Explore all the options, and create an instance group with the following features:
  - Name: mig-serverless-europe-west1.
  - Location: various areas, region: europe-west1.
  - Instance template: the newly-created one.
  - Minimum number of instances: 3.
  - Automatic repair: tcp-health-check.
Now repeat the steps to create another regional external TCP load balancer for the serverless managed instance group:
1. Reserve an external IP called network-lb-ip-serverless with the gcloud compute addresses create network-lb-ip-serverless --region europe-west1 command.
2. Create a network-lb-backend-service-serverless backend service with the command:
3. gcloud compute backend-services create network-lb-backend-service-serverless \
4. --protocol TCP \
5. --health-checks tcp-health-check \
6. --health-checks-region europe-west1 \
7. --region europe-west1
8. Add the instance group to the backend service as a backend:
9. gcloud compute backend-services add-backend network-lb-backend-service-serverless \
10. --instance-group mig-serverless-europe-west1 \
11. --instance-group-zone europe-west1-b \
12. --region europe-west1
13. Create the regional external TCP load balancer by creating a forwarding rule to the backend service:
14. gcloud compute forwarding-rules create network-lb-forwarding-rule-serverless \
15. --load-balancing-scheme external \
16. --region europe-west1 \
17. --ports 80 \
18. --address network-lb-ip-serverless \
19. --backend-service network-lb-backend-service-serverless
20. The load balancer will take a couple of minutes to become available. You can check the status and, once available, the external IP or EXTERNAL_IP, with the command gcloud compute forwarding-rules describe network-lb-forwarding-rule-serverless --region europe-west1.
21. Once available, check that the load balancer and the managed instance group with the application are working correctly by looping requests to its external IP: while true; do curl -m1 EXTERNAL_IP; done (_note:_ you can cancel the command with  **CTRL + X** ).
We have thus created a managed instance group with an external load balancer.

_DELIVERABLES:_
1. M2U2-3-task_2-file_1-screenshot_1.jpg: Screenshot of the details page of the managed serverless instance group.
2. M2U2-3-task_2-file_2-screenshot_2.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
**Create a stateful managed instance group** Stateful managed instance groups are very similar to serverless ones, with minor differences in behaviour to preserve instance state or not having auto-scaling.To create a stateful managed instance group, follow the steps above to create a managed instance group, except:
1. Select **New Managed Instance Group (stateful)**.
2. Under  **Stateful settings** , mark the startup disk as  **Stateful**.
3. As the number of instances, indicate 3.
Now follow the same steps to create a third load balancer:
1. Reserve an external IP called network-lb-ip-stateful with the gcloud compute addresses create network-lb-ip-stateful --region europe-west1 command.
2. Create a network-lb-backend-service-stateful backend service with the command:
3. gcloud compute backend-services create network-lb-backend-service-stateful \
4. --protocol TCP \
5. --health-checks tcp-health-check \
6. --health-checks-region europe-west1 \
7. --region europe-west1
8. Add the instance group to the backend service as a backend:
9. gcloud compute backend-services add-backend network-lb-backend-service-stateful \
10. --instance-group mig-stateful-europe-west1 \
11. --instance-group-zone europe-west1-b \
12. --region europe-west1
13. Create the regional external TCP load balancer by creating a forwarding rule to the backend service:
14. gcloud compute forwarding-rules create network-lb-forwarding-rule-stateful \
15. --load-balancing-scheme external \
16. --region europe-west1 \
17. --ports 80 \
18. --address network-lb-ip-stateful \
19. --backend-service network-lb-backend-service-stateful
20. The load balancer will take a couple of minutes to become available. You can check the status and, once available, the external IP or EXTERNAL_IP, with the command gcloud compute forwarding-rules describe network-lb-forwarding-rule-stateful --region europe-west1.
21. Once available, check that the load balancer and the managed instance group with the application are working correctly by looping requests to its external IP: while true; do curl -m1 EXTERNAL_IP; done (_note:_ you can cancel the command with  **CTRL + X** ).
We have thus created a managed instance group with an external load balancer.

_DELIVERABLES:_
1. M2U2-3-task_2-file_3-screenshot_3.jpg: Screenshot of the details page of the managed stateful instance group.
2. M2U2-3-task_2-file_4-screenshot_4.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
**Task 3: Updating Applications in Instance Groups** Let's see how we can update a serverless and stateful managed instance group. **Update Base Disk Image** First of all, we need to update our application and generate a new disk image with the new version:
1. From the navigation menu, navigate to  **Compute Engine > VM Instances**.
2. Click the  **Create Instance**  button to open the wizard.
3. Create an instance with the following features, changing the start-up script:
  - Zone: europe-west1-b.
  - Type of machine: e2-micro.
  - Boot Disk: Debian GNU/Linux 11.
  - Firewall: Allow HTTP and HTTPS traffic.
  - As a startup script, enter the following:
4. #! /bin/bash
5. apt-get update
6. apt-get install apache2 -y
7. a2ensite default-ssl
8. a2enmod ssl
9. vm_hostname="$(curl -H "Metadata-Flavor:Google" http://169.254.169.254/computeMetadata/v1/instance/name)"
10. echo "Version 2 - Page served from: $vm\_hostname" | tee /var/www/html/index.html
11. systemctl restart apache2
12. Once the VM instance is created, connect to it via SSH with the console's  **SSH**  button or any of the other paths we've seen before.
13. Check the local site: curl localhost:80.

- _Note:_ If the website does not work properly, try running the start-up script in the SSH terminal of the instance.
- Check access to the instance by clicking on its external IP on the  **Compute Engine > VM Instances**  page (_note:_ make sure you connect via HTTP and not HTTPS).
- Once you have checked everything, you can close the SSH connection with exit.
- Navigate to  **Compute Engine > Images**  and create new a disk image:

- Name: webserver-v2.
- Source: Instance boot disk.
- Family: webserver (same as above).
- Location: multi-regional &quot;eu&quot;.
- Continue running instance.
- Once the disk image is created, you can delete the instance.Thus, we have removed a new disk image with the new version of our application, using the same family of images. **Update Instance Template** To do this, we need a new instance template with the new version of our application:
1. Navigate to  **Compute Engine >Instances Templates**.
2. Select the previous instance template and press  **Copy**.
3. The options that will appear must be the same as in the previous template, check if this is the case.
4. Update the boot disk image to the new webserver-v2 image.
5. Save the new instance template with a descriptive name, e.g., by adding v2 to the previous one.
We have thus generated an instance template with the new version of the application included. Now let's use the new instance template to update the instance groups. **Update Unmanaged Instance Group** In an unmanaged instance group, we do not have any service-managed options, so we do not have self-update.To update the application of an unmanaged instance group, it is best to create the new instances manually, create a new backend service with them and direct the load balancer to it:
1. Navigate to  **Compute Engine > VM Instances**.
2. Create 3 new different VM instances manually from the new instance template in europe-west1-b.
Now create a new unmanaged instance group and add the 3 new instances:
1. Navigate to  **Compute Engine > Instance groups**.
2. Create an unmanaged instance group with the following characteristics:
  - name uig-europe-west1-v2
  - Location: europe-west1-b
  - Subnet: default.
  - Instances: Add the 3 new instances created.
Let's create the new backend service and redirect the load balancer from the previous unmanaged instance group to the new one:
1. Create a network-lb-backend-service-uig-v2 backend service with the command:
2. gcloud compute backend-services create network-lb-backend-service-uig-v2 \
3. --protocol TCP \
4. --health-checks tcp-health-check \
5. --health-checks-region europe-west1 \
6. --region europe-west1
7. Add the instance group to the backend service as a backend:
8. gcloud compute backend-services add-backend network-lb-backend-service-uig-v2 \
9. --instance-group uig-europe-west1-v2 \
10. --instance-group-zone europe-west1-b \
11. --region europe-west1
12. Update the regional external TCP load balancer to point to the new backend service:
13. gcloud compute forwarding-rules update network-lb-forwarding-rule-uig \
14. --region europe-west1 \
15. --backend-service network-lb-backend-service-uig-v2
16. The load balancer will take a couple of minutes to update. You can check the status and, once available, the external IP or EXTERNAL_IP (like before), with the command gcloud compute forwarding-rules describe network-lb-forwarding-rule-uig --region europe-west1.
17. Once available, check that the load balancer and the unmanaged instance group with the updated application are working correctly by looping requests to its external IP: while true; do curl -m1 EXTERNAL_IP; done (_note:_ you can cancel the command with  **CTRL + X** ).

_DELIVERABLES:_
1. M2U2-3-task_3-file_1-screenshot_1.jpg: Screenshot of the details page of the unmanaged instance group.
2. M2U2-3-task_3-file_2-screenshot_2.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
**Updating the serverless managed instance group** In a serverless managed instance group, we only need to instruct it to follow the new instance template so that the auto-update policy starts to recreate the instances following the new template:
1. Navigate to  **Compute Engine > Instance Groups** , click on the mig-serverless-europe-west1 group and click on the  **Update VMs** button.
2. Select the new instance template.
3. Under  **Update settings** , explore the options and select the  **Automatic**  type as **Update type**.
4. Click on  **Update VMs**  and return to the group details page while reviewing the update status.
Once the group has been updated, you can check the application with the IP of its load balancer:
1. Retrieve the external IP or EXTERNAL_IP with the command gcloud compute forwarding-rules describe network-lb-forwarding-rule-stateless --region europe-west1.
2. Once available, check the managed instance group by looping requests to its external IP: while true; do curl -m1 EXTERNAL_IP; done (_note:_ you can cancel the command with  **CTRL + X** ).
We have this upgraded a serverless managed instance group to a new version with a rolling update.

_DELIVERABLES:_
1. M2U2-3-task_3-file_3-screenshot_3.jpg: Screenshot of the details page of the managed serverless instance group.
2. M2U2-3-task_3-file_4-screenshot_4.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
**Updating the stateful managed instance group** Updates in a stateful managed instance group are made in the same way as for serverless instances, with the only difference being that the state of the instances is maintained.Follow the instructions in the previous point to update your stateful managed instance group and finally check it when all instances have been updated.

_DELIVERABLES:_
1. M2U2-3-task_3-file_5-screenshot_5.jpg: Screenshot of the details page of the managed serverless instance group.
2. M2U2-3-task_3-file_6-screenshot_6.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
**Deliverables Summary**
1. M2U2-4-questions.txt: Answers to all questions raised in the exercise.
2. M2U2-3-task_1-file_1-screenshot_1.jpg: Screenshot of the details page of the unmanaged instance group.
3. M2U2-3-task_1-file_2-screenshot_2.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
4. M2U2-3-task_2-file_1-screenshot_1.jpg: Screenshot of the details page of the managed serverless instance group.
5. M2U2-3-task_2-file_2-screenshot_2.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
6. M2U2-3-task_2-file_3-screenshot_3.jpg: Screenshot of the details page of the managed stateful instance group.
7. M2U2-3-task_2-file_4-screenshot_4.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
8. M2U2-3-task_3-file_1-screenshot_1.jpg: Screenshot of the details page of the unmanaged instance group.
9. M2U2-3-task_3-file_2-screenshot_2.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
10. M2U2-3-task_3-file_3-screenshot_3.jpg: Screenshot of the details page of the managed serverless instance group.
11. M2U2-3-task_3-file_4-screenshot_4.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.
12. M2U2-3-task_3-file_5-screenshot_5.jpg: Screenshot of the details page of the managed serverless instance group.
13. M2U2-3-task_3-file_6-screenshot_6.jpg: Screenshot the webpage of the application showing the IP of the load balancer and its response.

**Clean up resources** Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.
1. In  **Network Services > Load Balancing** , remove load balancers.
2. In  **Compute Engine > Instance Groups** , delete the instance groups created.
3. In  **Compute Engine > VM Instances** , delete instances that have not been previously deleted.
4. In  **Compute Engine > Disks** , delete disks if they have not been automatically deleted.
5. In  **Compute Engine > Instance Templates** , delete the instance templates.
6. In  **Compute Engine > Images** , delete the created images.
7. In  **VPC Network > External IP Addresses** , release the reserved external IP addresses.
