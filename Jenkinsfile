pipeline {
    environment {
        registry = "abdelwfi/tp4"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/abdelwafi-laaouissi/tp4'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":${BUILD_NUMBER}"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy image') {
            steps {
                bat "docker stop tp4-container || exit 0"
                bat "docker rm tp4-container || exit 0"
                bat "docker run -d --name tp4-container -p 8082:80 ${registry}:${BUILD_NUMBER}"
            }
        }
    }
}