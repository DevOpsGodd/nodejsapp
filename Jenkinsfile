pipeline {
    agent any

    environment {
        DOCKERFILE_PATH = 'Dockerfile' // Path to your Dockerfile
        IMAGE_NAME = 'my-node-app' // Name for your Docker image
        NODEJS_VERSION = '14' // Node.js version you want to use
        DOCKER_HUB_REPO = 'uthycloud/nodejs-app' // Replace with your Docker Hub username and repo name
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_NUMBER}", "--build-arg NODEJS_VERSION=${env.NODEJS_VERSION} -f ${env.DOCKERFILE_PATH} .")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        // Define the Docker image you want to push
                        def dockerImage = docker.image("${env.IMAGE_NAME}/${env.BUILD_NUMBER}")

                        // Push the Docker image to Docker Hub
                        dockerImage.push("${env.DOCKER_HUB_REPO}/${env.BUILD_NUMBER}")
                        dockerImage.push("${env.DOCKER_HUB_REPO}:latest")
                    }
                }
            }
        }
    }
}

