pipeline {
    agent any

    environment {
        DOCKERFILE_PATH = 'Dockerfile' // Path to your Dockerfile
        IMAGE_NAME = 'my-node-app' // Name for your Docker image
        NODEJS_VERSION = '14' // Node.js version you want to use
    }

    stage('Checkout') {
    steps {
        script {
            // Checkout the 'devops' branch
            checkout([$class: 'GitSCM', branches: [[name: '*/devops']]])
        }
    }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    def dockerImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_NUMBER}", "--build-arg NODEJS_VERSION=${env.NODEJS_VERSION} -f ${env.DOCKERFILE_PATH} .")

                    // Push the Docker image to a registry if needed
                    // dockerImage.push()
                }
            }
        }
    }
}

