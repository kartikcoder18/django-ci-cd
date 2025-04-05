# Django CI/CD with Jenkins and Docker

This project demonstrates a CI/CD pipeline using Jenkins for a Django application with Docker containerization.

## 📖 Table of Contents
- [💡 Introduction](#introduction)
- [✅ Pre-Requisites](#pre-requisites)
- [🚀 Project Implementation](#project-implementation)
  - [Step 1: Create GitHub Repository](#step-1-create-github-repository)
  - [Step 2: Create Django Project Locally](#step-2-create-django-project-locally)
  - [Step 3: Create requirements.txt](#step-3-create-requirementstxt)
  - [Step 4: Test Locally with Docker](#step-4-test-locally-with-docker)
  - [Step 5: Add Jenkinsfile and Dockerfile to GitHub](#step-5-add-jenkinsfile-and-dockerfile-to-github)
  - [Step 6: Open Jenkins and Configure Pipeline](#step-6-open-jenkins-and-configure-pipeline)
  - [Step 7: Trigger Build and Monitor Logs](#step-7-trigger-build-and-monitor-logs)
  - [Step 8: Access the Application](#step-8-access-the-application)
- [🎯 Conclusion](#conclusion)

## 💡 Introduction

This project aims to automate the deployment of a Django application using Docker and Jenkins. We will build a CI/CD pipeline to ensure continuous integration and deployment with the following technologies:

- **GitHub**: For version control and code hosting.
- **Docker**: For containerizing the Django application.
- **Jenkins**: For automating the build and deployment process.

## ✅ Pre-Requisites

Before starting, make sure you have the following installed and configured:

- **Git**: For version control and pushing code to GitHub.
- **Docker**: To containerize the Django application.
- **Jenkins**: To set up the CI/CD pipeline.
- **AWS EC2 (Optional)**: To host the application.

## 🚀 Project Implementation

### Step 1: Create GitHub Repository

Go to GitHub, log in, and create a new repository named `django-ci-cd`.

Clone the repository to your local machine:

```bash
git clone https://github.com/kartikcoder18/django-ci-cd.git
cd django-ci-cd
Step 2: Create Django Project Locally
Clone your GitHub repository and set up the Django project:
bashCopy# Clone the repository
git clone https://github.com/kartikcoder18/django-ci-cd.git
cd django-ci-cd

# Create a new Django project
django-admin startproject mynotes .

# Create a new Django app named "notes"
python manage.py startapp notes
This will create the basic Django structure inside your repository.
Step 3: Create requirements.txt
After setting up Django and the notes app, you need to create a requirements.txt file to list the necessary dependencies:
bashCopypip freeze > requirements.txt
This file should include at least the following dependencies:
CopyDjango>=4.2
gunicorn
Step 4: Test Locally with Docker
To test everything locally before integrating Jenkins, you can use Docker to build and run the project.
Create a Dockerfile for building the Docker container:
dockerfileCopy# Use the official Python image
FROM python:3.9

# Set the working directory inside the container
WORKDIR /app

# Copy the requirements file into the container
COPY requirements.txt /app/

# Install the required dependencies
RUN pip install -r requirements.txt

# Copy the entire project into the container
COPY . /app/

# Expose port 8000 for Django
EXPOSE 8000

# Run the Django development server
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "mynotes.wsgi:application"]
Build and Run the Docker Container:
bashCopydocker build -t django-notes .
docker run -p 8000:8000 django-notesRetryClaude does not have the ability to run the code it generates yet.Kbro give the code in that type if in step 1 link of index clicked then step 1 pr jayega aise  toh pura sath mein deek sathEditI understand now. You want the code with clickable links that will navigate to the different sections within the README. Here's the full code that you can copy and paste directly:
markdownCopy# Django CI/CD with Jenkins and Docker

This project demonstrates a CI/CD pipeline using Jenkins for a Django application with Docker containerization.

## 📖 Table of Contents
- [💡 Introduction](#-introduction)
- [✅ Pre-Requisites](#-pre-requisites)
- [🚀 Project Implementation](#-project-implementation)
  - [Step 1: Create GitHub Repository](#step-1-create-github-repository)
  - [Step 2: Create Django Project Locally](#step-2-create-django-project-locally)
  - [Step 3: Create requirements.txt](#step-3-create-requirementstxt)
  - [Step 4: Test Locally with Docker](#step-4-test-locally-with-docker)
  - [Step 5: Add Jenkinsfile and Dockerfile to GitHub](#step-5-add-jenkinsfile-and-dockerfile-to-github)
  - [Step 6: Open Jenkins and Configure Pipeline](#step-6-open-jenkins-and-configure-pipeline)
  - [Step 7: Trigger Build and Monitor Logs](#step-7-trigger-build-and-monitor-logs)
  - [Step 8: Access the Application](#step-8-access-the-application)
- [🎯 Conclusion](#-conclusion)

## 💡 Introduction

This project aims to automate the deployment of a Django application using Docker and Jenkins. We will build a CI/CD pipeline to ensure continuous integration and deployment with the following technologies:

- **GitHub**: For version control and code hosting.
- **Docker**: For containerizing the Django application.
- **Jenkins**: For automating the build and deployment process.

## ✅ Pre-Requisites

Before starting, make sure you have the following installed and configured:

- **Git**: For version control and pushing code to GitHub.
- **Docker**: To containerize the Django application.
- **Jenkins**: To set up the CI/CD pipeline.
- **AWS EC2 (Optional)**: To host the application.

## 🚀 Project Implementation

### Step 1: Create GitHub Repository

Go to GitHub, log in, and create a new repository named `django-ci-cd`.

Clone the repository to your local machine:

```bash
git clone https://github.com/kartikcoder18/django-ci-cd.git
cd django-ci-cd
Step 2: Create Django Project Locally
Clone your GitHub repository and set up the Django project:
bashCopy# Clone the repository
git clone https://github.com/kartikcoder18/django-ci-cd.git
cd django-ci-cd

# Create a new Django project
django-admin startproject mynotes .

# Create a new Django app named "notes"
python manage.py startapp notes
This will create the basic Django structure inside your repository.
Step 3: Create requirements.txt
After setting up Django and the notes app, you need to create a requirements.txt file to list the necessary dependencies:
bashCopypip freeze > requirements.txt
This file should include at least the following dependencies:
CopyDjango>=4.2
gunicorn
Step 4: Test Locally with Docker
To test everything locally before integrating Jenkins, you can use Docker to build and run the project.
Create a Dockerfile for building the Docker container:
dockerfileCopy# Use the official Python image
FROM python:3.9

# Set the working directory inside the container
WORKDIR /app

# Copy the requirements file into the container
COPY requirements.txt /app/

# Install the required dependencies
RUN pip install -r requirements.txt

# Copy the entire project into the container
COPY . /app/

# Expose port 8000 for Django
EXPOSE 8000

# Run the Django development server
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "mynotes.wsgi:application"]
Build and Run the Docker Container:
bashCopydocker build -t django-notes .
docker run -p 8000:8000 django-notes
After this, you can visit your project on: http://localhost:8000
Step 5: Add Jenkinsfile and Dockerfile to GitHub
Create a Jenkinsfile for the CI/CD pipeline:
groovyCopypipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kartikcoder18/django-ci-cd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t django-notes .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh 'docker run -p 8000:8000 django-notes'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Add your deployment logic here
                }
            }
        }
    }
}
Push the changes to GitHub:
bashCopygit add .
git commit -m "Add Django project, Jenkinsfile, Dockerfile"
git push -u origin main  # or 'master' depending on your default branch
Step 6: Open Jenkins and Configure Pipeline
If you haven't already installed Jenkins, follow these steps to set it up on your EC2 instance:
bashCopy# SSH into your EC2 instance
ssh -i <your-key>.pem ubuntu@<your-ec2-public-ip>

# Update package list and install Java
sudo apt update
sudo apt install openjdk-17-jre

# Verify Java installation
java -version

# Install necessary dependencies
sudo apt-get install -y ca-certificates curl gnupg

# Add Jenkins repository key and source list
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update package list again and install Jenkins
sudo apt-get update
sudo apt-get install jenkins

# Enable Jenkins to start on boot
sudo systemctl enable jenkins

# Start Jenkins service
sudo systemctl start jenkins

# Check if Jenkins is running
sudo systemctl status jenkins
Access Jenkins Web Interface
Once Jenkins is installed and running, access the Jenkins Web Interface by navigating to: http://<your-ec2-public-ip>:8080
Make sure that port 8080 is open in your EC2 security group settings to allow external access to the Jenkins UI.
Step 7: Trigger Build and Monitor Logs

Trigger Build: Go to your Jenkins pipeline project and click "Build Now".
Monitor Console Output: Click on the build number (e.g., #1) to view the logs and monitor the deployment process.

Step 8: Access the Application
Once the build is complete and Jenkins has deployed your application, you can access the Django Notes application via your EC2 instance's public IP at: http://<your-ec2-public-ip>:8000
🎯 Conclusion
This project successfully:

Automates the deployment of Django applications using Docker and Jenkins.
Improves CI/CD workflows by automating the build, test, and deployment pipeline.
Enhances skills in Jenkins, Docker, and Django CI/CD automation.
