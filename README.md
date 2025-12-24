# Notes on working in the cloud and using cloud tools
This repository is all about me building with AWS resources and learning new tools. Here I will share my thoughts on my cloud projects. 

## Project_1: Creating a personal website and hosting on AWS CloudFront
I have a personal website that I am migrating to AWS so it can be hosted on CloudFront. Updates coming soon. 

## Project_2: Adding security headers to CloudFront distribution to secure my website
I watched this YouTube on how to add HTTP security headers to CloudFront: https://www.youtube.com/watch?v=x_QbJaSKSgU
used ssllabs to verify behavior changes were in effect. 
Followed the steps, tested and passed SSL server test 

## Creating a EC2 instances using terraform 
Using powershell to import resources from Terraform, instead of my Mac. Since this is a Windows new computer so I need to install apps to complete the project. Here are some commands I used along the way:

## Installing WSL 

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
