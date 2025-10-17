pipeline {
    agent any
    environment {
        APP_NAME = 'springboot-docker-app'
        APP_PORT = '8080'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/purushotham0912/spring-boot-docker-example.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests -T 1C -Dmaven.compiler.fork=true'
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
                    docker stop ${APP_NAME} || true
                    docker rm ${APP_NAME} || true
                fi
                """
            }
        }
        stage('Docker Run') {
            steps {
                sh "docker run -d --name ${APP_NAME} -p ${APP_PORT}:${APP_PORT} --restart unless-stopped ${APP_NAME}:latest"
            }
        }
    }
}
