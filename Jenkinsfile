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
