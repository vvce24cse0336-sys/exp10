
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "anil-demo-app"

        PATH = "/Users/anishkmahadevan/.docker/bin/docker"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                sh 'python3 -m py_compile app.py'
            }
        }

        stage('3. Check Docker') {
            steps {
                sh 'which docker'
                sh 'docker --version'
                sh 'docker info'
            }
        }

        stage('4. Build & Tag Docker Image') {
            steps {
                sh """
                    docker build \
                    -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} \
                    -t ${REGISTRY}/${IMAGE_NAME}:latest .
                """
            }
        }

        stage('5. Push Image to Registry') {
            steps {
                sh "docker push ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                sh "docker push ${REGISTRY}/${IMAGE_NAME}:latest"
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded! Docker image pushed successfully."
        }

        failure {
            echo "Pipeline failed. Check the console output."
        }
    }
}
