# Cloud-project-notes
Learning and building with AWS resources

## Hosting a personal website using CloudFront

## Adding security headers to CloudFront distribution
watched this YT to add HTTP security headers to CloudFront: https://www.youtube.com/watch?v=x_QbJaSKSgU
used ssllabs to verify behavior changes were in effect. 
tested and passed SSL server test 

## Creating a ec2 instances using terraform 
using powershell import resources from terraform, instead of mac. new computer so I need to install apps to complete the project

# Checking the policies to download chocolately
`Get-ExecutionPolicy`


# Changed the policy to allow me to download chocolately
`Set-ExecutionPolicy AllSigned -Scope LocalMachine`

# Installing chocolately
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))


# Install putty
`choco install putty.install`

# Install terraform
`choco install terraform`

## Importing existing infrastructue resources into terraform
terraform CLI tools
  terraform import (the following are the arguments the tool uses)
    ADDR – the address of the resource in terraform (e.g. aws_instance.instance_name)
    ID – the resource ID in the cloud provider/k8s/database service/vcs service/etc

## Writing configuration 
I'm doing this on a Windows OS instead of macOS. So, since I'm installing terraform for a second time it's smoother due to prior troubleshooting. 
For ease, and probably the only way I could have executed on Windows, I installed windows subsystem for linux (WSL) and install Kali linus distro. I did this to follow the terraform tutorial because the commands were all in bash. 
I installed: 
  1) chocolately to install WSL
  2) putty - in case I need to SSH for any reason (but I still need to figure out how to set it up correctly) 
  3) installed Kali linux distro
  4) homebrew
  5) AWS cli
  6) terraform

Now, I'm following the Terraform tutorial to create infrastructure 
  remember that you need the keys and token in your path 

Successful. I was able to install, create and destroy infrastructure (3 of 5 for the tutorial)

## Importing ec2 (https://spacelift.io/blog/importing-exisiting-infrastructure-into-terraform)
I'm now working on importing resources to terraform. 
When I ran `terraform init` initially, I got an error message. I had to run `terraform init -upgrade` to allow selection of new versions
Remember to put keys in the path before running the command 
Questions to ask: how do I create multiple main.tf files? Or do I just have them in different folders?
Successful. I was able to import my ec2 instance to be managed by terraform 

containers ec2-install-run docker (hello world) connect to that over the internet 
container over a Kubernetes over a ec2 cluster - how are you ensuring security on the cluster 

docker 
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
