# Jenkins-Docker-Project
# CI/CD Pipeline Project using Jenkins, Maven & Docker

## Project Overview

This project demonstrates how to build and deploy a Java web application using:

- Maven
- GitHub
- Jenkins
- Docker
- AWS EC2

The Jenkins pipeline automates:
1. Source Code Checkout
2. Maven Build
3. Docker Image Creation
4. Docker Container Deployment

---

# Tech Stack

- Java 17
- Maven
- Jenkins
- Docker
- GitHub
- AWS EC2 (Ubuntu)

---

# Architecture Flow

```text
Developer Pushes Code to GitHub
                ↓
        Jenkins Pipeline Triggered
                ↓
            Maven Build
                ↓
         Docker Image Build
                ↓
      Docker Container Deploy
                ↓
        Application Accessible
```

---

# Prerequisites

Before starting, ensure you have:

- AWS Account
- Ubuntu EC2 Instance
- GitHub Repository
- Internet Connection

---

# Step-1 : Create AWS EC2 Instance

Create Ubuntu EC2 instance with:

- Instance Type : t2.medium
- Storage : Minimum 15 GB

Enable below ports in Security Group:

| Port | Purpose |
|------|----------|
| 22 | SSH |
| 8080 | Jenkins |
| 8081 | Application |

---

# Step-2 : Install Java

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```

---

# Step-3 : Install Jenkins

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins
```

---

# Step-4 : Start Jenkins

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

---

# Step-5 : Access Jenkins

Open browser:

```text
http://<public-ip>:8080/
```

Get admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Complete:
- Install Suggested Plugins
- Create Admin User

---

# Step-6 : Configure Maven in Jenkins

Navigate to:

```text
Manage Jenkins
    ↓
Tools
    ↓
Maven Installation
```

Add Maven with name:

```text
Maven-3.9.9
```

---

# Step-7 : Install Docker

```bash
curl -fsSL get.docker.com | /bin/bash
```

Add users to Docker group:

```bash
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

Verify Docker:

```bash
sudo docker version
```

---

# Step-8 : Create Jenkins Pipeline Job

Create a Pipeline Job in Jenkins and use below pipeline script.

## Jenkinsfile

```groovy
pipeline {

    agent any

    tools {
        maven 'Maven-3.9.9'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/ashokitschool/maven-web-app.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ashokit/mavenwebapp .'
            }
        }

        stage('Run Docker Container') {
            steps {

                sh 'docker stop javaapp || true'

                sh 'docker rm javaapp || true'

                sh 'docker run -d -p 8081:8080 --name javaapp ashokit/mavenwebapp'
            }
        }
    }
}
```

---

# Step-9 : Trigger Jenkins Job

Click:

```text
Build Now
```

Pipeline will:
- Clone code from GitHub
- Build Maven project
- Create Docker image
- Deploy Docker container

---

# Step-10 : Access Application

Open browser:

```text
http://<public-ip>:8081/maven-web-app/
```

---

# Pipeline Stages

| Stage | Description |
|------|-------------|
| Clone Repository | Pull source code from GitHub |
| Build Application | Build project using Maven |
| Build Docker Image | Create Docker image |
| Run Docker Container | Deploy application container |

---

# Useful Commands

## Check Jenkins Status

```bash
sudo systemctl status jenkins
```

## Restart Jenkins

```bash
sudo systemctl restart jenkins
```

## Check Running Containers

```bash
docker ps
```

## Check Docker Images

```bash
docker images
```

---

# Output

After successful execution:

✅ Jenkins Installed  
✅ Maven Configured  
✅ Docker Installed  
✅ CI/CD Pipeline Created  
✅ Application Deployed Successfully  

---

# Future Enhancements

- Integrate SonarQube
- Add Kubernetes Deployment
- Add GitHub Webhooks
- Implement CI/CD using Kubernetes

---

# Conclusion

This project demonstrates a complete CI/CD pipeline using Jenkins, Maven, Docker, and AWS EC2 for automated application deployment.

---

# Author

Rishav Kumar
B.Tech - Computer Science & Engineering
