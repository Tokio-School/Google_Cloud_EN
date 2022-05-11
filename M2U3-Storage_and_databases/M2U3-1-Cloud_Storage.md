# **Cloud storage**

Unit M2U3 - Exercise 1

**What are we going to do?**

1. Create a bucket in Cloud Storage.
2. Manage objects from the web console.
3. Manage objects and synchronise directories from the command line.
4. Manage Cloud Storage access control lists.
5. Set life cycle rules.
6. Enable object versioning.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U3-1-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Manage buckets and objects**

In this first task we will create a bucket and manage objects within it.

**Create a bucket from the console**

Let&#39;s create a bucket from the console:

1. In the console, navigate to  **Storage > Browser**  and select  **Create Bucket**.
2. Explore all options and create a bucket with the following features:
  - Name: Choose a globally unique name, such as your project ID plus a suffix such as -bucket. We will refer to it as BUCKET_NAME.
  - Location: regional in europe-west1.
  - Default storage class: standard.
  - Disable the prevention of public access to the bucket.
  - Access control: accurate.

This way we can create GCS buckets from the web console.

**Manage objects from the console.**

To upload a file to the bucket using the console:

1. In the console, under  **Storage > Browser** , click on your bucket.
2. Select  **Upload files**  and select any local file.
3. Once uploaded, you can click on the name of the object and see its information. If it is an image, it will be displayed below its features.

To download the object you can do it:

1. In the list of objects, with the down arrow button to the right of the object line, or by selecting it and clicking on  **Download**.
2. Within the object details page, click on  **Download**.

To delete the object, you can:

1. In the list of objects, select it and press  **Delete**.
2. Within the object details page, click on  **Delete**.

This allows us to upload files and folders to Cloud Storage, manage them and delete them.

**Manage objects with gsutil**

Now let's manage objects from the command line, using the gsutil tool:

1. Create an object locally: echo "hello world!" > sample_file.txt.
2. Upload the file to the bucket: gsutil cp sample_file.txt gs://BUCKET_NAME/sample_file.txt.
3. List the buckets created: gsutil ls.
4. Lists the objects within the bucket: gsutil ls gs://BUCKET_NAME/.
5. Download the object with another name: gsutil cp gs://BUCKET_NAME/sample_file.txt downloaded_file.txt.
6. Remove the bucket object: gsutil rm gs://BUCKET_NAME/sample_file.txt.

This allows us to manage the buckets and objects from the command line.

**Synchronise Directories**

Finally we will use the command line to synchronise files and subdirectories between local and Cloud Storage:

1. Create a local file structure:
  1. Rename the previously created file: mv downloaded_file.txt sample_file1.txt.
  2. Create a subdirectory: mkdir sample_dir.
  3. Create 2 files within the subdirectory: touch sample_dir/sample_file2.txt and touch sample_dir/sample_file3.txt.
2. Synchronise files with gsutil -m rsync . gs://BUCKET_NAME.
3. Check the files in Cloud Storage:
  1. With gsutil: gsutil ls gs://BUCKET_NAME.
  2. In the console, in  **Storage >Browser** , click on the name of the bucket.
4. Now delete one of the files in Cloud Storage.
5. Synchronise changes from Cloud Storage to local: gsutil rsync gs://BUCKET_NAME..
6. Check your synced changes locally.

This way we can synchronise subdirectories and files between on-premises and Cloud Storage.

_DELIVERIES:_ 
M2U3-1-task_1-file_1-screenshot_1.jpg: Screenshot of the list of objects in the GCS bucket.

**Task 2: Manage ACLs at the granular level**

In this task we will manage the access control lists (or "ACLs") of the GCS buckets and objects.

To control access we have 2 options at the bucket level:

- Uniform: ACLs will be applied uniformly to all objects in the bucket.
- Precise: ACLs can be applied individually to each object in the bucket.

**At bucket level**

Let's edit the permissions at the bucket level:

1. In the console, under  **Storage > Browser** , click on your bucket.
2. Select the  **Permissions** tab.
3. Explore the permissions granted. For example, identities with the assigned roles of "project/owner" and "project/editor" will have read and write access, and with "project/viewer" read access.
4. To edit ACLs, you can click  **Add** , press the edit button (pencil icon), or select an identity and delete it.

Try adding a different Google Account, if you have one, to check your access.

This way we can control the access permissions at bucket level in GCS.

**At object level**

Now let's repeat the operation at an individual object level:

1. Within the Bucket  **Objects**  view, click on any of the uploaded files.
2. On the object details page, select  **Modify permissions**.

Like before, try adding a different Google Account, if you have one, to check your access.

This way we can control the access permissions at an individual object level in GCS.

**Make object public**

Now we're going to make an object public, which will anyone to see it. This is useful if we want to serve objects from GCS such as static web files (CSS, images, JS, etc.) or host downloadable files of any kind.

1. On the object details page, select  **Modify permissions**.
2. Add an entry and select  **Public**  and  **allUsers**.
3. We can make a public object for any unauthenticated user (such as a static web file) with **allUsers** or only for authenticated requests, which will allow us to log who has accessed the file, with  **allAuthenticatedUsers**.
4. Remember to select  **Read-only access**  with  **Reader**.

This will generate a new API endpoint to access the file. Check both endpoints:

1. Public URL.
2. Authenticated URL.

_QUESTION: What are the differences between the two URLs? And after accessing the link?_

Try opening both links in a new incognito window: _QUESTION: What happens when you access each of them?_

Now edit the permissions again and remove the entry from  **allUsers**._QUESTION: What Happens to the URLs? What happens when you access each of them?_

**Task 3: Lifecycle rules and object versioning**

In this task we will work with the automatic life cycle rules and the versioning of the objects in GCS:

**Life cycle rules.**

Let's set a lifecycle rule for the sample bucket:

- The object is created in the default standard storage class.
- 30 days later, it migrates to nearline.
- 60 days later, it migrates to coldline.
- 90 days later, it migrates to archive.
- 2 years after its creation, it is automatically deleted.

1. In the console, on the bucket details page, activate the **Lifecycle** tab.
2. Click on  **Add a rule**  and explore the different options.
3. Create 4 rules to apply these changes, taking into account the following conditions for applying the rules:
  - Only to files of the corresponding storage class.
  - Depending on how long it has been since the file was created.

We can also check and set the rules with gsutil:

1. Download the lifecycle policy in JSON: gsutil lifeecycle get gs://BUCKET_NAME format.
2. Check the contents of the file: cat filename.json.

_DELIVERABLES:_ 
M2U3-1-task_3-file_1-screenshot_1.jpg: Screenshot of GCS bucket lifecycle rules listing.

**Enable object versioning**

Now let's work with GCS versioned objects.

First we are going to enable object versioning, which cannot be done from the console:

1. Enable versioning: gsutil versioning set on gs://BUCKET_NAME.
2. Check that it is activated correctly: gsutil versioning get gs://BUCKET_NAME.

Now let's upload and manage versions of objects in the bucket:

1. 1.	Create a new file locally and upload it to Cloud Storage: echo "hello world - v1" > sample_versioned_file.txt and gsutil cp sample_versioned_file.txt gs://BUCKET_NAME/.
2. Check that it is working:
  1. List the uploaded objects: gsutil ls gs://BUCKET_NAME.
  2. List all versions of the object: gsutil ls -a gs://BUCKET_NAME (the generation number of each version is shown after #).
  3. Check the contents of the current live object: gsutil ls gs://BUCKET_NAME/sample_versioned\_file.txt.
3. Now repeat the steps to upload a new object, changing v1 to v2, and check the uploaded objects, versions, and the current live object.
4. Write down the v1 version generation number and access it: gsutil cat gs://BUCKET_NAME/sample_versioned_file.txt#NUMBER_GENERATION.
5. Now upload a new v3 version and double-check the uploaded objects, versions and the current live object.
6. Access v1 and v2 versions.
7. Remove version v1: gsutil rm gs://BUCKET_NAME/sample_versioned_file.txt#NUMBER_GENERATION.
8. Check the uploaded objects, versions, and the current live object.
9. Restore version v2 as the current object: gsutil cp gs://BUCKET_NAME/sample_versioned_file.txt#NUMBER_GENERATION gs://BUCKET_NAME/sample_versioned_file.txt.
10. Double-check uploaded objects, versions, and the current live object.

Now enable an additional lifecycle rule to only keep the latest 3 versions:

- Action: Delete the object.
- Condition: Number of latest versions, 3.

Finally, let's check that only those last 3 versions are kept:

1. Go back to the terminal and verify how many current versions there are of your object: gsutil ls -a gs://BUCKET_NAME.
2. Upload a couple of files with v4 and v5 respectively.
3. Check the current live file and how many versions of the object are kept.

This allows us to keep and manage multiple versions of an object and use a lifecycle rule to delete older versions.

_DELIVERIES:_ 
M2U3-1-task_3-file_2-screenshot_2.jpg: Screenshot of the object's version list.

**Deliverables Summary**

1. M2U3-1-questions.txt: Answers to all questions raised in the exercise.
2. M2U3-1-task_1-file_1-screenshot_1.jpg: Screenshot of the list of objects in the GCS bucket.
3. M2U3-1-task_3-file_1-screenshot_1.jpg: Screenshot of GCS bucket lifecycle rules listing.
4. M2U3-1-task_3-file_2-screenshot_2.jpg: Screenshot of the object's version list.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the GCS bucket.
2. (Optional) Delete local objects.
