pipeline {
    agent any

    environment {
        DOCKERFILE_PATH = 'Dockerfile' // Path to your Dockerfile
        IMAGE_NAME = 'my-node-app' // Name for your Docker image
        NODEJS_VERSION = '14' // Node.js version you want to use
        DOCKER_HUB_REPO = 'uthycloud/nodejs-app' // Replace with your Docker Hub username and repo name
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    def dockerImage = docker.build("${env.DOCKER_HUB_REPO}", "--build-arg NODEJS_VERSION=${env.NODEJS_VERSION} -f ${env.DOCKERFILE_PATH} .")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'credentials') {
                        // Define the Docker image you want to push
                        def dockerImage = docker.image("${env.DOCKER_HUB_REPO}")

                        // Push the Docker image to Docker Hub
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                script {
		    sh "ssh -i /home/ubuntu/nodejs.pem ubuntu@ec2-18-144-48-99.us-west-1.compute.amazonaws.com 'docker pull uthycloud/nodejs-app:latest && docker run -d --name node-app uthycloud/nodejs-app:latest'"
                }
            }
        }
    }
}
