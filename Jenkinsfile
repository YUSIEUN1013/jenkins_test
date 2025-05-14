pipeline {
    agent {
        kubernetes {
            label 'docker-agent'
            defaultContainer 'docker'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: docker-agent
spec:
  containers:
  - name: docker
    image: selinux1/jenkins-agent-docker  # Docker CLI 포함한 이미지
    command:
    - cat
    tty: true
    volumeMounts:
      - name: docker-sock
        mountPath: /var/run/docker.sock
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
"""
        }
    }

    environment {
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub_id'   // Jenkins Credentials ID
        DOCKERHUB_USERNAME = 'selinux1'
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/jenkinstest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                container('docker') {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                container('docker') {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS_ID}") {
                            sh 'docker push $IMAGE_NAME'
                        }
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
