# Notes on working in the cloud and use tools
This repository is all about me building with AWS resources and learning new tools. Here I will share my thoughts on my experiences. 

## Adding security headers to CloudFront distribution
I watched this YouTube on how to add HTTP security headers to CloudFront: https://www.youtube.com/watch?v=x_QbJaSKSgU
used ssllabs to verify behavior changes were in effect. 
Followed the steps, tested and passed SSL server test 

## Creating a EC2 instances using terraform 
Using powershell to import resources from Terraform, instead of mac. new computer so I need to install apps to complete the project. Here are some commands I used along the way:

## Checking the policies to download chocolately
`Get-ExecutionPolicy`

## Changed the policy to allow me to download chocolately
`Set-ExecutionPolicy AllSigned -Scope LocalMachine`

## Installing chocolately
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

## Install putty
`choco install putty.install`

## Install terraform
`choco install terraform`


Docker 
files > series of commands, copy, install, running main process (starting point)

image > code, runtime, libraries, env. variables, config files (executable package); inherit from a base image containing pre-installed dependencies that you can build on
- publicly available images available 

containers > runnable instance of an image
- deploy on your local machine or cloud provider

container orchestration system > docker swamp, k8s, ECS; if you need containers to run constantly 

following the YT video docker tutorial.
confirmed at docker was installed correctly: `docker --version`

initiated docker to build the dependencies necessary to build a container: `docker init`

building the container:`docker build -t first-time .`

I was able to get the container created, but the application is not running. I am attempting to troubleshoot using AI (gemini). Successfully able to run the container and verify the app was running. Screenshots. 

running the container: `docker run -p 127.0.0.1:8080:8080 first-time`

volumes: 
# Mapping a folder from my local machine to the container
 `docker run -v C:\Users:/container_folder first-time`

#creating a shared volume
`docker volume create shared_volume`
`docker run -v shared_volume:/my_path first-time`

# deploying multiple containers using docker compose
I had the biggest issue here with the code. I keep getting an error message with container stack only half deployed. The issue is the password.txt file is not being read by properly by Docker. Trying to use AI to troubleshoot, but no luck. I will move past and revisit

# deploying container to aws 

1. Logged into ECR via docker terminal: `aws ecr get-login-password --region <region>| docker login --username AWS --password-stdin <awsaccountname>.dkr.ecr.<region>.amazonaws.com`

2. Tagged the image: `docker tag first-time:latest <awsaccountname>.dkr.ecr.<region>.amazonaws.com/first-time:latest`

3. Created a repository in the AWS console

4. Pushed the image to ECR `docker push <awsaccountname>.dkr.ecr.<region>.amazonaws.com/first-time:latest`

5. Created a cluster "cluster-demo-2". The first time I tried to create the cluster is failed. After researching, I learned that I needed to create an IAM for cluster. In the console, I went to IAM > Roles > Create role > Trusted entity type: AWS service > Use case: Elastic Container Service. I gave the role a name and created it. Upon retrying the cluster, it was a success. 

6. Created the task definition "flask-definition"

7. Assigned service name to deploy the container: flask-app-service-u8oap0gc

8. Container is running, but I had to configure a security group to allow access from the port my container was listening on. Success!
