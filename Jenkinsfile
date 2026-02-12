pipeline {
    agent any

    environment {
        ACR_LOGIN_SERVER = "acrjenkinsdemo123.azurecr.io"
        IMAGE_NAME = "java-demo"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  sudo docker build -t ${ACR_LOGIN_SERVER}/${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Login to Azure Container Registry') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'acr-creds',
                    usernameVariable: 'ACR_USER',
                    passwordVariable: 'ACR_PASS'
                )]) {
                    sh '''
                      sudo docker login ${ACR_LOGIN_SERVER} -u ${ACR_USER} -p ${ACR_PASS}
                    '''
                }
            }
        }

        stage('Push Image to ACR') {
            steps {
                sh '''
                  sudo docker push ${ACR_LOGIN_SERVER}/${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }
}

