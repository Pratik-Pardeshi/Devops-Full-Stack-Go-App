pipeline {
    agent any

    stages {
        stage('Build Frontend') {
            steps {
                git 'https://github.com/TechVerito-Software-Solutions-LLP/devops-fullstack-app.git'
                dir('frontend') {
                    sh 'docker build -t frontend-app .'
                }
            }
        }
        stage('Build Backend') {
            steps {
                git 'https://github.com/TechVerito-Software-Solutions-LLP/devops-fullstack-app.git'
                dir('backend') {
                    sh 'docker build -t backend-app .'
                }
            }
        }
    }
}
