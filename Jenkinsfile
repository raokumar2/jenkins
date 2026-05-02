pipeline {
    agent any
    environment {
        // Replace with your Docker Hub username and image name
        DOCKER_IMAGE = "raokumar2/firstapp"
        DOCKER_HUB_CREDS = credentials('docker-hub-creds') 
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Pulls code from the Git repo where this Jenkinsfile lives
                checkout jenkins
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Builds the image using the Dockerfile in your root directory
                    dockerApp = ("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    // Authenticates and pushes the image
                    docker.withRegistry('', 'docker-hub-creds') {
                        dockerApp.push("${env.BUILD_ID}")
                        dockerApp.push("latest")
                    }
                }
            }
        }
    }
    post {
        always {
            // Optional: Clean up local images to save disk space
            sh "docker rmi ${DOCKER_IMAGE}:${env.BUILD_ID}"
        }
    }
}
