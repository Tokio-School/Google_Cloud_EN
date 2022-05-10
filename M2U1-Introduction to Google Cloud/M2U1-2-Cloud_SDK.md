# **Cloud SDK**

Unit M2U1 - Exercise 2

**What are we going to do?**

1. Install and initialise Cloud SDK locally.
2. Manage components.
3. Create settings.
4. Connect to and mount Cloud Shell locally.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Install and boot Cloud SDK locally.**

In this task we will install and boot Cloud SDK on your local PC. For this you can use any OS: Linux, macOS or Windows. If you prefer not to install anything on your local PC, you can skip the steps needed to perform a local installation and follow the remaining steps using Cloud Shell. The following exercises will always use Cloud Shell, although you can also follow them from the local Cloud SDK if you install the necessary tools and language runtimes.

You can also follow the steps using...

1. [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) for Windows (recommended).
2. A local Linux VM, and delete it after finishing.

**Install Cloud SDK**

To install Cloud SDK, follow the instructions for your OS in the [documentation](https://cloud.google.com/sdk/docs/install) (review the notes below before).

_NOTES:_

1. _Don't boot Cloud SDK yet, we'll do this on the next step._
2. _For Debian/Ubuntu you can also follow the general instructions for Linux if you prefer to manage the Cloud SDK components manually with the Cloud SDK versioning._
3. _For Linux and macOS, we recommend following the main step to add the Cloud SDK to the $PATH._
4. _Compute Engine VM instances will already have Cloud SDK installed_.

**Start and authenticate in Cloud SDK**

_NOTE: You can follow these instructions with Cloud Shell._

Once the Cloud SDK is installed locally, we must initialise it as a first step.

Initialising it will create a base configuration for the CLI tools and authenticate us with our user account ([docs](https://cloud.google.com/sdk/docs/initializing)).

Initialise the Cloud SDK with the command gcloud init, following the instructions in the terminal.

_NOTE: If you are asked for a default region and zone for Compute Engine, use the europe-west1 region and the europe-west1-b zone._

At the end, check your configuration and authentication, with the gcloud config list or gcloud auth list command.

**Task 2: Manage components and configurations**

_NOTE: You can follow these instructions with Cloud Shell._

In this task we will install and update Cloud SDK components and see how to create and manage various default configurations.

**Manage components.**

Cloud SDK consists of multiple components, such as CLI tools, emulators, plugins for other tools, etc.

Follow the instructions below to manage them:

1. To quickly check the currently installed version of Cloud SDK and its components, use the gcloud --version command.
2. Check the installed and available components with the gcloud components list command.
3. Update the installed components with the gcloud components update command (this may take a few minutes).
4. Install some new component(s) with the gcloud components install COMPONENT_ID command.
5. Check the available components again with the gcloud components list command.

**Manage Settings**

Cloud SDK uses different settings to maintain default values. Settings are useful if you need to work on different projects, with different authentications, etc. ([docs](https://cloud.google.com/sdk/docs/configurations))

To work with multiple projects and/or users, an alternative is Cloud Shell, as the instance is independent for each user and by selecting a project in the console or in the Cloud Shell session tab we can change the active project for our commands.

To manage the available settings, follow the instructions below:

1. Lists the configurations available with the gcloud config configurations list command (only one will be shown).
2. Check the parameters of the current configuration with the gcloud config list command.
3. To create a configuration, you will be asked to optionally include a default region and zone. You can check the available regions and zones with the gcloud compute regions list and gcloud compute zones list commands, or in the [documentation](https://cloud.google.com/compute/docs/regions-zones).
4. Create a new configuration with the gcloud config configurations create CONFIG_NAME command, following the instructions.
  1. Use the same project and user ID, as you will not have any other project or user available.
  2. Use a new combination of region and zone, e.g.
5. Change the active configuration to the new one with the command gcloud config configurations activate NAME_CONFIG.
6. Check the parameters of the newly activated configuration with the gcloud config list command.
7. Change any of the properties of the new configuration, such as region or zone, with the gcloud config set PROPERTY PROPERTY_VALUE command.
8. Check the new settings of the new configuration enabled with the gcloud config list command.
9. List the configurations available with the gcloud config configurations list command (it will show both on this occasion).
10. Activate the original settings.
11. Delete the new configuration with the gcloud config configurations delete CONFIG_NAME command.

**Task 3: Connect Cloud Shell to your on-premises environment**

In this task we will explore several ways of connecting to Cloud Shell from the local terminal with Cloud SDK and transmit files in both directions.

**Connect from local**

_NOTE: To follow these instructions you must have the Cloud SDK installed in a local UNIX environment._

We can connect from our local terminal to Cloud Shell over SSH to use Cloud Shell tools as a remote development environment.

1. Open your local terminal: Linux, macOS, WSL, Linux VM, etc.
2. Connect to Cloud Shell with the gcloud cloud-shell ssh --authorize-session command.
3. Check that you have Cloud Shell language tools and runtimes, such as Cloud SDK, Nano, Git, Python, kubectl, etc.
4. You can always disconnect from an SSH session with the exit command.

**Connecting files from local**

To work from on-premises with Cloud Shell or to copy files between on-premises and Cloud Shell, we can use the following commands to copy and mount on-premises files to Cloud Shell:

1. For the following instructions we will copy files. If you need to create a new file on Linux, you can do it with the command echo "hello world" > file.txt.
2. Copy a local file to Cloud Shell with the command gcloud cloud-shell scp localhost:~/file.txt cloudshell:~/file.txt.
3. Copy a Cloud Shell file to local with the command gcloud cloud-shell scp cloudshell:~/file.txt localhost:~/file.txt.
4. Check the files copied to local and Cloud Shell with the ls command in the corresponding directory.

_Optional step:_ Mount the $HOME Cloud Shell directory in a local directory:

1. Install "sshfs" following the instructions in the official repository: [github.com/libfuse/sshfs](https://github.com/libfuse/sshfs).
2. Locally choose an MOUNT_PATH directory (e.g./cloud-shell) to mount the $HOME Cloud Shell on your local file-system.
3. Mount Cloud Shell on your local file system with the gcloud cloud-shell get-mount-command MOUNT_path command.
4. In Cloud Shell, create a file in your $HOME, and then make sure it's available locally, and you can also edit it with a local text editor.
5. If you want to unmount Cloud Shell from your local file-system, use the fusermount -u MOUNT_PATH (Linux) or unmount mountpoint (BSD and macOS) command.
