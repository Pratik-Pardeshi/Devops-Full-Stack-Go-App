pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker')
        DOCKERHUB_USERNAME = 'rithik1504'
        FRONTEND_IMAGE = "$rithik1504/frontend:latest"
        BACKEND_IMAGE = "$rithik1504/backend:latest"
        KUBECONFIG_CREDENTIALS = credentials('kubeconfig-credentials-id')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code
                git url: 'https://github.com/Iamthor15/devops-fullstack-app', branch: 'patch-1'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build Docker images
                    sh 'docker build -t ${FRONTEND_IMAGE} ./frontend'
                    sh 'docker build -t ${BACKEND_IMAGE} ./backend'
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    // Login to Docker Hub
                    sh 'echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin'
                    
                    // Push Docker images
                    sh 'docker push ${FRONTEND_IMAGE}'
                    sh 'docker push ${BACKEND_IMAGE}'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Write kubeconfig to a temporary file
                    writeFile file: 'kubeconfig', text: "${KUBECONFIG_CREDENTIALS}"
                    
                    // Set KUBECONFIG environment variable
                    withEnv(["KUBECONFIG=kubeconfig"]) {
                        // Apply Kubernetes configuration files
                        sh 'kubectl apply -f k8s/frontend-deployment.yaml'
                        sh 'kubectl apply -f k8s/frontend-service.yaml'
                        sh 'kubectl apply -f k8s/backend-deployment.yaml'
                        sh 'kubectl apply -f k8s/backend-service.yaml'
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up
            sh 'docker logout'
            deleteDir()
        }
    }
}
