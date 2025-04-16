pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "resume-analyzer-image"
        DOCKER_CONTAINER = "resume-analyzer-container"
        PORT = "8501"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Cloning from the main branch
                git branch: 'main', url: 'https://github.com/akritiprabha/Smart-Ai-Resume-.git'
            }
        }

        stage('Remove Old Container and Image') {
            steps {
                echo '=== Stopping and Removing Old Container ==='
                // Stop the container if it's running
                bat 'docker ps -q -f name=%DOCKER_CONTAINER% && docker stop %DOCKER_CONTAINER% || exit 0'
                // Remove the container if it exists
                bat 'docker ps -a -q -f name=%DOCKER_CONTAINER% && docker rm %DOCKER_CONTAINER% || exit 0'

                echo '=== Removing Old Docker Image ==='
                // Remove the image only if it exists
                bat 'docker images -q %DOCKER_IMAGE% && docker rmi %DOCKER_IMAGE% || exit 0'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '=== Building New Docker Image ==='
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '=== Running New Container ==='
                bat "docker run -d -p %PORT%:8501 --name %DOCKER_CONTAINER% %DOCKER_IMAGE%"
            }
        }
    }
}
