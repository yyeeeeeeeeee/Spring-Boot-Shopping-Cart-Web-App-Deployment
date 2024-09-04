pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-cred' // Docker credentials ID
        DOCKER_IMAGE = 'yongyee/shopping-cart:latest'  // Docker image name
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yyeeeeeeeeee/Spring-Boot-Shopping-Cart-Web-App-Deployment.git'
            }
        }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: DOCKER_CREDENTIALS_ID) {
                        sh "docker build -t ${DOCKER_IMAGE} -f Dockerfile ."
                    }
                }
            }
        }

        stage('Publish Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: DOCKER_CREDENTIALS_ID) {
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }
}
