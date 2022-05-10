# Work environment
Unit M2U1 - Exercise 0

## What are we going to do?
1.  Create our Google Cloud project and billing account.
2.	Activate free credits for new users.
3.	Configure the work environment.

## Initial creation of the work environment
*NOTE:* Remember to get used to creating the practice delivery folder and subfolders for each exercise before you start them.

### Assignment 1: Manage billing and free credits
We will activate the free credits program for new users in order to use it during the course.

#### User account
The first thing you need to use Google Cloud is a Google user account.

For user accounts, you can use a GMail email account, an @google.com account, an account created in Google Workspace (formerly known as GSuite) for business or academia, Cloud Identity, or a Google user account without an associated GMail email address.

If you do not have any of them, you can create a user account with GMail linked or not, here:[Google Sign in](https://accounts.google.com/servicelogin).

Once your user account is available, log in to  [google.com](https://www.google.com/) with it.

#### Enable free credits
The Google Cloud free credit program has 2 advantages:
1. $300 in free Google Cloud credits for new users for 90 days.
1. "Always free" services that, if we do not exceed a certain level of use, will always be free.

You can view the terms and conditions of the program here: [Google Cloud Free Program](https://cloud.google.com/free/docs/gcp-free-tier/).

*Note: If you have any problems meeting the conditions or during the activation of the program, please contact your teacher via the platform.*

***Important note:** To prove your identity and that you are a new, real user, you will be asked for your debit or credit card, but no charges will be made after the $300 has run out if you do not upgrade your billing account to a paid account. More information in the program conditions.*

You can sign up for the program by following the instructions on the  **Start for free** button (top right corner) which you will find at the following link: [Google Cloud Free Program](https://cloud.google.com/free).

By doing this:
- A billing account will be created with the free credits activated.
-	A Google Cloud project called "My First Project" will be created so that you can work directly on it.


Example videos:
- [The Google Cloud Platform Free Trial and Free Tier](https://www.youtube.com/watch?v=P2ADJdk5mYo)
- [How to activate Google Cloud Platform's Free Trial account detailed steps to get $300 for 90 days?](https://www.youtube.com/watch?v=jbpSPO9np3E)

Once the program is activated, you will access the Google Cloud web console or [Google Cloud Console](https://console.cloud.google.com).

When you access the console, you will see a top bar with information about how to use the free program.

You can also always access the billing account information created with the credits enabled in the **Billing** section of the navigation menu (hamburger menu in the upper left corner).

### Task 2: Set up the project in Google Cloud
Now let's set up the automatically created Google Cloud project.

To do this, in the console, using the navigation menu navigate to [**IAM and administration > Settings**](https://console.cloud.google.com/iam-admin/settings) (direct link to the console section).

The project ID is an immutable value, but you can change the project name to a more descriptive one.

You can also create a new project with a different globally unique project ID by navigating to [**IAM and admin > Create Project**](https://console.cloud.google.com/projectcreate) (direct link to the console section), selecting the automatically created billing account.

For the practicals, we will only use one project, so please select one project and use the same one for all of them. You can always delete a project created in [**IAM and admin > Settings**](https://console.cloud.google.com/iam-admin/settings) (***Note: be very careful not to delete all your projects or you will have to follow the link in the previous paragraph to create a new one)***).

#### Grant permission to the teacher
In order to be able to help you if you have any problems during the course and to correct any practicals if necessary, you must grant the teacher access to your work project.

To do so:
1. Navigate to [**IAM and Administration > IAM**](https://console.cloud.google.com/iam-admin/iam) (direct link to the console section).
1. Click the **Add** button.
1. Add the teacher as a new member: info@indavelopers.com - Marcos Manuel Ortega
1. For function or role, select  **Basic > Owner**.
1. Click **Save**.

*DELIVERABLE:*
As deliverable of this exercise, if you have done everything correctly, you should receive an automatic email with the project invitation for your teacher, who will check that he/she has access and that everything works correctly.

Therefore, it generates one file "M2U1-0-questions.txt" with the following information:
- Your full name.
- The email address used to log in to Google Cloud.
- Your ID project (*note*: the unique ID of the project, not the name of the project).

## Deliverables Summary
1. 1.	M2U1-0-questions.txt: Name, email and ID of the project.

