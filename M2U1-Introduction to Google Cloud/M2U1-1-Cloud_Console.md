# **Cloud Console**

Unit M2U1 - Exercise 1

**What are we going to do?**

1. Explore the Google Cloud console.
2. Explore Cloud Shell options.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the &quot;\&gt;\_&quot; icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Google Cloud Console**

In this task you will explore the console, browse through the main parts and set your desired configuration.

**Check the user and project**

Start this exercise on the main page of the console. You can always return to it by clicking on  **Google Cloud Platform**  to the left of the blue top bar.

**Main Dashboard**

1. Check the main dashboard of the console, you will see that there is a lot of information available.
2. Analyse the different cards that comprise it.
3. Locate the most important parts of the console, which we will analyse in the following points:
  1. Navigation menu: side menu with &quot;hamburger&quot; button in the upper left corner.
  2. Button to return to the main dashboard:  **Google Cloud Platform**  after the previous one.
  3. Project selection menu: 3 white hexagons and the name of the project or &quot;Select a project&quot; after the previous one.
  4. Main search box for services or resources: next, in the centre of the blue top bar.
  5. Button to activate Cloud Shell: button  **>_** on the right of the blue top bar.
  6. Notifications: bell button after the previous one.
  7. Preferences: button with 3 vertical points after the previous one.
  8. Logged in user: rightmost button in the top blue bar, with your Google account avatar.
  9. **Dashboard, activity and recommendations** tabs, at the top left of the dashboard, under the blue top bar.

**Verify User**

1. Under the user button (mentioned in the previous point), check the user you are logged in as.
2. If you need to, add another account, log in with another account, or sign out to sign in again.

**Check project**

1. In the project selection drop-down menu (mentioned in the first point of the task), check if you have your project selected.
2. Make sure it appears under  **No organisation**  if you&#39;re using any Google Workspace or Cloud Identity accounts for Google Cloud. Otherwise, the drop-down may not appear.
3. If you have multiple projects, you can optionally click on the star icon of your project to add it to the  **Featured** list.
4. Remember to always verify your user and project as a first step when working in Google Cloud.
5. If you ever need to change projects, you can find the option here.
6. On the main console dashboard, locate the  **Project Information**  card and review it.
  1. Try to memorise the project ID, or always remember that you can find it quickly here or in the project selection menu.
  2. Explore the  **Go to Project Settings** card link.

**Browse Services**

Now let&#39;s navigate to the services side menu:

**Navigation menu**

1. Locate the side navigation menu again.
2. Navigate through it slowly, taking your time. Browse the different services, subsections, option groups, etc. Try to get a good idea of the options available and where the most common options or services are located.
3. Next to each service in the menu a  **Pin**  button will appear when hovering over. Pin a couple of services to the top of the menu to test it, unpin them if you want to, and remember this option if you find that during the course you mainly use some services or find it difficult to find some of them.
4. Always remember that you can also use the search bar in the blue top bar to search for services and sections.
5. To expand the menu again, you can click on the &quot;hamburger&quot; button in the top left corner.

As examples, search the menu and the search bar for the following sections:

1. **Compute Engine - VM Instances**
2. **Cloud Storage > Browser**
3. **VPC Network > VPC Networks**
4. **IAM & admin > IAM**
5. **Invoicing**
6. **IAM & Admin > Fees**
7. **Compute Engine > Disks**
8. **VPC Network > Firewall**
9. **Network Services > Load Balancing**

**Console Configuration**

We will configure the console to set your default settings:

1. In the blue top bar on the right, go to **Settings and Utilities (3 vertical dots) \&gt; Preferences**.
2. Browse and explore the available sections and set your preferences.
3. Make sure to set your preferences for  **language and region**. For this course we will try to include the Spanish names of the sections and options in the instructions, although it is often advisable to use the console in English to avoid problems with translations.

Return to the main dashboard to configure it:

1. At the top right you will find the  **Customise** button.
2. Configure the cards to your liking, enabling and disabling them, and sorting them according to your preferences. Remember that you can always change this customisation during the course.

Within the console there are multiple tutorials available in each section. Sometimes, when using a new service, the  **Learning**  side menu will be activated with tutorials and/or links to the documentation, which you can always close if you do not wish to use it at that time.

To find the tutorials again, you can search for the  **Learning**  button on the pages and services where they are available.

For example, look for this button on the following pages:

1. **Compute Engine > Instance Templates**
2. **Cloud Storage > Navigation**
3. **App Engine > Panel**

**Enable APIs**

Let&#39;s explore the APIs of the available services and how to enable them:

1. Navigate to  **APIs and Services > Dashboard**  and explore the main dashboard.
2. Navigate to  **the API Library**  and explore the available options.
3. To enable an API when prompted by the instructions, we must look for it in the  **API Library**  and enable it.
4. We can also search for it in the search bar of the blue top bar.
5. Search, explore the information and enable or verify that the following APIs are already enabled:
  1. Compute Engine API
  2. Cloud Storage API
  3. Kubernetes Engine API

**Task 2: Cloud Shell**

In this task we will explore Cloud Shell options.

**Activate and check Cloud Shell**

Start by activating Cloud Shell with the button on the console:

1. Remember that you can use the built-in Cloud Shell in the same browser window of the console or in a different tab.
2. Change the size of the Cloud Shell drop-down by clicking on its top border.
3. Minimise and maximize the dropdown with the 2-arrowed button, third button on the right in the white Cloud Shell bar.
4. Open Cloud Shell in a new tab/window with the 2nd button on the right and check that your session has been recovered, and that the console window/tab shows the session as transferred (you can close it in the console).
5. To access Cloud Shell directly in your browser, you can also use the direct link: [shell.cloud.google.com](https://shell.cloud.google.com/).

Check your Cloud Shell settings. This check is recommended every time we open Cloud Shell:

1. Verify that the correct project is selected. Sometimes, even though the project is selected in the console, when opening Cloud Shell, it is not selected correctly. You can do this in the following ways:
  1. Verifying that the project ID appears in yellow in the terminal &quot;prompt&quot; and also in yellow in the welcome text of the terminal.
  2. Verifying that the project ID appears in the name of the tab or session of the terminal.
  3. Verifying that the project ID appears when you run the gcloud config list command, which contains all the Cloud SDK settings.
  4. Verifying that the project ID appears as an environment variable when executing the echo $DEVSHELL\_PROJECT\_ID command, created when initialising Cloud Shell.
2. The easiest way to change your project is to open a new Cloud Shell tab:
  1. Open a new tab with the same settings with the  **+**  button in the Cloud Shell top bar.
  2. Open a new tab by selecting a new project with the down arrow button next to  **+**  and selecting the desired project in the Cloud Shell top bar.
  3. Remember, when you open a new session you will not have the previous session&#39;s settings, such as history and _environment variables_, which you will have to set again.

Also check out the components available in Cloud Shell:

1. Verify that the Cloud SDK is installed and updated with gcloud --version.
2. Check the Cloud SDK components installed with gcloud components list.
  1. Remember that you can install or update any component if needed with gcloud components install ID\_COMPONENT.
3. Check the tools, languages and their versions that appear in the documentation: [Cloud Shell: tools available](https://cloud.google.com/shell/docs/how-cloud-shell-works#tools).
  1. In particular, try to familiarise yourself with one of the three text editors available: Nano, Vim or Emacs. If you're not familiar with any, we recommend using Nano.
  2. You can search for references, documentation and troubleshoot these editors by searching for the information on Google or Stack Overflow.
4. Check the available storage and its mount points with the df command. Remember that the 5 GB max. in the $HOME directory is the only thing that is retained between Cloud Shell sessions, and that the rest of the storage will be reset.

**IDE**

We will work with the integrated IDE in Cloud Shell, based on [Theia](https://theia-ide.org/), in turn inspired by Visual Studio Code.

1. Activate the editor:
  1. On the Cloud Shell button of a pencil in the white top bar.
  2. With the direct URL to [ide.cloud.google.com](https://ide.cloud.google.com/).
2. Explore the editor¡s welcome page, where you can access your workspaces or your $HOME Cloud Shell directory.
3. Go to your $HOME directory and review the editor options.
4. Using the IDE, create a new file in your $HOME directory (corresponding to /home/USERNAME), e.g. hello_world.txt with any text.
5. Check that you have the options of having the Cloud Shell terminal, the IDE or both open, by playing around with the buttons in the top right hand corner.
6. Go back to the Cloud Shell terminal and check that you can access the file created with the commands ls and cat hello_world.txt.
7. Create a file on the Cloud Shell terminal with the echo command "hello world again!" > hello_world2.txt.
8. Review the file created with the IDE.
9. You can always edit a Cloud Shell terminal file in the IDE directly, if you prefer to use a CLI text editor such as Nano, with the command edit hello_world2.txt or cloudshell edit hello_world2.txt.

This way you can see how to work with the Cloud Shell terminal and IDE at the same time.

**Cloud Shell Actions**

Let's check out some actions available in Cloud Shell:

**Restart the VM**

1. In the 3 vertical dots menu on the Cloud Shell terminal (white top bar), select  **Restart**.

**Upload and download files**

1. To upload a file, in the 3 vertical dots menu of the Cloud Shell terminal (white top bar), select  **Upload**.
2. Upload any local file from your PC.
3. Check the file with the ls path_TO_FILE command.
  1. You can delete it with rm PATH_TO_FILE.
4. To download a file, e.g. one of the previously created files, you can choose to:
  1. From the Cloud Shell terminal, in the 3 vertical dots menu of the Cloud Shell terminal (white top bar), select  **Download**  and enter the path to the file or select it from the list.
  2. From the Cloud Shell IDE, open the file to download and select  **File \&gt; Download**.

**Preview Site**

To preview any web application that we run locally in Cloud Shell, we can do it with the corresponding option. To check this:

1. Install the Apache 2 web server with the commands sudo apt-get install httpd and sudo service apache2 start (it will be automatically discarded as nothing will persist outside the $HOME after the session).
2. Modify the port linked to the Apache web server by modifying the/etc/apache2/ports.conf file:
  1. sudo nano /etc/apache2/ports.conf (the IDE can only edit files in the $HOME directory).
  2. Change the port from Listen 80 to Listen 8080.
  3. Save and close the file with  **Control + X, and, ENTER**.
  4. Restart the service with sudo service apache2 restart.
3. Check the default website with curl localhost:8080
4. Open the web preview with the button in the white top bar of Cloud Shell, on port 8080.

Remember that, since the Cloud Shell instance is not running locally but the connection is through a browser, we cannot connect directly to the Cloud Shell IP as if it were local, but we have to use this preview to access any port or application running on Cloud Shell.

**"Open in Cloud Shell" button**

Sometimes, in the documentation, tutorials and online resources you can find a button called  **Open in Cloud Shell**.

This link will open the Cloud Shell terminal and clone the linked repository to give you quick access to it so you can follow the tutorial, run the application or script, etc.

**Equivalent commands in gcloud**

Throughout the console, on pages related to resource creation, you will sometimes find a link at the bottom of the page with the message  **command line equivalent**  and  **REST equivalent**.

If you open those links, you&#39;ll see the gcloud command and a button to copy it to Cloud Shell.

To check:

1. Navigate to  **Compute Engine > VM Instances**.
2. Select  **Create Instance**.
3. At the bottom of the page, click on  **equivalent command line**.
4. Select  **Run on Cloud Shell**.
  1. _NOTE:_ Do not run the command to create the instance, or remember to delete it later in the console.

These commands will have all the options included, even some that match the default values, so they will usually be much longer than usual.
