pipeline {
    agent any

    tools {
        jdk 'JDK17'     
        maven 'Maven3'  
    }

    environment {
        DOCKER_IMAGE = 'sairajsharma/finance:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sharmasairaj/finance-Project.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '410ab98a-4945-49dc-b8c0-f9066ec7e97f', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker run -d --name finance-container -p 8081:8080 ${DOCKER_IMAGE}"
                }
            }
        }
    }
}
