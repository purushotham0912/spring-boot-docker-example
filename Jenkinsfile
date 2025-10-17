pipeline {
    agent any
    environment {
        APP_NAME = 'springboot-docker-app'
        APP_PORT = '8080'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/purushotham0912/spring-boot-docker-example.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t ${APP_NAME}:latest ."
            }
        }
        stage('Docker Stop & Remove') {
            steps {
                sh """
                if [ \$(docker ps -q -f name=${APP_NAME}) ]; then
                    docker stop ${APP_NAME}
                    docker rm ${APP_NAME}
                fi
                """
            }
        }
        stage('Docker Run') {
            steps {
                sh "docker run -d --name ${APP_NAME} -p ${APP_PORT}:${APP_PORT} ${APP_NAME}:latest"
            }
        }
    }
}
