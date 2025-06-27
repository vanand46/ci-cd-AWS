# Automating CI/CD Pipeline for Spring Boot Application Deployment on AWS

To implement a CI/CD pipeline using AWS services for automating the deployment of a Spring
Boot application on Amazon ECS with Docker, integrating CodePipeline, CodeBuild and ECR for
seamless updates

## Real-time scenario

Imagine a retail company that has a Spring Boot-based inventory management system. The
system is used by several departments and is hosted in containers to ensure scalability. The
company wants to reduce the manual intervention required to deploy updates by automating
the build and deployment process. By setting up a CI/CD pipeline, developers can push updates
to a GitHub repository, automatically triggering a sequence of actions that build, containerize,
and deploy the application without downtime. This ensures faster and more reliable system
updates, minimizing operational delays.


## CI/CD Pipeline Flowchat

![alt text](image.png)

### How to Read the Flowchart:

1.  **GitHub.com**: The process starts when a developer pushes code to the GitHub repository.

2.  **CI/CD Pipeline (AWS CodePipeline)**: This push automatically triggers the AWS CodePipeline, which orchestrates the entire deployment process.

3.  **CodeBuild**: The pipeline's first step is to use AWS CodeBuild to compile the application and package it into a Docker container image.

4.  **ECR (Elastic Container Registry)**: The newly created image is then pushed to and stored in AWS ECR, a secure registry for container images.

5.  **ECS (Elastic Container Service)**: CodePipeline then instructs AWS ECS to pull the latest image from ECR and deploy it as a running container.

6.  **Live Application**: The deployment is complete, and the Spring Boot application is now running and accessible to users through the endpoint provided by the ECS service.

## Prerequisites

- **AWS Account**: You have an active AWS account with appropriate permissions to access ECS, ECR, CodePipeline, CodeBuild, IAM, and other related services.

- **GitHub Account**: You have a GitHub account with a repository containing your Spring Boot application code.

- **AWS CLI & IAM Configuration**: AWS CLI is installed and configured on your local machine or development environment with access credentials.

- **Spring Boot Application**: A Spring Boot application that is container-ready (includes a Dockerfile).

- **IAM Roles and Policies**: Necessary IAM roles and policies are created or will be created during the process to allow services like CodeBuild and CodePipeline to interact with ECS and ECR.

## Git Repo
You can find the Spring Boot application code repository at the following link: https://github.com/vanand46/aws-devops-springbootapp


## 1. Create ECR (Elastic Container Registry)

![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)

**Note** Copy and paste the contents from the dialog box after creation of the Registry somewhere. 

## 2. Go to GitHub Repo

- Click on buildspec.yml -> Edit the file
    ![alt text](image-4.png)

- Update the line 10 with the content copied in Step 1.
    ![alt text](image-5.png)

## 3. Go back to AWS -> ECR -> Copy the ECR image URI

![alt text](image-6.png)

## 4. Go to opened buildspec.yml

- Paste the copied ECR image URI on line number 12
    ![alt text](image-7.png)

- Commit the changes   

## 5. Go to AWS Code Build

- ![alt text](image-8.png) 

- Create the Project
    ![alt text](image-9.png)

- Click on Manage account credentials
    ![alt text](image-10.png)
    ![alt text](image-11.png)

- Upon clicking on Create new GitHub connection, a pop will open
    ![alt text](image-12.png) 
    ![alt text](image-13.png)  

- It will redirect to a Webpage with GitHub Authorize Button, then click the Authorize Button
   ![alt text](image-14.png) 
   ![alt text](image-15.png)

- After authorize, it will redirect AWS GitHub connection page, then press Connect
    ![alt text](image-16.png)
    ![alt text](image-17.png)

- Scroll down 
    ![alt text](image-18.png)
    ![alt text](image-19.png)

- Create build project
    ![alt text](image-20.png)

**YOU WILL GET BUILD FAILURE** After creating the build project, we may face the following error
![alt text](image-21.png)

**Note**:- Now when we build the image we will see the custom image is build on ECR but the problem
is the code build does not have the permission to access ecr and ecs resources…this we
need to give

## 6. Go to IAM -> Roles -> Select the role which created while create aws code build

- ![alt text](image-22.png)

- Add permissions -> Attach policies
    ![alt text](image-23.png)

- Add following Permission Policies
    - AdministratorAccess
    - AmazonEC2ContainerRegistryFullAccess
    - AmazonEC2ContainerRegistryPowerUser
    - ![alt text](image-24.png)
    - ![alt text](image-25.png)

- Add Permissions

## 7. Go to AWS Code Build Project created in Step 5

- Start Build
    ![alt text](image-26.png)

- We can see that build is successful
    ![alt text](image-27.png)

## 8. Go to ECS (Elastic Container Service)

- Create Cluster
    ![alt text](image-28.png)
    ![alt text](image-29.png)
    ![alt text](image-30.png)
    ![alt text](image-31.png)
    ![alt text](image-32.png)

- Create Task  
    ![alt text](image-33.png)
    ![alt text](image-34.png)
    ![alt text](image-35.png)
    ![alt text](image-36.png)

- Scroll down and keep the name of the container as spring-demo-ecr otherwise you need to change the name of the container from the buildspec.yml line number 30
    ![alt text](image-37.png)

    **Note** Go to ECR -> COPY THE IMAGE URI FROM THERE
    ![alt text](image-38.png)
    ![alt text](image-39.png)
    ![alt text](image-40.png)

## 8. Go To ECS -> Cluster -> Click on the cluster created

- ![alt text](image-41.png)

- Create a Service
    ![alt text](image-42.png)
    ![alt text](image-43.png)
    ![alt text](image-44.png)
    ![alt text](image-45.png)

- Create new security group and enable all traffic
    ![alt text](image-46.png)

- Then Press Create
    ![alt text](image-47.png)

- Wait for the task to change from pending to running 
    ![alt text](image-48.png)

- Copy the public from the Task Overview Page and Open it in browser and put :8080/demo/data 
    ![alt text](image-49.png)
    ![alt text](image-50.png)

## 9. Build the code pipeline and automate everything in code pipeline 

- ![alt text](image-51.png)

- ![alt text](image-52.png)

- ![alt text](image-53.png)

- ![alt text](image-54.png)

- Connect to github, A popup will appear, Select authorise code suite
    ![alt text](image-55.png)
    ![alt text](image-56.png)

- Next

- Click on other build providers

![alt text](image-57.png)

- Next
    ![alt text](image-58.png)
    ![alt text](image-59.png)
    ![alt text](image-60.png)

- Click on release change
    - ![alt text](image-61.png)

- Pipeline would be executed successfully
    ![alt text](image-62.png)
    ![alt text](image-63.png)

## 10. The application output

![alt text](image-64.png)




 



    

    






    










