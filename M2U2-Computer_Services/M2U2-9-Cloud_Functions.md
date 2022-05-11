# **Cloud Functions**

Unit M2U2 - Exercise 9

**What are we going to do?**

1. Deploy a serverless function with Cloud Functions.
2. Deploy an activated function based on messages.
3. Deploy an event-activated feature.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Deploy a function with HTTP endpoint**

In this task we will deploy a serverless function with endpoint to respond to HTTP requests.

Before you begin, enable the Cloud Functions and Cloud Build APIs:

1. In the console, navigate to  **APIs and Services > Library**.
2. Look for the Cloud Functions and Cloud Build APIs and enable or verify that they are already enabled.

**Check the function locally**

First we will test the function locally, which allows us to check our code or test in a CI/CD environment before we deploy the function to the cloud, and have to wait for it to deploy, for the endpoint to update, and so on.

_Note:_ We recommend using Cloud Shell, but you can also use a local environment with Python 3, and other dependencies installed.

Create on-premises environment and download the code:

1. Create a local working directory, e.g. mkdir ~/M2U2-9-Cloud_Functions.
2. Create a subdirectory for the first task, e.g. mkdir ~/M2U2-9-Cloud_Functions/task_1.
3. Access the subdirectory of the first task, e.g. cd ~/M2U2-9-Cloud_Functions/task_1.
4. Create a script with the content of the hello_http function code (only the hello_http function code and the line that imports from flask import escape), in the functions/helloworld/main.py file in the [GitHub repository](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/functions/helloworld/main.py).
5. Create a Python virtual environment: python3 -m venv env and source env/bin/activate.
6. Install the Function Frameworks library to test the application in a development environment: pip install functions-framework (or pip3, depending on your Python installation).
7. Create a requirements.txt dependency file with the contents of the [GitHub repository](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/6f17aaaf8962b1ec540ec4afcf61f1c784bcac89/functions/helloworld/requirements.txt) functions/helloworld/requirements.txt file.
8. Install the specified function dependencies: pip install -r requirements.txt.

Check that it is working by running it locally:

1. Deploy the function to a local server with the functions_framework --target hello_ttp --port 8080 --signature_type http command.
2. Now check that it is working:
  1. From the command line: curl localhost:8080.
  2. From the Cloud Shell web preview or in your local browser: http://localhost:8080.

This allows us to test the functionality by performing tests in a local development environment or in a CI/CD environment.

_DELIVERABLES_:

1. M2U2-9-task_1-file_1-screenshot_1.jpg: Screenshot curl localhost:8080 command.

**Deploying the feature in the cloud**

Finally, let's deploy the feature to the cloud:

1. In the console, navigate to  **Cloud Functions**  and click  **Create Function**.
2. Review the different options and create a function with these features:
  - Region: europe-west1.
  - Activator: HTTP
  - Authentication: Allow unauthenticated invocations.
  - Runtime: Python 3.9
  - Source code: Use the direct editor to copy the code from the script (including imports) and dependency file.
3. Once the function is deployed, let&#39;s check it:

  1. Go to the URL directly to receive the message.
  2. The function allows you to receive a name within a JSON. You can display the function menu in the  **Actions** column, click  **Test function**  and send a test JSON with the name key and the value you want.

This allows us deploy our function in the cloud and perform tests from the web console.

_DELIVERABLES_:

1. M2U2-9-task_1-file_2-screenshot_2.jpg: Screenshot the browser with the function response, showing the URL of the endpoint.

**Task 2: Deploy a message-activated function**

In this task we are going to deploy a serverless function with an event-based activation:

**Check the function locally**

Again, let&#39;s first check the application in a local development environment:

1. Create a subdirectory for the second task, e.g. mkdir ~/M2U2-9-Cloud_Functions/task_2.
2. Access the subdirectory of the second task, e.g. cd ~/M2U2-9-Cloud_Functions/task_2.
3. Create a script with the content of the hello\_pubsub function code from the functions/helloworld/main.py file in [the GitHub repository](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/functions/helloworld/main.py).
4. Create a Python virtual environment: python3 -m venv env and source env/bin/activate (you can deactivate your current environment with deactivate if necessary).
5. Install the Function Frameworks library in the new virtual environment: pip install functions-framework (o pip3, depending on your Python installation).
6. Create a requirements.txt dependency file with the contents of the [GitHub repository](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/6f17aaaf8962b1ec540ec4afcf61f1c784bcac89/functions/helloworld/requirements.txt) functions/helloworld/requirements.txt file, or copy the previous one.
7. Install the specified function dependencies: pip install -r requirements.txt.

Check that it is working by running it locally:

1. Deploy the function to a local server with the functions_framework --target hello_pubsub --port 8080 --signature_type event command.
2. Now check the function from the command line with a curl POST request and an event in JSON format:

# 'world' base64-encoded is 'd29ybGQ='

curl localhost:8080 \

-X POST \

-H ";Content-Type: application / json" \

-d &#39;{

&quot;context&quot;: {

&quot;eventId&quot;:&quot;1144231683168617&quot;,

&quot;timestamp&quot;:&quot;2020-05-06T07:33:34.556Z&quot;,

&quot;eventType&quot;:&quot;google.pubsub.topic.publish&quot;,

&quot;resource&quot;:{

&quot;service&quot;:&quot;pubsub.googleapis.com&quot;,

&quot;name&quot;:&quot;projects/sample-project/topics/gcf-test&quot;,

&quot;type&quot;:&quot;type.googleapis.com/google.pubsub.v1.PubsubMessage&quot;

}

},

&quot;data&quot;: {

&quot;@type&quot;: &quot;type.googleapis.com/google.pubsub.v1.PubsubMessage&quot;,

&quot;attributes&quot;: {

&quot;attr1&quot;:&quot;attr1-value&quot;

},

&quot;data&quot;: &quot;d29ybGQ=&quot;

}

}&#39;

This allows us to test the functionality by performing tests in a local development environment or in a CI/CD environment.

_DELIVERABLES_:

1. M2U2-9-task\_2-file\_1-\_screenshot1.jpg: Screenshot curl localhost:8080 command.

**Deploying the feature in the cloud**

To deploy the function, follow the steps above to deploy the code from the web console.

1. Deploy a new function with these features:
  - Region: europe-west1.
  - Activator: Cloud Pub/Sub.
    - Create a new Pub/Sub theme
  - Copy the function code and the requirements.txt file into the online editor.
2. To check the function, navigate to  **Pub/Sub** , go to the newly created theme, and click on  **Post messages**.

  1. In the message body field, add a data attribute with the message d29ybGQ= (&quot;world&quot; base64-encoded).
  2. Then navigate to Cloud Functions again, click  **Actions**  for your function, and then click  **View Logs**.

This means we have developed and deployed a function activated by messages or events.

_DELIVERABLES_:

1. M2U2-9-task\_2-file\_2-screenshot\_2.jpg: Screenshot of function logs, showing Cloud Pub/Sub message information.

**Task 3: Deploy a Cloud Storage Event Triggered Feature**

Finally, let&#39;s check and deploy a feature activated by Cloud Storage events.

To do this, repeat the steps above to check and deploy the feature to the cloud with the following changes or considerations:

1. Create another Python subfolder and virtual environment.
2. Use the hello\_gcs function code from the same Python file above.
3. As --signature\_type, use again event.
4. Check it locally with:

curl localhost:8080 \

-X POST \

-H &quot;Content-Type: application / json&quot; \

-d "{

"context": {

"eventId": "1147091835525187",

"timestamp": "2020-04-23T07:38:57.772Z",

"eventType": "google.storage.object.finalize",

"resource": {

"service": "storage.googleapis.com",

"name": "projects/_/buckets/MY_BUCKET/MY_FILE.txt",

"type": ";storage#object"

}

},

"data": {

"bucket": "MY_BUCKET",

"contentType": "text/plain",

"kind": "storage#object",

"md5Hash": "...",

"metageneration": "1",

"name": "MY_FILE.txt",

"size": "352",

"storageClass": "MULTI\_REGIONAL",

"timeCreated": "2020-04-23T07:38:57.230Z",

"timeStorageClassUpdated": "2020-04-23T07:38:57.230Z",

"updated&quot;: "2020-04-23T07:38:57.230Z"

}

}'

1. Create another function with Cloud Storage event activation and create a Cloud Storage bucket linked to the function.
2. To test the function in the cloud, upload a file to the bucket and review the Cloud Functions function logs.

_DELIVERABLES_:

1. M2U2-9-task_3-file_1-screenshot_1.jpg: Screenshot curl localhost:8080 command.
2. M2U2-9-task_3-file_2-screenshot_2.jpg: Screenshot of function logs, showing Cloud Storage object information.

**Deliverables Summary**

1. M2U2-9-task_1-file_1-screenshot_1.jpg: Screenshot curl localhost:8080 command.
2. M2U2-9-task_1-file_2-screenshot_2.jpg: Screenshot the browser with the function response, showing the URL of the endpoint.
3. M2U2-9-task_2-file_1-screenshot1.jpg: Screenshot curl localhost:8080 command.
4. M2U2-9-task_2-file_2-screenshot_2.jpg: Screenshot of function logs, showing Cloud Pub/Sub message information.
5. M2U2-9-task_3-file_1-screenshot_1.jpg: Screenshot curl localhost:8080 command.
6. M2U2-9-task_3-file_2-screenshot_2.jpg: Screenshot of function logs, showing Cloud Storage object information.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. (Optional) Remove directories from the local work environment.
2. Remove Cloud Functions.
3. Remove the Cloud Pub/Sub theme and Cloud Storage bucket.
