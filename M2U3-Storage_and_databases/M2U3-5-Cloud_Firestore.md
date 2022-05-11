# **Cloud Firestore**

Unit M2U3 - Exercise 5

**What are we going to do?**

1. Create an instance of Cloud Firestore.
2. Manage data with client libraries.
3. Manage data from the console.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Create Cloud Firestore intstance**

First let's create the Cloud Firestore instance. Cloud Firestore is based on the old Cloud Datastore service, which can still be used as Cloud Firestore in Datastore mode and maintains the limitation of only being able to deploy one instance per project, so we cannot have instances in different locations or in different native/Datastore modes in the same project.

Create a Cloud Firestore instance.

1. On the console, navigate to  **Firestore**.
2. Select native mode.
3. As the location, select europe-west3.

Once the instance is created, we will use the client libraries to work with data, and then the console again to access it.

**Task 2: Manage data with client libraries**

In this task we will manage data with the Python client library.

_Note:_ You can follow the instructions below from Cloud Shell or your local Cloud SDK installation.

**Creating the local environment**

Now let's create a service account to authenticate our requests from client library scripts:

1. 1.	Create a folder in your local environment and access it: mkdir M2U3-5-Cloud_Firestore && cd M2U3-5-Cloud_Firestore.
2. Create a service account called SA_NAME: gcloud iam service-accounts create SA_NAME.
3. Assign the Firestore user role to the service account: gcloud projects add-iam-policy-binding PROJECT_ID --member ="serviceAccount: SA_NAME @ PROJECT_ID.iam.gserviceaccount.com" --role="roles/datastore.user".
4. Download a local credential key: gcloud iam service-accounts keys create credentials.json --iam-account = SA_NAME @PROJECT_ID.iam.gserviceaccount.com.
5. Enable the "Application Default Credentials" with the path to the credentials file: export GOOGLE_application_credentials ="PATH/DIRECTORY/ACTUAL/CREDENTIALS.JSON" (you can find out the path of the current directory with pwd).

Now create a Python virtual environment and install the library:

1. virtual pip installenv.
2. virtual env.
3. source env/bin/activate.
4. pip install -u google-cloud-firestore.

You can browse the [snippets.py](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/firestore/cloud-client/snippets.py) file that includes functions with the code we're going to use.

Now prepare a Python script that we will run with the code for each step:

1. Create a script called script.py.
2. In that file, copy the following Python lines of code (replacing PROJECT_ID):

from google.cloud import firestore

db = firestore.Client(project='PROJECT_ID’)

1. In each step, add the indicated code in subsequent lines, replacing the code from previous steps, but always keeping the 2 previous lines.
2. To run it at each step, do it with python script.py.

**Upload data**

To add a document with a linkID to the users collection, run the following code:

doc_ref = db.collection(u'users').document(u'alovelace')
doc_ref.set({
    u'first': u'Ada',
    u'last': u'Lovelace',
    u'born': 1815
})

To add another document with an aturing ID to the same collection, run the following code:

doc_ref = db.collection(u'users').document(u'aturing')
doc_ref.set({
    u'first': u'Alan',
    u'middle': u'Mathison',
    u'last': u'Turing',
    u'born': 1912
})


**Check data**

To view all documents in the users collection, run the following code:

users_ref = db.collection(u'users')
docs = users_ref.stream()

for doc in docs:
    print(f'{doc.id} => {doc.to_dict()}')


_BONUS:_ To learn more about how to manage data with the Firestore library for Python, you can follow the steps provided in the documentation:

1. [Add data](https://cloud.google.com/firestore/docs/manage-data/add-data).
2. [Search and filter data](https://cloud.google.com/firestore/docs/query-data/queries).
3. [Sort and limit data](https://cloud.google.com/firestore/docs/query-data/order-limit-data).

**Task 3: Manage data from the console**

Finally, in this task, we will consult and manage data through the console:

**Check data**

1. Navigate to  **Firestore > Data**.
2. To view a document or collection in a specific path, navigate to the interface or press the  **Edit Path**  button (pencil icon).
3. To filter data in a collection, open the collection, press the filter button (3 lines in inverted pyramid) and add some filters.

_DELIVERABLES:_ 

M2U3-5-task_3-file_1-screenshot_1.jpg: Screenshot of a document from a collection in the web console.

**Add data**

1. To add a collection, press the  **Start Collection**  button and use any ID or let Firestore generate one automatically.
2. To add a document, click the  **Add Document**  button and add fields or start a sub-collection.
3. To edit data, go to a document and tap a field to edit it, or click on  **Add Field**  or  **Start Collection**  to add fields or start a sub-collection.

**Clear data**

1. To delete a document, select the document and click the menu icon and click  **Delete Document**.
2. To delete a collection, select the collection and click the menu icon and click  **Delete Collection**.

**Deliverables Summary**

1. M2U3-5-task_3-file_1-screenshot_1.jpg: Screenshot of a document from a collection in the web console.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. Delete created collections and documents
2. (Optional) Delete the on-premises environment with the Python files and virtual environment.
