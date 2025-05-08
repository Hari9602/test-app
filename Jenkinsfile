pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-docker-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Hari9602/test-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove if already running
                    sh "docker stop flask_container || true"
                    sh "docker rm flask_container || true"

                    sh "docker run -d -p 5555:5555 --name flask_container ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Test Container') {
            steps {
                script {
                    sleep 5 // Give the container time to start
                    sh "curl -f http://localhost:5555 || echo 'App did not start correctly'"
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker stop flask_container || true"
                    sh "docker rm flask_container || true"
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished, cleaning up if needed."
        }
    }
}
