pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker-cred' // Jenkins credentials ID
        DOCKERHUB_USERNAME = 'salona1993'
    }

    stages {
        stage('Build and Push Microservices') {
            steps {
                script {
                    // List of microservices
                    def services = ['slab-AI/backend/paymentService', 'slab-AI/backend/projectService','slab-AI/backend/userService']

                    // Loop through each service
                    services.each { service ->
                        dir("${service}") { // Enter the service directory
                            echo "Building and Pushing Docker Image for ${service}"

                            def imageTag = "${DOCKERHUB_USERNAME}/${service.toLowerCase()}:${env.BUILD_NUMBER}"

                            // Build Docker image
                            sh "sudo docker build -t ${imageTag} ."

                            // Login to Docker Hub
                            withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }

                            // Push Docker image to Docker Hub
                            sh "sudo docker push ${imageTag}"
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker images after the build
                sh "docker system prune -f || true"
            }
        }
    }
}
