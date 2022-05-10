# **Google APIs Explorer**

Unit M2U1 - Exercise 3

**What are we going to do?**

1. Explore the Google Cloud APIs.
2. Use Google Cloud APIs to create and manage resources.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Google Cloud APIs**

In this exercise, we&#39;re going to work with Google Cloud APIs using the API developer portal.

Best practice is to check the API calls in advance on this Google APIs Explorer portal when developing an application or script that makes API calls.

**APIs Explorer**

Explore the Google APIs Explorer portal until you're comfortable using it in a real project:

1. Access and become familiar with the [Google APIs Explorer](https://developers.google.com/apis-explorer) portal.
  1. The portal is a search engine for Google APIs (including Google Cloud APIs) that links to the API documentation reference.
2. Explore some Google Cloud APIs in the reference, to understand how they are structured.
3. In the reference, explore some API methods. Doing so will open the side drop-down menu called  **Try this API**  or "Try this API" (on the right).
4. Explore the parameters of the request, and relate them to the reference in the documentation.
5. Do not worry about authentication, as the portal will retrieve your user login with your Google account to authenticate the request, if the  **Google OAuth 2.0**  and  **API key** options are activated.

Once you have explored the portal, use it to explore the following APIs and complete the following instructions yourself.

_NOTES:_

1. _From the  **Try this API** drop-down menu, on a square button, or on the  **Try it now**  link in the reference, you can maximise the options for testing the API._
2. _You can execute API requests from the  **Try this API**  portal or with curl from local or Cloud Shell, if you create an API key and access token, following the instructions on the portal._
3. _Carefully review the reference and the necessary parameters, since the objective of the exercise is to familiarise yourself with the use of the APIs._
4. _In the APIs, as a  **project** parameter, use your project ID._

**Create Cloud Storage bucket**

Use the Cloud Storage API (JSON) to:

_NOTE: Review the deliverables after the instructions so that you can complete them step by step._

1. List the available GCS buckets.
2. Create a GCS bucket.
  1. As a globally unique bucket name you can use ID\_PROJECTO-M2U1-3.
3. List the buckets again, showing the newly created one.
4. Check the bucket created in the Google Cloud console, under  **Storage > Browser**.
5. Get the details of the created bucket, with the API.
6. Delete the created bucket.

_DELIVERABLES:_

1. M2U1-3-task\_1-file\_1-screenshot\_1.jpg: Screenshot with the request and result of creating the GCS bucket.
2. M2U1-3-task\_1-file\_2-screenshot\_2.jpg: Screenshot with request and result of getting GCS bucket details.
3. M2U1-3-task\_1-file\_3-screenshot\_3.jpg: Screenshot with the request and result of deleting the GCS bucket.

**Create a VM Instance**

Similarly, use the Compute Engine v1 API to:

_NOTE: Review the deliverables after the instructions so that you can complete them step by step._

1. List the available VM instances.
2. Create a new VM instance (in any area, of any type, with any OS, etc.).
3. List the available VM instances again.
4. Check the instance created in the Google Cloud console, in  **Compute Engine > VM Instances**.
5. Get the details of the instance created, with the API.
6. Delete the created instance.

_DELIVERABLES:_

1. M2U1-3-task\_1-file\_4-screenshot\_4.jpg: Screenshot with the request and result of creating the GCE instance.
2. M2U1-3-task\_1-file\_5-screenshot\_5.jpg: Screenshot with request and result of getting GCE instance details.
3. M2U1-3-task\_1-file\_6-screenshot\_6.jpg: Screenshot with the request and result of deleting the GCE instance.

**Deliverables Summary**

1. M2U1-3-task\_1-file\_1-screenshot\_1.jpg: Screenshot with the request and result of creating the GCS bucket.
2. M2U1-3-task\_1-file\_2-screenshot\_2.jpg: Screenshot with request and result of getting GCS bucket details.
3. M2U1-3-task\_1-file\_3-screenshot\_3.jpg: Screenshot with the request and result of deleting the GCS bucket.
4. M2U1-3-task\_1-file\_4-screenshot\_4.jpg: Screenshot with the request and result of creating the GCE instance.
5. M2U1-3-task\_1-file\_5-screenshot\_5.jpg: Screenshot with request and result of getting GCE instance details.
6. M2U1-3-task\_1-file\_6-screenshot\_6.jpg: Screenshot with the request and result of deleting the GCE instance.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Verify in the web console that the Cloud Storage bucket and the Compute Engine VM instance have been deleted successfully.
