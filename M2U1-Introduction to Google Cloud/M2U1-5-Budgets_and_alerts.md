# **Budgets and alerts**

Unit M2U1 - Exercise 5

**What are we going to do?**

1. Create budgets and alerts.
2. Receive programmatic alert notifications.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Create budgets and alerts**

We will create a budget and alerts to avoid problems with excessive expenses during the course.

**Create a budget and alerts**

1. In the web console, go to  **Billing > Budgets and Alerts**.
2. Explore all options to analyse their possibilities.
3. Create a quote with these features, leaving the rest with the default values:
  - Time interval: per month.
  - Projects: all.
  - Services : All
  - Credits: discounts and promotions and other credits.
  - Type of budget and amount: €100.
  - Actions: limits of 50%, 90% and 100% of the actual monthly budget.
  - Notifications: alerts to billing account administrators and users.

**Task 2: Receive programmatic alert notifications**

We are now going to set up some programmatic test notifications to selectively control spending, shutting down some VM instances but not deleting them or deleting Cloud Storage data, using an automatically executed Cloud Functions feature.

**Create environment**

Before continuing, enable the Cloud Functions API from the web console, navigating to  **APIs and services > Library**.

1. Create 2 VM instances with the following configuration:
  - Zone: europe-west1-b.
  - Image and image family: Debian 10 Buster
  - Machine type: e2-micro (_Note: you can check the available types with __gcloud compute machine-types list --zones europe-west1-b_)
  - Use the comman gcloud compute instances create VM_NAME --image =debian-10-buster-v20200309 --image-project= PROJECT_ID --machine-type="e2-micro" ([docs](https://cloud.google.com/compute/docs/instances/create-start-instance#publicimage))
2. Create a Cloud Storage bucket with the command gsutil mb -p PROJECT_ID -c STANDARD -L EUROPE-WEST1 -b on gs://BUCKET_NAME  (_Note: do not modify the_ _STANDARD_ _AND_ _EUROPE-WEST1_ strands of _the command_).
3. In the web console, navigate to  **Compute Engine > VM Instances**  and verify that they have been created successfully.
4. Navigate to  **Storage > Browser**  and verify that the bucket has been created successfully.
5. Access the Cloud Storage bucket and, using the web console, upload any file from local.
  - **Programmatic Notifications**
  - We are now deploying a Cloud Functions feature that will receive an automatic alert sent via Cloud Pub/Sub whenever a budget limit is reached, and reaching 100% of the budget will shut down VM instances to selectively control spending.
  - To do this, follow the instructions publicly available in the [documentation](https://cloud.google.com/billing/docs/how-to/notify#selectively_control_usage):

- Create a Cloud Pub/Sub topic by following the [documentation](https://cloud.google.com/billing/docs/how-to/budgets-programmatic-notifications#manage-notifications).
- Edit the code to be deployed in the function and add your project ID and europe-west1-b as zone.
- The latest runtime versions of Node.js and Python have the GCP_PROJECT environment variable available.
- Follow the instructions to add the "Compute Instance Admin (v1)" role to the service account that uses Cloud Functions by default.
- To test the process, follow the instructions in the [documentation](https://cloud.google.com/billing/docs/how-to/notify#test-your-cloud-function).
- To check that the instances have been turned off, navigate to  **Compute Engine > VM Instances** in the console.
- If any errors occur, analyse the Cloud Functions logs by navigating to  **Cloud Functions** , accessing your function and the option to access the function logs.

- _DELIVERABLE:_ M2U1-5-task\_2-file\_1-screenshot.jpg: Screenshot the Cloud Functions logs showing the names of the VM instances that are turned off.
- **Deliverables Summary**

1. M2U1-5-task_2-file_1-screenshot.jpg: Screenshot of the Cloud Functions logs showing the names of the VM instances that are turned off.

- **Clean up resources**
- Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instances created, in  **Compute Engine > VM Instances**.
2. Delete the Cloud Storage bucket, in  **Storage > Browser**.
3. Disable the programmatic notifications option for budget alerts.
4. Remove the Cloud Functions feature from  **Cloud Functions**.
5. Remove the topic from Cloud Pub/Sub, in  **Cloud Pub/Sub > Topics**.

-
