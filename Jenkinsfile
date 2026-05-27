pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        IMAGE_NAME = "snakegame"
        CONTAINER_NAME = "snakegame-container"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/iter-palak-goyal/SnakeGame-SpringBoot.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '/opt/homebrew/bin/docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                /opt/homebrew/bin/docker stop $CONTAINER_NAME || true
                /opt/homebrew/bin/docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                /opt/homebrew/bin/docker run -d \
                --name $CONTAINER_NAME \
                -p 9090:8080 \
                $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Snake Game deployed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}