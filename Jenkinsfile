pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment.git'
            }
        }

        stage('Build') {
            steps {
                powershell 'mvn package' // Runs the Maven package command to compile the project and package it into a JAR
            }
        }

        stage('Test') {
            steps {
                powershell 'mvn test' // Runs the Maven test command to execute unit tests
            }
        }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    docker.build('shopping-cart:latest')
                }
            }
        }

        stage('Publish Docker Image') {
            steps {
                withDockerRegistry([credentialsId: '78e6fab5-8a1f-4f3d-be7a-f09e7a5bcbf7', url: 'https://index.docker.io/v1/']) {
                    script {
                        docker.image('shopping-cart:latest').push('latest')
                    }
                }
            }
        }
    }
}
