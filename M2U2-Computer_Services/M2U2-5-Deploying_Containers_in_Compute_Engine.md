# **Deploying Containers in Compute Engine**

Unit M2U2 - Exercise 5

**What are we going to do?**

1. Contain an application locally and upload it to a container image registry in the Artifact Registry.
2. Create a VM instance with the container running automatically.
3. Update the application on the VM instance.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

_Note:_ This exercise can be followed with Cloud Shell or with a local Cloud SDK environment that has Docker installed on Linux (Linux, macOS, WSL, etc.).

**Task 1: Containerise the application**

First of all, let's start by containing and checking the application locally.

**Check the application**

Let's clone the repository with the sample application and check it locally:

1. You can review the application's source code in the [GitHub](https://github.com/docker/labs/tree/master/beginner/flask-app) repository.
2. Clone the repository in your environment: git clone https://github.com/docker/labs.git.
3. Access the application subdirectory: cd labs/beginner/flask-app.
4. Create a Python virtual environment and install the application dependencies on it:
  1. Create a Python virtual environment: python3 -m venv env and source env/bin/activate.
  2. Install dependencies: pip3 install -r requirements.txt.
  3. Deploy the web app locally: python3 app.py.
  4. Check the web app:
    1. If you're using Cloud Shell, turn on preview on port 5000.
    2. If you're using a local environment, go to http://localhost:5000 (or the corresponding port depending on the web server response) in your browser.
  5. Once checked, stop the local web server with CTRL + C.

In doing so, we have downloaded and checked the application locally.

**Containerise the application**

Once the application has been checked, we will containerise it and upload it to a container image repository in the Google Artifact Registry.

Containerise the application by generating a container image:

1. Review the Dockerfile file that we'll use to containerise the app.
2. Generate a container image of the webserver/pywebapp application (or REPOSITORY_NAME/CONTAINER_NAME): docker build -t webserver/pywebapp..
3. Re-check the application locally, this time containerised:
  1. Deploy the container with the web application locally: docker run -d --name pywebapp -p 8080:5000 webserver/pywebapp.
  2. Check the web app:
    1. If you're using Cloud Shell, turn on preview on port 8080.
    2. If you're using a local environment, go to http://localhost:8080 (or the corresponding port depending on the web server response) in your browser.
  3. Once checked, stop the container via the local web server with pywebapp stop docker.

**Upload the image to a remote repository**

Create the remote repository in Google Artifact Registry to upload the image for download during the creation of the VM instances:

1. In the console, navigate to  **Artifact Registry > Repositories**  and click  **Create Repository**.
2. Explore the options and create a repository with the following features:
  - Name: webserver.
  - Format: Docker
  - Location type: multi-regional "europe".

Upload the image to the remote Artifact Registry repository:

1. Configure Docker to use gcloud authentication when accessing the Artifact Registry service: gcloud auth configure-docker europe-docker.pkg.dev.
  1. _Note_:\* If the command responds with a warning that it already has an authentication for that repository, continue, there's no need to worry about this.
2. Label the container image with the name of the remote repository to identify it: docker tag webserver/pywebapp europe-docker.pkg.dev/ID_PROJECT/webserver/pywebapp.
  1. Verify that you have 2 container images pointing to the same local image ID: docker image ls.
3. Upload the container image to the remote repository: docker push europe-docker.pkg.dev/ID_PROJECT/webserver/pywebapp.
4. Make sure you can download the image: docker pull europe-docker.pkg.dev/ID_PROJECT/webserver/pywebapp.
  1. _Note:_ This will show that the image is already available locally.
5. Verify that the image is available in the console in  **Artifact Registry > Repositories > webserver**.

By doing so, we have labelled the container image with the name of the remote repository and uploaded it to the remote repository.

_DELIVERABLES:_ 
M2U2-5-task_1-file_1-screenshot_1.jpg: Screenshot of the container image details in the console (by clicking on the name of the pywebapp container image).

**Task 2: Create Containerised VM Instance**

In this task we are going to create a VM instance with the automatically deployed container image.

First, locate the URL of your container image:

1. In the console, navigate to  **Artifact Registry > Repositories > webservers**.
2. In the upper area, next to pywebapp, locate the button to copy the URL (2 overlapping rectangles) and click on it.
  1. It will take the form of europe-docker.pkg.dev/ID_PROJECT/webserver/pywebapp. Verify that the URL of the container image has been copied to the clipboard correctly.

Now create the VM instance with the automatically deployed container:

1. Navigate to  **Compute Engine > VM Instances**  and click on  **Create Instance**.
2. Under  **Container** , confirm ** Implement a container image on this VM instance** , explore the options under  **Advanced container options and indicate the following options** :
  - Container Image: The URL of the previously copied container.
3. Create the VM instance with the following options:
  - Zone: europe-west1-b.
  - Type of machine: e2-medium.
4. Create a firewall rule to enable traffic to port 5000 of the instance: gcloud compute firewall-rules create vm-container --allow TCP:5000.

- _Note:_ The Dockerfile exposes port 5000 of the container, which will also be the port of the instance where it listens.

After that, wait for the instance to be created to check that the application is working properly:

1. On the VM Instances page, tap on the external IP of the instance or copy it into a browser tab and check the web application.
  1. Remember to use the HTTP protocol and enter port 5000: http://EXTERNAL_IP_INSTANCE:5000.
2. You can also connect via SSH to the instance and log in internally with curl localhost:5000.

This allows us to deploy an instance that automatically downloads the container image and runs it.

_DELIVERABLES:_

1. M2U2-5-task_2-file_1-screenshot_1.jpg: Screenshot showing instance details, showing the URL of the container image.
2. M2U2-5-task_2-file_2-screenshot_2.jpg: Screenshot showing the application in the browser and its URL.

**Task 3: Update the application**

In this task we will see how we can update an instance with a new version of the application.

To do this, we must update the container image, by updating the application and uploading a new container image to the repository and edit and restart the instance.

First, edit the application code, containerise it and upload it to the repository again, following the same steps as in the previous tasks:

1. Edit the templates/index.html file and add <p>v2version</p> on a line between the <p><small>Courtesy:... line and </div>.
2. Containerise the application again with the same name, webserver/pywebapp, which will assign it the latest tag.
3. Check that you have both images locally: docker image ls.
4. Run it and check it locally.
5. Re-label the web server/pywebapp container image to europe-docker.pkg.dev/ID_PROJECT/webserver/pywebapp and upload it to the remote repository.
6. In the console, check that the 2 images are available and copy the URL of the new container image.

To update the instance:

1. Navigate to  **Compute Engine > VM Instances** , stop the instance and then start it again to restart it.
2. Once restarted, verify that you have downloaded and are running the new version of the application.

_DELIVERABLES:_

1. M2U2-5-task_3-file_1-screenshot_1.jpg: Screenshot showing the application in the browser and its URL.

**Deliverables Summary**

1. M2U2-5-task_1-file_1-screenshot_1.jpg: Screenshot of the container image details in the console (by clicking on the name of the pywebapp container image).
2. M2U2-5-task_2-file_1-screenshot_1.jpg: Screenshot showing instance details, showing the URL of the container image.
3. M2U2-5-task_2-file_2-screenshot_2.jpg: Screenshot showing the application in the browser and its URL.
4. M2U2-5-task_3-file_1-screenshot_1.jpg: Screenshot showing the application in the browser and its URL.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete the VM instance.
2. Remove the repository and container images from the Artifact Registry.
3. Remove the vm-container firewall rule in  **VPC Network > Firewall**.
4. Optional: Remove containers and container images from your on-premises environment (_note:_ Cloud Shell will discard them when you log out).
