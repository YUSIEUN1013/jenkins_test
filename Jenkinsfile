pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: python
      image: python:3.10
      command:
        - cat
      tty: true
"""
        }
    }
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'Github', url: 'https://github.com/YUSIEUN1013/jenkins_test.git'
            }
        }
        stage('Run Python') {
            steps {
                container('python') {
                    sh 'python3 hello.py'
                }
            }
        }
    }
}
