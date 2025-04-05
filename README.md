# Django CI/CD with Jenkins and Docker

This project demonstrates a CI/CD pipeline using Jenkins for a Django application with Docker containerization.

---

## 📖 Table of Contents  
- [💡 Introduction](#-introduction)  
- [✅ Pre-Requisites](#-pre-requisites)  
- [🚀 Project Implementation](#-project-implementation)  
  - [Step 1: Create GitHub Repository](#step-1-create-github-repository)
  - [Step 2: Create Django Project Locally](#step-2-create-django-project-locally)
  - [Step 3: Create `requirements.txt`](#step-3-create-requirementstxt)
  - [Step 4: Test Locally with Docker](#step-4-test-locally-with-docker)
  - [Step 5: Add Jenkinsfile and Dockerfile to GitHub](#step-5-add-jenkinsfile-and-dockerfile-to-github)
  - [Step 6: Open Jenkins and Configure Pipeline](#step-6-open-jenkins-and-configure-pipeline)
  - [Step 7: Trigger Build and Monitor Logs](#step-7-trigger-build-and-monitor-logs)
  - [Step 8: Access the Application](#step-8-access-the-application)
- [🎯 Conclusion](#-conclusion)  

---

## 💡 Introduction  
This project aims to automate the deployment of a Django application using Docker and Jenkins. We will build a CI/CD pipeline to ensure continuous integration and deployment with the following technologies:
- **GitHub**: For version control and code hosting.
- **Docker**: For containerizing the Django application.
- **Jenkins**: For automating the build and deployment process.

---

## ✅ Pre-Requisites  
Before starting, make sure you have the following installed and configured:
- **Git**: For version control and pushing code to GitHub.
- **Docker**: To containerize the Django application.
- **Jenkins**: To set up the CI/CD pipeline.
- **AWS EC2**: (Optional) To host the application.

---

## 🚀 Project Implementation  

### Step 1: Create GitHub Repository

1. Go to [GitHub](https://github.com/), log in, and create a new repository named `django-ci-cd`.
2. Clone the repository to your local machine:


git clone https://github.com/kartikcoder18/django-ci-cd.git
cd django-ci-cd
Step 2: Create Django Project Locally
Clone your GitHub repository and set up the Django project:

bash
Copy
Edit
# Clone the repository
git clone https://github.com/kartikcoder18/django-ci-cd.git
cd django-ci-cd

# Create a new Django project
django-admin startproject mynotes .

# Create a new Django app named "notes"
python manage.py startapp notes
This will create the basic Django structure inside your repository.

Step 3: Create requirements.txt
After setting up Django and the notes app, you need to create a requirements.txt file to list the necessary dependencies.

bash
Copy
Edit
pip freeze > requirements.txt
This file should include at least the following dependencies:

txt
Copy
Edit
Django>=4.2
gunicorn
Step 4: Test Locally with Docker
To test everything locally before integrating Jenkins, you can use Docker to build and run the project.

Create a Dockerfile for building the Docker container:

dockerfile
Copy
Edit
# Use the official Python image
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

bash
Copy
Edit
docker build -t django-notes .
docker run -p 8000:8000 django-notes
After this, you can visit your project on:

http://localhost:8000

Step 5: Add Jenkinsfile and Dockerfile to GitHub
Create a Jenkinsfile for the CI/CD pipeline:


pipeline {
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


git add .
git commit -m "Add Django project, Jenkinsfile, Dockerfile"
git push -u origin main  # or 'master' depending on your default branch
Step 6: Open Jenkins and Configure Pipeline
Install Jenkins on EC2 Instance (if you haven’t already):

bash
Copy
Edit
ssh -i <your-key>.pem ubuntu@<your-ec2-public-ip>
sudo apt update
sudo apt install openjdk-17-jre
java -version  # Verify Java installation
sudo apt-get install -y ca-certificates curl gnupg
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins  # Check if Jenkins is running
Access Jenkins Web Interface: Jenkins runs on port 8080. In your web browser, navigate to:

http://<your-ec2-public-ip>:8080

Make sure port 8080 is open in your EC2 security group settings.

Initial Jenkins Setup:

Retrieve the Admin Password by running:

bash
Copy
Edit
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Login to Jenkins using the retrieved password.

Install Suggested Plugins.

Create an Admin User.

Step 7: Create Jenkins Pipeline Project
Create a New Item in Jenkins:

Click on New Item on the left sidebar.

Enter a name for your project (e.g., django-ci-cd-pipeline).

Select Pipeline and click OK.

Configure GitHub Project:

In the General section, check GitHub project and provide your repository URL: https://github.com/kartikcoder18/django-ci-cd.git

Pipeline Script:

In the Pipeline section, select Pipeline script from SCM.

Configure it with:

SCM: Git

Repository URL: https://github.com/kartikcoder18/django-ci-cd.git

Branch Specifier: */main

Save the Pipeline Project:

Click Save to create the Jenkins project.

Step 8: Trigger Build and Monitor Logs
Trigger Build:

Go to your Jenkins pipeline project and click Build Now.

Monitor Console Output:

Click on the build number (e.g., #1) to view the logs and monitor the deployment process.

Step 9: Access the Application
Once the build is complete and Jenkins has deployed your application, you can access the Django Notes application via your EC2 instance’s public IP at:

http://<your-ec2-public-ip>:8000

##🎯 Conclusion
This project successfully:

Automates the deployment of Django applications using Docker and Jenkins.

Improves CI/CD workflows by automating the build, test, and deployment pipeline.

Enhances skills in Jenkins, Docker, and Django CI/CD automation.

