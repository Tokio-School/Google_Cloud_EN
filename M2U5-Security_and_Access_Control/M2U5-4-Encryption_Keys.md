# **Encryption Keys**

Unit M2U5 - Exercise 4

**What are we going to do?**

1. Use customer-managed encryption keys (CMEK) for Cloud Storage and persistent disks.
2. Use customer-supplied encryption keys (CSEKs) for Cloud Storage and persistent disks.
3. Use the API to encrypt and decrypt information.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U0-0-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Create customer-managed encryption keys**

- Enable API
- Create keyring
- Create 2 keys with automatic rotation

**Task 2: Customer-managed Encryption Keys (CMEK)**

**CMEK in Cloud Storage**

- give access to GCS SA to use key 1
- create bucket and set CMEK
- upload object
- check the encryption key they are using
- encrypt object with 2nd key and upload with gsutil
- check with "gsutil ls -L gs://file_path"
- rotate key 1 manually

**CMEK on persistent disks**

- create disk and use CMEK
- create vm and allocate disk
- check - write data and read

**Task 3: Customer-Supplied Encryption Keys (CSEK)**

**Generate Keys**

- generate CSEK - name includes bucket and pseudo-random number
- save key to file

**CSEK in Cloud Storage**

- create .boto file and update with key
- upload object to GCS and list
- delete, download and check locally
- generate 2nd key
- rotate keys in .boto
- upload 2nd object and list
- delete, download and check locally
- re-encrypt 1st object and re-upload
- remove CSEK and check access to objects

**CSEK on persistent disks**

- key wrap
- create disk with CSEK wrapped
- create instance with CSEK wrapped
- take screenshot with CSEK wrapped

**Task 4: Encrypt and decrypt information with the API**

- encrypt with API
- decrypt with API

**Deliverables Summary**

1. M2U0-0-questions.txt: Answers to all questions raised in the exercise.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instance and persistent disks.
2. Delete the GCS bucket.
3. Delete objects in the local environment.
4. Delete the Cloud KMS encryption keys and the .boto file.
