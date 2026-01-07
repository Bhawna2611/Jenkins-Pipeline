Jenkins RBAC & SSO Implementation
Project Overview

This project focuses on implementing User Authentication and User Authorization in Jenkins. The goal is to set up a Role-Based Access Control (RBAC) system for three distinct engineering teams and integrate Single Sign-On (SSO) for administrative access.
Objectives

    Job Creation: Set up 9 specialized jobs (3 for Dev, 3 for Test, 3 for DevOps).

    Authentication: Create local users for each team.

    Authorization: Implement a "Role-Based Strategy" to restrict who can see, build, and configure specific jobs.

    SSO Integration: Configure Google OAuth for the Admin user.

Permissions Logic
User Role	Can See	Can Build/Configure	Can See Others?
Developer	Dev Jobs	Dev Jobs	No
Testing	Test Jobs	Test Jobs	View Dev Jobs
DevOps	DevOps Jobs	DevOps Jobs	View Dev & Test Jobs
Admin	All	All	All (Full Access)
Step 1: Create the Jenkins Jobs

Before we handle permissions, we need to create the "dummy" jobs.

Task:

    Go to your Jenkins Dashboard.

    Click New Item.

    Create 9 "Freestyle project" jobs with the following names:

        dev-1, dev-2, dev-3

        test-1, test-2, test-3

        devops-1, devops-2, devops-3

    In each job's configuration:

        Scroll down to Build Steps.

        Add Execute shell (Linux) or Execute Windows batch command.

        Enter this command:
        Bash

        echo "Job Name: ${JOB_NAME}"
        echo "Build Number: ${BUILD_NUMBER}"

Step 2: Create the Users

Now we need to create the users that will eventually be assigned to roles.

Task:

    Go to Manage Jenkins > Users.

    Create the following users:

        developer-1, developer-2

        testing-1, testing-2

        devops-1, devops-2

        admin-1

Comparison of Authorization Strategies

The assignment asks you to look at the different strategies. Here is a quick breakdown so you can choose the best one:

    Legacy Mode: Very old; you are either an admin or a read-only user. (Not suitable here).

    Matrix-based Security: You assign permissions user-by-user in a big table. (Hard to manage for many users).

    Project-based Matrix Authorization: Like Matrix, but defined per specific job.

    Role-Based Strategy (Best for this task): You create "Roles" (like Developer) and assign "Users" to those roles. This is the most professional way to handle your assignment.
