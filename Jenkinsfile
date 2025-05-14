pipeline {
    agent {
        kubernetes {
            label 'docker'
        }
    }

    environment {
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub_id'   // Jenkins Credentials에 등록한 ID
        DOCKERHUB_USERNAME = 'selinux1'
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/jenkinstest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS_ID}") {
                        sh 'docker push $IMAGE_NAME'
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
