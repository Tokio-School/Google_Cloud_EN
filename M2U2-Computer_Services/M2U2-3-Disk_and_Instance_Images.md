# **Disk and Instance Images**

Unit M2U2 - Exercise 3

**What are we going to do?**

1. Create disk images from a disk and a snapshot.
2. Clone an instance from a disk image.
3. Create an instance image and use it to recreate an instance from an instance image.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_There are no questions for this exercise._

**Task 1: Disk Images**

Let&#39;s find out how to work with disk images to recreate disks and instances.

**Create a custom VM Instance**

The first thing we&#39;ll need is a base custom VM instance:

1. From the navigation menu, navigate to  **Compute Engine > VM Instances**.
2. Click the  **Create Instance**  button to open the wizard.
3. Create an instance with the following characteristics (_Note:_ if no values are specified for a characteristic, this means that you use the default value):
  - Zone: europe-west1-b.
  - Type of machine: e2-micro.
  - Boot Disk: Debian GNU/Linux 11.
4. Once the VM instance is created, connect to it via SSH with the console's  **SSH**  button or any of the other paths we've seen before.
5. Let's customise the disk by creating a file on it: echo "hello world!" > $HOME/sample_file.txt.
6. Once you have checked everything, you can close the SSH connection with exit.

**Create an instance snapshot**

We are going to create a snapshot of the instance, as we did in the previous exercise, in order to use it when working with disk images:

1. Go to the instance boot disk page. You can access it from several points:
  1. Navigate to  **Compute Engine > Disks**  and click on the corresponding disk (same name as the instance), or
  2. Navigate to  **Compute Engine > VM Instances** , click on the instance and, on the details page, on its boot disk.
2. Create a snapshot of the disk, using the  **Create Snapshot**  button on the disk details page.
  1. _Note:_ In the previous section of the disc listing, you can also find an equivalent button in the  **Actions** column.
3. Create a disk snapshot with the following options:

- Source disk: Boot disk of the third instance.
- Location: multi-regional "eu".

_DELIVERABLE:_ M2U2-3-task_1-file_1-screenshot_1.jpg: Screenshot of snapshot section, showing the snapshot created.

**Create a disk image from the disk**

Let's create a disk image from the instance boot disk. With this disk image we can create new disks and instances with the same configuration and data to recreate, clone or move the instance:

1. In the console, navigate to  **Compute Engine > Images**  and select  **Create Image**.
2. Creates an image from the instance disk with the following characteristics:
  - Source: Instance boot disk.
  - Location: multi-regional "eu".
  - Family: image-app.
  - Continue running instance.

With these simple steps, after a couple of minutes we will have our disk image available.

**Create a disk image from the snapshot of the instance**

Now let&#39;s see how we can also create an image from a disk snapshot:

1. In the console, navigate to  **Compute Engine > Images**  and select  **Create Image**.
2. Creates an image from the instance disk with the following characteristics:
  - Name: Please give a different name to above.
  - Source: the snapshot previously created.
  - Location: multi-regional "eu".
  - Family: image-app.

We will then have a second image available of the same family, in this case with the same information.

Image families can be used to store multiple related images in a versioned form, e.g. from the same application.

_DELIVERABLE:_ M2U2-3-task_1-file_2-screenshot_2.jpg: Screenshot the images section, showing the created images.

**Move the instance to another zone**

The operation of moving an instance to another zone can be done manually or automatically ([docs](https://cloud.google.com/compute/docs/instances/moving-instance-across-zones)).

To move a running instance from one zone to another, follow these steps:

1. Activate Cloud Shell in the console (make sure you have the project active) or use Cloud SDK locally.
2. To move the instance, simply run the gcloud compute instances move INSTANCE\_NAME --zone EUROPE-WEST1-B --destination-zone europe-west1-c command.
3. The operation will take a couple of minutes. You can check its status in the console, in  **Compute Engine > VM Instances**.
4. Once completed, connect to it via SSH.
5. Check the customisation done previously with echo $HOME/sample_file.txt.
6. Once you have checked everything, you can close the SSH connection with exit.

With this simple option we can move a VM instance to a destination in the same region.

**Recreate the instance in another region**

To "move" or re-create the instance in another region or project, we need to follow the manual re-creation steps:

1. To move a region instance in the same project, we can recreate the instance from a snapshot or image.
2. To move a project instance (regardless of the source and target zone and region), we can only recreate the instance from an image, since snapshots cannot be shared with other projects.

To recreate the instance in another region, follow these steps:

1. From the navigation menu, navigate to  **Compute Engine > VM Instances**.
2. Click the  **Create Instance**  button to open the wizard.
3. Create an instance with the following characteristics:
  - Zone: europe-west3-c.
  - Type of machine: e2-micro.
  - Boot disk: based on the previously created snapshot.
  - _Note_: note that when selecting a boot disk, you can select from the custom images from this or other projects to which you have access.
4. Once the VM instance is created, connect to it via SSH with the console's  **SSH**  button or any of the other paths we've seen before.
5. Check the customisation done previously with echo $HOME/sample_file.txt.
6. Once you have checked everything, you can close the SSH connection with exit.

This way we can recreate an instance in any location or project from a disk image.

_DELIVERABLE:_ M2U2-3-task_1-file_3-screenshot_3.jpg: Screenshot the instances section, showing the moved and recreated instances.

**Task 2: Machine Images**

Machine images are different from disk images, since they combine instance information and snapshots of all disks. They are used to recreate instances with all the available information, unlike images or _disk_ snapshots, which do not keep the instance settings, only the information of the disks individually.

**Create a machine image**

We are going to create a machine image from the original _machine_ or VM instance in a few simple steps:

1. In the console, navigate to  **Compute Engine > Machine Images**.
2. Select  **Create Machine Image**  and create a machine image with the following features:
  - Source VM Instance: Pre-created VM instance.
  - Location: multi-regional "eu".
3. The machine image will take a couple of minutes to create, as it also has to take a snapshot of the instance disks.

Once created, we will have a machine image available to recreate, clone or move the instance with all the information incorporated, more quickly and directly.

_DELIVERABLE:_ M2U2-3-task_2-file_1-screenshot_1.jpg: Screenshot of the machine images section, showing the created machine image.

**Recreate the instance from the machine image**

To recreate an instance from a machine image, we can do it with the same original configuration or by substituting some properties, if for example the same original instance is still running in the same zone or if we want to recreate it in another region, where we have to change the regional properties (subnet, etc.).

To recreate an instance by modifying some properties, follow these steps:

1. Navigate to  **Compute Engine > VM Instances**  and click on  **Create New Instance**.
2. In the left column, select  **New VM Instance from a machine image**.
3. Select your machine image and change the following properties:
  - Name (_note:_ although 2 instances in different zones may have the same name, we will change it for clarity).
  - Region: europe-west3.
4. Create the instance.

With these simple steps we can recreate a VM instance in another region or recover an instance in case of any problem in the fastest and most direct way possible.

_DELIVERABLE:_ M2U2-3-task_2-file_2-screenshot_2.jpg: Screenshot the instances section, showing the instances created.

**Deliverables Summary**

1. M2U2-3-task_1-file_1-screenshot_1.jpg: Screenshot of snapshot section, showing the snapshot created.
2. M2U2-3-task_1-file_2-screenshot_2.jpg: Screenshot the images section, showing the created images.
3. M2U2-3-task_1-file_3-screenshot_3.jpg: Screenshot the instances section, showing the moved and recreated instances.
4. M2U2-3-task_2-file_1-screenshot_1.jpg: Screenshot of the machine images section, showing the created machine image.
5. M2U2-3-task_2-file_2-screenshot_2.jpg: Screenshot the instances section, showing the instances created.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. In  **Compute Engine > VM Instances** , delete VM instances created.
2. In  **Compute Engine > Disks** , make sure the disks have been deleted.
3. In  **Compute Engine > Machine Images** , delete the machine image created.
4. In  **Compute Engine > Snapshots** , delete the snapshot created.
5. In  **Compute Engine > Images** , delete the disk image created.
