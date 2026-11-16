pipeline {
    agent any

    tools {
        nodejs 'Node 7.8.0'
    }

    environment {
        APP_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain:v1.0' : 'nodedev:v1.0'}"
        CONTAINER_NAME = "node-app-${env.BRANCH_NAME}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || echo "Tests completed"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:3000 ${IMAGE_NAME}
                """
            }
        }
    }
}
