# **Cloud Pub/Sub**

Unit M2U6

**What are we going to do?**

1. Create an app that sends and receives messages.
2. Create an outline for a topic.
3. Use pull and push subscriptions.
4. Deploy architectures from 1 to 1 and 1 to several.
5. Explore message filtering and sorting options.
6. Use dead letter themes.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U6-1-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You'll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Deploy "publisher"/"subscriber" schema**

xxx

**Create a topic and pull subscription**

- Create a topic
- create a topic pull subscription

**Check messages with the console**

- send message with console
- check message with console
- confirm message with console

**Check messages with Cloud SDK**

- send message with gcloud
- check message with gcloud
- confirm message with gcloud

**Deploy sender and receiver**

- deploy sender in CF
- deploy receiver in CF
- send messages and check for receipt

**Task 2: Schemas, filtering and sorting of dead letter messages and topics**

xxx

**Create a topic with a schema**

- create schema
- create topic and assign schema to it
- create subscription
- send incorrect message
- check that it is not going
- send correct message
- check shipment

**Message sorting**

- create topic with sorted
- send message - id 1
- confirm receipt
- send message - id 3
- confirm receipt
- send messy message - id 2
- confirm receipt of id 2 and id 3

**Message filtering**

- create subscription with filtering (previous topic)
- send normal message - does not pass filter
- confirm receipt in 1st susc
- send message pass filter
- confirm receipt in both susc

**Dead Letter topics**

- add dead letter topic to topic
- configure quick retry
- create subscription for dead letter topic
- display that returns an error directly
- send message
- confirm receipt in dead letter subscription with console/gcloud

**Task 3: Push-type subscriptions**

xxx

**Send messages in parallel**

- create theme
- create 2 push subscriptions
- create 2 CF
- send messages with gcloud
- confirm messages in both CFs

**Deliverables Summary**

1. M2U6-1-questions.txt: Answers to all questions raised in the exercise.
2.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. step1
2. step2
