pipeline {
  agent any
  environment {
    IMAGE_NAME = 'zeinputra/simple-app'
    REGISTRY = 'https://index.docker.io/v1/'
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
        sh 'echo "Build di Windows"'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh "docker build -t ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ."
        sh "docker tag ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ${env.IMAGE_NAME}:latest"
      }
    }
    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: env.REGISTRY_CREDENTIALS, 
          usernameVariable: 'USER', 
          passwordVariable: 'PASS'
        )]) {
          sh "echo \$PASS | docker login -u \$USER --password-stdin ${env.REGISTRY}"
          sh "docker push ${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
          sh "docker push ${env.IMAGE_NAME}:latest"
        }
      }
    }
  }
}