# Django CI/CD with Jenkins and Docker

This project demonstrates how to set up a **CI/CD pipeline** using **Jenkins** for a **Django application** with **Docker containerization**.

---

## 🔧 Step 1: Create GitHub Repository

1. **Go to GitHub**:  
   Visit [GitHub](https://github.com/) and log in.

2. **Create a New Repository**:  
   - Click on the **+** icon in the top-right corner and select **New repository**.
   - Name it `django-ci-cd` (or any name you prefer).
   - Choose **Public** or **Private**, depending on your preference.
   - Click **Create repository**.

3. **Clone the repository**:
   Open your terminal and run the following command to clone the repository to your local machine:

   ```bash
   git clone https://github.com/kartikcoder18/django-ci-cd.git
   cd django-ci-cd
⚙️ Step 2: Set Up Django Project Locally
Create a new Django project:

bash
Copy
Edit
django-admin startproject mynotes .
Create a new Django app named notes:

bash
Copy
Edit
python manage.py startapp notes
Structure of the project:
This will create the basic Django structure inside your repository.

📜 Step 3: Create requirements.txt
After setting up the Django project and notes app, you need to create a requirements.txt file that lists the necessary dependencies.

Run this command to generate the requirements.txt:

bash
Copy
Edit
pip freeze > requirements.txt
Ensure the requirements.txt includes the following dependencies:

txt
Copy
Edit
Django>=4.2
gunicorn
🚀 Step 4: Test Locally with Docker
Create a Dockerfile to containerize your Django project.

Create a file named Dockerfile and add the following content:

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
Build the Docker container:

bash
Copy
Edit
docker build -t django-notes .
Run the Docker container:

bash
Copy
Edit
docker run -p 8000:8000 django-notes
You can access your Django app at http://localhost:8000.

📂 Step 5: Add Jenkinsfile and Dockerfile to GitHub
Create a Jenkinsfile for CI/CD pipeline. In your project root, create a file named Jenkinsfile and add the following pipeline script:

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
Push changes to GitHub:

After adding the Dockerfile and Jenkinsfile, push your changes to GitHub:

bash
Copy
Edit
git add .
git commit -m "Add Django project, Jenkinsfile, Dockerfile"
git push -u origin main
💻 Step 6: Open Jenkins and Configure Pipeline
1. Install Jenkins on EC2 Instance
Connect to your EC2 Instance via SSH:

bash
Copy
Edit
ssh -i <your-key>.pem ubuntu@<your-ec2-public-ip>
Install Java (required by Jenkins):

bash
Copy
Edit
sudo apt update
sudo apt install openjdk-17-jre
java -version  # Verify Java installation
Install Jenkins:

bash
Copy
Edit
sudo apt-get install -y ca-certificates curl gnupg
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins  # Check if Jenkins is running
2. Access Jenkins Web Interface
Jenkins runs on port 8080. In your web browser, navigate to:

plaintext
Copy
Edit
http://<your-ec2-public-ip>:8080
Make sure port 8080 is open in your EC2 security group settings.

3. Initial Jenkins Setup
Retrieve Admin Password:

bash
Copy
Edit
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Login to Jenkins using the retrieved password.

Install Suggested Plugins:
Once logged in, Jenkins will suggest a set of plugins. Click Install suggested plugins.

Create Admin User:
After the plugins are installed, create an admin user for Jenkins access.

🛠️ Step 7: Create Jenkins Pipeline Project
Create a New Jenkins Project:

In Jenkins, click on New Item in the left sidebar.

Name your project (e.g., django-ci-cd-pipeline).

Select Pipeline and click OK.

Configure GitHub Project:

In the General section, check GitHub project and provide the URL of your repository:

plaintext
Copy
Edit
https://github.com/kartikcoder18/django-ci-cd.git
Configure Pipeline Script:

In the Pipeline section, select Pipeline script from SCM.

Configure it with:

SCM: Git

Repository URL: https://github.com/kartikcoder18/django-ci-cd.git

Branch Specifier: */main

Save the Jenkins Project:
Click Save to create the Jenkins project.

🚀 Step 8: Trigger Build and Monitor Logs
Trigger the Build:
Go to your Jenkins pipeline project and click Build Now.

Monitor Console Output:
Click on the build number (e.g., #1) to view the logs and monitor the deployment process.

🌐 Step 9: Access the Application
Once the build is complete and Jenkins has deployed your application, you can access the Django Notes application via your EC2 instance’s public IP at:

plaintext
Copy
Edit
http://<your-ec2-public-ip>:8000
