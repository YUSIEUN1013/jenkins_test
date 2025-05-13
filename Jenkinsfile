pipeline {
    agent {
        docker {
            image 'docker:24.0.2-cli' // Docker CLI 포함 이미지
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub_id'   // Jenkins에 등록된 DockerHub 계정
        DOCKERHUB_USERNAME = 'selinux1'
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/jenkinstest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
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
