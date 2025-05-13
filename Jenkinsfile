pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub_id'   // Jenkins에 등록된 DockerHub 계정
        DOCKERHUB_USERNAME = 'selinux1'
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/jenkinstest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS_ID}") {
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker image pushed successfully!"
        }
        failure {
            echo "❌ Build failed!"
        }
    }
}
