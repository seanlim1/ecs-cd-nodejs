## Brief

### Preparation

- AWS account with permissions to create ECR repository and push Docker images.
- GitHub account with a repository containing Dockerfile and other necessary files for your Docker image.
- Docker installed locally on your development machine.
- Basic familiarity with Docker and GitHub Actions.

### Lesson Overview

Docker has revolutionized how applications are deployed and managed by allowing developers to package their applications and dependencies into a single container that can run consistently across different environments. AWS Elastic Container Registry (ECR) is a fully managed container registry that makes it easy to store, manage, and deploy Docker images to run containerized applications on AWS.

GitHub Actions is a powerful automation tool provided by GitHub that allows you to automate various tasks, including building and pushing Docker images, based on triggers such as code pushes, pull requests, or scheduled events. In this tutorial, we will walk through the steps to build and push Docker images to AWS ECR using GitHub Actions, enabling a seamless and automated workflow for your containerized applications.


---

## Part 1 - Create an ECR Repository

Redo prev. lesson to create repository (3.4	Cloud Native Application - Containerization I & 3.5	Cloud Native Application - Containerization II)

---

## Part 2 - Configure AWS Credentials

Redo prev. lesson to create repository (3.4	Cloud Native Application - Containerization I & 3.5	Cloud Native Application - Containerization II)

---

## Part 3 - Create GitHub Actions Workflow

Now, we will create a GitHub Actions workflow that will build and push Docker images to AWS ECR whenever changes are pushed to the repository. Follow these steps:

- Navigate to the root of your GitHub repository and create a `.github/workflows` directory if it doesn't exist.
- Inside the `.github/workflows` directory, create a new YAML file, for example `docker-ecr.yml`, to define the workflow.
- Open the YAML file in a text editor and define the workflow as follows:

```
name: Deploy to ECR

on:
 
  push:
    branches: [ master ]

jobs:
  
  build:
    
    name: Build Image
    runs-on: ubuntu-latest

   
    steps:

    - name: Check out code
      uses: actions/checkout@v2
    
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ap-south-1

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1

    - name: Build, tag, and push image to Amazon ECR
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        ECR_REPOSITORY: docker_nodejs_demo
        IMAGE_TAG: nodejs_demo_image
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
```

