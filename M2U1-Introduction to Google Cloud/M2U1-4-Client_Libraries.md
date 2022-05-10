# **Client Libraries**

Unit M2U1 - Exercise 4

**What are we going to do?**

1. Install Google Cloud Client Libraries and Google APIs client libraries.
2. Explore authentication options.
3. Use them to manage objects in Cloud Storage as an example.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

To answer all the questions of the exercise collectively, you can create a file called "M2U1-4-questions.txt" before starting the exercise. Remember to identify and sort each question followed by your answer.

You&#39;ll find the questions in italics: _QUESTION: What is the name of the Google Cloud VMs Instance service?_

**Task 1: Google Cloud Client Libraries**

In this first task, we'll explore and use the Google Cloud client libraries.

**Install client library**

First explore the available libraries: [Cloud Client Libraries](https://cloud.google.com/apis/docs/cloud-client-libraries).

1. Pick a library and study it thoroughly. The following instructions will refer to the Python library, but you can use the one you prefer.
2. Create a working directory in Cloud Shell or on-premises, e.g. with mkdir m2u1-4-client_libraries & cd m2u1-4-client_libraries.
3. Install the general or Cloud Storage client library (depending on the language and library) in your environment. If you are using a virtual environment, create it in a subfolder of that working directory.
  1. For example, instructions for installing the Cloud Storage library for Python: [Python Client for Google Cloud Storage](https://github.com/googleapis/python-storage)

**Authentication**

In module 5 on "Security and access control" we will look in depth at how to manage authentication and authorisation of users and applications.

For now, we'll create a service account as an identity for the script that the client library uses, and give you broad access to the project.

1. Create a new service account with the command gcloud iam service-accounts create ID_SA --description="DESCRIPTION_SA" --display-name="SA_NAME", replacing the capitalised values (note: use quotation marks in the parameters if you use spaces or special characters in them).
2. Give the service account the role of "editor" to give it ample access to perform actions on the project, with the command gcloud projects add-iam-policy-binding PROJECT_ID --member="serviceAccount:ID_SA@ ID_PROJECT.iam.gserviceaccount.com" --role="roles/editor", replacing the capitalised values ([docs](https://cloud.google.com/iam/docs/creating-managing-service-accounts#iam-service-accounts-create-gcloud)).
3. Create and download a credential file for the service account with the command gcloud iam service-accounts keys create FILE_NAME --IAM-ACCOUNT =ID_SA@ID_PROJECT.iam.gserviceaccount.com ([docs](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)).
4. Locate the JSON file FILE_NAME.json with the credentials of the downloaded service account.
5. Move the file FILE_NAME.json to the working directory you created.

**Sample Script for Cloud Storage**

Now let's run a small local script that uses the client library to create a Cloud Storage bucket.

Before you begin, check in the web console that the Cloud Storage API is enabled, or enable it if necessary. To do this, navigate to  **APIs and services > Library**  and search for the API.

1. Create a file and copy the sample code to create a Cloud Storage bucket from the library you're going to use: [docs](https://cloud.google.com/storage/docs/creating-buckets#storage-create-bucket-code_samples).
  1. As a bucket name, use a globally unique name such as ID_PROJECT-M2U1-4-task1-bucket or some variation of this to create multiple buckets.
2. Run the script, for example for python with the command python3 name_SCRIPT.py.
  1. Be careful, the example python script needs a line that runs the function at the end, such as print(create_bucket_class_location('NAME\_BUCKET')).
3. Check the result, _what happened?_
  1. You can check the available buckets with the gsutil ls command.
  2. If the script has worked:
    1. _QUESTION: In which project has it been created?_
    2. _QUESTION: With what authentication/user has the script and operation been run?_
    3. To do so, you can check in the  **ACTIVITY**  tab of the main console dashboard who has performed this action.
  3. If it did not work, which can happen when working locally, it is because the default application credentials or "ADC" have not been set. Continue with the next points and don't worry.
4. Add specific credentials for the script: To do this, follow the authentication instructions in the library documentation when creating the client to add application-specific service account credentials ([docs](https://cloud.google.com/docs/authentication/production#passing_code)).
5. Run the script again and check the result. _NOTE: If the bucket was previously created successfully, you will need to use a different bucket name._
  1. _DELIVERABLE_: M2U1-4-task_1-file_1-script_auth.[py|js|etc]: script with explicit authentication.
6. Now remove the script credentials to set a default application credentials or "ADC".
7. Set the "ADC" with an environment variable, e.g. in Linux export GOOGLE_APPLICATION_CREDENTIALS="PATH_FILE_CREDENTIALS" ([docs](https://cloud.google.com/docs/authentication/production#passing_variable)).
8. Run the script again and check the result. _NOTE: If the bucket was previously created successfully, you will need to use a different bucket name._
  1. _DELIVERABLE_: M2U1-4-task_1-file_2-script_adc.[py|js|etc]: script with authentication by "ADC".
9. Check the result of the previous actions in the web console, navigating to  **Storage > Browser**.

**Task 2: Google API libraries**

_BONUS: Could you repeat the steps for the Google APIs client libraries ?_[Google API Client Libraries](https://developers.google.com/api-client-library/)

_OPTIONAL DELIVERABLE_: M2U1-4-task_2-file_1-script.[py|js|etc]: script with "ADC" or explicit authentication.

**Deliverables Summary**

1. M2U1-4-questions.txt: Answers to all questions raised in the exercise.
2. M2U1-4-task_1-file_1-script_auth.[py|js|etc]: script with explicit authentication.
3. M2U1-4-task_1-file_2-script_adc.[py|js|etc]: script with authentication by "ADC".
4. (_Optional_) M2U1-4-task_2-file_1-script.[py|js|etc]: script with "ADC" or explicit authentication.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. In the web console, navigate to  **Storage > Browser**  and delete all created buckets.
2. In the console, navigate to  **IAM > Service Accounts**  and delete the service account created.
