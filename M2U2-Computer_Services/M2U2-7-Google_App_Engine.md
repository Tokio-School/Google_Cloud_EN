# **Google App Engine**

Unit M2U2 - Exercise 7

**What are we going to do?**

1. Deploy a serverless web application to Google App Engine.
2. Upgrade the application to a new version with a step-by-step deployment.
3. Deploy an application in GAE's flexible environment.
4. Customise the application runtime in the flexible environment.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U2-7-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

_Note:_ To check these applications locally you need to have Python 3 and Docker installed. You can install it on your on-premises Cloud SDK environment or use Cloud Shell (recommended).

**Task 1: App Engine, Standard Environment**

Let&#39;s start by deploying a Python 3 serverless web application in App Engine.

**Check the web application**

Let&#39;s clone the repository with a sample application and check it locally ([GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/tree/master/appengine/standard)):

1. Activate your on-premises Cloud Shell environment or Cloud SDK installation.
2. Clone the GitHub repository with the source code: git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git.
3. Access the directory with the sample application: cd python-docs-samples/appengine/standard_python3/hello_world.
4. Browse the configuration files and code in the repository.
5. Try the web app locally:
  1. Create a Python virtual environment: python3 -m venv env and source env/bin/activate
  2. Install dependencies: pip3 install -r requirements.txt
  3. Deploy the web application locally: python 3 main.py.
  4. Check the web app:
    1. If you're using Cloud Shell, turn on preview on port 8080.
    2. If you're using a local environment, go to http://localhost:8080 in your browser
  5. Once checked, stop the local web server with CTRL + C.

This is so that we can explore how to develop and test a local web application before deploying it to the App Engine.

**Deploy the web application**

Now let&#39;s deploy the web application in App Engine:

1. In the console, navigate to  **APIs and Services > Library**  and search and activate the App Engine API.
2. Navigate to  **App Engine > Dashboard** , select  **Create app**  and follow the instructions to create an app or App Engine environment in europe-west3.
3. Finally, go back to the terminal and deploy your app in GAE: gcloud app deploy.
4. Once deployed, go to the URL that returns the command and check your application in the browser.
  1. You can also retrieve the URL with gcloud app browse.

This is a simple way to deploy the code of our web application to the standard App Engine environment.

**Please Update your web App**

To update the web application, we just need to modify the source code, check the application locally and redeploy it in App Engine. In this case, we are going to deploy a new version without migrating the traffic to it, we are going to perform an A/B test with 50% of traffic to each version and finally we are going to migrate all the traffic to the new version.

Update your web app:

1. In the folder with the application, edit the main.py file and change the return line 'Hello World!' to return 'Hello World! - v2'.
2. Follow the steps above to try your app again locally.
3. Once tested and working correctly, we are going to deploy it without migrating all the traffic to the new version v2 with the command gcloud app deploy --version v2 --no-promote.
4. When deployed, navigate to  **App Engine > Versions**  and verify that both versions are deployed and 100% of traffic is directed to the first version.
5. You can click on the first and second version to open the URL in your browser and check both.
6. _QUESTIONS:_
  - _What is the canonical link to the application, the one you used to access the application when you first deployed it, and the one shown in gcloud app deploy?_
  - _What is the link to the second version v2? Can the default version and all other versions be accessed at all times?_
  - _What is the structure of the canonical link to the default version and the explicitly indicated versions?_

Now we are going to perform an A/B test with both versions, directing 50% of traffic to each version:

1. In  **App Engine > Versions** , select both versions and click on  **Split traffic**.
2. Select  **Cookie**  as the split method and assign 50% of the traffic to each version.
3. In  **App Engine > Panel**  or  **App Engine > Services** , find the canonical link of the application (without any specific version indicated in the subdomain).
4. Open a new incognito window and check the application. Repeat the operation several times, with new incognito windows, to check that it redirects to both applications (_note:_ we use incognito windows because the session is kept to a single version using a cookie automatically included in the first response).

Finally, after checking the new version, we will migrate all traffic to the new version:

1. In  **App Engine > Versions** , select v2 and click on  **Migrate traffic**  to begin migrating.
2. Once migrated, access the application via the canonical link to the default version and check that it now always sends to the new version.

On this occasion we have deployed a new version without redirecting traffic to it in order to split it and migrate it later, but if we want all traffic to automatically be redirected to the new version when we update it, we can do so with the gcloud app deploy --version NAME_VERSION command without the --no-promote option.

_DELIVERABLES_:

1. M2U2-7-task_1-file_1-screenshot_1.jpg: Screenshot the versions section showing both versions.
2. M2U2-7-task_1-file_2-screenshot_2.jpg: Screenshot of the web browser showing the web application response and its URL.

**Task 2: App Engine, flexible environment**

Now we are going to deploy an application with custom runtime based on a container with NGINX in the flexible environment, deploying it with a Dockerfile.

**Check the application locally**

_NOTE:_ For this step we need Docker installed locally, so Cloud Shell is recommended.

To check the application locally, follow these steps ([GitHub](https://github.com/GoogleCloudPlatform/appengine-custom-runtimes-samples/tree/master/nginx)):

1. Clone the repository locally, to a folder outside the repository in the previous step:
  1. For example, return to the cd $HOME user directory.
  2. Clone the repository: git clone https://github.com/GoogleCloudPlatform/appengine-custom-runtimes-samples.git.
2. Access the directory with the sample application: cd appengine-custom-runtimes-samples/nginx.
3. Build a container image with the Dockerfile: docker build -t nginx_app .
4. Run the container locally to check the application:
  1. docker run -d -p 8080:80 nginx_app --name test_app nginx -g "daemon off;"
  2. Access port 8080 in the Cloud Shell preview or in your browser if you are using a local environment with http://localhost:8080.
  3. You can stop the container with docker stop test_app.

**Deploy the web application**

Now deploy the web application in a new service, to structure our application in micro-services:

1. Edit the app.yaml file and add this line: service: service-nginx
2. Deploy the app as the first version of a new service with gcloud app deploy.
3. Once deployed, go to the URL that returns the command and check your application in the browser.
  1. You can also retrieve the URL with gcloud app browse.
4. Navigate to  **App Engine > Services**  and check that there is a new service-nginx next to the default.
5. Navigate to  **App Engine > Versions**  and check the available versions of both services.

We have thus deployed a web application with a custom runtime with a Dockerfile in the flexible App Engine environment.

_DELIVERABLES_:

1. M2U2-7-task_2-file_1-screenshot_1.jpg: Screenshot the versions section showing the version of the service-nginx service.
2. M2U2-7-task_2-file_2-screenshot_2.jpg: Screenshot of the web browser showing the web application response and its URL.

**Deliverables Summary**

1. M2U2-7-questions.txt: Answers to all questions raised in the exercise.
2. M2U2-7-task_1-file_1-screenshot_1.jpg: Screenshot the versions section showing both versions.
3. M2U2-7-task_1-file_2-screenshot_2.jpg: Screenshot of the web browser showing the web application response and its URL.
4. M2U2-7-task_2-file_1-screenshot_1.jpg: Screenshot the versions section showing the version of the service-nginx service.
5. M2U2-7-task_2-file_2-screenshot_2.jpg: Screenshot of the web browser showing the web application response and its URL.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. To remove all App Engine services and versions, navigate to  **App Engine > Settings**  and disable the app. All resources will be deleted.
2. Some Cloud Storage buckets are automatically created with code, application artifacts, and static files. You can navigate to  **Storage > Browser**  and delete them manually.
