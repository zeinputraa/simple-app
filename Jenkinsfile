pipeline {
    agent any

    environment {
        IMAGE_NAME = 'zeinputra/simple-app'
        REGISTRY_URL = 'https://index.docker.io/v1/'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials-zein'
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
        DOCKER_HOST = "unix:///var/run/docker.sock"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Melakukan checkout dari SCM...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Mulai build aplikasi...'
                sh 'echo "Build selesai ✅"'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Membangun Docker image...'
                    dockerImage = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                    echo "✅ Image berhasil dibuat: ${IMAGE_NAME}:${env.BUILD_NUMBER}"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo '🚀 Push image ke Docker Hub...'
                    docker.withRegistry("${REGISTRY_URL}", "${REGISTRY_CREDENTIALS}") {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            echo '🏁 Pipeline selesai dijalankan'
        }
        success {
            echo "✅ Build & Push image berhasil!"
        }
        failure {
            echo "❌ Terjadi kegagalan selama pipeline"
        }
    }
}
