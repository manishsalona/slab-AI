pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/manishsalona/slab-AI.git'
        DOCKER_CRED_ID = 'docker-cred'
        DOCKERHUB_USERNAME = 'salona1993'
        DOCKERHUB_REPO = 'slab-ai'
        DOCKER_AUTH_SERVICE = './backend/paymentService'
        DOCKER_BASE_SRVICE = './backend/projectService'
        DOCKER_CALCULATION_SERVICE = './backend/calculationService'
        DOCKER_API_SERVICE = './backend/userService'
        
    }

    stages {
        stage('Clone the source code') {
            steps {
                git branch: 'main', url: "${env.GIT_REPO}"
            }
        }
        stage('Build the docker images auth service'){
            steps {
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:payment-service-${env.BUILD_ID}", "${env.DOCKER_AUTH_SERVICE}")
                }
            }
        }
        stage('Build the docker images base service'){
            steps {
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:project-service-${env.BUILD_ID}", "${env.DOCKER_BASE_SRVICE}")
                }
            }
        }
        stage('Build the docker images base service'){
            steps {
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:user-service-${env.BUILD_ID}", "${env.DOCKER_BASE_SRVICE}")
                }
            }
        }
        stage('Push the Image to DockerHub Repository') {
            steps {
                script {
                    docker.withRegistry('', "${env.DOCKER_CRED_ID}") {
                        docker.image("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:payment-service-${env.BUILD_ID}").push()
                        docker.image("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:project-service-${env.BUILD_ID}").push()
                        docker.image("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:user-service-${env.BUILD_ID}").push()
                    }
                }
            }
        }
    }
}