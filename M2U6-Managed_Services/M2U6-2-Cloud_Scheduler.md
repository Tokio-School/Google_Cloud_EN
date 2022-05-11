# **Cloud Scheduler**

Unit M2U6

**What are we going to do?**

1. Configure message cron jobs to Cloud Pub/Sub and HTTPS endpoints.
2. Make calls to Google Cloud APIs directly.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U6-2-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Configure cron jobs**

xxx

**Sending messages via Cloud Pub/Sub**

- create topic p/s
- create cron job that sends p/s - schedule message every minute/30 s?
- confirm receipt on console
- bonus: set up GAE app to receive - with code

**Receiving messages in serverless endpoint**

- create CF
- create cron job that sends CF - schedule message every minute/30 s?
- confirm receipt on console logs
- bonus: set up VM-creating API call

**Task 2: Make calls to Google Cloud APIs**

xxx

- create SA
- give roles to SA
- create VM
- create cron job calling API to turn on VM - authentication
- create cron job calling API to shut down VM - authentication
- check jobs manually

**Deliverables Summary**

1. M2U6-2-questions.txt: Answers to all questions raised in the exercise.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. step1
2. step2
