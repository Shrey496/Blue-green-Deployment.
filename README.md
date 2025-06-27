# Blue-Green deployment using Github as source, Jenkins for Continuous Integration, and CodeDeploy for Continous Deployment on the AWS platform

# Objective:
The objective of this project is to deploy an application on EC2 instances in a Blue-Green environment using AWS CodeDeploy, with GitHub as the source code repository and Jenkins to build the code and trigger deployments.

# Blue-Green Deployment Overview
Blue-Green deployment is a powerful strategy to minimize downtime and reduce deployment risk by running two identical production environments:
Blue (currently live)


Green (idle during initial phase)


# How it works:
* The live environment (Blue) serves production traffic
* A new version of the application is deployed and tested in the idle environment (Green)
* Once validated, traffic is switched from Blue to Green
* If issues occur, rollback is as simple as routing traffic back to Blue

This approach ensures zero-downtime deployments and provides a quick recovery option.

# Tools & Technologies Used:
* AWS EC2 - Hosting Blue and Green environments
* AWS CodeDeploy - Automating deployment process
* Jenkins - Automating build & deployment trigger
* GitHub - Source code repository

# Prerequisites:
Before beginning this lab, ensure you have the following:
* An active AWS account
* A Virtual Machine or cloud-based Jenkins server
* A GitHub repository with your application source code
* Jenkins installed and configured with CodeDeploy plugin

# Deployment Workflow Overview:
* Push code to GitHub
* Jenkins pulls the latest code and creates a build
* Jenkins triggers AWS CodeDeploy
* CodeDeploy deploys the new version to the Green environment
* After testing, traffic is switched from Blue to Green
* Rollback by switching back to Blue if needed


# Steps followed on the AWS UI:
* Ensure that the user trying to setup this lab has sufficient permissions to interact and work with the following AWS services Auto Scaling Group, CodeDeploy, S3, IAM, & EC2.
* Create an S3 bucket. It is used in the following way:
  1. Jenkins builds the app and packages it
The .zip file is uploaded to an S3 bucket
CodeDeploy is triggered and pulls the artifact from that bucket
It deploys the app to the target EC2 instances (Blue or Green)

Create a CodeDeploy policy by navigating to IAM, allowing access to S3. Assign it to the role created for your EC2 instance (ensuring EC2 instances have access to necessary AWS services)


Similar to above point, create another role, this time for the CodeDeploy service to access S3 and other AWS services in order to facilitate the deployment and termination of applications on instances


Create an Application Load Balancer, with appropriate security group and target group configuration


Create an EC2 instance template using default settings for the upcoming step


Create an Auto-Scaling Group and choose the above created instance template in order for it to use the same for deploying the application EC2 instances (Ensure that you enable Load Balancing, and choose the created Target group)


Create a CodeDeploy application, add the service role that was created in the previous steps for the CodeDeploy service


Create a Deployment group within the CodeDeploy application. Ensure that you choose Blue/green as the deployment type,mention your autoscaling group under environmental configuration, choose your load balancer


# Steps followed for Jenkins:
Create a Jenkins freestyle job

Select source code as Git, and enter the Github repo. URL

Select a Post-Build job as Deploy an application to AWS CodeDeploy, and enter the details of your configuration on AWS

Build the project

Make a quick change to the index.html file, trigger a build right after making the change 
