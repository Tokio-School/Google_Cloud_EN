# **Cloud Storage Access Control**

Unit M2U5 - Exercise 3

**What are we going to do?**

1. Control access with the ACLs at the bucket and object level.
2. Allow temporary access with signed URLs.
3. Allow file uploads from a static web form.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U0-0-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Control access with ACLs**

**Create Cloud Storage Bucket and Service Account**

- Create Service Account
- Check permissions to SA
- Create GCS bucket

**At bucket level**

- check access
- switch with console
- switch with gsutil
- check with gsutil impersonating "gsutil -i "[service-account@google.com](mailto:service-account@google.com) " ls gs://pub"

**At object level**

- check access
- switch with console
- switch with gsutil
- check with gsutil impersonating "gsutil -i "[service-account@google.com](mailto:service-account@google.com) " ls gs://pub"

**Task 2: Signed URLs**

- create signed URL
- create VMs with scope without GCS
- upload object to GCS
- check access to object with signed URL

**Task 3: Signed Policy Papers**

- create signed policy doc
- create local web with sample HTML
- check object upload

**Deliverables Summary**

1. M2U0-0-questions.txt: Answers to all questions raised in the exercise.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the Cloud Storage bucket created.
2. Delete the service account created.
3. Delete the website in the local environment.
