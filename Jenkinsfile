pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker-cred' // Jenkins credentials ID
        DOCKERHUB_USERNAME = 'salona1993'
        DOCKERHUB_REPO = 'slab-ai'            // Predefined Docker Hub repository
    }

    stages {
        stage('Build and Push paymentService') {
            steps {
                script {
                    def imageTag = "${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}/paymentservice:${env.BUILD_NUMBER}"
                    
                    // Build Docker image
                    sh """
                        cd backend/paymentService
                        sudo docker build -t ${imageTag} .
                    """
                    
                    // Login and Push Docker image
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            sudo docker push ${imageTag}
                        """
                    }
                }
            }
        }

        stage('Build and Push projectService') {
            steps {
                script {
                    def imageTag = "${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}/projectservice:${env.BUILD_NUMBER}"
                    
                    // Build Docker image
                    sh """
                        cd backend/projectService
                        sudo docker build -t ${imageTag} .
                    """
                    
                    // Login and Push Docker image
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            sudo docker push ${imageTag}
                        """
                    }
                }
            }
        }

        stage('Build and Push userService') {
            steps {
                script {
                    def imageTag = "${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}/userservice:${env.BUILD_NUMBER}"
                    
                    // Build Docker image
                    sh """
                        cd backend/userService
                        sudo docker build -t ${imageTag} .
                    """
                    
                    // Login and Push Docker image
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            sudo docker push ${imageTag}
                        """
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
