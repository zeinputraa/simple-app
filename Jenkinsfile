pipeline {
    agent any
    environment {
        IMAGE_NAME = 'zeinputra/simple-app'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Building application..."'
            }
        }
        stage('Check Docker') {
            steps {
                script {
                    // Test Docker availability
                    sh '''
                        echo "Checking Docker installation..."
                        docker --version || echo "Docker not installed"
                        docker ps || echo "Cannot connect to Docker daemon"
                    '''
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        echo "Current directory:"
                        pwd
                        ls -la
                        echo "Building Docker image..."
                    """
                    sh "docker build -t ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: env.REGISTRY_CREDENTIALS, 
                    usernameVariable: 'DOCKER_USER', 
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    script {
                        sh "echo '${DOCKER_PASS}' | docker login -u '${DOCKER_USER}' --password-stdin"
                        sh "docker push ${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
                        sh "docker tag ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ${env.IMAGE_NAME}:latest"
                        sh "docker push ${env.IMAGE_NAME}:latest"
                    }
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}