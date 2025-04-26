pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials') 
        IMAGE_NAME = 'jainvidhuu21/node-app' 
        TAG = 'latest' 
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/jainvidhuu21/node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub using credentials
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS) {
                        // Push the Docker image to Docker Hub
                        docker.image("${IMAGE_NAME}:${TAG}").push()
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Clean up the Docker image after the pipeline
            sh 'docker system prune -f'
        }
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'The build failed. Please check the logs for details.'
        }
    }
}
