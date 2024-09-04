pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yyeeeeeeeeee/Spring-Boot-Shopping-Cart-Web-App-Deployment.git'
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
                    docker.build('yongyee/shopping-cart:latest')
                }
            }
        }

        stage('Publish Docker Image') {
            steps {
                script {
                    docker.image('shopping-cart:latest').push('latest')
                }
            }
        }
    }
}
