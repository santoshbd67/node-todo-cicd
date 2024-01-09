pipeline {
    agent any
     environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub_id')
        IMAGE_NAME = 'santoshbd67/nodetodo'
        IMAGE_TAG = 'v24'
    }
    
    stages {
        
        stage("code"){
            steps{
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
    }
}
       
     
