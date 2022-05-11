# **Service Accounts**

Unit M2U5 - Exercise 1

**What are we going to do?**

1. Use the default Google Cloud service accounts.
2. Create custom service accounts, manage their credentials, and use them manually and in applications.
3. Assign roles to service accounts and permissions to use them for other identities.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U0-0-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Default Service Accounts**

**Check default service accounts**

- check SAs created

**Create Cloud Storage bucket**

- Create GCS bucket - name with project ID + m2u5-1

**GCE Default service account and its scopes**

- create VM instance with default SA and scope for GCS
- upload object to GCS - read object

**Task 2: Custom Service Accounts**

**Create service accounts**

- create SA - check roles and permissions, do not assign

**Role Assignment and Service Account Access**

- assign SA user to user
- assign SA role object storage owner

**Task 3: Service Account Assignment**

**Assignment to VM Instances**

- assign custom SA to new VM
- upload and list objects, check access

**Assignment to applications**

- turn off VM and remove SA
- lift VM, assign SA on envvar
- upload and list objects, check access

**Impersonate Service Accounts**

- remove SA from envvar
- impersonate SA with gcloud
- upload and list objects with gsutil, check access

**Deliverables Summary**

1. M2U0-0-questions.txt: Answers to all questions raised in the exercise.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the Cloud Storage bucket created.
2. Delete the VM instances created.
3. Delete the service account created.
