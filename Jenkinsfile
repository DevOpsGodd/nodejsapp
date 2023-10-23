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
                    def remoteServer = 'ec2-18-144-48-99.us-west-1.compute.amazonaws.com'
                    def remoteUser = 'ubuntu' // Replace with your SSH username
                    def privateKeyFile = '/user/ubuntu/nodejs.pem' // Replace with the path to your .pem file

                    // SSH into the remote server using the private key
                    sshCommand remote: [
                        host: remoteServer,
                        user: remoteUser,
                        identityFile: privateKeyFile
                    ], command: '''
                        # Copy the Bash script to the remote server
                        scp -i ${privateKeyFile} /user/ubuntu/bash.sh ${remoteUser}@${remoteServer}:/home/ubuntu/

                        # Log in to the remote server and execute the script
                        ssh -i ${privateKeyFile} ${remoteUser}@${remoteServer} 'bash /home/ubuntu/bash.sh'
                    '''
                }
            }
        }
    }
}
