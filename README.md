# Django CI/CD with Jenkins & Docker

A comprehensive guide to implementing a CI/CD pipeline for a Django application, utilizing Jenkins for automation and Docker for containerization.

---

## 💡 Introduction

This repository demonstrates how to automate the deployment of a Django application using Docker and Jenkins. It covers building a CI/CD pipeline for continuous integration and deployment with the following technologies:

- **GitHub**: Version control and code hosting
- **Docker**: Containerization of the Django application
- **Jenkins**: Automation of build and deployment processes

---

## ✅ Prerequisites

Before you begin, ensure you have the following installed and configured:

- **Git**: For version control and pushing code to GitHub
- **Docker**: To containerize the Django application
- **Jenkins**: To configure the CI/CD pipeline
- **AWS EC2** (optional): For hosting the application

---

## 🚀 Project Implementation

### Step 1: Create GitHub Repository

1. Log in to GitHub and create a new repository named `django-ci-cd`.
2. Clone the repository to your local machine:

    ```bash
    git clone https://github.com/kartikcoder18/django-ci-cd.git
    cd django-ci-cd
    ```

---

### Step 2: Initialize Django Project

1. Create a new Django project:
    ```bash
    django-admin startproject mynotes .
    ```
2. Create a Django app named "notes":
    ```bash
    python manage.py startapp notes
    ```
3. This sets up the basic structure of your Django project and app.

---

### Step 3: Define Dependencies

1. Generate a `requirements.txt` file to record dependencies:
    ```bash
    pip freeze > requirements.txt
    ```
2. Ensure at least the following are included:
    ```
    Django>=4.2
    gunicorn
    ```

---

### Step 4: Dockerize the Django Application

1. **Create a Dockerfile** in the root directory:

    ```dockerfile
    FROM python:3.10-slim

    ENV PYTHONDONTWRITEBYTECODE 1
    ENV PYTHONUNBUFFERED 1

    WORKDIR /app

    COPY requirements.txt /app/
    RUN pip install --upgrade pip && pip install -r requirements.txt

    COPY . /app/

    RUN mkdir -p /app/static
    RUN python manage.py collectstatic --noinput

    CMD ["sh", "-c", "python manage.py migrate && python manage.py runserver 0.0.0.0:8000"]
    ```

2. **Create a `.dockerignore`** file to exclude unnecessary files:

    ```
    __pycache__
    *.pyc
    *.pyo
    *.pyd
    .env
    db.sqlite3
    /static/
    ```

3. **Build and Test Docker Container Locally**:

    ```bash
    docker-compose up --build
    ```

    This builds the Docker image and starts the application on port 8000.

4. **Access the Application**:

    Open your browser to `http://localhost:8000` to view the running Django app.

---

### Step 5: Provision AWS EC2 and Install Dependencies

1. **Launch an AWS EC2 Instance**:
    - Use Ubuntu 24.04 LTS.
    - Open required ports: 22 (SSH), 80 (HTTP).

2. **Connect to EC2 Instance**:

    ```bash
    ssh -i <your-key>.pem ubuntu@<your-ec2-public-ip>
    ```

3. **Update the Instance**:

    ```bash
    sudo apt update
    ```

4. **Install Java for Jenkins**:

    ```bash
    sudo apt install openjdk-17-jre
    java -version
    ```

5. **Install Jenkins**:

    ```bash
    sudo apt-get install -y ca-certificates curl gnupg
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt-get install jenkins
    sudo systemctl enable jenkins
    sudo systemctl start jenkins
    sudo systemctl status jenkins
    ```

6. **Access Jenkins**:  
   Jenkins runs on port 8080.  
   Visit `http://<instance_public_ip>:8080` in your browser.

7. **Configure AWS Security Group**:  
   Add a rule to allow inbound traffic on port 8080.

---

### Step 6: Jenkins Initial Setup

1. **Retrieve the Jenkins Admin Password**:

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

2. **Complete Jenkins Setup**:
    - Log in with the admin password.
    - Install suggested plugins.
    - Create an admin user.

---

### Step 7: Configure Jenkins Pipeline

1. **Create a New Pipeline Project**:
    - Click **New Item** > **Pipeline** in Jenkins.
    - Enter a project name.
    - Check 'GitHub project' and provide your repo URL.

2. **Create a Jenkinsfile** in your GitHub repository:

    ```groovy
    pipeline {
        agent any

        stages {
            stage('Clone') {
                steps {
                    git branch: 'main', url: 'https://github.com/kartikcoder18/django-ci-cd'
                }
            }

            stage('Build Docker Image') {
                steps {
                    script {
                        sh 'docker build -t django-notes-app .'
                    }
                }
            }

            stage('Run Container') {
                steps {
                    script {
                        sh 'docker run -d -p 8000:8000 django-notes-app'
                    }
                }
            }
        }
    }
    ```

3. **Save the Jenkins Project**.

---

### Step 8: Install Docker & Docker Compose on EC2

1. **Install Docker**:

    ```bash
    sudo apt install docker.io
    ```

2. **Add Jenkins User to Docker Group**:

    ```bash
    sudo usermod -aG docker jenkins
    sudo reboot
    ```

3. **Install Docker Compose**:

    ```bash
    sudo apt-get update
    sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-compose
    docker --version
    docker-compose --version
    ```

---

### Step 9: Deploy the Application

1. **Trigger a Build in Jenkins**:  
   Click **Build Now** in your Jenkins pipeline project.

2. **Monitor Build Output**:  
   View the console output to follow the deployment process.

3. **Access Your Application**:  
   Once complete, open `http://<your-ec2-public-ip>:8000` in your browser.

---

## 📄 License

MIT License

---

## 👤 Author

[kartikcoder18](https://github.com/kartikcoder18)
