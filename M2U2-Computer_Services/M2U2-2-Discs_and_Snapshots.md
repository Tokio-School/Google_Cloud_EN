# **Discs and Snapshots**

Unit M2U2 - Exercise 2

**What are we going to do?**

1. Work with the boot disk of a VM instance.
2. Reassign the boot disk from one instance to a different instance.
3. Assign additional disks to an instance in read-write mode and read-only to multiple instances at once.
4. Extend the buffer storage of a hot-swappable disk.
5. Create disk snapshots manually and automatically and recreate instances from them.
6. Work with local SSDs.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U2-2-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Boot Disks**

In this task, we will explore how to work with &quot;boot&quot; disks for our VM instances.

**Create a VM Instance**

Let's create a VM instance, review the public images available for the boot disk, and mount the boot disk on another VM without deleting the first VM instance:

1. From the navigation menu, navigate to  **Compute Engine > VM Instances**.
2. Click the  **Create Instance**  button to open the wizard.
3. Create an instance with the following characteristics (_Note:_ if no values are specified for a characteristic, this means that you use the default value):
  - Zone: europe-west1-b.
  - Type of machine: e2-micro.
  - Boot Disk: Debian GNU/Linux 10 or 11.
  - Firewall: Allow HTTP and HTTPS traffic.
4. Once the VM instance is created, connect to it via SSH with the console's  **SSH**  button or any of the other paths we've seen before.
5. Let's customise the boot disk by installing the Apache web server:

  1. Update the list of available packages: sudo apt-get update.
  2. Install the Apache web server: sudo apt-get install apache2.
  3. Check the installation and its operation: sudo systemctl status apache2.
  4. Check the local site: curl localhost:80.
 
1. Once you have checked everything, you can close the SSH connection with exit.

Now let's create another VM instance to see how to move the boot disk from the first to the second:

1. Create an instance with the following characteristics:
  - Name: Please use a different name to above.
  - Zone: europe-west1-b.
  - Type of machine: e2-micro.
  - Boot Disk: Debian GNU/Linux 10 or 11.
  - Firewall: Allow HTTP and HTTPS traffic.
2. Connect via SSH to the second instance and check that it is working properly.

We remove the boot disk from the first instance:

1. Stop the first image by selecting it and pressing the STOP  **button**.
2. _QUESTION: Why does it take much longer to stop an instance than to start it? What was the limit for running shutdown scripts on regular instances?_
3. Wait for the instance to stop (_Note_: you can check the notification button on the little bell or number on the right of the blue top bar).
4. Click on the name of the VM instance to access its details page, and click the  **Edit**  button to edit it.
5. In the  **Boot Disk** section, locate the boot disk (_Note:_ It will normally have the same name as the instance) and click the  **X**  button to unlink the boot disk from it.
6. Save the instance changes by pressing the  **Save**  button at the bottom of the page.
7. Check that the boot disk is still available by navigating to  **Compute Engine > Disks**  (or by clicking  **Disks**  in the left column), and by checking the column  **In use by**  for that disk.

_QUESTION: Can the first instance be started without an associated boot disk?_

Mount the boot disk in the second instance, replacing the available one:

1. Navigate to  **Compute Engine > VM Instances**  and stop it.
2. Once stopped, click on the name of the second instance and click on the  **Edit** button.
3. Unlink your boot disk and select the boot disk from the first instance.
4. Save your changes with the  **Save**  button at the bottom of the page.
5. Start the second instance again and connect to it via SSH.
6. The Apache2 web server must be available with the curl localhost:80 command.

This way you have been able to check how to change the boot disk of a VM instance. This disk can be used to create a new instance, swap it for another instance or mount it as an additional persistent disk in another instance.

_DELIVERABLES:_

1. M2U2-2-task_1-file_1-screenshot_1.jpg: Screenshot of the details section of the first instance, showing that it has no boot disk associated.
2. M2U2-2-task_1-file_2-screenshot_2.jpg: Screenshot of the details section of the second instance, showing the associated boot disk.
3. M2U2-2-task_1-file_3-screenshot_3.jpg: Screenshot of the Compute Engine disk section, showing both disks.

**Task 2: Additional persistent disks**

Let&#39;s see how to create additional persistent disks for a VM instance.

**Create an additional persistent disk**

1. Start creating a third instance with the following features, but do not finish creating it:
  - Name: Please use a different name to above.
  - Zone: europe-west1-b.
  - Type of machine: e2-micro.
  - Boot Disk: Debian GNU/Linux 10 or 11.
2. **Administration, Security, Disks, Networks, and Single User \&gt; Disks \&gt; Additional Disks** : Click on  **Add New Disk**  and review the available options.
3. Create an additional persistent disk with the following features:
  - Balanced type.
  - Size = 10 GB.
  - Mode: Reading/Writing:

Let's mount the additional persistent disk onto the instance:

1. Once the instance is created, connect via SSH.
2. Mount the additional persistent disk onto the instance:
  1. List the disks available with the command lsblk.
  2. Find the name of the unmounted disk, e.g. "sdb". We will refer to it as DEVICE_NAME.
  3. Format the disk with the comman sudo mkfs.ext4 -m 0 -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/DEVICE_NAME ([docs](https://cloud.google.com/compute/docs/disks/add-persistent-disk#format_and_mount_linux)).
  4. Create a mount directory in the file system at your preferred location with e.g. the command sudo mkdir -p /mnt/disks/MOUNT_DIRECTORY.
  5. Before continuing, check that it was created correctly with ls /mnt/disks/.
  6. Mount the disk in that directory with the sudo mount -or discard command, defaults/dev/DEVICE_NAME/mnt/disks/MOUNT_DIRECTORY.
  7. Configure write access to the mount directory with the sudo command chmod a+w /mnt/disks/MOUNT_DIRECTORY.
3. Make sure it works properly:
  1. Create a new file, e.g. with the echo command "hello world!" > /mnt/disks/MOUNT_DIRECTORY/sample_file.txt.
  2. Check the new file with the command cat /mnt/disks/MOUNT\_DIRECTORY/sample_file.txt.
4. Configure automatic disk mounting when the instance restarts:
  1. Create a backup of the current fstab configuration: sudo cp /etc/fstab /etc/fstab.backup.
  2. Find the UUID of the additional disk: sudo blkid /dev/DEVICE_NAME. We will refer to it as UUID_ADDITIONAL_DISK
  3. Edit the/etc/fstab file with the text editor of your choice, e.g. Nano: sudo nano /etc/fstab.
  4. Add an additional line/entry with the following information: UUID=UUID_DISCO_ADICONAL /mnt/disks/MOUNT_DIRECTORY ext4 discard,defaults,nofail 0 2 (_note:_ the entry starts with the text UUID= not to be modified, but UUID\_ADDITIONAL \_DISK and MOUNT\_DIRECTORY).
  5. Save the file and verify that it has been edited correctly: cat /etc/fstab.
    1. _Note:_ The operation of saving and closing a file in Nano is done with the steps  **CTRL + X, Y, ENTER**.

Now let's see how you can expand the storage space reserved for a disk without stopping the instance or writes:

1. In the SSH terminal, check the current size of the additional disk with the df -TH command and look for the entry corresponding to/dev/DEVICE_NAME, which will be approx. 10 G.
2. In the web console, navigate to  **Compute Engine > Disks** , select the third instance (current) disk, and click on  **Edit**  (without stopping the instance).
3. Resize the disk to a larger size, such as 50 GB.
4. _QUESTION: What happens if you try to change the size to one smaller than the current one?_
5. For boot disks, in an instance based on Compute Engine public images, the size of the disk file system will be automatically expanded if the instance is restarted. If we do not want to restart the instance, we should follow the next steps. We must follow them as we are working with an additional persistent disk, not a boot disk ([docs](https://cloud.google.com/compute/docs/disks/working-with-persistent-disks#resize_partitions)):
  1. Use the lsblk command to identify the available disks and search for the additional disk.
  2. Use the df -Th command to check how much additional disk storage is available in the file system.
  3. _QUESTION: Is there a discrepancy between the two values?_
  4. To extend the file system, use the command sudo resize2fs /dev/DEVICE_NAME.
  5. Double-check the available storage on that disk with df -Th.
  6. Make sure the file you originally created is still available: cat /mnt/disks/MOUNT\_DIRECTORY/sample_file.txt.

_DELIVERABLES_:

1. M2U2-2-task_2-file_1-screenshot_1.jpg: Screenshot of the instance details section, showing both associated persistent disks.
2. M2U2-2-task_2-file_2-screenshot_2.jpg: Screenshot the SSH terminal with just the result of the commands lsblk, sudo blkid /dev/DEVICE_NAME and cat /etc/fstab (_note:_ you can clear the terminal of other commands with clear).

**Mount an additional persistent disk in multiple instances in read-only mode**

Now let's see how we can mount the same persistent disk in more than one instance in read-only mode. We are currently unable to mount a disk in multiple instances in read/write mode.

1. In the console, navigate to  **Compute Engine > VM Instances** , click on the third instance, and click  **Edit**.
2. Under  **Additional Disks** , change the mode to "read-only" and save the disk and instance changes.

Now mount the disk in the second instance in read mode:

1. In the console, navigate to  **Compute Engine \&gt; VM Instances** , click on the second instance, and click  **Edit**.
2. Under  **Additional disks** , add the additional disk in "read-only" mode and save the changes to the disk and instance (_Note:_ Do not confuse with the previously removed boot disk).
3. Follow the steps above to mount the disc, in this case in the second instance, with the following instructions:
  1. Don't format the disc.
  2. Do not assign read permissions to the mount directory.
4. Finally, make sure the file you originally created is still available: cat /mnt/disks/MOUNT_DIRECTORY/sample_file.txt.

_Note:_ If any problems arise, you can try...

1. Restarting the instances.
2. Dismounting the disk from the first instance and mounting it from the beginning in read mode in both instances.
3. _If all else fails, don&#39;t worry, it&#39;s not always easy to manage disks in Linux, you can move on to the next point._

_DELIVERABLES_:

1. M2U2-2-task\_2-file\_1-screenshot\_1.jpg: Screenshot of the instance details section, showing both associated persistent disks for one instance.
2. M2U2-2-task\_2-file\_1-screenshot\_1.jpg: Screenshot of the instance details section, showing both associated persistent disks for the other instance.
3. M2U2-2-task\_2-file\_5-screenshot\_5.jpg: Screenshot of the SSH terminal with only the result of the commands lsblk, sudo blkid /dev/DEVICE\_NAME and cat /etc/fstab for an instance.
4. M2U2-2-task\_2-file\_6-screenshot\_6.jpg: Screenshot of the SSH terminal with only the result of the commands lsblk, sudo blkid /dev/DEVICE\_NAME and cat /etc/fstab for the other instance.

To unmount the disk and leave the instances ready for the next tasks, repeat these instructions for both instances:

1. Edit the fstab file with sudo nano fstab or another text editor.
2. Remove the mount line from the additional disk, which had this pathway: UUID=UUID\_ADDITIONAL\_DISK /mnt/disks/MOUNT\_DIRECTORY ext4 discard,defaults,nofail 0 2
  1. You can find the UUID of the disk with the commands lsblk and sudo blkid /dev/DEVICE\_NAME.
3. Unmounts the disk with sudo umount /mnt/disk/MOUNT\_DIRECTORY.
4. In the console, navigate to  **Compute Engine \&gt; VM Instances** , edit both instances, and under  **Additional Disks**  unlink the additional disk.

**Task 3: Snapshots**

We are going to see how to work with disk snapshots to perform backups of our disks, both boot disks and additional disks. We can create manual snapshots, automatic or recurring snapshots, and use snapshots to recreate a disk or create an instance from it.

\*Note: To configure automatic mounting of an additional disk, we must modify the/etc/fstab file of an instance, so we must delete this information before creating a snapshot that we would use in the future to create a new instance without that disk.

**Manual Snapshot**

Let&#39;s create a snapshot of the disk:

1. Go to the third instance boot disk page. You can access it from several points:
  1. Navigate to  **Compute Engine \&gt; Disks**  and click on the corresponding disk (same name as the instance), or
  2. Navigate to  **Compute Engine \&gt; VM Instances** , click on the instance and, on the details page, on its boot disk.
2. Create a snapshot of the disk, using the  **Create Snapshot**  button on the disk details page.
  1. _Note:_ In the previous section of the disc listing, you can also find an equivalent button in the  **Actions** column.
3. Browse the page options and create a disk snapshot with the following options:

- Source disk: Boot disk of the third instance.
- Location: multi-regional &quot;eu&quot;.

**Recurring Snapshots**

Now let&#39;s create a recurring snapshot policy and assign it to that disk:

1. Navigate to  **Compute Engine \&gt; Snapshots**  and click  **Create Snapshot Schedule**.
2. Explore the options and create a schedule with the following features:
  - Region: europe-west1.
  - Location: multi-regional &quot;eu&quot;.
  - Daily frequency, from 4:00 to 5:00
3. Now navigate to  **Compute Engine \&gt; Disks** , click on the boot disk of the third snapshot, and then click **Edit**.
4. Under  **Scheduling Snapshots** , assign the created schedule to it and save the changes.

In this way we can create snapshots manually and automatically to make a backup of our data or clone an instance.

_Note:_ We will clone instances from snapshots in the next exercise.

_DELIVERABLES_:

1. M2U2-2-task\_3-file\_1-screenshot\_1.jpg: Screenshot the details section of the manually created snapshot.
2. M2U2-2-task\_3-file\_2-screenshot\_2.jpg: Screenshot the details section of the disk, showing the associated snapshot schedule.

**Task 4: Local SSD Disks**

Now let&#39;s create a new instance with an available local SSD disk. Remember the restrictions on such local SSDs.

Because of these restrictions, only a local SSD disk can be added when creating the instance and cannot be used as a boot disk.

**Instance Creation**

1. Create a new instance with the following characteristics:
  - Name: Please use a different name to above.
  - Zone: europe-west1-b.
  - Machine Type: n1-standard-2 (_Note:_ Local SSDs are not available for E2 or shared core instances).
  - Boot Disk: Debian GNU/Linux 11.
  - **Management, security, disks, networks and single user \&gt; Disks** : Adds 1 NVMe local SSD (with 375 GB/disk).

As for any other disk, we must first format and mount the disk, following steps similar to the previous ones:

1. Once the instance is created, connect to it via SSH.
2. Format the local SSD disk:
  1. List the disks available with the command lsblk.
  2. Find the name of the unmounted disk, e.g. &quot;nvme0n1&quot;. We will refer to it as DEVICE\_NAME.
  3. Format the disk with the command sudo mkfs.ext4 -F /dev/DEVICE\_NAME ([docs](https://cloud.google.com/compute/docs/disks/local-ssd#formatindividual)).
3. Mount the additional persistent disk onto the instance:
  1. Create a mount directory in the file system at your preferred location with e.g. the command sudo mkdir -p /mnt/disks/MOUNT\_DIRECTORY.
  2. Before continuing, check that it was created correctly with ls /mnt/disks/.
  3. Mount the disk in that directory with the sudo mount /dev/DEVICE\_NAME/mnt/disks/MOUNT\_DIRECTORY.
  4. Configure write access to the mount directory with the sudo command chmod a+w /mnt/disks/MOUNT\_DIRECTORY.
4. Configure automatic disk mounting when the instance restarts:
  1. Create a backup of the current fstab configuration: sudo cp /etc/fstab /etc/fstab.backup.
  2. Find the UUID of the additional disk: sudo blkid /dev/DEVICE\_NAME. We will refer to it as UUID\_ADDITIONAL\_DISK
  3. Edit the/etc/fstab file with the text editor of your choice, e.g. Nano: sudo nano /etc/fstab.
  4. Add an additional line/entry with the following information: UUID=UUID\_DISCO\_ADICONAL /mnt/disks/MOUNT\_DIRECTORY ext4 discard,defaults,nofail 0 2 (_note:_ the entry starts with the text UUID= not to be modified, but UUID\_ADDITIONAL \_DISK and MOUNT\_DIRECTORY).
  5. Save the file and verify that it has been edited correctly: cat /etc/fstab.
    1. _Note:_ The operation of saving and closing a file in Nano is done with the steps  **CTRL + X, Y, ENTER**.
5. Make sure it works properly:
  1. Create a new file, e.g. with the echo command &quot;hello world!&quot; \&gt; /mnt/disks/MOUNT\_DIRECTORY/sample\_file.txt.
  2. Check the new file with the command cat /mnt/disks/MOUNT\_DIRECTORY/sample\_file.txt.

**Usual workflow**

Since the information on a local SSD disk does not persist if the instance is stopped, if the instance suffers a problem and cannot be recovered we may lose the information on it. Therefore, such disks are only used as scratch disks while the operations are running and then the information is copied to a persistent system such as a persistent disk or Cloud Storage.

To simulate this workflow, follow these steps:

1. Returns the SSH terminal to the new instance.
2. Create an original file on the persistent disk of the instance, which will simulate a file to be processed, e.g. with the echo command &quot;data to be processed&quot; \&gt; $HOME/sample\_file.txt.
3. Copy the data file to be processed to the local SSD: cp $HOME/sample\_file.txt /mnt/disks/MOUNT\_DIRECTORY/sample\_file.txt.
4. Simulate the processing of such data on the scratch disk by adding a new line to the same echo &quot;processed data&quot; \&gt;\&gt; /mnt/disks/MOUNT\_DIRECTORY/sample\_file.txt.
5. Check the processed file: cat /mnt/disks/MOUNT\_DIRECTORY/sample\_file.txt.
6. After data processing is complete, copy the file to the persistent disk: cp /mnt/disks/MOUNT\_DIRECTORY/sample\_file.txt $HOME/sample\_file.txt
7. Check the file with the processed data: cat $HOME/sample\_file.txt.

_DELIVERABLE:_ M2U2-2-task\_4-file\_1-screenshot\_1.jpg: Screenshot the details section of the instance, showing the persistent boot disk and the local SSD disk.

**Simulated VM Stop**

As we discussed, an instance with a local SSD disk cannot stop, and if it stops due to an unexpected problem, it will start again, so we cannot &quot;restart&quot; an instance.

We can simulate a hardware problem or any other that would stop the instance by stopping it from the OS:

1. On the SSH terminal of the instance, stop it with the command sudo shutdown now.
  1. _Note:_ Stopping the instance will immediately lose the SSH connection to it.
2. In the console, in the  **Compute Engine \&gt; VM Instances** section, you will see how the new instance appears stopped and cannot be restarted.
  1. _Note:_ Sometimes, it will take a while to show this status. Refresh the page in the console, reload the page, enter the instance details, etc., and it will finally display itself accordingly.

**Deliverables Summary**

1. M2U3-3-questions.txt: Answers to all questions raised in the exercise.
2. M2U2-2-task\_1-file\_1-screenshot\_1.jpg: Screenshot of the details section of the first instance, showing that it has no boot disk associated.
3. M2U2-2-task\_1-file\_2-screenshot\_2.jpg: Screenshot of the details section of the second instance, showing the associated boot disk.
4. M2U2-2-task\_1-file\_3-screenshot\_3.jpg: Screenshot of the Compute Engine disk section, showing both disks.
5. M2U2-2-task\_2-file\_1-screenshot\_1.jpg: Screenshot of the instance details section, showing both associated persistent disks.
6. M2U2-2-task\_2-file\_2-screenshot\_2.jpg: Screenshot the SSH terminal with just the result of the commands lsblk, sudo blkid /dev/DEVICE\_NAME and cat /etc/fstab (_note:_ you can clear the terminal of other commands with clear).
7. M2U2-2-task\_2-file\_1-screenshot\_1.jpg: Screenshot of the instance details section, showing both associated persistent disks for one instance.
8. M2U2-2-task\_2-file\_1-screenshot\_1.jpg: Screenshot of the instance details section, showing both associated persistent disks for the other instance.
9. M2U2-2-task\_2-file\_5-screenshot\_5.jpg: Screenshot of the SSH terminal with only the result of the commands lsblk, sudo blkid /dev/DEVICE\_NAME and cat /etc/fstab for an instance.
10. M2U2-2-task\_2-file\_6-screenshot\_6.jpg: Screenshot of the SSH terminal with only the result of the commands lsblk, sudo blkid /dev/DEVICE\_NAME and cat /etc/fstab for the other instance.
11. M2U2-2-task\_3-file\_1-screenshot\_1.jpg: Screenshot the details section of the manually created snapshot.
12. M2U2-2-task\_3-file\_2-screenshot\_2.jpg: Screenshot the details section of the disk, showing the associated snapshot schedule.
13.  M2U2-2-task\_4-file\_1-screenshot\_1.jpg: Screenshot the details section of the instance, showing the persistent boot disk and the local SSD disk.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. In  **Compute Engine \&gt; VM Instances** , delete all instances created.
2. In  **Compute Engine \&gt; Snapshots** , delete all snapshots and snapshot scheduling.
3. In **Computer Engine \&gt; Disks** , make sure that all disks have been removed when deleting instances, and delete the ones that have not.
