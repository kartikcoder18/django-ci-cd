🚀 Django CI/CD with Jenkins and Docker
This project demonstrates a CI/CD pipeline using Jenkins for a Django application with Docker containerization.

📖 Table of Contents
💡 Introduction

✅ Pre-Requisites

🚀 Project Implementation

Step 1: Create GitHub Repository

Step 2: Create Django Project Locally

Step 3: Create requirements.txt

Step 4: Test Locally with Docker

Step 5: Add Jenkinsfile and Dockerfile to GitHub

Step 6: Open Jenkins and Configure Pipeline

Step 7: Trigger Build and Monitor Logs

Step 8: Access the Application

🎯 Conclusion

💡 Introduction
This project aims to automate the deployment of a Django application using Docker and Jenkins.

We will build a CI/CD pipeline using the following technologies:

GitHub – Version control and code hosting

Docker – Containerizing the Django application

Jenkins – Automating the build and deployment process

✅ Pre-Requisites
Ensure the following are installed and configured:

Git – For version control

Docker – To containerize the application

Jenkins – For CI/CD

AWS EC2 (Optional) – To host the application

🚀 Project Implementation
Step 1: Create GitHub Repository
Create a new repository named django-ci-cd, then clone it:

bash
Copy
Edit
git clone https://github.com/kartikcoder18/django-ci-cd.git
cd django-ci-cd
Step 2: Create Django Project Locally
Create a new Django project and app:

bash
Copy
Edit
# Create Django project
django-admin startproject mynotes .

# Create Django app
python manage.py startapp notes
Step 3: Create requirements.txt
Generate the requirements file:

bash
Copy
Edit
pip freeze > requirements.txt
Make sure it includes:

shell
Copy
Edit
Django>=4.2
gunicorn
Step 4: Test Locally with Docker
Create a Dockerfile:

Dockerfile
Copy
Edit
# Use the official Python image
FROM python:3.9

# Set the working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt /app/
RUN pip install -r requirements.txt

# Copy the rest of the project
COPY . /app/

# Expose the port
EXPOSE 8000

# Run the server using gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "mynotes.wsgi:application"]
Build and run the Docker container:

bash
Copy
Edit
docker build -t django-notes .
docker run -p 8000:8000 django-notes
Visit your project at: http://localhost:8000

Step 5: Add Jenkinsfile and Dockerfile to GitHub
Create a Jenkinsfile:

groovy
Copy
Edit
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
Push everything to GitHub:

bash
Copy
Edit
git add .
git commit -m "Add Django project, Jenkinsfile, Dockerfile"
git push -u origin main
Step 6: Open Jenkins and Configure Pipeline
Install Jenkins on EC2
bash
Copy
Edit
# SSH into your EC2 instance
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>

# Update system and install Java
sudo apt update
sudo apt install openjdk-17-jre

# Verify Java installation
java -version

# Install dependencies
sudo apt-get install -y ca-certificates curl gnupg

# Add Jenkins repository
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update and install Jenkins
sudo apt-get update
sudo apt-get install jenkins

# Enable and start Jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins

# Check Jenkins status
sudo systemctl status jenkins
Access Jenkins UI
Go to:
http://<EC2_PUBLIC_IP>:8080
Ensure port 8080 is open in your EC2 security group.

Step 7: Trigger Build and Monitor Logs
Go to your pipeline project in Jenkins

Click on "Build Now"

Click the build number (e.g., #1) to view Console Output and monitor the process

Step 8: Access the Application
Once deployed, access your Django app at:
http://<EC2_PUBLIC_IP>:8000

🎯 Conclusion
This project successfully:

Automates Django application deployment using Docker & Jenkins

Implements an efficient CI/CD pipeline

Enhances your skills in DevOps, CI/CD, and containerization
