# **Cloud Run**

Unit M2U2 - Exercise 8

**What are we going to do?**

1. Deploy a web application to a serverless container in Cloud Run.
2. Deploy an integrated CI/CD pipeline.
3. Update the application and manage traffic between versions.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Deploy a web application**

In this first task we will deploy a serverless container with a Python web application from our local development environment.

We could opt for several routes to build the container:

1. Generate the container image locally and upload it to a repository of a container image registry in the Cloud Artifact Registry.
2. Generate the container image with Cloud Build and upload it to Cloud Artifact Registry.
3. Generate the container image and deploy it to Cloud Run in a single step using the command line.

To save us the steps of manually generating the container image, we will deploy the container using the Cloud SDK.

Before you begin, enable the Cloud Run, Cloud Build, Artifact Registry, and Cloud Source Repositories APIs:

1. In the console, navigate to  **APIs and Services > Library**.
2. Look for the Cloud Run, Cloud Build, Artifact Registry, and Cloud Source Repositories APIs and enable or verify that they are already enabled.

_Note:_ We recommend using Cloud Shell, but you can also use a local environment with Docker, Python 3, and other dependencies installed.

**Check app locally**

First let's create the local environment and test the application in it.

Create on-premises environment and download the code:

1. Create a local working directory, e.g. mkdir ~/M2U2-8-Cloud_Run.
2. Create a subdirectory for the first task, e.g. mkdir ~/M2U2-8-Cloud_Run/task_1.
3. Access the subdirectory of the first task, e.g. cd ~/M2U2-8-Cloud_Run/task_1.
4. Create a script named main.py with the contents of the [GitHub repository](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/run/helloworld/main.py) run/helloworld/main.py file.
5. Create a Python virtual environment: python3 -m venv env and source env/bin/activate.
6. Create a requirements.txt dependency file with the contents of the [GitHub repository](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/501c09072fa07e04d0923a0a121b1af69c305c13/run/helloworld/requirements.txt) run/helloworld/requirements.txt file.
7. Install the specified function dependencies: pip install -r requirements.txt.
8. Repeat the steps to create a Dockerfile ([GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/501c09072fa07e04d0923a0a121b1af69c305c13/run/helloworld/Dockerfile)) and .dockerignore ([GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/501c09072fa07e04d0923a0a121b1af69c305c13/run/helloworld/.dockerignore)).

Check that it is working by running it locally:

1. Deploy the function on a local server with the command gunicorn --bind :8080 --workers 1 --threads 8 --timeout 0 main:app.
2. Now check that it is working:
  1. From the command line: curl localhost:8080.
  2. From the Cloud Shell web preview or in your local browser: http://localhost:8080.
3. You can stop the local server with CTRL + C.

This allows us to test the functionality by performing tests in a local development environment or in a CI/CD environment.

_DELIVERABLES_:

1. M2U2-8-task_1-file_1-screenshot_1.jpg: Screenshot of the browser showing the application and URL.

This has enabled us to prepare our local environment, download the code and check the application before deploying it.

**Deploy the application**

Now we are going to deploy the application to Cloud Run using Cloud SDK. The advantage of using the Cloud SDK is that we can directly deploy our code and Dockerfile, while using the console we would have to generate the container image and upload it to a repository manually.

First, modify the web application, modifying the end line of the main.py file, changing debug=True to debug=False.

Then Deploy your app using Cloud SDK from Cloud Shell or your local installation:

1. Run the gcloud run deploy command:
  1. When asked about the location of the source code, press ENTER to indicate the current directory.
  2. Choose a service name such as helloworld.
  3. Choose europe-west1 as your region.
  4. Choose to create an Artifact Registry repository and allow unauthenticated invocations.
2. Once the service is deployed, go to the URL of the endpoint displayed in your browser and check your application.

_DELIVERABLES_:

1. M2U2-8-task_1-file_2-screenshot_2.jpg: Screenshot of the application in the browser showing the URL of the Cloud Run endpoint.

**Task 2: Configure an integrated CI/CD pipeline**

We are now going to configure the service/application with a CI/CD pipeline integrated with Cloud Build to automatically redeploy the application from a code repository.

First create a code repository:

1. Create a repository in Cloud Source Repositories: gcloud source repos create helloworld.
2. Create a new working subdirectory: mkdir ~/M2U2-8-Cloud_Run/task2 and cd ~/M2U2-8-Cloud_Run/task2.
3. Clone the Cloud Source Repositories repository: gcloud source repos clone helloworld.

Then set up continuous Cloud Run deployment:

1. In the console, navigate to  **Cloud Run** , click on the  **helloworld**  service, and then click  **Configure Continuous Deployment**.
2. For  **Repository Provider** , choose  **Cloud Source Repositories**.
3. For  **Repository** , choose  **helloworld**.
4. For  **Branch** , choose **\***  to select any branch.
5. Choose  **Dockerfile**  and as location **/Dockerfile**.
6. Click **Save**.

Finally, update the application and run the CI/CD pipeline with a commit to the repository:

1. Copy the application files to the new subdirectory:
  1. cp ~/M2U2-8-Cloud_run/task_1/Dockerfile..
  2. cp ~/M2U2-8-Cloud_run/task_1/.dockerignore..
  3. cp ~/M2U2-8-Cloud_run/task_1/requirements.txt..
  4. cp ~/M2U2-8-Cloud_run/task_1/main.py..
2. Edit the main.py application code and add - v2 to the server response to leave it as return "Hello {} - v2!".format(name).
3. Add the files and make a commit: git add ., git commit -m "version 2" and git push origin master.
4. In the console, within the Cloud Run  **helloworld**  service details page, select the  **Revisions**  tab and wait for the deployment of the new revision to complete.
5. Once the service is deployed, access the service URL in your browser and check your application.

This way we were able to easily deploy a CI/CD pipeline integrated with Cloud Build.

_DELIVERABLES_:

1. M2U2-8-task_1-file_3-screenshot_3.jpg: Screenshot of the application in the browser showing the URL of the Cloud Run endpoint.

**Task 3: Update the application**

Like with other services, we can divide traffic between versions:

1. In the console, within the Cloud Run  **helloworld**  service details page, select the  **Revisions** tab.
2. Click on  **Manage traffic**  and split it 50/50 between both revisions.
3. Once updated, go to the URL of the service in your browser and verify that it redirects to both revisions with a 50/50 ratio.

By doing so, we have been able to manage the traffic of our application by dividing it between several revisions, to make canary updates or A/B tests.

**Deliverables Summary**

1. M2U2-8-task_1-file_1-screenshot_1.jpg: Screenshot of the browser showing the application and URL.
2. M2U2-8-task_1-file_2-screenshot_2.jpg: Screenshot of the application in the browser showing the URL of the Cloud Run endpoint.
3. M2U2-8-task_1-file_3-screenshot_3.jpg: Screenshot of the application in the browser showing the URL of the Cloud Run endpoint.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. (Optional) Remove directories from the local work environment.
2. Remove the Cloud Run service.
3. Remove automatically created Cloud Storage buckets.
4. Remove repositories from Cloud Artifact Registry and Cloud Source Repositories.
