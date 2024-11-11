
![](https://cdn-images-1.medium.com/max/3072/1*RPLwoQLQZD0udHCUhp2-lw.jpeg)

## AWS Project: Create a Continuous Delivery Pipeline

### Introduction

This project is a **Continuous Delivery (CD) pipeline** designed on AWS, utilizing **AWS CodePipeline**, **AWS CodeBuild**, and **AWS Elastic Beanstalk**. The pipeline automates the process of deploying code changes to production, enhancing the efficiency, consistency, and reliability of deployments. By implementing this architecture, I explored how to achieve automated, repeatable deployments that support high availability for web applications.

This CD pipeline is ideal for any development team looking to streamline their workflow, reduce human error, and implement rapid, consistent deployment processes.

### Tech Stack

* **AWS CodePipeline**: Manages the entire deployment flow, coordinating the build, test, and deployment stages.

* **AWS CodeBuild**: Automates the building and testing of code, ensuring that each change is thoroughly validated before deployment.

* **AWS Elastic Beanstalk**: Manages the deployment of the web application, ensuring it is hosted in a high-availability, scalable environment.

* **Amazon EC2 with Auto Scaling**: Ensures scalable, fault-tolerant infrastructure to support application load, with ALB (Application Load Balancer) for even traffic distribution.

### Prerequisites

* **AWS Account**: Required for configuring the CD pipeline and associated services.

* **Code Repository**: A source code repository such as **GitHub** or **CodeCommit** that integrates with CodePipeline.

* **Basic CI/CD Knowledge**: Familiarity with the principles of continuous integration and continuous delivery.

* **AWS CLI**: To facilitate configuration and command-line management.

### Problem Statement or Use Case

**Problem**: Manually deploying updates to web applications is error-prone, time-consuming, and can lead to inconsistent deployment practices, especially in fast-paced development environments.

**Solution**: The Continuous Delivery Pipeline ensures that each code change is automatically built, tested, and deployed to a managed environment, reducing manual intervention. This setup allows developers to push code more frequently, get faster feedback, and minimize deployment risks.

**Real-World Relevance**: In production settings, a CD pipeline is crucial for **agile teams** who need reliable, frequent deployments without impacting application uptime or performance. This project demonstrates how AWS can automate and manage application deployments, making it ideal for high-availability and fast-paced development scenarios.

### Architecture Diagram

![](https://cdn-images-1.medium.com/max/3840/1*V9c-nK9g3xTabZ4U2fBuQg.gif)

### Component Breakdown

 1. **Code Repository**: A source control repository (GitHub or CodeCommit) triggers the pipeline when new code is committed, enabling continuous integration.

 2. **AWS CodePipeline**: Automates the entire CI/CD process, orchestrating each stage from source to build to deployment.

 3. **AWS CodeBuild**: Builds and tests the code. CodeBuild compiles, runs unit tests, and verifies the application to ensure it is ready for deployment.

 4. **AWS Elastic Beanstalk**: Deploys the built application onto a highly available environment with Auto Scaling capabilities, which provides a robust infrastructure layer for the application.

### Step-by-Step Implementation

## Module 1: Set Up Git Repo

### Fork the starter repo

* This tutorial assumes you have an existing GitHub account and Git installed on your computer. If you don’t have either of these two installed, you can follow these [step-by-step instructions](https://docs.github.com/en/github/getting-started-with-github/quickstart).

 1. In a new browser tab, navigate to [GitHub](https://github.com/) and make sure you are logged into your account.

 2. In that same tab, open the [aws-elastic-beanstalk-express-js-sample](https://github.com/aws-samples/aws-elastic-beanstalk-express-js-sample) repo.

 3. Choose the white Fork button on the top right corner of the screen. Next, you will see a small window asking you where you would like to fork the repo.

 4. Verify it is showing your account and choose Create a fork. After a few seconds, your browser will display a copy of the repo in your account under Repositories.

### Push a change to your new repo

 1. Go to the [repository](https://github.com/aws-samples/aws-elastic-beanstalk-express-js-sample) and choose the green Code button near the top of the page.

 2. To clone the repository using HTTPS, confirm that the heading says *Clone with HTTPS.* If not, select the Use HTTPS link.

 3. Choose the white button with a clipboard icon on it (to the right of the URL).

![](https://cdn-images-1.medium.com/max/2000/0*Qv1fDI3RauSkmi1h.png)

4. If you’re on a Mac or Linux computer, open your terminal. If you’re on Windows, launch Git Bash.

5. In the terminal or Bash platform, whichever you are using, enter the following command and paste the URL you just copied in Step 2 when you clicked the clipboard icon. Be sure to change “YOUR-USERNAME” to your GitHub username. You should see a message in your terminal that starts with *Cloning into.* This command creates a new folder that has a copy of the files from the GitHub repo.

    git clone https://github.com/YOUR-USERNAME/aws-elastic-beanstalk-express-js-sample

6. In the new folder there is a file named app.js. Open app.js in your favorite code editor.

7. Change the message in line 5 to say something other than “Hello World!” and save the file.

8. Go to the folder created with the name aws-elastic-beanstalk-express-js-sample/ and Commit the change with the following commands:

    git add app.js
    git commit -m "change message"

9. Push the local changes to the remote repo hosted on GitHub with the following command. Note that you need to configure Personal access tokens (classic) under Developer Settings in GitHub for remote authentication.

    git push

### Test your changes

 1. In your browser window, open [GitHub](https://github.com/).

 2. In the left navigation panel, under Repositories, select the one named aws-elastic-beanstalk-express-js-sample.

 3. Choose the app.js file. The contents of the file, including your change, should be displayed.

## Application architecture

Here is what our architecture looks like right now:

![](https://cdn-images-1.medium.com/max/3840/0*gllxsA4U8AHODZYW.png)

We have created a code repository containing a simple web app. We will be using this repository to start our continuous delivery pipeline. It’s important to set it up properly so we push code to it.

## Module 2: Deploy Web App

## Implementation

### Configure an AWS Elastic Beanstalk app

 1. In a new browser tab, open the [AWS Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk/home?region=us-west-2#/welcome).

 2. Choose the orange Create Application button.

 3. Choose Web server environment under the Configure environment heading.

 4. In the text box under the heading Application name, enter DevOpsGettingStarted*.*

 5. In the Platform dropdown menu, under the Platform heading, select Node.js . Platform branch and Platform version will automatically populate with default selections.

 6. Confirm that the radio button next to Sample application under the Application code heading is selected.

 7. Confirm that the radio button next to Single instance (free tier eligible) under the Presets heading is selected.

 8. Select Next.

![](https://cdn-images-1.medium.com/max/3308/0*n0ygHvVoW6KOYkx7.png)

7. On the Configure service access screen, choose Use an existing service role for Service Role.

8. For EC2 instance profile dropdown list, the values displayed in this dropdown list may vary, depending on whether you account has previously created a new environment.

9. Choose one of the following, based on the values displayed in your list.

* If *aws-elasticbeanstalk-ec2-role* displays in the dropdown list, select it from the EC2 instance profile dropdown list.

* If another value displays in the list, and it’s the default EC2 instance profile intended for your environments, select it from the EC2 instance profile dropdown list.

* If the EC2 instance profile dropdown list doesn’t list any values to choose from, expand the procedure that follows, *Create IAM Role for EC2 instance profile*.

* Complete the steps in [Create IAM Role for EC2 instance profile](https://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-create-iam-instance-profile.html) to create an IAM Role that you can subsequently select for the EC2 instance profile. Then, return back to this step.

* Now that you’ve created an IAM Role, and refreshed the list, it displays as a choice in the dropdown list. Select the IAM Role you just created from the EC2 instance profile dropdown list.

10. Choose Skip to Review on the Configure service access page.

This will select the default values for this step and skip the optional steps.

![](https://cdn-images-1.medium.com/max/3248/0*V2MhADiskImFNJ1_.png)

11. The Review page displays a summary of all your choices.

12. Choose Submit at the bottom of the page to initialize the creation of your new environment.

![](https://cdn-images-1.medium.com/max/3196/0*m5IJ3rlYE8Pqfgns.png)

While waiting for deployment, you should see:

* A screen that will display status messages for your environment.

* After a few minutes have passed, you will see a green banner with a checkmark at the top of the environment screen.

Once you see the banner, you have successfully created an AWS Elastic Beanstalk application and deployed it to an environment.

### Test your web app

 1. To test your sample web app, select the link under the name of your environment.

![](https://cdn-images-1.medium.com/max/4000/0*zMlLYQ9cADk8z-cp.png)

* 2. Once the test has completed, a new browser tab should open with a webpage congratulating you!

![](https://cdn-images-1.medium.com/max/2016/0*j5eSQhpJKEo7yVng.png)

## Application architecture

Now that we are done with this module, our architecture will look like this:

![](https://cdn-images-1.medium.com/max/3840/0*38va3NUoNRYSlDpg.png)

We have created an AWS Elastic Beanstalk environment and sample application. We will be using this environment and our continuous delivery pipeline to deploy the Hello World! web app we created in the previous module.

## Module 3: Create Build Project

### Configure the AWS CodeBuild project

 1. In a new browser tab, open the [AWS CodeBuild console](https://console.aws.amazon.com/codesuite/codebuild/start?region=us-west-2).

 2. Choose the orange Create project button.

 3. In the Project name field, enter *Build-DevOpsGettingStarted.*

 4. Select GitHub from the Source provider dropdown menu.

 5. Confirm that the Connect using OAuth radio button is selected.

 6. Choose the white Connect to GitHub button. A new browser tab will open asking you to give AWS CodeBuild access to your GitHub repo.

 7. Choose the green Authorize aws-codesuite button.

 8. Enter your GitHub password.

 9. Choose the orange Confirm button.

 10. Select Repository in my GitHub account.

 11. Enter *aws-elastic-beanstalk-express-js-sample* in the search field.

 12. Select the repo you forked in Module 1. After selecting your repo, your screen should look like this:

![](https://cdn-images-1.medium.com/max/3224/0*2F5rdi14dLrxrGt3.png)

13. Confirm that Managed Image is selected.

14. Select Amazon Linux 2 from the Operating system dropdown menu.

15. Select Standard from the Runtime(s) dropdown menu.

16. Select aws/codebuild/amazonlinux2-x86_64-standard:3.0 from the Image dropdown menu.

17. Confirm that Always use the latest image for this runtime version is selected for Image version.

18. Confirm that Linux is selected for Environment type.

19. Confirm that New service role is selected.

### Create a Buildspec file for the project

 1. Select Insert build commands.

 2. Choose Switch to editor.

 3. Replace the Buildspec in the editor with the code below:

    version: 0.2
    phases:
        build:
            commands:
                - npm i --save
    artifacts:
        files:
            - '**/*'

4. Choose the orange Create build project button. You should now see a dashboard for your project.

### Test the CodeBuild project

 1. Choose the orange Start build button. This will load a page to configure the build process.

 2. Confirm that the loaded page references the correct GitHub repo.

 3. Choose the orange Start build button.

 4. Wait for the build to complete. As you are waiting you should see a green bar at the top of the page with the message *Build started,* the progress for your build under Build log, and, after a couple minutes, a green checkmark and a *Succeeded* message confirming the build worked.

## Application architecture

Here’s what our architecture looks like now:

![](https://cdn-images-1.medium.com/max/3840/0*CkTSMxJ5B7As9wy-.png)

We have created a build project on AWS CodeBuild to run the build process of the Hello World! web app from our GitHub repository. We will be using this build project as the build step in our continuous delivery pipeline, which we will create in the next module.

## Module 4: Create Delivery Pipeline

### Create a new pipeline

 1. In a browser window, open the [AWS CodePipeline console](https://console.aws.amazon.com/codesuite/codepipeline/start?region=us-west-2).

 2. Choose the orange Create pipeline button. A new screen will open up so you can set up the pipeline.

 3. In the Pipeline name field, enter *Pipeline-DevOpsGettingStarted.*

 4. Confirm that New service role is selected.

 5. Choose the orange Next button.

### Configure the source stage

 1. Select GitHub version 1 from the Source provider dropdown menu.

 2. Choose the white Connect to GitHub button. A new browser tab will open asking you to give AWS CodePipeline access to your GitHub repo.

 3. Choose the green Authorize aws-codesuite button. Next, you will see a green box with the message *You have successfully configured the action with the provider.*

 4. From the Repository dropdown, select the repo you created in Module 1.

 5. Select main from the branch dropdown menu.

 6. Confirm that GitHub webhooks is selected.

 7. Choose the orange Next button.

### Configure the build stage

 1. From the Build provider dropdown menu, select AWS CodeBuild.

 2. Under Region confirm that the US West (Oregon) Region is selected.

 3. Select Build-DevOpsGettingStarted under Project name.

 4. Choose the orange Next button.

### Configure the deploy stage

 1. Select AWS Elastic Beanstalk from the Deploy provider dropdown menu.

 2. Under Region, confirm that the US West (Oregon) Region is selected.

 3. Select the field under Application name and confirm you can see the app DevOpsGettingStarted created in Module 2.

 4. Select DevOpsGettingStarted-env from the Environment name textbox.

 5. Choose the orange Next button. You will now see a page where you can review the pipeline configuration.

 6. Choose the orange Create pipeline button.

### Watch first pipeline execution

* While watching the pipeline execution, you will see a page with a green bar at the top. This page shows all the steps defined for the pipeline and, after a few minutes, each will change from blue to green.

 1. Once the Deploy stage has switched to green and it says *Succeeded,* choose AWS Elastic Beanstalk. A new tab listing your AWS Elastic Beanstalk environments will open.

 2. Select the URL in the Devopsgettingstarted-env row. You should see a webpage with a white background and the text you included in your GitHub commit in Module 1.

## Application architecture

Here’s what our architecture looks like now:

![](https://cdn-images-1.medium.com/max/3840/0*dANKt9vzJTzvVtYy.png)

We have created a continuous delivery pipeline on AWS CodePipeline with three stages: source, build, and deploy. The source code from the GitHub repo created in Module 1 is part of the source stage. That source code is then built by AWS CodeBuild in the build stage. Finally, the built code is deployed to the AWS Elastic Beanstalk environment created in Module 3.

## Module 5: Finalize Pipeline and Test

### Create a review stage in pipeline

 1. Open the [AWS CodePipeline console](https://console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-west-2).

 2. You should see the pipeline we created in Module 4, which was called Pipeline-DevOpsGettingStarted. Select this pipeline.

 3. Choose the white Edit button near the top of the page.

 4. Choose the white Add stage button between the Build and Deploy stages.

 5. In the Stage name field, enter *Review.*

 6. Choose the orange Add stage button.

 7. In the Review stage, choose the white Add action group button.

 8. Under Action name, enter *Manual_Review.*

 9. From the Action provider dropdown, select Manual approval.

 10. Confirm that the optional fields have been left blank.

 11. Choose the orange Done button.

 12. Choose the orange Save button at the top of the page.

 13. Choose the orange Save button to confirm the changes. You will now see your pipeline with four stages: Source, Build, Review, and Deploy.

### Push a new commit to your repo

 1. In your favorite code editor, open the app.js file from Module 1.

 2. Change the message in Line 5.

 3. Save the file.

 4. Open your preferred Git client.

 5. Navigate to the folder created in Module 1.

 6. Commit the change with the following commands:

    git add app.js
    git commit -m "Full pipeline test"

7. Push the local changes to the remote repo hosted on GitHub with the following command:

    git push

### Monitor the pipeline and manully approve the change

 1. Navigate to the [AWS CodePipeline console](https://console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-west-2).

 2. Select the pipeline named Pipeline-DevOpsGettingStarted. You should see the Source and Build stages switch from blue to green.

 3. When the Review stage switches to blue, choose the white Review button.

 4. Write an approval comment in the Comments textbox.

 5. Choose the orange Approve button.

 6. Wait for the Review and Deploy stages to switch to green.

 7. Select the AWS Elastic Beanstalk link in the Deploy stage. A new tab listing your Elastic Beanstalk environments will open.

 8. Select the URL in the Devopsgettingstarted-env row. You should see a webpage with a white background and the text you had in your most recent GitHub commit.

* Congratulations! You have a fully functional continuous delivery pipeline hosted on AWS.

## Application architecture

With all modules now completed, here is the architecture of what you built:

![](https://cdn-images-1.medium.com/max/3840/0*pMU98EwfZqznVL_6.png)

We have used AWS CodePipeline to add a review stage with manual approval to our continuous delivery pipeline. Now, our code changes will have to be reviewed and approved before they are deployed to AWS Elastic Beanstalk.

## Clean up resources

### Delete AWS Elastic Beanstalk application

 1. In a new browser window, open the [AWS Elastic Beanstalk Console](https://console.aws.amazon.com/elasticbeanstalk/home?region=us-west-2#/applications).

 2. In the left navigation menu, click on “Applications.” You should see the “DevOpsGettingStarted” application listed under “All applications.”

 3. Select the radio button next to “DevOpsGettingStarted.”

 4. Click the white dropdown “Actions” button at the top of the page.

 5. Select “Delete application” under the dropdown menu.

 6. Type “DevOpsGettingStarted” in the text box to confirm deletion.

 7. Click the orange “Delete” button.

### Delete pipeline in AWS CodePipeline

 1. In a new browser window, open the [AWS CodePipeline Console](https://console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-west-2).

 2. Select the radio button next to “Pipeline-DevOpsGettingStarted.”

 3. Click the white “Delete pipeline” button at the top of the page.

 4. Type “delete” in the text box to confirm deletion.

 5. Click the orange “Delete” button.

### Delete pipeline resources from Amazon S3 bucket

 1. In a new browser window, open the [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/home?region-us-west-2).

 2. You should see a bucket named “codepipeline-us-west-2” followed by your AWS account number. Click on this bucket. Inside this bucket, you should see a folder named “Pipeline-DevOpsGettingStarted.”

 3. Select the checkbox next to the “Pipeline-DevOpsGettingStarted” folder.

 4. Click the white “Actions” button from the dropdown menu.

 5. Select “Delete” under the dropdown menu.

 6. Click the blue “Delete” button.

### Delete build project in AWS CodeBuild

 1. In a new browser window, open the [AWS CodeBuild Console](https://console.aws.amazon.com/codesuite/codebuild/projects?region=us-west-2).

 2. In the left navigation, click on “Build projects” under “Build.” You should see the “Build-DevOpsGettingStarted” build project listed under “Build project.”

 3. Select the radio button next to “Build-DevOpsGettingStarted.”

 4. Click the white “Delete build project” button at the top of the page.

 5. Type “delete” in the text box to confirm deletion.

 6. Click the orange “Delete” button.

## Congratulations!

You successfully built a continuous delivery pipeline on AWS! As a great next step, dive deeper into specific AWS technologies and take your application to the next level.

### Challenges Faced and Solutions

* **Deployment Failures Due to Environment Variables**: At times, missing environment variables caused builds to fail.

* **Solution**: Ensured that the required environment variables were securely stored in Elastic Beanstalk and made accessible to the application during deployment.

* **CodePipeline Integration with External Repositories**: Faced issues when integrating CodePipeline with GitHub.

* **Solution**: Configured OAuth permissions carefully and verified GitHub webhook functionality to trigger pipeline events.

### Conclusion

This project showcases a **Continuous Delivery pipeline on AWS** that enables teams to automate the entire deployment process, from code commit to production deployment. With CodePipeline, CodeBuild, and Elastic Beanstalk, this architecture supports scalable, high-availability web applications that can handle rapid code changes.

By implementing this pipeline, development teams can focus more on code quality and less on manual deployment steps, leading to faster feature releases and more reliable application performance.

## Code Repository

To explore and experiment with the project’s code and documentation, visit the [**GitHub Repository](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/7.%20Create%20a%20Continuous%20Delivery%C2%A0Pipeline)**. Here you can access the complete code, follow along with detailed instructions, and customize the applications to fit your own use cases.
>  ***Asif Khan — Aspiring Cloud Architect | Weekly Cloud Learning Chronicler***

[**LinkedIn](https://www.linkedin.com/in/asif0108/)**/[**Twitter](https://x.com/asif26073)**/[**GitHub](https://github.com/Dark-Cookie)**
