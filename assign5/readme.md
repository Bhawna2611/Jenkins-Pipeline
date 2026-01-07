Java CI/CD Pipeline with Declarative Jenkinsfile
📌 Project Overview

This project demonstrates a robust Continuous Integration (CI) pipeline for a Java-based web application. The pipeline is designed using Jenkins Declarative Syntax and incorporates industry-standard practices such as parallel execution, automated quality gates, and manual approval triggers.
🚀 Key Features

    Parallel Scanning: Executes unit tests, code quality checks, and code coverage simultaneously to reduce build time.

    Skip Logic (Parameters): Provides flexibility to the user to skip specific scans (Stability, Quality, or Coverage) during build execution.

    Quality Reporting: Generates and archives HTML reports for Checkstyle and Jacoco.

    Manual Approval Gate: Implements a pause point for human intervention to verify reports before the final artifact is published.

    Artifact Management: Packages the application into a .war file and archives it within Jenkins.

    Post-Build Notifications: Automated Email/Console notifications based on the success or failure of the build.

🛠 Tech Stack

    CI Tool: Jenkins (Declarative Pipeline)

    Build Tool: Maven 3.x

    Language: Java (JDK 8)

    Code Quality: Checkstyle

    Code Coverage: Jacoco

    Artifact: Web Application Archive (.war)

📋 Pipeline Stages
1. Code Checkout

Pulls the latest source code from the Git repository.
2. Parallel Analysis Scans

This stage executes three sub-stages in parallel:

    Code Stability: Runs JUnit tests using mvn test.

    Code Quality: Performs static code analysis using Checkstyle to ensure coding standards.

    Code Coverage: Uses Jacoco to measure the percentage of code covered by automated tests.

3. Archive Reports

Consolidates all generated HTML reports from the parallel stages and attaches them to the Jenkins build dashboard for easy viewing.
4. Approval Gate

A mandatory manual step where the pipeline waits for an authorized user to "Approve" or "Deny" the publication based on the analysis results.
5. Publish Artifacts

Once approved, the pipeline runs mvn package (skipping tests for speed) to generate the final .war file and saves it as a build artifact.
6. Post-Build Actions

Final execution block that triggers notifications (Email/Slack/Logs) to inform the team of the build status.
⚙️ How to Run

    Open your Jenkins Job.

    Select "Build with Parameters".

    (Optional) Check the boxes if you wish to skip specific scans.

    Click "Build".

    Monitor the Parallel Stage View for progress.

    Once the pipeline reaches the Approval Gate, review the archived reports and click "Proceed" to generate the final artifact.

📄 Final Artifacts Location

After a successful run, the following files can be found in the Jenkins build workspace:

    **/target/*.war (Deployable Application)

    **/target/site/** (Full Quality & Coverage Reports)

Would you like me to generate a text file containing the final Jenkinsfile code as well to keep everything organized?
