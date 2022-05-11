# **Cloud Filestore**

Unit M2U3 - Exercise 2

**What are we going to do?**

1. Mount a Cloud Filestore instance on multiple VM instances at the same time.
2. Copy data from Cloud Storage and local to Cloud Filestore.
3. Work with Cloud Filestore backups.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Mount a Cloud Filestore instance on multiple VM instances**

We are going to create a Cloud Filestore instance and mount it on 2 VMs instances in read/write mode.

**Create a Cloud Filestore Instance**

Let's start by creating a Cloud Filestore instance:

1. In the console, navigate to  **Filestore > Instances**  and click  **Create Instance**.
2. Explore the options and create an instance with the following features:
  - Instance ID: nfs-instance.
  - Instance Type: Basic.
  - Storage type and capacity: 1TB HDD.
  - Zone: europe-west1-b.
  - Assigned IP Range: Automatic.
  - Shared file/volume name: nfs_volume.
  - Access control: to all VPC customers.

**Create VM Instances**

Create 2 VM instances with the following features: - Zone: europe-west1-b. - Machine type: e2-micro. - Boot disk: based on Debian 11. - Service account: default GCE service account. - Access permission: configure access for each API with read/write access for Google Storage.

**Mount the Cloud Filestore instance as NFS on the 2 VM instances**

Now connect to both instances via SSH and mount the Cloud Filestore volume on both:

1. Install NFS: sudo apt-get update and sudo apt-get install -y nfs-common.
2. Create a directory as a mount point: sudo mkdir -p /mnt/nfs.
3. Mount the instance in that directory: sudo mount IP_file:/nfs_volume /mnt/nfs.
4. Enable read/write permissions in the mount directory: sudo chmod go+rw /mnt/nfs.

So, we have created a Cloud Filestore NFS instance and mounted it in R/W mode on 2 VM instances.

**Check joint access**

Now let's check the shared access to the NFS volume from both VM instances:

1. Connect to the first instance by SSH (we recommend using the SSH button on the console and leaving the window open).
2. Create a file in the shared NFS: echo "hello world!" > /mnt/nfs/sample_file.txt.
3. Check the reading from the first instance: cat /mnt/nfs/sample_file.txt.
4. Connect to the second instance (again we recommend using the SSH button on the console and leaving the window open alongside the first).
5. Check the reading from the second instance: cat /mnt/nfs/sample_file.txt.
6. 6.	Create a new file in the shared NFS from the second instance: echo "hello world! 2" > /mnt/nfs/sample_file2.txt
7. Check the reading from the first instance: cat /mnt/nfs/sample_file2.txt.

This allows us to check access to an NFS shared between both instances.

_DELIVERIES:_ 
M2U3-2-task_1-file_1-screenshot_1.jpg: Screenshot of Cloud Filestore instance detail page.

**Task 2: Copy data from on-premises and Cloud Storage to shared NFS**

Now let's see how to copy files between on-premises, Cloud Storage and shared NFS.

**Copy files from local**

_Note:_ You can follow the instructions below in Cloud Shell or on a local Cloud SDK installation.

We will copy files from a local environment to the shared NFS directly:

1. C1.	Create a local file: echo "hello world!" > local_file.txt.
2. Copy it to the NFS via one of the instances: gcloud compute scp local_file.txt INSTANCE_FIRST_NAME:/mnt/nfs --zone=europe-west1-b.
3. Connect to the second instance via SSH and check the shared NFS content: cat /mnt/nfs/local_file.txt.

This allows us to copy files to the NFS through any of the instances where it is mounted.

**Create files in Cloud Storage**

Now let's create a bucket to sync with the shared NFS:

1. Create a GCS bucket with the following features:
  - Name: globally unique name.
  - Location: europe-west1 region.

**Sync files to and from Cloud Storage**

Now let's sync directories to and from GCS:

1. Connect to one of the instances via SSH.
2. Synchronise the NFS content with the GCS bucket: gsutil -m rsync /mnt/nfs/ gs://BUCKET_NAME.
3. Check the contents of the bucket: gsutil ls gs://BUCKET_NAME.
4. Modify the contents of the local_file.txt file:
  1. Do it from the environment where you originally created it, or
  2. Download the file: gsutil cp gs://BUCKET_NAME/local_file.txt..
  3. Modify the content.
  4. Copy it back to the bucket: gsutil cp local\_file.txt gs://BUCKET_NAME/local_file.txt.
5. Re-synchronise the NFS content with the GCS bucket in the opposite direction: gsutil -m rsync gs://BUCKET_NAME/mnt/nfs/.
6. From the other instance, check the shared NFS content: ls /mnt/nfs.

This way we can synchronise directories between the shared NFS and a GCS bucket in both directions.

_DELIVERIES._

1. M2U3-2-task_2-file_1-screenshot_1.jpg: Screenshot of an SSH connection to the first instance showing the contents of the/mnt/nfs directory.
2. M2U3-2-task_2-file_2-screenshot_2.jpg: Screenshot of an SSH connection to the second instance showing the contents of the/mnt/nfs directory.
3. M2U3-2-task_2-file_3-screenshot_3.jpg: Screenshot of an SSH connection to one of the second instances showing the contents of the bucket.

**Task 3: Working with Cloud Filestore backups**

In this task we will work on creating and restoring Cloud Filestore backups.

**Create an instance backup**

Let's create a backup with the shared NFS information:

1. In the console, navigate  **to Filestore > instances** , and for the instance created, in the 3 vertical dots menu, click  **Create backup**.
2. Explore the different options and create a backup with the following features:
  1. Region: europe-west1.
3. Once the backup is created, it will appear in **Filestore > backups**.

This way we can create a backup of the instance with a backup copy of the information.

**Restore a backup to an instance**

Now let's restore a backup to the Cloud Filestore instance created:

First we will modify the content of the NFS to check the changes:

1. Access any of the VM instances via SSH.
2. Create a new file to check if it appears after recovering the backup echo "new information" > /mnt/nfs/new_file.txt.
3. Check the contents of the file: cat /mnt/nfs/new_file.txt.

Now let's restore the backup to the instance already created:

1. In the console, navigate to  **Filestore > Instances**  and click on the instance.
2. Click  **Restore from a backup**  and select the backup you created.
3. From an SSH connection to one of the VM instances, check the contents of the shared NFS: cat /mnt/nfs/new_file.txt (the file should not exist).

This way we can restore the information from a backup.

**Deliverables Summary**

1. M2U3-2-task_1-file_1-screenshot_1.jpg: Screenshot of Cloud Filestore instance detail page.
2. M2U3-2-task_2-file_1-screenshot_1.jpg: Screenshot of an SSH connection to the first instance showing the contents of the/mnt/nfs directory.
3. M2U3-2-task_2-file_2-screenshot_2.jpg: Screenshot of an SSH connection to the second instance showing the contents of the/mnt/nfs directory.
4. M2U3-2-task_2-file_3-screenshot_3.jpg: Screenshot of an SSH connection to one of the second instances showing the contents of the bucket.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Remove the Cloud Filestore instance and backup.
2. Delete the VM instances created.
