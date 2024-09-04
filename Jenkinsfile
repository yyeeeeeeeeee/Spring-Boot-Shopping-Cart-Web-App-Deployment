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
                sh "mvn package -DskipTests=true" // Runs the Maven package command to compile the project and package it into a JAR
            }
        }

        stage('Test') {
            steps {
                sh "mvn test -DskipTests=true" // Runs the Maven test command to execute unit tests
            }
        }
pipeline {
    agent any

    tools {
        jdk 'JDK-21' 
    }

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


        stage('Deploy') {
            steps {
                script {
                    sh 'docker build -t shopping-cart:latest .'
                    
                    sh '''
                        docker run -d \
                        --name shopping-cart \
                        -p 8070:8070 \
                        shopping-cart:latest
                    '''
                        }
                    }
                }
            }
        }
    }
}
