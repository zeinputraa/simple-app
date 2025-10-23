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
        sh 'docker build -t zeinputra/simple-app:2 .'
      }
    }
    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: env.REGISTRY_CREDENTIALS, usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh """docker login -u %USER% -p %PASS%"""
          sh """docker push ${env.IMAGE_NAME}:${env.BUILD_NUMBER}"""
          sh """docker tag ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ${env.IMAGE_NAME}:latest"""
          sh """docker push ${env.IMAGE_NAME}:latest"""
        }
      }
    }
  }
}

 IMAGE_NAME = 'zeinputra/simple-app'
    REGISTRY = 'https://index.docker.io/v1/'
    REGISTRY_CREDENTIALS = 'dockerhub-credentials'