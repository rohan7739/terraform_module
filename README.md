## Overview

This project sets up a CI/CD pipeline using Jenkins running on an Ubuntu EC2 instance. The pipeline automates the process of:
1. Checking out code from a GitHub repository.
2. Initializing Terraform.
3. Planning the Terraform infrastructure changes.
4. Applying the Terraform configuration to create resources (like an S3 bucket).
5. Destroying the Terraform-managed infrastructure.

## Tools Used

- **Jenkins:** An open-source automation server used to automate parts of the software development process.
- **Ubuntu:** A Linux distribution used as the operating system for the EC2 instance.
- **EC2:** Amazon Elastic Compute Cloud, used to launch and manage the Ubuntu instance.
- **Terraform:** An open-source infrastructure as code software tool used to define and provision infrastructure.
- **S3:** Amazon Simple Storage Service, where the created bucket will be hosted.

## Prerequisites

- An AWS account with access to EC2 and S3.
- A GitHub account with the repository containing your Terraform configuration.
- Basic knowledge of Jenkins, Terraform, and AWS.

## Setup Instructions

### 1. Launch Ubuntu EC2 Instance

1. Log in to your AWS account.
2. Navigate to the EC2 dashboard.
3. Launch a new Ubuntu EC2 instance.
4. Configure the security group to allow SSH (port 22) and HTTP (port 8080) access.
5. Connect to your EC2 instance via SSH.

### 2. Install Jenkins

1. Update your package index:
    ```bash
    sudo apt update
    ```
2. Install Java:
    ```bash
    sudo apt install openjdk-11-jdk -y
    ```
3. Add Jenkins repository and key:
    ```bash
    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    ```
4. Install Jenkins:
    ```bash
    sudo apt update
    sudo apt install jenkins -y
    ```
5. Start Jenkins:
    ```bash
    sudo systemctl start jenkins
    sudo systemctl enable jenkins
    ```

### 3. Configure Jenkins

1. Open a web browser and navigate to `http://<your-ec2-public-ip>:8080`.
2. Follow the setup instructions to unlock Jenkins and install the suggested plugins.
3. Create an admin user.

### 4. Install Terraform

1. Download the Terraform binary:
    ```bash
    wget https://releases.hashicorp.com/terraform/1.0.0/terraform_1.0.0_linux_amd64.zip
    ```
2. Unzip the binary and move it to `/usr/local/bin`:
    ```bash
    unzip terraform_1.0.0_linux_amd64.zip
    sudo mv terraform /usr/local/bin/
    ```

### 5. Create Jenkins Pipeline Script

1. Open Jenkins and create a new pipeline project.
2. In the pipeline configuration, use the following script:

## Pipeline Script

```groovy
pipeline {
    agent any
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rohan7739/terraform_module.git'
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }
        stage('Terraform Apply') {
            steps {
                sh 'terraform apply --auto-approve'
            }
        }
        stage('Terraform Destroy') {
            steps {
                sh 'terraform destroy --auto-approve'
            }
        }
    }
}
```

## Stages
![terraform_pipeline_stage](https://github.com/rohan7739/terraform_module/assets/140694225/dae52aaf-9b2b-4d8a-b907-56381a81599b)

## Output
![s3_bucket_terraform](https://github.com/rohan7739/terraform_module/assets/140694225/9d2dd39d-9fcc-4378-b232-bcea21db7cab)
