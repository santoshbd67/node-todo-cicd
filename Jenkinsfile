pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub_id')
        IMAGE_NAME = 'santoshbd67/nodetodo'
        IMAGE_TAG = 'v24'
        CONTAINER_NAME = 'nodetodo-container'
        PORT_MAPPING = '8000:8000'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "https://github.com/santoshbd67/node-todo-cicd.git"
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")

                    // Authenticate with Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_id') {
                        // Push Docker image to Docker Hub
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run Docker container
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}")
                        .run('--name ${CONTAINER_NAME} -p ${PORT_MAPPING} -d')
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline successfully completed. Docker image built, pushed, and container is running.'
        }

        failure {
            echo 'Pipeline failed. Check the logs for errors.'
        }
    }
}
