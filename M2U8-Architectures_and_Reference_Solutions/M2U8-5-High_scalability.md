# **High scalability**

Unit M2U8 - Exercise 5

**What are we going to do?**

1. Deploy a serverless, highly scalable, event-based architecture

This exercise is a challenge exercise, where you will have to deploy the described architecture without associated step-by-step instructions. To complete it, we recommend that you draw from what you have seen in other exercises, the official documentation or any other online resource (tutorials, StackOverflow, videos, etc.).

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Single task**

For this exercise we will follow a publicly available workshop held by the Google Cloud devRel team:

- [Pic-a-Daily Serverless Workshop](https://github.com/Tokio-School/Google_Cloud/blob/main/M2U8-Arquitecturas_y_soluciones_de_referencia/g.co/codelabs/serverless-workshop)
- [GitHub Repository](https://github.com/GoogleCloudPlatform/serverless-photosharing-workshop)
- License: [Apache 2.0](https://github.com/GoogleCloudPlatform/serverless-photosharing-workshop/blob/master/LICENSE)

The purpose of the workshop is to deploy a complete event-based architecture using serverless services: [![](RackMultipart20220511-1-onrlfm_html_1acab8123daf1373.png)](https://raw.githubusercontent.com/GoogleCloudPlatform/serverless-photosharing-workshop/master/pic-a-daily-architecture-events.png)

In the final lab we will turn it into an event-driven architecture that is orchestrated rather than choreographed:[![](RackMultipart20220511-1-onrlfm_html_88b8cb86f4a7aeee.png)](https://raw.githubusercontent.com/GoogleCloudPlatform/serverless-photosharing-workshop/master/pic-a-daily-architecture-workflows.png)

Please complete the 6 labs in order in your GCP environment/project, following the step-by-step instructions for each one and completing the deliverables listed after each lab individually.

_Note:_ Do not clean up your environment with the &quot;clean up&quot; steps between labs, except between the 5th and 6th lab, where you must remove all resources created using the &quot;clean up&quot; steps of labs 1-5, as the 6th lab starts from an empty environment. Also follow the &quot;clean up&quot; steps of lab 6 after finishing it.

**Deliverables Summary**

Screenshots of log, results or web application pages similar to those indicated in each last test section of each lab:

1. Lab 1 (step 13): Screenshot of Cloud Functions feature invocation logs and data in Cloud Firestore.
2. Lab 2 (step 11): screenshot of the app logs in Cloud Run.
3. Lab 3 (step 10): Screenshot of app logs in Cloud Run.
4. Lab 4 (step 7): Screenshot of the App Engine app.
5. Lab 5 (step 11): screenshot of the app logs in Cloud Run.
6. Lab 6 (step 15): Screenshot of the App Engine app.

_Note:_ Follow the same numbering proposed up to now: &quot;M2U[Unit #]-[Exercise #] -task\_[Task #] -file\_[File # on Task]-[File Type: Screenshot, Export, Script]\_[File # if more than 1 of the same type]&quot;, e.g. &quot;M2U8-1-task\_1-file\_1-screenshot\_1.jpg&quot;.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete all resources created, making sure not to leave any that may continue to incur costs or lead to an error in another exercise.
